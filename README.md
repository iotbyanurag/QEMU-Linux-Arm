# QEMU-Linux-Arm
# QEMU-Linux-Arm



Steps to Boot Raspbian Jessie with QEMU
1. Ensure QEMU is Installed
Install QEMU using the following command if not done already:

For Ubuntu/Debian:

sudo apt-get install qemu-system qemu-efi
2. Convert Your Disk Image
You can convert your 2017-04-10-raspbian-jessie.img to qcow2 format for better performance with QEMU:

bash
qemu-img convert -f raw -O qcow2 2017-04-10-raspbian-jessie.img raspbian-jessie.qcow2


The `-redir` option has been deprecated in newer versions of QEMU. You can use the newer `-netdev` and `-device` options for network configuration and port forwarding. Here's the updated command that replaces `-redir`:

```bash
qemu-system-arm \
  -kernel kernel-qemu-5.10.63-bullseye \
  -cpu arm1176 \
  -m 256 \
  -M versatilepb \
  -dtb versatile-pb.dtb \
  -no-reboot \
  -serial stdio \
  -append "root=/dev/sda2 panic=1 rootfstype=ext4 rw" \
  -hda raspbian-jessie.qcow2 \
  -netdev user,id=mynet0,hostfwd=tcp::5022-:22 \
  -device rtl8139,netdev=mynet0
```

Explanation of the changes:
- `-netdev user,id=mynet0,hostfwd=tcp::5022-:22`: This sets up a user-mode network backend with port forwarding from host port 5022 to guest port 22 (SSH).
- `-device rtl8139,netdev=mynet0`: This attaches a network device (`rtl8139`) to the `mynet0` backend created above.

Run this command, and it should fix the issue with port forwarding in the latest QEMU versions. Let me know how it goes!


By default, the login credentials for the Raspbian Jessie image (and many other Raspbian versions) are usually:

- **Username**: `pi`
- **Password**: `raspberry`

You can log in with these credentials once the image has booted. If these credentials don't work, let me know, and we can troubleshoot further!
