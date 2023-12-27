# DebianOnQEMU

Debian qcow2 multi-arch images on QEMU.

If this project helps you, please give it a star!

You may be also interested in the side project: [qemu-full](https://github.com/wtdcode/qemu-full)

## Quick Start

Download `vmlinuz`, `initrd` and `qcow2` image from the release page, and start your virtual machine with QEMU.

In most cases, you want to access the virtual machine via SSH, so don't forget to add a port forwarding or bridge your network interfaces.

Below are a few quick start command lines. Note the file names should be replaced by the ones you download.

### AMD64

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-x86_64 -m 512 -kernel ./vmlinuz-5.10.0-13-amd64 \
                   -initrd ./initrd.img-5.10.0-13-amd64 \
                   -append "console=ttyS0 debug root=/dev/sda net.ifnames=0" \
                   -hda ./debian-bullseye-amd64.qcow2 -nographic \
                   -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

## i386

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-i386 -m 512 -kernel ./vmlinuz-5.10.0-26-686 \
                 -initrd ./initrd.img-5.10.0-26-686 \
                 -append "console=ttyS0 debug root=/dev/sda net.ifnames=0" \
                 -hda ./debian-bullseye-i386.qcow2 -nographic \
                 -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### ARM

```bash
docker run -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-arm -m 512 -M virt -cpu cortex-a15 \
                -kernel ./vmlinuz-5.10.0-13-armmp \
                -initrd ./initrd.img-5.10.0-13-armmp \
                -hda ./debian-bullseye-armhf-armmp.qcow2 \
                -append "root=/dev/vda rw console=ttyAMA0 rodata=n" \
                -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22 -nographic
```

### ARM64

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-aarch64 -m 512 -M virt -cpu cortex-a57 -kernel ./vmlinuz-5.10.0-26-arm64 \
                    -initrd ./initrd.img-5.10.0-26-arm64 \
                    -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                    -hda ./debian-bullseye-arm64.qcow2 -nographic \
                    -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

`cortext-a8` or `cortext-a9` are not supported.

### RISCV64

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-riscv64 -m 512 -M virt -kernel ./vmlinux-6.6.8-riscv64\
                    -initrd ./initrd.img-6.6.8-riscv64 \
                    -append "rw console=ttyS0 debug root=/dev/vda net.ifnames=0" \
                    -device virtio-blk-device,drive=hd0 \
                    -object rng-random,filename=/dev/urandom,id=rng0 -device virtio-rng-device,rng=rng0 \
                    -drive file=./debian-bullseye-riscv64.qcow2,id=hd0 -nographic \
                    -device virtio-net-device,netdev=usernet -netdev user,id=usernet,hostfwd=tcp::5555-:22 
```

Refer to [QEMU doc](https://wiki.qemu.org/Documentation/Platforms/RISCV) and [Debian doc](https://wiki.debian.org/RISC-V) for more configurations.

### S390x

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-aarch64 -m 512 -kernel ./vmlinuz-5.10.0-26-arm64 -initrd ./initrd.img-5.10.0-26-arm64 \
                    -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                    -hda ./debian-bullseye-arm64.qcow2 \
                    -nographic -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### PPC


```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-ppc64 -m 512 -cpu power9 -kernel ./vmlinux-5.10.0-26-powerpc64le \
                  -initrd ./initrd.img-5.10.0-26-powerpc64le \
                  -append "console=hvc0 debug root=/dev/sda net.ifnames=0" \
                  -hda ./debian-bullseye-ppc64el.qcow2 -nographic \
                  -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### MIPS

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-mipsel -m 512 -M malta -kernel ./vmlinuz-5.10.0-26-4kc-malta \
                   -initrd ./initrd.img-5.10.0-26-4kc-malta \
                   -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                   -hda ./debian-bullseye-mipsel-malta.qcow2 -nographic \
                   -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full \
qemu-system-mips64el -m 512 -M malta -cpu 5KEc -kernel ./vmlinuz-5.10.0-26-5kc-malta \
            -initrd ./initrd.img-5.10.0-26-5kc-malta \
            -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
            -hda ./debian-bullseye-mips64el-malta.qcow2 \
            -nographic -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### Loongarch64

So far, Loongarch64 Debian ports are [not complete](https://wiki.debian.org/Ports/loong64) but we can boot Loong Archlinux and run debian userland inside.

```bash
docker run -p 5555:5555 -v `pwd`:/work --rm -it lazymio/qemu-full:v8.2.0 \
qemu-system-loongarch64 -m 1024m -cpu la464-loongarch-cpu \
                    -M virt -append "console=ttyS0 rw debug root=/dev/vda" \
                    -kernel ./vmlinuz-loong64 -initrd ./initrd.img-loong64 \
                    -bios ./bios-loong64-8.1.bin \
                    -hda ./debian-bullseye-loong64.qcow2 --nographic \
                    -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

After the system is up, SSH into it and use:

```
[root@debian-bullseye-loong64 ~]# chroot /debian /bin/bash
I have no name!@debian-bullseye-loong64:/# apt
apt 2.7.7 (loong64)
Usage: apt [options] command

apt is a commandline package manager and provides commands for
searching and managing as well as querying information about packages.
It provides the same functionality as the specialized APT tools,
like apt-get and apt-cache, but enables options more suitable for
interactive use by default.

Most used commands:
  list - list packages based on package names
  search - search in package descriptions
  show - show package details
  install - install packages
  reinstall - reinstall packages
  remove - remove packages
  autoremove - automatically remove all unused packages
  update - update list of available packages
  upgrade - upgrade the system by installing/upgrading packages
  full-upgrade - upgrade the system by removing/installing/upgrading packages
  edit-sources - edit the source information file
  satisfy - satisfy dependency strings

See apt(8) for more information about the available commands.
Configuration options and syntax is detailed in apt.conf(5).
Information about how to configure sources can be found in sources.list(5).
Package and version choices can be expressed via apt_preferences(5).
Security details are available in apt-secure(8).
                                        This APT has Super Cow Powers.
I have no name!@debian-bullseye-loong64:/#
```

Once Debian ports is available we can jump over ArchLinux and boot Debian directly.

## Usage

Two users created: `root:root` and `debian:debian` and ssh server is up by default.