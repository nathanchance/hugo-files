---
title: "Bisecting a Linux Kernel boot failure due to changed compiler flags"
date: 2022-04-28T14:21:18-07:00
---

Occasionally, compiling the Linux kernel with a new compiler flag will result in a boot failure. If you are lucky, there will be some output to the serial console but that may not happen if the issue happens in early boot code before the serial driver has loaded. When this happens, it usually requires building part of the kernel without the compiler flag (or the "negative" version of it) to try and figure out the exact translation unit and function that causes the problem. I'll go over this process at a high level to help others who might encounter this same issue.

I will honest up front, this process is not super clean cut; there is some "feel" about it, but I will do my best to explain that right up front. If any of this is confusing, please let me know where you were confused and I will do my best to clarify in this post for future travellers.

## 1. Figure out what flag is causing the issue

Hopefully you are not making multiple code generation flag changes at once ;) but if you are, you need to figure out which flag is causing the problem.

The whole preface of this blog post came from the ClangBuiltLinux issue [INIT_STACK_ALL_ZERO - Framework Laptop system freezes (kernel oops?) on boot](https://github.com/ClangBuiltLinux/linux/issues/1626). The reporter already figured out that `CONFIG_INIT_STACK_ALL_ZERO` was responsible for the change, which we can see in the main kernel `Makefile` corresponds to `-ftrivial-auto-var-init=zero`:

```
$ sed -n '826,832p' Makefile
ifdef CONFIG_INIT_STACK_ALL_ZERO
KBUILD_CFLAGS   += -ftrivial-auto-var-init=zero
ifdef CONFIG_CC_IS_CLANG
# https://bugs.llvm.org/show_bug.cgi?id=45497
KBUILD_CFLAGS   += -enable-trivial-auto-var-init-zero-knowing-it-will-be-removed-from-clang
endif
endif
```

## 2. Figure out if there is a "negative" flag

Once the problematic flag has been uncovered, we want to know if the flag has a "negative" version; in other words, we want to know if there is a flag to turn off the problematic flag. In the case of `-ftrivial-auto-var-init=zero`, we can go to [Clang's cpmmand line reference](https://clang.llvm.org/docs/ClangCommandLineReference.html#cmdoption-clang-ftrivial-auto-var-init) and see that the default of `-ftrivial-auto-var-init=` is `uninitialized`, which means that `-ftrivial-auto-var-init=uninitialized` will "undo" `-ftrivial-auto-var-init=zero`. This allows us to append `-ftrivial-auto-var-init=uninitialized` to certain files to see if the issue disappears, which will allow us to discover the translation unit that has the issue.

If there is not a negative flag, you will have to resort to removing the flag from folders and translation units, which is possible, but I am not going to cover that in this post.

## 3. Strategically start adding negative flag to translation units

The Linux kernel's build system (Kbuild) allows one to include flags for individual translation units (`.o`) and entire folders. You can read about the details of each in the [Linux Kernel Makefiles documentation](https://www.kernel.org/doc/html/latest/kbuild/makefiles.html#compilation-flags). First, we will use `subdir-ccflags-y` to add the negative flag to entire subfolders then slowly work towards individual flags. Picking the folder or folders that we start in is one of the hardest parts of this process, as it is not the same every single time.

To start, it is pretty safe to start with `arch/<your_architecture>/Kbuild` and `mm/Makefile`, as that is all code that runs early in boot, which would look like:

```diff
diff --git a/arch/x86/Kbuild b/arch/x86/Kbuild
index 5a83da703e87..4bb2323eba7c 100644
--- a/arch/x86/Kbuild
+++ b/arch/x86/Kbuild
@@ -30,3 +30,5 @@ obj-$(CONFIG_KEXEC_FILE) += purgatory/

 # for cleaning
 subdir- += boot tools
+
+subdir-ccflags-y := -ftrivial-auto-var-init=uninitialized
diff --git a/mm/Makefile b/mm/Makefile
index 4cc13f3179a5..30717aad76de 100644
--- a/mm/Makefile
+++ b/mm/Makefile
@@ -3,6 +3,8 @@
 # Makefile for the linux memory manager.
 #

+subdir-ccflags-y := -ftrivial-auto-var-init=uninitialized
+
 KASAN_SANITIZE_slab_common.o := n
 KASAN_SANITIZE_slab.o := n
 KASAN_SANITIZE_slub.o := n
```

If the issue is resolved with this diff, we have a known good kernel and a known bad kernel, which means it is possible to do further bisection. If the issue is not resolved, it means that these folders do not have the code responsible for the issue. The next place I would start is somewhere in `drivers/`, as it is possible that a driver problem can cause issues with the device fully starting up. For example, a problem in `drivers/gpu/drm` might result in no graphical output:

```diff
diff --git a/drivers/gpu/drm/Makefile b/drivers/gpu/drm/Makefile
index c2ef5f9fce54..a3ed2f48fa84 100644
--- a/drivers/gpu/drm/Makefile
+++ b/drivers/gpu/drm/Makefile
@@ -3,6 +3,8 @@
 # Makefile for the drm device driver.  This driver provides support for the
 # Direct Rendering Infrastructure (DRI) in XFree86 4.1.0 and higher.

+subdir-ccflags-y := -ftrivial-auto-var-init=uninitialized
+
 drm-y       := drm_aperture.o drm_auth.o drm_cache.o \
                drm_file.o drm_gem.o drm_ioctl.o \
                drm_drv.o \
```

## 4. Start moving down the folder that is problematic

At this point, you will want to try and narrow the problem down to one top level folder (such as `arch/<your_architecture>`, `drivers`, or `mm`) so that you can focus on moving down the directory structure quickly.

Once you have the folder (I'll be using `arch/x86` as an example), you will want to find all the `Makefile`s that are directly below where you started. For example, in `arch/x86`:

```
$ fd -d 2 Makefile arch/x86
arch/x86/Makefile
arch/x86/Makefile.um
arch/x86/Makefile_32.cpu
arch/x86/boot/Makefile
arch/x86/coco/Makefile
arch/x86/crypto/Makefile
arch/x86/entry/Makefile
arch/x86/events/Makefile
arch/x86/hyperv/Makefile
arch/x86/ia32/Makefile
arch/x86/kernel/Makefile
arch/x86/kvm/Makefile
arch/x86/lib/Makefile
arch/x86/math-emu/Makefile
arch/x86/mm/Makefile
arch/x86/net/Makefile
arch/x86/pci/Makefile
arch/x86/platform/Makefile
arch/x86/power/Makefile
arch/x86/purgatory/Makefile
arch/x86/realmode/Makefile
arch/x86/tools/Makefile
arch/x86/um/Makefile
arch/x86/video/Makefile
arch/x86/xen/Makefile
```

We want to remove the original `subdir-ccflags-y` from the higher `Makefile` and move it down into the individual `Makefile`s. Once that is done, boot your kernel to make sure the issue is still fixed, as it should be.

Once that is done, remove the addition to half of the Makefile and see if the issue is resolved. If it is, we know the issue is in the folders that still have the negative flag. If it is not, we know the issue is in the folders that we just removed the negative flag from. From there, move the flags further and further down until you arrive at a folder with no subdirectories. I would recommend committing your changes via `git`, as that will make it easier to verify what kernel you are testing and undoing changes is much easier.

## 5. Bisect individual translation units

Once you have arrived at a folder with just `Makefile` and some `.c` files, you are ready to figure out the problematic translation unit (or units!).

Remove any `subdir-ccflags-y` that you have added. We need to generate a set of `CFLAGS` for the individual translation units to test. Assuming you stil have the object files from your previous build, you can generate this using a shell command such as:

```
$ for file in <subdir>/*.o; do
echo CFLAGS_$(basename "$file") := <negative_flag>
done >><subdir>/Makefile
```

For example, if I were testing `mm/`:

```
$ for file in mm/*.o; do
echo CFLAGS_$(basename "$file") := <negative_flag>
done >>mm/Makefile
```

At this point, make sure the issue is still fixed. If it is, do the same process as above by either deleting or commenting out the flags and seeing if the issue is still resolved. If it is, the issue is in one of the files that still has the flags applied. If the issue comes back, you know the issue is in one of the files that just had the flag removed. Repeat this process until you are left with a minimal set of files that does not have the issue.

## 6. Bisect individual functions

Once there is a set of translation units, it might be possible to further narrow down what function causes the problem using function or variable attributes. By this point, it might be obvious why there is a problem but if not, you will basically apply the attribute to the types that need it and do a similar process.

In the case of `-ftrivial-auto-var-init=uninitialized`, there is `__attribute((uninitialized))`, which can be applied to local variables, such as:

```diff
diff --git a/mm/hmm.c b/mm/hmm.c
index 3fd3242c5e50..54f53bbe3632 100644
--- a/mm/hmm.c
+++ b/mm/hmm.c
@@ -235,7 +235,7 @@ static int hmm_vma_handle_pte(struct mm_walk *walk, unsigned long addr,
        struct hmm_vma_walk *hmm_vma_walk = walk->private;
        struct hmm_range *range = hmm_vma_walk->range;
        unsigned int required_fault;
-       unsigned long cpu_flags;
+       __attribute((uninitialized)) unsigned long cpu_flags;
        pte_t pte = *ptep;
        uint64_t pfn_req_flags = *hmm_pfn;
```

From there, do the same process of applying the attribute, testing, and removing until you are left with a single set of variables. After that, analysis can commence.

## Comments

If you have any problems or need help, feel free to comment down below or reach out to me via [email](mailto:nathan@kernel.org) or [Twitter](https://twitter.com/nathanchance)! I will do my best to keep this post up to date over time.