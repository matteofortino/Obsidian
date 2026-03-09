#seminar
# How to compile and instal the Linux Kernel
## Which Kernel?
See [kernel](https://kernel.org) 
Linux mainline (the one from Linus Torvalds), is the frequently updated, unstable release.
The stable release is found on the github of the user **gregkh**. 
## Compiling
Using just `make` doesn't work. Linux support multiple architectures so we need to specify the one we want to build on.
```sh
make menuconfig
```
This command offers a GUI for all the compiling options.
Items selected with a M are *modularized*, they are built apart from the kernel and loaded on the disk when needed.
To see all the modules loaded on to the system we use `lsmod`. 

> In /boot/config-version we find every option selected fro the compilation.

After we have decided the options: 
```sh
make -j $(nproc)
```
## How to install the kernel
First command we run is:
```sh
make binrpm-pkg
# or
make install # this install the kernel in the current machine
# and
make modules_install # this install the modulues, just the kernel may not be enough
```
This command generate RMPs which are packets for the kernel
```sh
dnf install *rpm
```
dnf is the fedora packeg manager, other distro may have it different.

## Finding aliases from the modules
``` sh
less modules.alias
```
# What is virtio-vsock
## VirtIO
VirtIO is a standard for creating virtual devices.
When virtualizing, we do not need to emulate every part of a machine, so we use **para-virtualization**: the guest know it is a vm, and doesn't utilize every part of the computer.
# virtio-vsock
Create by, VMware, it is a driver for virtual machines sockets, it is needed for communicating between virtual and physical machine.
# How does the virtio-vsock driver work?
# How to contribute to Linux
It's not as simple as sending PR to the github repo.
Everything is done by email, because it is fully decentralized.

Linux is divided into subtrees.
There is a maintainers file containing the emails of maintainers.
Every subtree has maintainers.

```shell
git diff
git add -u
git commit -s -m "<commit messages>"
git log
git format-patch -1
git send-email --to=<email> <commit message>
~/.gitconfig # contains configurations for the email and so on.
```