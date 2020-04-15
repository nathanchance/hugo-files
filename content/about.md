---
title: "About"
date: 2020-04-14T19:09:55-07:00
---

![](/profile.jpg)

I am a 25 year old university student at Grand Canyon University, majoring in Information Technology and an amateur Linux kernel and LLVM hacker.

I am interested in performance analysis, compilers, security, and operating systems.

Below are links to some of my open source contributions. If you have any questions about them, feel free to reach out to me with the links on the home page.


## ClangBuiltLinux

ClangBuiltLinux is a collaborative organization between several engineers across different companies like Google, Linaro, and IBM to improve building the Linux kernel with the LLVM tools such as clang and lld. I have contributed various fixes to [the mainline Linux kernel](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/log/?qt=author&q=Nathan+Chancellor) and [LLVM](https://github.com/llvm/llvm-project/commits/master?author=nathanchance), helped triage and debug issues on [the issue tracker](https://github.com/ClangBuiltLinux/linux/issues?q=commenter%3Anathanchance), improved the continuous integration setup through a set of [build scripts](https://github.com/ClangBuiltLinux/continuous-integration/commits/master?author=nathanchance), [boot scripts](https://github.com/ClangBuiltLinux/boot-utils/commits/master?author=nathanchance), and [Docker images](https://github.com/ClangBuiltLinux/dockerimage/commits/master?author=nathanchance), and [developed a set of toolchain build scripts](https://github.com/ClangBuiltLinux/tc-build/commits/master?author=nathanchance) for myself and others to use for consistent testing/developing.


## Flash Kernel

I supported the [Pixel 2 (XL) [walleye/taimen]](https://github.com/nathanchance/wahoo), [Pixel (XL) [sailfish/marlin]](https://github.com/nathanchance/marlin), [OnePlus 6 (enchilada)](https://github.com/nathanchance/op6), [OnePlus 5/T (cheeseburger/dumpling)](https://github.com/nathanchance/op5), and [Nexus 6P (angler)](https://github.com/nathanchance/angler).

All of my kernels focused on stability/security by merging in the stable updates from [kernel.org](https://www.kernel.org), any relevant updates from Qualcomm, and building with newer compilers to fix warnings during compilation. Below are the highlights for each kernel.

* Pixel 2 (XL): First to merge in linux-stable and build with up to date versions of Clang. Added EAS 1.5 from kernel/common.

* Pixel (XL): First standalone custom kernel to be compiled properly with Clang, fixing the vast majority of warnings.

* OnePlus 6: I recieved this device as part of [OnePlus's developer program](https://www.xda-developers.com/oneplus-6-developer-application/). I was the first to merge in linux-stable and the updates from Code Aurora Forum, build with Clang, and port over the 32-bit vDSO for arm64 from the Pixel 2 kernels (initially created on a 4.4 base).


## Miscellaneous open source contributions

* [android-linux-stable](https://github.com/android-linux-stable): Several Android kernel trees with the latest stable tags from [kernel.org](https://www.kernel.org) merged into them, along with conflict resolution notes and a how-to process for other developers and information for users to understand the process. Testing includes merging into my own Flash Kernel repositories linked above and building with all of the relevant defconfigs/compilers. I report results back to the stable tree maintainers, receiving praise for my efforts on a couple occasions ([one](https://lore.kernel.org/lkml/20171117083016.GA20306@kroah.com/) and [two](https://lore.kernel.org/lkml/20180805140301.GA17056@kroah.com/)). I streamlined the maintenance of these repos into [a script](https://github.com/nathanchance/scripts/blob/master/workstation/als).

* [android-kernel-clang](https://github.com/nathanchance/android-kernel-clang): Collected the core Clang patchset for Android kernels from the Pixel 2 and Chromium kernel repositories, supplementing them with fixes for warnings from out of tree code.

* [AnyKernel3](https://github.com/osm0sis/AnyKernel3/commits/master?author=nathanchance): A kernel flashing utility for TWRP, responsible for unpacking the boot image, applying any requested ramdisk changes, repacking the files, and flashing them to the boot image partition.

* [kernel/common](https://android-review.googlesource.com/q/(project:kernel/common+OR+project:kernel/manifest)+owner:natechancellor%2540gmail.com): Google's common Android kernel.

* [sonyxperiadev/kernel](https://github.com/sonyxperiadev/kernel/pulls?q=author%3Anathanchance): The Linux kernel used for [Sony's Open Devices program](https://developer.sony.com/develop/open-devices/).


## Miscellaneous interesting personal repos

* [My scripts](https://github.com/nathanchance/scripts): The scripts I use for my workflow and personal configuration files, focusing on increasing productivity by decreasing manual intervention needed to doing repetitive tasks.

* [WSL2-Linux-Kernel](https://github.com/nathanchance/WSL2-Linux-Kernel): My personal kernel for Windows Subsystem for Linux 2.
