---
title: How to Install Linux on an M1 Mac With Apple Silicon
date: 2021-12-21 12:39:13
keywords: Apple M1,Mac OS,Linux
categories: Mac OS
tags:
  - virtual machine
---

![ubuntu_on_utm](https://s2.loli.net/2021/12/21/1WqQRbpjOJG4HMK.png)

# You Can’t Run Linux Natively on Apple Silicon (Yet)

Native Linux support for Apple Silicon chips is coming. If you can’t wait any longer, you can run Linux right now in a virtual machine.

You can do this for free with a virtual machine (VM) app called [UTM](https://mac.getutm.app/). There’s also a paid $9.99 [Mac App Store version](https://apps.apple.com/au/app/utm-virtual-machines/id1538878817?mt=12) that you can buy to support developers and gain access to automatic updates through the store interface.

You can use this app to emulate a large number of processor architectures, including x86-64 (“proper” desktop Linux) as well as **ARM** and **PowerPC**.

To get started, download UTM and a Linux distribution of your choosing, then get started on creating a virtual machine with UTM.

**Note:** You can use UTM to run ARM64 versions of Linux at “near native” speeds, 	while lower performance emulation can be achieved in 32-bit and 64-bit x86 versions. It’s up to you what you choose, but you’ll need to match your version to your system architecture in the instructions below.

#  Install Ubuntu on an M1 Mac With Apple Silicon

##  Downloads

- [UTM for Mac](https://github.com/utmapp/UTM/releases) (v2.0.15 or higher)
- [Ubuntu Server for ARM](https://ubuntu.com/download/server/arm) (20.04.1 or higher)

## Instructions

1. Open UTM and create a new virtual machine.

![ubuntu_screen_1](https://s2.loli.net/2021/12/21/1FPGJY3NLCfkecV.png)

2. Give the VM a name and optionally choose an icon.

![ubuntu_screen_2](https://s2.loli.net/2021/12/21/AYebBKH1FgkmILu.png)

3. In System, select the “ARM64 (aarch64)” architecture, and specify the amount of memory. At least half of your computer’s total memory is recommended for performance.

![ubuntu_screen_3](https://s2.loli.net/2021/12/21/1VtA7DmFJeHyLRl.png)

4. In Drives, create a new drive. This will be your install drive. It is recommended to give it at least 10,240 MB (10 GB).



![ubuntu_screen_4](https://s2.loli.net/2021/12/21/sbYVoixw7E9JSuC.png)

5. Create another drive. This will be the installation disk drive. Make sure “removable” is checked.

![ubuntu_screen_5](https://s2.loli.net/2021/12/21/dMjcHx2igpUqWJF.png)

6. Save the VM and select it in the sidebar. Click the Browse button on the bottom right and select the Ubuntu installation ISO.

![ubuntu_screen_6](https://s2.loli.net/2021/12/21/WBjVQA3zYbxT9Nh.png)

7. Start the VM and choose to install Ubuntu server. (If you cannot boot to the installer, go to the troubleshooting section at the bottom.) Follow the installation wizard, all the default options are recommended.

![ubuntu_screen_7](https://s2.loli.net/2021/12/21/eSdyhpHcl95QWgb.png)

8. At the end of the installation, the screen will be black with a blinking cursor. Eject the ISO with the CD icon on the toolbar. Press the restart icon on the toolbar (third from the left) to restart into your installed Ubuntu.

## Installing Ubuntu Desktop

At the end of the installation, you will have Ubuntu Server installed without any GUI. To install Ubuntu Desktop, log in and run:

```bash
$ sudo apt install tasksel
$ sudo tasksel install ubuntu-desktop
$ sudo reboot
```

(Note `tasksel` may fail the first time and you can just run it again.)

## Enable clipboard and directory sharing

With the VM turned off, open the settings, and make sure these two options are checked.

![ubuntu_screen_sharing](https://s2.loli.net/2021/12/21/Pw1bG4nBYUSqCFu.png)

```bash
$ sudo apt install spice-vdagent spice-webdavd
```

Your shared directory shows up as a WebDAV server on `http://127.0.0.1:9843/`. You can use a WebDAV client to access it, or `mount.davfs` to mount it.

## Troubleshooting

### Cannot boot into installer

If you start the VM and are stuck at the EFI screen (`BdsDxe: failed to load Boot0001` or `UEFI Interactive Shell`), try the following in order.

1. Make sure you have the installer ISO selected. Click the disk icon on the toolbar and check that there is a menu option for `CD/DVD (ISO) Image (usb): ubuntu-xxx.iso`. If it says `CD/DVD (ISO) Image (usb): none`, then highlight that menu and choose `Change` and then select the ISO. If you don’t have any selectable menu option, follow the guide again and make sure you have added a removable drive. Then restart the VM.
2. Next, try to get into the EFI Shell. If you see `UEFI Interactive Shell` then you are already in the shell. Otherwise, restart the VM and quickly press the Esc key to enter the shell.
3. In the EFI shell make sure you see `FS0: Alias(s):CD0h0a0a::BLK1:` near the top or something similar. If not, then double check your configuration and make sure you have a removable drive configured and the installer ISO mounted. Also check that your ISO is valid.
4. Type in: `fs0:\efi\boot\bootaa64.efi` and you should see GRUB. Then select `Ubuntu Server` to continue with the install.

### Networking is unavailable after switching between Console Only and Full Graphics modes

When switching between display modes, the network adapter may be renamed by Ubuntu. To fix this, you need to set up networking again.

1. Run `ip link show` and look at the last adapter name. For example, it may be listed as `enp0s9`.
2. Edit `/etc/netplan/00-installer-config.yaml` and copy your configuration for `enp0s8` (or whatever the old adapter was named) and paste it immediately after for `enp0s9` (or whatever the new adapter is named).
3. Reboot and you should be able to use networking in both display modes.
