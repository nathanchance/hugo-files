---
title: "Working with git bisect"
date: 2020-04-17T15:23:45-07:00
tags:
  - git
  - linux
  - llvm
---

Before Google+ shut down, I had a post on there giving a brief overview of `git bisect`, which a lot of people found useful. Unfortunately, I forgot to save it and move it somewhere else before the shutdown deadline. As a result, I am going to redo it here and spice it up a bit.

One caveat before I start: there is [great official documentation](https://git-scm.com/docs/git-bisect) for `git bisect`, from which I have taken quite a bit of information. If I say something that contradicts what the documentation says, assume the documentation is right.

## What is git bisect?

`git bisect` allows you to find out specifically which change or commit caused a particular issue. This can be a build error/warning, test failure, or a runtime issue. It does this by taking a good commit and a bad commit then running a binary search on it using your input through `git bisect good` or `git bisect bad`. I will give an overview of how exactly to do this in the next section.

This tool is incredibly powerful because it massively cuts down the time that it takes to hunt down exactly where something went wrong. There are times where you might pull in two hundred commits and run into a weird error in a file that was not touched by any of those commits. Without `git bisect`, testing all two hundred changes individually would take a long time. Additionally, it saves you from manually binary searching yourself. I did this once when I was a `git` noob and it was not fun.

## How to do a bisect

1. Find a good commit.

    `git bisect` only works when there was a point where the issue did not happen. If the issue has happened since the beginning of the project then it is not a regression and the problem will have to be isolated and solved via other means.

2. Start the bisect.

    You start a bisect by running `git bisect start <bad_commit> <good_commit>`. If your current HEAD is the bad commit, you can just put `HEAD` for `<bad_commit>`. These can be branches, tags, or raw commit hashes. For example, if I was bisecting a bug in the Linux kernel that happened between 5.5 and 5.6, I would run `git bisect start v5.6 v5.5.`.

3. Run the test case.

    This can be anything that you need to do to verify the issue. For example, if you are bisecting a bug in a compiler like clang or GCC, you might build the compiler from the current source then take that freshly built compiler and run it to see if it produces the bug. If you are bisecting a kernel not booting, build the kernel and try to boot it.

    A word of caution: Make sure that your test case is as objective and reliable as possible. The result of `git bisect` is only as accurate at the results of the test case. I was tracking down some boot flakiness in the Linux kernel a couple of weeks ago and there were times where the same kernel would boot one time and not boot another time. To make sure of the result when bisecting, I did a boot ten times over and only considered it passing when it booted all ten times.

4. Run `git bisect good` or `git bisect bad` based on that result

    This is pretty self explanatory. If the current source reproduces the bug, run `git bisect bad`. If it does not, run `git bisect good`.

5. Repeat steps 3 and 4 until `git` produces a bad commit.

    `git bisect` will tell you how many more runs it approximately has to do before it can find a bad commit so you are not left in the dark. When `git bisect` has converged on the commit, it will print out the commit hash and its message.

6. Confirm result.

    Once `git bisect` is done, you are left either on the bad commit or the commit right before, depending on how `git bisect` converged. At this point, I recommend saving the output of `git bisect log` to a file like so:

    ```bash
    $ git bisect log > bisect.log
    ```

    It can be handy for reproducing the result later or allowing someone else to confirm the results.

    After this, I usually will try to see if I can revert the problematic commit on the current version of the software to confirm that it was the source of the issue. To get back to that point, run `git bisect reset`, `git checkout` to the current version of the software, and attempt to `git revert` the commit. Sometimes this is not always easy, especially if there have been further changes around that area. Use your best judgement when deciding to report the issue.

## Some tips and notes around bisecting

`git bisect` has its quirks but it does also have some kind of hidden things. I am going to call out all of the ones that I can think of below.

### git bisect converged on a merge commit

There are times where `git bisect` will claim the first bad commit is a merge commit. This can be for a couple of reasons:

1. Two trees are independently fine but break when put together

    This might seem somewhat obvious but there are times where series of commits are developed completely independently and work fine by themselves; an issue only occurs when they are merged together. This can happen a lot in projects that have subsystems (independently maintained folders/code in one repository).
    
    For example, in the Linux kernel, maintainers typically will base their trees on the previous -rc1 (so changes targeting 5.8 will be based on 5.7-rc1). If a change in the tree of maintainer A changes an interface that is used by a new driver in the tree of maintainer B, a bisect would converge on the merge of whatever tree was merged second because each tree is fine by themselves.

    This is why integration trees (such as linux-next) exist: to try and tease these out so that they can be fixed during the merge instead of afterwards. I reported [an issue](https://lore.kernel.org/linux-next/20200327185055.GA22438@ubuntu-m2-xlarge-x86/) that occurs from a scenario such as the one described above before the Linux kernel's merge window (where all maintainer trees get merged into the main one) but neither maintainer noticed it so the issue had to be fixed up [after the fact](https://lore.kernel.org/lkml/CAHk-=whG84d5bGHU5HLOMgR59BqUcuawPTxGgVDm3JWiWJHi6A@mail.gmail.com/).

    Getting around this usually involves cherry-picking the changes from one branch on top of the other and then running a bisect on that so that the problematic commit can be converged on easier (if it is not immediately obvious).

2. A conflict resolution caused the issue

    Similar to the previous note, there are times where doing a merge results in a conflict, which again happens when two trees are developed independently of each other. A quick `grep` on the logs of the Linux kernel brings up an example of where this happened, commit [4a601f109614](https://git.kernel.org/linus/4a601f109614929aee45e58ca3514ec93da070bb) ("net: mscc: ocelot: adjust maxlen on NPI port, not CPU").

    This is why I try to leave what the conflicts were in my commit messages; it makes it easier for others to audit my choices.

    If you suspect this is the source of a problem, you can redo the merge and see what the conflicts were. For example, we'll use the merge commit above:

    ```bash
    $ git show -s --format=short 1d343579312311aa9875b34d5a921f5e2ec69f0a
    commit 1d343579312311aa9875b34d5a921f5e2ec69f0a
    Merge: a8eceea84a3a 0d81a3f29c0a
    Author: David S. Miller <davem@davemloft.net>

        Merge git://git.kernel.org/pub/scm/linux/kernel/git/netdev/net

    $ git checkout 0d81a3f29c0a
    ...
    HEAD is now at 0d81a3f29c0a Merge tag 'drm-fixes-2020-03-13' of git://anongit.freedesktop.org/drm/drm

    $ git merge a8eceea84a3a
    ...
    CONFLICT (content): Merge conflict in drivers/net/ethernet/mscc/ocelot.c
    ...
    ```

    I will not go into the details but looking at the conflicts and comparing how they were resolved in the actual commit can help narrow down where the bug is coming from (again, if it is not immediately obvious).

### What to do if you chose wrong when bisecting

When I bisect, I will often use the arrow key to scroll back to find my last `git bisect ...` command and hit enter. The problem is I might chose the wrong one (`git bisect good` when I meant to chose `git bisect bad` or vice versa). Unfortunately, that will result in my bisect not converging properly. To fix this, I will have to start my bisect all over, right?

Thankfully, you can use `git bisect log` + `git bisect replay` to get back to the state before that choice. 

1. Run `git bisect log > bisect.log`.

    This dumps the log of the current bisect, which we will edit to remove that last choice.

2. Open `bisect.log` and remove everything after the wrong choice.

    For example, this is the bisect log from one of my recent bisects:

    ```
    $ cat bisect.log
    # bad: [0cc124c186a5211ae5a734fe7708d61b5a150bc2] [llvm-objdump][test] Improve PowerPC branch offset tests
    # good: [5852475e2c049ce29dcb1f0da3ac33035f8c9156] Bump the trunk major version to 11
    git bisect start 'origin/master' 'llvmorg-11-init'
    # good: [60fea2713d3f37d70383aacaa75f61344cc3234a] AMDGPU/GlobalISel: Improve 16-bit bswap
    git bisect good 60fea2713d3f37d70383aacaa75f61344cc3234a
    # good: [31e03317633909c50ead53edf8a19b60698075cc] [ORC] Skip ST_File symbols in MaterializationUnit interfaces / resolution.
    git bisect good 31e03317633909c50ead53edf8a19b60698075cc
    # bad: [263c4a3c75a46996bec1f23264874b7f1334426a] Fix compiler warning when compiling without asserts
    git bisect bad 263c4a3c75a46996bec1f23264874b7f1334426a
    # good: [b28ed9cec8dd7225164eb8c0884aa463654ef3fc] [clang-format] cleanup from D75517
    git bisect good b28ed9cec8dd7225164eb8c0884aa463654ef3fc
    # good: [a46dba24fa35ab52e9a1bbaa52666bcc37859927] [AMDGPU] Extend macro fusion for ADDC and SUBB to SUBBREV
    git bisect good a46dba24fa35ab52e9a1bbaa52666bcc37859927
    # bad: [df90a15b1ac938559a8c3af12126559c1e1e9558] [lldb] Clear all settings during a test's setUp
    git bisect bad df90a15b1ac938559a8c3af12126559c1e1e9558
    # good: [ab69cd0779c529519eb7d26e0fa1b8dfb505f838] [X86] Support intrinsic _mm_cldemote
    git bisect good ab69cd0779c529519eb7d26e0fa1b8dfb505f838
    # bad: [4327a9b46b46d587816f765c619838ea3e01cd19] [AMDGPU] Use progbits type for .AMDGPU.disasm section
    git bisect bad 4327a9b46b46d587816f765c619838ea3e01cd19
    # bad: [c4d23d8854840294bf49c524f93e2be85a401f00] Add a missing include to clang unit tests
    git bisect bad c4d23d8854840294bf49c524f93e2be85a401f00
    # good: [c700e0317c25f3f397a8ba368752c4960f4ab975] [JITLink] Read symbol linkage from the correct field.
    git bisect good c700e0317c25f3f397a8ba368752c4960f4ab975
    # bad: [b47c9f535c8a0fffeb7634a82e3901d416915938] [libc] Add initial assert definition
    git bisect bad b47c9f535c8a0fffeb7634a82e3901d416915938
    # bad: [6aebf0ee56e52afad3887b4230d7ed1beaf0bede] Specify branch probabilities for callbr dests
    git bisect bad 6aebf0ee56e52afad3887b4230d7ed1beaf0bede
    # good: [c4cbc5806218bf4232523771805580fa40b83244] [NFC][PowerPC] Add a new MIR file te test ppc-early-ret pass
    git bisect good c4cbc5806218bf4232523771805580fa40b83244
    # first bad commit: [6aebf0ee56e52afad3887b4230d7ed1beaf0bede] Specify branch probabilities for callbr dests
    ```

    Let's say that I meant to mark `a46dba24fa35ab52e9a1bbaa52666bcc37859927` as `bad` instead of `good`. I would edit `bisect.log` to look like this:

    ```
    $ cat bisect.log
    # bad: [0cc124c186a5211ae5a734fe7708d61b5a150bc2] [llvm-objdump][test] Improve PowerPC branch offset tests
    # good: [5852475e2c049ce29dcb1f0da3ac33035f8c9156] Bump the trunk major version to 11
    git bisect start 'origin/master' 'llvmorg-11-init'
    # good: [60fea2713d3f37d70383aacaa75f61344cc3234a] AMDGPU/GlobalISel: Improve 16-bit bswap
    git bisect good 60fea2713d3f37d70383aacaa75f61344cc3234a
    # good: [31e03317633909c50ead53edf8a19b60698075cc] [ORC] Skip ST_File symbols in MaterializationUnit interfaces / resolution.
    git bisect good 31e03317633909c50ead53edf8a19b60698075cc
    # bad: [263c4a3c75a46996bec1f23264874b7f1334426a] Fix compiler warning when compiling without asserts
    git bisect bad 263c4a3c75a46996bec1f23264874b7f1334426a
    # good: [b28ed9cec8dd7225164eb8c0884aa463654ef3fc] [clang-format] cleanup from D75517
    git bisect good b28ed9cec8dd7225164eb8c0884aa463654ef3fc
    # bad: [a46dba24fa35ab52e9a1bbaa52666bcc37859927] [AMDGPU] Extend macro fusion for ADDC and SUBB to SUBBREV
    git bisect bad a46dba24fa35ab52e9a1bbaa52666bcc37859927
    ```

    Basically, you need to remove everything after the incorrect choice and you need to change the line where you made the wrong choice to make the right one. I also edited the comment (lines that start with `#` are not run as commands) but that is not necessary.

    Save that file once you have properly edited it.

3. Restart the bisect using the file.

    To do this, you will run the following commands:

    ```
    $ git bisect reset

    $ git bisect replay bisect.log
    ```

    The `git bisect reset` gets us back to where we were when we ran `git bisect start` and the `git bisect replay` uses the `bisect.log` file to get us back to where we should have been been. Continue bisecting from there!

### Automating bisects

If there is anything that you need to know about me, it is that I LOVE automation. If I am going to run a command more than twice, I usually automate it. I could not live without [my scripts](https://github.com/nathanchance/scripts). As a result, I love using `git bisect run`.

`git bisect run` takes a shell script and uses it to automate running `git bisect good` or `git bisect bad`. This helps avoid issues like in the previous section, where choosing wrong means that you go down the wrong path and end up at the wrong problematic commit.

I am not going to go into a huge amount of detail about writing shell scripts as I assume if you care about something like this, you understand how to do that.

To do an automated bisect, you start the same way (`git bisect start <bad_ref> <good_ref>`) but instead of running the test case and choosing `git bisect good`/`git bisect bad`, you run `git bisect run <path_to_script>`.

When writing the script, you should make sure that the exit code is `0` if `git bisect good` should be run and exits with an error code between `1` and `127` if `git bisect bad` should be run.

If the source code is untestable (cannot be built or there was some other breakage independent of the issue being tracked down), the script should exit with `125` so that `git bisect skip` is run. I will go over that command in the next section.

You can see an example of a `git bisect run` script that I made [here](https://github.com/ClangBuiltLinux/linux/issues/657). There are a couple in the `git bisect` documentation as well.

To get accurate bisect results, you need to make sure that the script handles all possible outcomes and accurately tests for the problem. This varies widely by projects so I cannot give more specific advice than that.

### Dealing with independent breakage

There are times when you are bisecting that the revision cannot be tested because of a problem unrelated to the one that you are bisecting. For example, when bisecting an compiler bug, the compiler cannot be tested if it cannot be built. There are two ways to handle this situation:

1. `git bisect skip`

    `git bisect skip` tells `git` that the revision it has chosen cannot be tested; in other words, it is neither `good` nor `bad` due to reasons outside of your control. `git` will try to find another commit around it to test. The caveat with this option is that if the commit you skipped is right next to the problematic one, `git` will not be able to tell you which one was the bad one (but that is usually easy enough to deduce).

2. Apply a fix on top of the tested revision

    I will often do this if there was a fix for the issue later in the history. For example, there was a change in clang 11 that broke building `scripts/dtc` within the Linux kernel. That was fixed by [this commit](https://git.kernel.org/linus/e33a814e772cdc36436c8c188d8c42d019fda639) but any revision in the Linux kernel that does not have that as an ancestor will fail to build, which can make bisecting difficult.

    What I will do in this case is temporarily apply that fix on top of whatever revision `git` is currently testing. I can do this automatically so that `git bisect run` can handle it.

    ```bash
    $ grep -q "YYLTYPE yylloc" scripts/dtc/dtc-lexer.l && \
    git format-patch -1 --stdout e33a814e772cdc36436c8c188d8c42d019fda639 | \
    git apply -3v
    ```

    This basically says if the line that was removed by the commit is present in the tree, apply (but do not commit) the change to the index.

    After I test this revision, it is important to get rid of that modification before running `git bisect <good|bad>`, otherwise there will be an error.

    ```bash
    $ git reset --hard

    $ git bisect good
    ```

    The `git bisect` documentation that I linked above has an example of doing this with `git bisect run`.

### Finding the commit that fixed something

Sometimes, it might be necessary to see what commit fixed something versus broke something. Unintentional fixes can be just as bad as unintentional breakages. The process is basically the same as the one described in the beginning but involves setting terms that are a little easier to work with.

```bash
$ git bisect start --term-old broken --term-new fixed

$ git bisect fixed <fixed_rev>

$ git bisect broken <broken_rev>
```

After that, the process is the exact same but `git bisect <fixed|broken>` is used instead of `git bisect <good|bad>`.

## Real time example

I recently came across a bug in LLVM exposed by building the RISC-V Linux kernel's `allyesconfig` target. I determined that it was working at [the flang merge](https://github.com/llvm/llvm-project/commit/b98ad941a40c96c841bceb171725c925500fce6c) but it was broken at [top of tree](https://github.com/llvm/llvm-project/commit/314f00a03489c84b764de2a6f4401996865ff281) (at the time).

To bisect this, I used [tc-build](https://github.com/ClangBuiltLinux/tc-build), an LLVM build script that builds a toolchain able to build the Linux kernel. If you run into an issue with LLVM and the Linux kernel, it can be very helpful (and I am more than willing to give a more in-depth tutorial around this if needed).

The basic principle is we build a toolchain and then use that toolchain to build the translation unit that crashes in the kernel and see what happens.

To demonstrate everything, I took a screen recording of my bisect ([run script here](https://gist.github.com/9a9be43ee4eb393952f6ac18e77fb7e2)):

[![asciicast](https://asciinema.org/a/MiNTU9USGiApGE4UidfTtZUUG.svg)](https://asciinema.org/a/MiNTU9USGiApGE4UidfTtZUUG)

When I did this bisect manually, it took me over an hour, between having to make sure that I typed the right command and remembering that I had a bisect running. When I did it automated without `ccache` (to make sure everything worked before I did the recording), it took 22 minutes, of which I was involved for 30 seconds :)

Hopefully this information was helpful in some way! If you have any questions or comments, please feel free to reach out to me on [Twitter](https://twitter.com/nathanchance)