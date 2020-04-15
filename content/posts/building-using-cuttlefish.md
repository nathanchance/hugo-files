---
title: "Building and using Cuttlefish"
date: 2020-01-31T18:52:00-07:00
tags:
  - android
  - clang
  - linux
---

Recently, I stumbled upon [a talk](https://youtu.be/xMtDDEj-02c?t=9502) that Alistair Delva, a Google engineer, gave at the 2018 Linux Plumbers Conference around Cuttlefish, an Android Virtual Device (AVD) that is used to validate the Android platform virtually (i.e. without a separate device). This is something that is really cool because it makes it easy to follow along with upstream Android development and see what changes they are making under the hood, all from `adb shell`. Cuttlefish boots in around 20 seconds on my machine and swapping out kernels is as simple as adding two command flags which I will go over later. If you are running Cuttlefish locally, you can even [view it](https://android.googlesource.com/device/google/cuttlefish/#so-you-want-to-see-cuttlefish) with TightVNC.

A brief demo:

[![asciicast](https://asciinema.org/a/tfDhVZKCL0URqP58lnxuRsKZd.svg)](https://asciinema.org/a/tfDhVZKCL0URqP58lnxuRsKZd)

A few things to note:

1. These commands assume that you understand Linux shell commands and structure; do not blindly copy and paste (even though everything should work without any issues). If you have any questions about what something is specifically doing and I did not make it clear enough here, feel free to reach out to me on Twitter (link on my homepage).

2. All of the commands are specifically for Ubuntu. This is the OS that I use with my [Packet](https://www.packet.com/) servers and all that I really test on. I tested both `bash` and `zsh` without any issues. Debian _should_ work but since we are dealing with fairly low level stuff like KVM, there might be minor differences that are not accounted for here.

## 1. Getting Cuttlefish running

The first thing to do is actually get the images and tools we need downloaded and installed. This has mostly been distilled from [the official README](https://android.googlesource.com/device/google/cuttlefish/#so-you-want-to-try-cuttlefish). Cuttlefish uses KVM for performance so your device must support virtualization. See [this page](http://www.linux-kvm.org/page/Processor_support) for how to determine that and consult Google for how to enable it in your BIOS if it is not already.

To start, we need to grab a bunch of packages that are essential to this process:

```bash
$ sudo apt install -y build-essential devscripts fakeroot git psmisc qemu-kvm unzip zip
```

Next, we will make a folder to contain all of the work we are doing. This can be anywhere but you will need to adjust the rest of the commands for wherever you put it.

```bash
$ mkdir -p "${HOME}"/cuttlefish-playground/src
```

After this, we will clone the android-cuttlefish repo locally. This will configure and install some tools that are necessary to boot Cuttlefish. Most of these tools are for development, the main reason to install this package is to get the groups that Cuttlefish needs configured properly. There is also the option of using a Docker image to do this but when I tried to use it, I could not get it to work.

```bash
$ git clone https://github.com/google/android-cuttlefish "${HOME}"/cuttlefish-playground/src/android-cuttlefish
$ cd "${HOME}"/cuttlefish-playground/src/android-cuttlefish
```

Once the repo has been successfully cloned, we need to install a couple of prerequisite packages specifically for this package we are about to build.

```bash
$ sudo apt install -y cdbs config-package-dev debhelper
```

After those have been installed, we will build the package:

```bash
$ debuild -i -us -uc -b
```

The package should have built successfully. If it did not, see if it says a package is missing or try Googling the error to get it resolved. Once it is built, try to install it.

```bash
$ sudo dpkg -i ../cuttlefish-common_*_amd64.deb
```

That step will fail. The packages that it depends on can be installed along with it by running the following command:

```bash
$ sudo apt-get install -f -y
```

Once it gets installed successfully, the `cuttlefish-*` packages can be removed to avoid cluttering up the workspace.

```bash
$ rm ../cuttlefish-*
```

After this, we need to add our user to the `cvdnetwork` and `kvm` network so that Cuttlefish is allowed to use KVM to boot and configure its network using vsock.

```bash
$ sudo usermod -aG cvdnetwork "${USER}"
$ sudo usermod -aG kvm "${USER}"
```

Once that is done, logout and log back in (or reboot your machine if it is a remote server).

After logging back in, we need to grab prebuilt versions of the Cuttlefish host tools (such as the `launch_cvd` and `stop_cvd` commands) as well as the filesystem images, much like a regular Android device, which include `super.img`, `vendor.img`, and `boot.img`. I will show you how to build these from source if you ever want to hack on Cuttlefish later.

We will first create an area for all of this to exist.

```bash
$ mkdir -p "${HOME}"/cuttlefish-playground/cuttlefish-fs
$ cd "${HOME}"/cuttlefish-playground/cuttlefish-fs
```

The files we need are all available from [ci.android.com](https://ci.android.com). There will be a LOT of different options, the one we care about is currently the second column: `aosp_cf_x86_phone`. If you click on the __userdebug__ button, it will automatically give you the last known good build. From there, click on the button with the arrow then click on __Artifacts__. The files we need to download are `aosp_cf_x86_phone-img-*.zip` and `cvd-host_package.tar.gz`.

This one-liner will grab the zip, unzip it, and remove it. I recommend using the latest, known good build, instead of the one that I have linked here.

```bash
$ curl -LSsO https://ci.android.com/builds/submitted/6173731/aosp_cf_x86_phone-userdebug/latest/aosp_cf_x86_phone-img-6173731.zip && \
unzip *.zip && \
rm *.zip
```

This one-liner will grab the host tools and untar it.

```bash
$ curl -LSs https://ci.android.com/builds/submitted/6173731/aosp_cf_x86_phone-userdebug/latest/cvd-host_package.tar.gz | tar -xzf -
```

Once those have finished downloading, we are ready to run Cuttlefish! We start Cuttlefish using the `launch_cvd` (Launch Cuttlefish Virtual Device).

```bash
$ HOME=${PWD} ./bin/launch_cvd -daemon
```

The `HOME=${PWD}` is present so that some Cuttlefish runtime files get contained to this folder. The `-daemon` flag is so that we can interact with Cuttlefish through `adb shell` in the same terminal window (i.e. it gets sent to the background). You should see a bunch of text fire up then it end with something like:

```
run_cvd I 01-31 20:07:50  9571  9571 main.cc:198] VIRTUAL_DEVICE_BOOT_COMPLETED
launch_cvd I 01-31 20:07:50  9543  9543 launch_cvd.cc:131] run_cvd exited successfully.
```

It takes about 20 seconds for Cuttlefish to boot up for me (which is extremely impressive in my opinion). After that, you can run `adb` to connect to it. You might need to run the command a couple of times so that `adb` can start and connect to the device but once you do, you should be able to interact with it like any other Android device.

```bash
$ ./bin/adb shell
vsoc_x86:/ $
```

I like to see what kernel it is running, which at this point is the `android-5.4` branch, the latest LTS kernel.

```bash
vsoc_x86:/ $ cat /proc/version
Linux version 5.4.15-gb2b96d09ef40 (android-build@abfarm-01061) (Android (6051079 based on r370808) clang version 10.0.1 (https://android.googlesource.com/toolchain/llvm-project b9738d6d99f614c8bf7a3e7c769659b313b88244)) #1 SMP PREEMPT Mon Jan 27 13:04:43 UTC 2020
```

You can see that it actively running tasks by typing in `top`. Once you are done exploring and want to move on, shutdown this instance by calling `stop_cvd`:

```bash
$ HOME=${PWD} ./bin/stop_cvd
```

## 2. Building your own kernel

Now that we have gotten Cuttlefish running, it's time to explore one of the main reasons that I use it, which is validating Android kernels. Android has cool technologies in the kernel such as LTO and CFI, which are not currently in mainline. Replacing Cuttlefish's prebuilt kernel is extremely easy, which is one of the main selling points; it makes it really easy for kernel hackers to get involved with it, especially with Google's kernel build system. This section is expanded from [Google's official documentation](https://source.android.com/setup/build/building-kernels).

First, we need to do is grab `repo`, Google's `git` management tool. If you already have `repo` installed, just skip this portion. Alternatively, you can install `repo` into `/usr/local/bin` so that it is automatically added to `PATH` but for the sake of this tutorial, I keep everything localized as much as possible.

Create a `bin` folder within the Cuttlefish area.

```bash
$ mkdir -p "${HOME}"/cuttlefish-playground/bin
```

Download `repo` into the `bin` folder.

```bash
$ curl -LSso "${HOME}"/cuttlefish-playground/bin/repo https://storage.googleapis.com/git-repo-downloads/repo
```

Make it executable.

```bash
$ chmod a+x "${HOME}"/cuttlefish-playground/bin/repo
```

Add the `bin` folder to `PATH` (NOTE: If you close out of your terminal session or start a new one, you will need to do this everytime):

```bash
$ export PATH=${HOME}/cuttlefish-playground/bin:${PATH}
```

If you have not already configured `git` with your email address and your full name, do so now:

```bash
$ git config --global user.email "you@domain.com"
$ git config --global user.name "Your Name
```

After all of this has been configured, we are going to download the kernel source. Depending on your internet speed, this might take some time.

We will create a folder for that source to be saved in. This will also download all of the prebuilt tools we need, such as the compiler and binaries used during the build process.

```bash
$ mkdir -p "${HOME}"/cuttlefish-playground/kernel-common
$ cd "${HOME}"/cuttlefish-playground/kernel-common
```

Next, we will download the source. I am using the `common-android-mainline` branch since it is the latest available and where most of the new development happens but you can use `common-android-5.4` or `common-android-4.19` branches if you would like. Any time that you want to get the latest new changes, you can just run `repo sync`.

```bash
$ repo init -u https://android.googlesource.com/kernel/manifest -b common-android-mainline
$ repo sync
```

Once it is done downloading, we need to make sure that we have the SSL development headers installed, otherwise our build will error out. Google's build is almost heremetic, except for this:

```bash
$ sudo apt install -y libssl-dev
```

Finally, we are ready to build the kernel. Google's build script takes care of everything as long as we specify a build config, which is `build.config.cuttlefish.x86_64` in our case. We need to copy the `bzImage` into the `dist` folder because Google's `build.config.cuttlefish.x86_64` is only intended to save the `initramfs.img`; however, the `build.config.gki.x86_64` is about to be fully demodularized and we are eventually going to use a different compiler than Google's official one, which will create a mismatch. As a result, we need to do that copy on our own (it isn't strictly necessary, it just makes everything end up in the right spot).

```bash
$ BUILD_CONFIG=common/build.config.cuttlefish.x86_64 ./build/build.sh
$ cp out/android-mainline/common/arch/x86/boot/bzImage out/android-mainline/dist/bzImage
```

Once it is done building, we can use that kernel and ramdisk to boot from. `launch_cvd` has two flags we need to use, `-initramfs_path` (for the ramdisk that has all of the kernel modules) and `-kernel_path` for the actual kernel binary. These two files are available in the `out/<branch>/dist` folder (e.g., `out/android-mainline/dist`). I would recommend exporting this to a variable that can be easily used later.

```bash
$ DIST_FOLDER=$(readlink -f out/android-mainline/dist)
```

Once you do that, we will move back to the `cuttlefish-fs` folder and use these freshly built files to boot Cuttlefish from:

```bash
$ cd "${HOME}"/cuttlefish-playground/cuttlefish-fs
$ HOME=${PWD} ./bin/launch_cvd -daemon -initramfs_path "${DIST_FOLDER}"/initramfs.img -kernel_path "${DIST_FOLDER}"/bzImage
```

After that, we can use `adb shell` to see that we indeed used our own kernel since it shows a newer version, different user/hostname, and the repo that we used and that all of the modules properly loaded.

```bash
$ ./bin/adb shell
vsoc_x86:/ $ cat /proc/version
Linux version 5.5.0-mainline-03266-g1dad8acc36fc (nathan@g2-large-x86) (Android (6051079 based on r370808) clang version 10.0.1 (https://android.googlesource.com/toolchain/llvm-project b9738d6d99f614c8bf7a3e7c769659b313b88244)) #1 repo:common-android-mainline SMP PREEMPT Fri Jan 31 21:42:55
vsoc_x86:/ $ lsmod
Module                  Size  Used by
vsock_loopback         16384  0
vmw_vsock_virtio_transport    24576  3
vmw_vsock_virtio_transport_common    36864  2 vsock_loopback,vmw_vsock_virtio_transport
vsock_diag             16384  0
vsock                  53248  16 vsock_loopback,vmw_vsock_virtio_transport,vmw_vsock_virtio_transport_common,vsock_diag
can_raw                20480  0
can                    28672  1 can_raw
snd_intel8x0           45056  1
snd_ac97_codec        229376  1 snd_intel8x0
ac97_bus               16384  1 snd_ac97_codec
virtio_input           20480  0
virtio_pci             28672  0
virtio_mmio            20480  0
gnss_cmdline           16384  0
gnss_serial            16384  1 gnss_cmdline
virtio_crypto          28672  0
dummy_cpufreq          16384  0
rtc_test               16384  1
dummy_hcd              32768  0
can_dev                32768  0
vcan                   16384  0
virtio_net             53248  0
net_failover           24576  1 virtio_net
failover               16384  1 net_failover
virt_wifi              24576  1
virtio_pmem            16384  1
nd_virtio              16384  1 virtio_pmem
virtio_blk             24576  5
virtio_gpu             57344  0
virtio_console         36864  0
virtio_rng             16384  0
virtio_ring            36864  11 vmw_vsock_virtio_transport,virtio_input,virtio_pci,virtio_mmio,virtio_crypto,virtio_net,nd_virtio,virtio_blk,virtio_gpu,virtio_console,virtio_rng
virtio                 24576  11 vmw_vsock_virtio_transport,virtio_input,virtio_pci,virtio_mmio,virtio_crypto,virtio_net,virtio_pmem,virtio_blk,virtio_gpu,virtio_console,virtio_rng
blk_mq_virtio          16384  1 virtio_blk
binfmt_misc            20480  0
```

Once you are done exploring, stop Cuttlefish.

```bash
$ HOME=${PWD} ./bin/stop_cvd
```

## 3. Using upstream LLVM

One of my hobbies is contributing to [ClangBuiltLinux](https://github.com/ClangBuiltLinux), which involves building various Linux kernels with the master version of Clang (11.0.0 at the time of writing this). As a part of that project, I have developed a script that can build a self-contained version of Clang suitable for building kernels. These steps will show you how to build that version of Clang then use it within the Android build system.

First, we need to install some packages that allow us build Clang efficiently (including Clang itself):

```bash
$ sudo apt install -y bc bison ccache clang cmake flex libelf-dev lld ninja-build python3 u-boot-tools zlib1g-dev
```

Next, we'll grab the repo that contains the Python script:

```bash
$ git clone git://github.com/ClangBuiltLinux/tc-build "${HOME}"/cuttlefish-playground/src/tc-build
```

By default, the script does a two stage LLVM build (builds a small version of LLVM then uses that to build a full version of LLVM).

```bash
$ "${HOME}"/cuttlefish-playground/src/tc-build/build-llvm.py
```

If for some reason your machine cannot handle that, you can just do a stage 1 build.

```bash
$ "${HOME}"/cuttlefish-playground/src/tc-build/build-llvm.py --build-stage1-only --install-stage1-only
```

If you have a machine with good performance, I would recommend using the `--pgo` flag to build with Profile Guided Optimization; this can result in a 20% speed up when compiling kernels.

```bash
$ "${HOME}"/cuttlefish-playground/src/tc-build/build-llvm.py --pgo
```

If for some reason there are any issues using the script, please report them [here](https://github.com/ClangBuiltLinux/tc-build/issues) so I can triage them.

Once the toolchain has been built, we will make it available to easily use by symlinking the `install` folder within `tc-build` into the `prebuilts` folder in `kernel-common`.

```bash
$ ln -s "${HOME}"/cuttlefish-playground/src/tc-build/install "${HOME}"/cuttlefish-playground/kernel-common/prebuilts-master/clang/host/linux-x86/cbl-clang-master
```

After that, we can use `sed` to automatically use that Clang when building the kernel.

```bash
$ cd "${HOME}"/cuttlefish-playground/kernel-common
$ sed -i 's/clang-r.*/cbl-clang-master\/bin/g' common/build.config.common
$ BUILD_CONFIG=common/build.config.cuttlefish.x86_64 ./build/build.sh
$ cp out/android-mainline/common/arch/x86/boot/bzImage out/android-mainline/dist/bzImage
```

Once the build is completed, we just use the same commands as before to boot and test:

```bash
$ DIST_FOLDER=$(readlink -f out/android-mainline/dist)
$ cd "${HOME}"/cuttlefish-playground/cuttlefish-fs
$ HOME=${PWD} ./bin/launch_cvd -daemon -initramfs_path "${DIST_FOLDER}"/initramfs.img -kernel_path "${DIST_FOLDER}"/bzImage
$ ./bin/adb shell
vsoc_x86:/ $ cat /proc/version
Linux version 5.5.0-mainline-03266-g1dad8acc36fc-dirty (nathan@g2-large-x86) (ClangBuiltLinux clang version 11.0.0 (git://github.com/llvm/llvm-project 64cb77b9469207799e570483dadc720dbf12c794)) #1 repo:common-android-mainline SMP PREEMPT Fri Jan 31 23:47:57
vsoc_x86:/ $ exit
$ HOME=${PWD} ./bin/stop_cvd
```

## 4. Building Cuttlefish userspace from scratch

If you want to test the latest and greatest from AOSP's master branch, you can build the things that are in the `cuttlefish-fs` folder from scratch.

This is not small so make sure that you have room on your hard drive for it. A fresh checkout as of today shows:

```bash
$ diskus .
92.68 GB (92,677,562,368 bytes)
```

First, use `repo` to go and grab the full AOSP source tree.

```bash
$ mkdir -p "${HOME}"/cuttlefish-playground/src/aosp
$ cd "${HOME}"/cuttlefish-playground/src/aosp
$ repo init -u https://android.googlesource.com/platform/manifest
$ repo sync
```

Next, we need source Android's environment setup script to adjust the PATH variable and add the functions that it uses.

```bash
$ . build/envsetup.sh
```

After that, we need to use the `lunch` command to tell the build system that we want to build Cuttlefish.

```bash
$ lunch aosp_cf_x86_phone-userdebug
```

Next, we're just going to build the world:

```bash
$ m
```

This will probably take a while since there are usually around 80,000+ files to build. A clean build on [g2.large.x86](https://www.packet.com/cloud/servers/g2-large/) takes about 38 minutes.

Once it is done, you can use the `launch_cvd` command available in `PATH` to use the new files you just build. The `HOME=` assignment should still be used to keep the Cuttlefish runtime files contained but I will keep them in the `out` folder.

```bash
$ HOME=${PWD}/out launch_cvd -daemon
```

Once it has booted, you can access with the `adb` that is in the path.

```bash
$ adb shell
vsoc_x86:/ $ cat /proc/version
Linux version 5.4.15-gb2b96d09ef40 (android-build@abfarm-01061) (Android (6051079 based on r370808) clang version 10.0.1 (https://android.googlesource.com/toolchain/llvm-project b9738d6d99f614c8bf7a3e7c769659b313b88244)) #1 SMP PREEMPT Mon Jan 27 13:04:43 UTC 2020
```

The kernel is still prebuilt but you can use a kernel that you build earlier with this new userspace just like before.

```bash
$ HOME=${PWD}/out launch_cvd -daemon -initramfs_path "${DIST_FOLDER}"/initramfs.img -kernel_path "${DIST_FOLDER}"/bzImage
$ adb shell
vsoc_x86:/ $ cat /proc/version
Linux version 5.5.0-mainline-03266-g1dad8acc36fc-dirty (nathan@g2-large-x86) (ClangBuiltLinux clang version 11.0.0 (git://github.com/llvm/llvm-project 64cb77b9469207799e570483dadc720dbf12c794)) #1 repo:common-android-mainline SMP PREEMPT Fri Jan 31 23:47:57
```

# Conclusion

Hopefully this was information for you! Some further things to potentially explore are things like [adeb](https://github.com/joelagnel/adeb), which allow you to use tracing tools and other Linux utilities on Android to poke around its internals and try and improve or break things or looking into some of the new cool things they are doing like Apex. If you have any questions or problems, feel free to reach out to me on Twitter or GitHub, both of which are linked on my homepage.