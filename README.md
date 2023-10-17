Autoinstall files for Ubuntu
============================

The contents of this directory can be used to automate fresh installation
of the Ubuntu lab environment without the help of Vagrant or Virtualbox. It uses
[Automated Server
installation](https://ubuntu.com/server/docs/install/autoinstall) feature
of Ubuntu server together with [cloud-init](https://cloud-init.io/). It can
run anywhere where Ubuntu 22.04 is supported, including ARM architecture
computers like M1/M2 Apple Hardware. It has been tested using
[UTM](https://mac.getutm.app/) hypervisor on both arm64 and x86_64
architectures. Other hypervisors and/or architectures might or might not
work.

Prerequisites for building UTM-based Virtual Machine
----------------------------------------------------
 - A computer running macOS based on either Intel or Apple Silicon
 - At least 10 GB of free disk space
 - [UTM](https://mac.getutm.app/)
 - Ubuntu Server ISO file for particular architecture:
   [Intel](https://ubuntu.com/download/server) or
   [ARM](https://ubuntu.com/download/server/arm)

Preparing virtual machine from scratch
--------------------------------------

1. In UTM, press "Create a New Virtual Machine."
2. Select "Virtualize", then "Linux". Provide ISO image. Provision at least 2048
   GB RAM and 10 GB disk. The rest can stay on default.
3. Edit machine settings, remove sound card by right clicking it and edit QEMU
   options providing path to Autoinstall files. At the very bottom, in the field
   with prompt "New…", put this path:
   
   ```
   -smbios type=1,serial=ds=nocloud-net;s=https://raw.githubusercontent.com/mikarnik/ubuntu-auto-install/main/ubuntu-auto-install
   ```
4. In case of Intel computer, adjust virtual machine setup like this:
   - Display: Emulated Display Card: `virtio-vga-gl`
   - Network: Emulated Network Card: `virtio-net-pci`
   - Hard disk: Interface: `VirtIO`

5. Run the VM. It should start booting from the ISO image and spit a lot of
   text.
6. You will be prompted "Continue with autoinstall? (yes/no)". Answer `yes`.
7. After several minutes, the installer will reboot the virtual machine. Do NOT
   boot the ISO image again, eject it or select "Boot from next volume."
8. During the first start, the installation of the lab environment takes place.
   Please wait at least minutes. You can log in using password "ubuntu" or
   just wait. Don't get bothered by "Login timed out after 60 seconds."
9. After installation is done, you will be logged in automatically. The last
   line before prompt should read: "status: done."

The lab environment should be fully accessible on a random IP address the
Virtual Machine got assigned. You can see the IP address by using command
`ip -br addr show enp0s1`. You can stop the VM gracefully by issuing `poweroff`.
