# DebianOnQEMU

Debian qcow2 multi-arch images on QEMU

# Quick Start

Download `vmlinuz`, `initrd` and `qcow2` image from the release page, and start your virtual machine with QEMU.

A sample for `amd64`:

```bash
qemu-system-x86_64 -m 512 -kernel ./vmlinuz-5.10.0-13-amd64 \
                   -initrd ./initrd.img-5.10.0-13-amd64 \
                   -append "console=ttyS0 debug root=/dev/sda net.ifnames=0" \
                   -hda ./debian-bullseye-amd64.qcow2 -nographic \
                   -nic user,model=virtio-net-pci,hostfwd=tcp::5555-:22
```

Different console names per architectures:

- `ttyS0`: `amd64`, `i386`, `mips`, `mips64`
- `ttyAMA0`: `arm`, `arm64`, `s390x`
- `hvc0`: `ppc`

Note there are two users created by default: `root:root` and `debian:debian`.