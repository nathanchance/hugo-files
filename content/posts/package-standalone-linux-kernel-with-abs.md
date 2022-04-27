---
title: "Package a standalone Linux kernel using the Arch Linux Build System"
date: 2022-04-27T14:06:01-07:00
tags:
  - clangbuiltlinux
  - linux
---

As a Linux kernel developer, I will often need to build and boot new kernels to hunt down issues or test new functionality for regressions. While it is possible to manually install these kernels on machines, it is easiest to use the distribution's package manager, as the kernel does not need to be built on the machine it is being installed on. With `.deb` and `.rpm`-based systems, it is easy to build a kernel package within the kernel source itself, using the `bindeb-pkg` and `binrpm-pkg` targets respectively. However, for Arch Linux, my distribution of choice, that is not so simple. Furthermore, when doing certain types of development, such as bisecting an issue, it is more convenient to do all the building in an actual source tree, rather than one that is managed by the Arch Build System. The following process might not be the most efficient or optimal way to do this process (the Arch wiki has [a whole article](https://wiki.archlinux.org/title/Bisecting_bugs_with_Git) about doing a `git bisect` with a `PKGBUILD`) but it works for me :)

The general idea is to let `makepkg` only do the final packaging, which frees us up to run whatever commands we need in order to generate the kernel and modules.

NOTE: This guide assumes some familiarity with the Arch Build System and building a Linux kernel.

## 1. Build the kernel

For the following example, we will build a mainline Linux kernel. First, grab the kernel source.

```
$ git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/
$ cd linux
```

Once the source is downloaded, set up the version string file like Arch does in the main `linux` `PKGBUILD`, which helps with identifying the current kernel running.

```
$ scripts/setlocalversion --save-scmversion
```

I recommend adding a suffix, such as `-debug`, which will help you make sure you are running the correct kernel.

```
$ echo -debug >localversion.10-pkgname
```

Grab the latest Arch Linux configuration to make sure that your kernel will have all the drivers that the stock kernel does. If you are running a stock kernel, it is available at `/proc/config.gz`:

```
$ zcat /proc/config.gz >.config
```

Otherwise, you can fetch it from the Arch Linux repo

```
$ curl -LSso .config https://github.com/archlinux/svntogit-packages/raw/packages/linux/trunk/config
```

Bring the configuration up to date without any input; if you do want to be prompted, use `oldconfig` instead of `olddefconfig`. There might be some warnings but they should not be problematic.

```
$ make -j"$(nproc)" olddefconfig
```

At this point, you can customize the kernel you are about to build by running `make menuconfig`. If you want to save some time compiling, consider using `make localmodconfig` to only build the modules that your system is doing to run. I recommend installing [`modprobed-db`](https://wiki.archlinux.org/title/Modprobed-db) and passing in the database it generates like so:

```
$ make -j"$(nproc)" LSMOD=$HOME/.config/modprobed.db localmodconfig
```

Next, we dump the kernel version to a file, which we will use in the packaging stage to ensure our files all end up in the correct location.

```
$ make -s kernelrelease >version
```

Finally, we will build the kernel, which can take some time depending on your hardware and such.

```
$ make -j"$(nproc)"
```

## 2. Set up `PKGBUILD` and packaging space

Once the kernel is all built, we need to set up our `PKGBUILD` and packaging area.

I usually maintain the packaging in a random folder on my hard drive. I will be using this random folder throughout the rest of the example, feel free to change it to wherever is convenient for you. For ease of iteration, which I will touch on later, I recommend you maintain this OUTSIDE of the git repository you are working in.

```
$ pkgroot="$HOME/tmp/linux-pkgbuild"
$ mkdir -p "$pkgroot"
```

Create `"$pkgroot"/PKGBUILD` with your favorite editor. My typical `PKGBUILD` for these situations looks like:

```
pkgname=linux-debug
pkgver=
pkgrel=1
arch=(x86_64)
license=(GPL2)
options=('!strip')

package() {
  pkgdesc="The Linux kernel and modules"
  depends=(coreutils kmod initramfs)
  optdepends=('crda: to set the correct wireless channels of your country'
              'linux-firmware: firmware images needed for some devices')
  provides=(VIRTUALBOX-GUEST-MODULES WIREGUARD-MODULE)
  replaces=(virtualbox-guest-modules-arch wireguard-arch)

  local pkgroot="${pkgdir//\/pkg\/$pkgname/}"
  rm -rf "$pkgroot"/pkg
  cp -rv "$pkgroot"/pkg-ext "$pkgroot"/pkg
}

# vim:set ts=8 sts=2 sw=2 et:
```

Basically, we are doing to manually package everything in `pkg-ext/`, then we are just going to copy that to `pkg/`, and let `makepkg` do the rest.

## 3. Package the kernel

Once the packaging folder and `PKGBUILD` have been set up, we are ready to put everything together! This is basically taken straight from the Arch Linux [`linux` `PKGBUILD`](https://github.com/archlinux/svntogit-packages/blob/packages/linux/trunk/PKGBUILD), check the `_package()` function for any updates to this process that might occur down the road.

Set up variable for future use, which ensures everything gets placed in the right location (`pkgroot` was set above, `linux-debug` might be different depending on your `pkgname` choice in your `PKGBUILD`):

```
$ pkg=linux-debug
$ pkgdir=$pkgroot/pkg-ext/$pkg
$ modulesdir="$pkgdir/usr/lib/modules/$(<version)"
```

Install the kernel:

```
$ install -Dm644 "$(make -s image_name)" "$modulesdir"/vmlinuz
```

Set up `pkgbase`, which is needed for `mkinitcpio`:

```
$ echo "$pkg" | install -Dm644 /dev/stdin "$modulesdir"/pkgbase
```

Install modules:

```
$ make -j"$(nproc)" DEPMOD=/doesnt/exist INSTALL_MOD_PATH="$pkgdir"/usr INSTALL_MOD_STRIP=1 modules_install
```

Remove `build` and `source` symlinks:

```
$ rm "$modulesdir"/{build,source}
```

Update `pkgver` in your `PKGBUILD`, which helps ensure you installed the correct kernel:

```
$ sed -i "s/pkgver=.*/pkgver=$(git describe | sed 's/-/./g')/g" "$pkgroot"/PKGBUILD
```

Finally, package it up!

```
$ cd "$pkgroot"
$ makepkg -R
```

After that, install it with `pacman` and configure your bootloader, [just like you would as if you were installing an official kernel](https://wiki.archlinux.org/title/Kernel). Make sure not to remove the stock `linux` package, as you might run into issues that requires a revert!

## Iteration

This whole process is very easily scriptable. If you need to do this process back to back, I would recommend:

* Removing `$pkgroot/pkg`, `$pkgroot/pkg-ext`, and `$pkgroot/*.tar.zst` before building the next kernel, as that will prevent you from accidentally packaging an old kernel (i.e., skipping the third step).

* Run `git clean -fxdq` in the `linux` folder between builds and repeat step 1 fully (aside from the initial clone), which will prevent weird build system cache invalidation issues.

## Getting help

I am happy to correct any part of this guide that felt unnecessary or confusing or improve it with any additional information. Feel free to comment below or reach out to me via [Twitter](https://twitter.com/nathanchance) or [email](mailto:nathan@kernel.org) with any feedback!