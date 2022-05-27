---
title: "Setting up a self-hosted GitHub Actions runner in a Fedora virtual machine using libvirt"
date: 2022-05-27T09:21:35-07:00
---

Recently, I started exploring setting up [a self-hosted GitHub Actions runner](https://docs.github.com/en/actions/hosting-your-own-runners) for the work-in-progress LLVM builds that ClangBuiltLinux is looking to distribute on kernel.org, as GitHub Actions hosted runners are pretty underwhelming in terms of performance and we want to soup these builds up with Profile Guided Optimization. Additionallly, GitHub Actions does not have a hosted arm64 Linux option, which is becoming increasingly important with chips such as Apple's M1 getting strong mainline Linux support.

In this guide, I'll go over how I set up a Fedora virtual machine using [libvirt](https://libvirt.org/) to run GitHub Actions workflows, including some of the oddities I ran into. This is not intended to be an end all be all guide but I believe it is important to share knowledge, as I am unlikely to be the last person looking to do this.

## Set up libvirt

I recommend running GitHub Actions in a virtual machine, which ensures that if you ever mess up a workflow or command, you are not risking damaging your host operating system. libvirt seems to be one of the most popular and well supported virtualization solutions on Linux so that is what I went with for this project.

Getting libvirt installed and configured varies by distribution, so I cannot really get too specific here. There are guides for [Arch Linux](https://wiki.archlinux.org/title/Libvirt), [Debian](https://wiki.debian.org/libvirt), and [Fedora](https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/). The important steps are making sure that the `libvirtd` service is enabled and started and your user is a part of the `libvirt` group (or whatever the `libvirt` specific group is on your distribution).

A few notes of issues/quirks I ran into:

1. libvirt has [different URIs (system and session), which have different permissions](https://wiki.libvirt.org/page/FAQ#What_is_the_difference_between_qemu:.2F.2F.2Fsystem_and_qemu:.2F.2F.2Fsession.3F_Which_one_should_I_use.3F). I hate using `sudo` if I do not have to ensure I am not messing my system up unnecessarily. By default, without root, you are in a user session, which only has access to QEMU's user networking mode, instead of the default network, which is faster. Additionally, autostarting virtual machines on boot (which we want for a runner, which should be as available for jobs as possible) is only available for system URIs. If you are part of the `libvirt` group, you can access `qemu:///system` without root but you have to specifically request it via the `--connect` parameter to `virsh` and `virt-install`. To avoid doing that for every single command, you can the `LIBVIRT_DEFAULT_URI` environment variable to `qemu:///session` in your shell start up file ([example for fish](https://github.com/nathanchance/env/commit/5ec0146072da4bd8ba75017fc2ec4e41f2521a50)). This will make `virsh` and `virt-install` use the system URI by default so that everything "just works TM".

2. The default network uses network address translation (NAT) via `iptables` to route traffic to and from the virtual machine. As a result, the virtual machine is not accessible to the network, which is just fine for our use case. If you need more flexibility, check out the [libvirt Networking Handbook](https://jamielinux.com/docs/libvirt-networking-handbook/index.html), which was very informative. I will be concerned with the default network for the rest of this guide.

3. If you are starting virtual machines on boot and you have set the machine to use KVM, the KVM modules must be in your initrd so that they are loaded before init starts, otherwise you might see an error like `"unsupported configuration: Domain requires KVM, but it is not available. Check that virtualization is enabled in the host BIOS, and host configuration is setup to load the kvm modules."`. Again, this varies by distribution; for Arch Linux, you can add your vendor module (`kvm_amd` or `kvm_intel`) to the `MODULES` array in `/etc/mkinitcpio.conf` and regenerate all initrds with `mkinitcpio -P`.

Once it is installed, make sure that the default network is set to automatically start on boot:

```
$ virsh net-autostart default
```

Make sure that the default network is currently started:

```
$ virsh net-start default
```

If it fails saying it is already started, that is obviously fine.

## Create and set up Fedora virtual machine

GitHub Actions supports [a few different distributions](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners#supported-architectures-and-operating-systems-for-self-hosted-runners) for the self-hosted runner application. I prefer Fedora as it is stable while still having up to date packages in its repositories so that is what this guide is going to cover but you can use a different operating system if you prefer; you're just on your own for figuring out how to install the virtual machine using `virt-install` :)

`virt-install` is the command line tool for creating virtual machine and `virsh` is the command line tool for managing them. `virt-manager` is a graphical user interface that helps create and manage virtual machines. I use all of my Linux machines completely headless so this part of the guide is going to cover using the first two tools.

`virt-install` has a ton of different options, see [the man page](https://linux.die.net/man/1/virt-install) for a full explanation of what they do.

### Running `virt-install`

My comnmand on an `x86_64` host looks like:

```
$ virt-install \
--name fedora \
--vcpus $(math $(nproc) / 2) \
--memory $(math $(nproc) x 1024) \
--cpu host \
--network network=default \
--boot uefi \
--location https://download.fedoraproject.org/pub/fedora/linux/releases/36/Server/$(uname -m)/os \
--disk size=50,format=qcow2 \
--virt-type kvm \
--console pty,target_type=serial \
--extra-args "console=ttyS0,115200n8" \
--graphics none
```

The `vcpus`, `memory`, and `disk` options will vary. The [`math` command](https://fishshell.com/docs/current/cmds/math.html) is specific to [`fish`](https://fishshell.com) so you will need to supply actual numbers or use `$(( $(nproc) / 2 ))`/`$(( $(nproc) * 2))` for `bash`/`zsh`. For my use case, I allocated half of the threads/cores of the host system to the virtual machine, 2GB of memory for each vCPU, and a 50GB disk image. The `memory` parameter is in `MiB`, so multiply how many gigabytes of memory you want by 1024.

The `console`, `extra-args`, and `graphics` values are due to running this machine headless; a graphical install might want something different, at which point I would probably just recommend using `virt-manager`.

For an `aarch64` host, the command is almost the same:

```
$ virt-install \
--name fedora \
--vcpus $(math $(nproc) / 2) \
--memory $(math $(nproc) x 1024) \
--cpu host-passthrough \
--network network=default \
--boot uefi \
--location https://download.fedoraproject.org/pub/fedora/linux/releases/36/Server/"$(uname -m)"/os \
--disk size=50,format=qcow2 \
--virt-type kvm \
--console pty,target_type=serial \
--extra-args "console=ttyAMA0,115200" \
--graphics none
```

* `--cpu host-passthrough` instead of `--cpu host` due to [this bug](https://bugzilla.redhat.com/show_bug.cgi?id=1531076).

* The `extra-args` `console=` value is different.

### Install Fedora using Anaconda's text mode

After running the `virt-install` command, you should see a bunch of kernel and systemd output then you should see the Anaconda installation page asking if you want to use VNC or the text based installer. I have not messed around with the VNC option, the text installer works fine in my experience.

Once the installer is loaded, move through each of the options. Here are some of my recommendations:

* Customize the language and time zone to how you see fit. I set the NTP server to `time.google.com`.
* The installation location should be set to the URL of `--location`; I did not bother selecting any of the package groups they offered because I wanted to keep the runner environment as simple as possible.
* I always stick with the default partition (and I'll go more into that later).
* In the networking options, I recommend setting the hostname to something unique so that you can potentially add multiple runners in the future (I used `fedora-github-action-runner-<arch>-<num>`, like `fedora-github-actions-runner-x86_64-1`).
* I set the root password to something strong and added a user account called `runner` that was not an administrator with no password so that the `runner` account was completely unpriviledged and it will be easy to log into the `runner` account for future configuration. Doing system administration will happen only under the `root` account.

After everything is configured, run the installer by pressing `b`; after a while, you will see the installation completed and press Enter to reboot the virtual machine.

### Configuring Fedora for GitHub Actions

Once you are at the login screen, log into the `root` account. We are going change a few `sshd` configuration options to allow us to log into the virtual machine via `ssh` for administration, as the serial console is not so nice to work with.

We need to change the setting of logging into the `root` account via `ssh`. To start, we will allow logging into the `root` account via either password or private key:

```
# sed -i 's/^#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
# systemctl restart sshd
```

To get the virtual machine's IP address, use `virsh net-dhcp-leases default`:

```
$ virsh net-dhcp-leases default
 Expiry Time           MAC address         Protocol   IP address           Hostname                   Client ID or DUID
----------------------------------------------------------------------------------------------------------------------------
 2022-05-27 15:58:37   52:54:00:5c:24:6c   ipv4       192.168.122.104/24   fedora-github-actions-vm   01:52:54:00:5c:24:6c
```

If you have a private `ssh` key, you can now use `ssh-copy-id` to add your key to `authorized_keys` then change the `PermitRootLogin` value to `prohibit-password`. Otherwise, ignore this step and just rely on logging in via the `root` password.

```
# sed -i 's/PermitRootLogin yes/PermitRootLogin prohibit-password/g' /etc/ssh/sshd_config
# systemctl restart sshd
```

Now you can press `Ctrl + ]` to exit the `virsh` console and log into the virtual machine via `ssh`:

```
$ ssh root@...
```

If you did not assign a password to your `runner` account, you need to allow logging in no password via `ssh`:

```
# sed -i 's/^#PermitEmptyPasswords no/PermitEmptyPasswords yes/g' /etc/ssh/sshd_config
# systemctl restart sshd
```

If you have a private `ssh` key, you can now authorize it via `ssh-copy-id` to the `runner` account then flip the value you changed above; otherwise, ignore this step.

```
# sed -i 's/^PermitEmptyPasswords yes/#PermitEmptyPasswords yes/g' /etc/ssh/sshd_config
# systemctl restart sshd
```

If you stuck with the default partitioning scheme during setup, we need to expand the root partition of our virtual machine, as for some reason, [the Fedora Server installer only allocates 15GB](https://unix.stackexchange.com/questions/616780/fedora-server-32-install-does-not-claim-full-disk).

```
# dev_mapper=$(df -H | grep /dev/mapper/ | cut -d ' ' -f 1)
# lvextend -l +100%FREE "$dev_mapper"
# xfs_growfs "$dev_mapper"
```

To use actions that use Docker containers, we need to actually install and configure Docker:

```
# dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
# dnf install -y \
containerd.io \
docker-ce \
docker-ce-cli \
docker-compose-plugin
# usermod -aG docker runner
# systemctl enable --now docker
```

I would recommend installing `git` to ensure you do not use the REST API to download repositories:

```
# dnf install -y git
```

After this, we are done configuring in the `root` account. Log into the `runner` account via `ssh` and make sure that Docker actually works:

```
$ ssh runner@...
$ docker run --rm hello-world
```

## Setting up GitHub Actions

Once that works, go to the repository that you want to add the runner to, click on the Setting tab, click on the Actions option in the sidebar, click on Runners, and finally click on the green button "New self-hosted runner". Click "Linux" and select the correct architecture of the virtual machine.

Follow the Download instructions (they are copy and paste). The validation step has `shasum -a 256`, I had to change that to `sha256sum` on Fedora.

Run the first step under the "Configure" section:

```
$ ./config.sh --url ... --token ...
```

For the second step, we are not going to use `./run.sh`; instead, we are going to configure the systemd service that is generated from the `./config.sh` step.

First, we need to install the service. I am assuming that you did not add the `runner` account to the `wheel` group so `sudo` will not work; we will use `su -c` + the `root` password instead, as this is a one-time setup steup.

```
$ su -c "./svc.sh install runner"
```

Next, I recommend [installing a "clean up" script](https://docs.github.com/en/actions/hosting-your-own-runners/running-scripts-before-or-after-a-job) to match the hosted GitHub Actions workflow, where every workflow run is completely clean. We do this before the service is actually started to ensure the variable is added to the environment.

```
$ cat <<'EOF' >"$HOME"/cleanup.sh
#!/usr/bin/env bash

rm -frv "${GITHUB_WORKSPACE%/*}"
EOF
$ chmod +x "$HOME"/cleanup.sh
```

SELinux is enabled on Fedora, which makes the GitHub Actions service [unhappy](https://github.com/actions/runner/issues/1606); this impact this script as well, since it is running under the GitHub Actions service context.

```
$ su -c "semanage fcontext --add --type initrc_exec_t $HOME/cleanup.sh"
$ restorecon -v "$HOME"/cleanup.sh
```

Add the script to the environment of the runner:

```
$ echo "ACTIONS_RUNNER_HOOK_JOB_COMPLETED=$HOME/cleanup.sh" >>"$HOME"/actions-runner/.env
```

We need to do the same SELinux workaround for the service script:

```
$ su -c "semanage fcontext --add --type initrc_exec_t $HOME/actions-runner/runsvc.sh"
$ restorecon -v "$HOME"/actions-runner/runsvc.sh
```

Finally, we start the runner!

```
$ su -c "./svc.sh start"
```

With any luck, you will now see the service started and you should be able to refresh the Runner setup screen on GitHub to see your runner in an idle status.

At this point, if you want your runner to be available after a reboot of the host machine, mark it for autostart:

```
$ virsh autostart fedora
Domain 'fedora' marked as autostarted


$ virsh dominfo fedora
Id:             1
Name:           fedora
UUID:           9385d2f8-2785-4aaf-8581-9d09f0d7a533
OS Type:        hvm
State:          running
CPU(s):         24
CPU time:       51390.2s
Max memory:     50331648 KiB
Used memory:    50331648 KiB
Persistent:     yes
Autostart:      enable
Managed save:   no
Security model: none
Security DOI:   0
```

## Questions

If you have any questions or comments, feel free to leave them in the comments down below or reach out to me on [Twitter](https://twitter.com/nathanchance).