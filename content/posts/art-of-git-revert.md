---
title: "Art of git revert"
date: 2020-04-15T15:11:58-07:00
tags:
  - git
---

I have always loved reading [good commit messages](https://chris.beams.io/posts/git-commit/) and I have tried myself to write good commit messages to inspire others to do the same. I see good commit messages as important for two reasons: it allows people who work on a project afterwards to understand the context behind a change (so that you don't have a [denvercoder9 situation](https://xkcd.com/979/)) and it allows other people to get familiar with your project by fully understanding the why behind a change. I learned a lot about the Linux kernel purely through reading the commit messages in certain subsystems.

I see some projects (like amateur Android ROMs and kernels) write really good commmit messages for "normal" commits but leave the default "This reverts commit..." message for reverts, with no additional comments. One of the first points I want to make with this post are __revert commits are still commits__. Just because `git` fills in the commit message does not mean that it should not be supplemented.

Some things worth considering when writing a commit message for a revert:

* __Why is the revert actually being done?__ Is it just not necessary? Did it break something? Leaving out the why of the revert defeats the purpose. Presumably, if the commit was applied in the first place, it had good reason for being there. If it did not, that should be in the commit message of the revert.

* __If it is broken, how would one notice?__ Forks of a common upstream like AOSP or the Linux kernel will often have overlapping commits. If someone finds an issue with one, the visible effects of that issue should be noted so that others have a chance to do their own investigation. For example, if a commit introduces a crash, the output of the stacktrace would be relevant for the commit message so that other people have the opportunity to analyze it.

* __How can issues be avoided in the future?__ Any time that a revert happens, it is a learning opportunity. Where did the breakdown happen in the original commit's process? Was it not reviewed properly (or at all)? Was it tested? If so, how? Are there tests that can expose an issue in the future? If not, is it worth writing one?

Some people are very liberal with reverts or removing commits through rebasing and force pushing. Reverts are an opportunity to look at your software development process and improve it so that they do not happen as much in the future. I did this a lot with my custom Android kernels. Initially, I would rebase and force push my repos when I found issues. Over time, I learned that it is best to fully review and test a commit or series of commits before pushing into a release so that reverting/rebasing was not necessary. It made for a much more stable and painless experience for me and as well as my users.

In future posts, I will explore some ways to justify a change initially so that reverting it does not happen so stay tuned. If you have any questions or comments, feel free to reach out to me on [Twitter](https://twitter.com/nathanchance).