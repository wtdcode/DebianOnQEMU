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
qemu-system-x86_64 -m 512 -kernel ./vmlinuz-5.10.0-13-amd64 \
                   -initrd ./initrd.img-5.10.0-13-amd64 \
                   -append "console=ttyS0 debug root=/dev/sda net.ifnames=0" \
                   -hda ./debian-bullseye-amd64.qcow2 -nographic \
                   -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

## i386

```bash
qemu-system-i386 -m 512 -kernel ./vmlinuz-5.10.0-26-686 \
                 -initrd ./initrd.img-5.10.0-26-686 \
                 -append "console=ttyS0 debug root=/dev/sda net.ifnames=0" \
                 -hda ./debian-bullseye-i386.qcow2 -nographic \
                 -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### ARM

```bash
qemu-system-arm -m 512 -M virt -cpu cortex-a15 \
                -kernel ./vmlinuz-5.10.0-13-armmp \
                -initrd ./initrd.img-5.10.0-13-armmp \
                -hda ./debian-bullseye-armhf-armmp.qcow2 \
                -append "root=/dev/vda rw console=ttyAMA0 rodata=n" \
                -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22 -nographic
```

### ARM64

```bash
qemu-system-aarch64 -m 512 -M virt -cpu cortex-a57 -kernel ./vmlinuz-5.10.0-26-arm64 \
                    -initrd ./initrd.img-5.10.0-26-arm64 \
                    -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                    -hda ./debian-bullseye-arm64.qcow2 -nographic \
                    -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

`cortext-a8` or `cortext-a9` are not supported.


### S390x

```bash
qemu-system-aarch64 -m 512 -kernel ./vmlinuz-5.10.0-26-arm64 -initrd ./initrd.img-5.10.0-26-arm64 \
                    -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                    -hda ./debian-bullseye-arm64.qcow2 \
                    -nographic -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### PPC


```bash
qemu-system-ppc64 -m 512 -cpu power9 -kernel ./vmlinux-5.10.0-26-powerpc64le \
                  -initrd ./initrd.img-5.10.0-26-powerpc64le \
                  -append "console=hvc0 debug root=/dev/sda net.ifnames=0" \
                  -hda ./debian-bullseye-ppc64el.qcow2 -nographic \
                  -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

### MIPS

```bash
qemu-system-mipsel -m 512 -M malta -kernel ./vmlinuz-5.10.0-26-4kc-malta \
                   -initrd ./initrd.img-5.10.0-26-4kc-malta \
                   -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                   -hda ./debian-bullseye-mipsel-malta.qcow2 -nographic \
                   -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

```bash
qemu-system-mips64el -m 512 -M malta -cpu 5KEc -kernel ./vmlinuz-5.10.0-26-5kc-malta \
                     -initrd ./initrd.img-5.10.0-26-5kc-malta \
                     -append "console=ttyAMA0 debug root=/dev/sda net.ifnames=0" \
                     -hda ./debian-bullseye-mips64el-malta.qcow2 \
                     -nographic -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

## Usage

Two users created: `root:root` and `debian:debian` and ssh server is up by default.