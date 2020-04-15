---
title: "Creating WSL 2 distributions from LXC images"
date: 2020-04-15T12:40:20-07:00
tags:
  - linux
  - wsl2
---

I have been a big fan of Windows Subsystem for Linux 2 as I need Windows for school but I am so used to the command line for remoting into my server and automating various tasks locally. For those of you who do not know, WSL 2 uses a Linux kernel under the hood (which I customize [here](https://github.com/nathanchance/WSL2-Linux-Kernel)) and all of [the various distributions](https://docs.microsoft.com/en-us/windows/wsl/install-manual#downloading-distros) that you can run are basically containers on top of it.

There is a little known option within `wsl.exe` named `--import` that allows you to create a WSL distribution from any Linux distribution's rootfs. I use the images available on the [LXC Jenkins instance](https://jenkins.linuxcontainers.org/). Here is a basic how-to (assuming you already have [WSL 2 installed](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install)):

### 1. Download and extract LXC image

Go to the [LXC images page](https://jenkins.linuxcontainers.org/view/Images/) and select a distribution that you want. I will use Alpine Linux because it is really small and nice for doing quick tasks/tests.

Click on the image link then find the "amd64" version of the image (assuming you have an Intel or AMD processor) with the particular version that you want (it will probably have a green dot). I am using [Alpine's edge version](https://jenkins.linuxcontainers.org/view/Images/job/image-alpine/architecture=amd64,release=edge,variant=default/). You should see a link for a `rootfs.tar.xz`; click on it to download it.

Once it is downloaded, extract it using something like [7-Zip](https://www.7-zip.org/) (I use the "Extract Here" option). Once you have the raw `rootfs.tar` file, we are good to go.

### 2. Import the rootfs

Open up a PowerShell instance (I use Windows Terminal). The format of `wsl.exe --import` is:

```powershell
> wsl.exe --import <distro_name> <distro_folder> <rootfs>
```

I have a `Linux` folder in my user account where I house all of my Linux files so that is where I am going to install it. By default, PowerShell spawns me in my user folder so I am going to use relative paths.

```powershell
> wsl.exe --import alpine .\Linux\alpine .\Downloads\rootfs.tar
```

### 3. Run the distro and start setting it up

Once you have imported the rootfs, the distribution is now available to spawn into with `wsl.exe`:

```powershell
> wsl.exe -d alpine
# uname
Linux
```

I recommend getting the distribution up to date right out of the gate (Google instructions for the specific distribution you chose):

```shell
# apk update
fetch http://dl-cdn.alpinelinux.org/alpine/edge/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/edge/community/x86_64/APKINDEX.tar.gz
v20200319-2352-g9a801feaea [http://dl-cdn.alpinelinux.org/alpine/edge/main]
v20200319-2346-g9aab507aee [http://dl-cdn.alpinelinux.org/alpine/edge/community]
OK: 12421 distinct packages available
# apk upgrade
OK: 39 MiB in 25 packages
```

After this, I recommend setting up a user account like an official WSL distribution would:

```bash
# adduser nathan
Changing password for nathan
New password:
Retype password:
passwd: password for nathan changed by root
```

After this, you should give yourself the ability to use `sudo` to avoid logging into the `root` account constantly. It is recommended that you use `visudo` and insert yourself after the `root    ALL=(ALL) ALL` line. There are plenty of resources on Google for how to give yourself `sudo` access, follow those instead of me :)

Once you have created your user account, you can tell WSL to use it by default when you run `wsl.exe -d <name>` through `/etc/wsl.conf` (obviously replace my name with the one you chose with `adduser` above):

```shell
# cat <<EOF > /etc/wsl.conf
[user]
default = nathan
EOF
```

To have these changes get reflected in the distro, restart it:

```shell
> wsl.exe -d alpine
# whoami
root
# exit
> wsl.exe --terminate alpine
> wsl.exe -d alpine
$ whoami
nathan
```

Hopefully this was somewhat informative. This process might work with Docker images as well since they are usually created from a raw rootfs as well (see [Debian's](https://github.com/debuerreotype/docker-debian-artifacts/tree/c7d149fa1214588199f3f0b8c30851b9cea47c6b/stable) for example), feel free to give it a try. I have been uncovering more and more things with WSL 2 lately that I hope to continue to share here. If you have any questions or comments, feel free to reach out to me on [Twitter](https://twitter.com/nathanchance).