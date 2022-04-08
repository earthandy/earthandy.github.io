---
title: Goodbye CentOS Say Hi Rocky
summary: Support for CentOS ends in 2021, but the project lives on as Rocky Linux.Rocky Linux is a godsend for Linux enthusiasts like me who are grieving the disappearance of CentOS 8.0 but don't really like Fedora. Here's how to install and configure Rocky Linux.
date: 2022-03-03 11:00:43
keywords: CentOS，Rocky，Install System,Linux
categories: Linux
tags:
- CentOS
- Rocky
---

Support for CentOS ends in 2021, but the project lives on as Rocky Linux.

![Linux](linux.png)

#### Rocky Linux is a rock solid distribution

Rocky Linux is a godsend for Linux enthusiasts like me who are grieving the disappearance of CentOS 8.0 but don't really like Fedora. This release is the most reliable introductory release I've seen. CentOS users switching to Rocky Linux will be pleasantly surprised by the distro's familiar look and feel. New Linux users will also have no problem navigating and traversing Rocky Linux.
The Linux community has responded positively and supported the distribution. Shortly after Rocky Linux was released, a subreddit was created, [/r/RockyLinux](https://www.reddit.com/r/RockyLinux), which now has nearly 6,000 users. Here's how to install and configure Rocky Linux.

#### Minimum requirements for installing Rocky Linux

Below are the minimum requirements to install Rocky Linux 8.5.
• 2 GB RAM or more
• 20 GB hard drive or larger
• 2 CPUs / vCPU (1.1 GHz processor)
• Internet access (optional)
• Bootable media (USB or DVD)

Further reading: [What is Rocky Linux and should you consider it?](https://www.makeuseof.com/what-is-rocky-linux/)

#### Download Rocky Linux and create a USB drive

Follow these instructions to download Rocky Linux and create a bootable USB drive.
1. Open a browser and navigate to [rockylinux.org/download](https://rockylinux.org/download).
2. Click Save File.
3. After downloading the files, create a Rocky Linux bootable USB from your machine. If you're not sure how to do this, see below:
a. [How to create a Windows 11 bootable USB drive](https://www.makeuseof.com/windows-11-create-bootable-usb-drive/)
b. [How to Make a Bootable USB Drive with Etcher in Linux](https://www.makeuseof.com/create-bootable-usb-drive-with-etcher-linux/)
c. [How to create and boot a Linux USB drive on a Mac](https://www.makeuseof.com/tag/how-to-boot-a-linux-live-usb-stick-on-your-mac/)
Once the installation media has been created, you can proceed.

#### Rocky Linux Installation Instructions

Follow these instructions to successfully install Rocky Linux 8.5
1. Insert the Rocky Linux bootable USB you created into the target PC/Laptop.
2. Start or restart the computer to boot from the USB drive. Once you see the Rocky Linux splash screen, select Install Rocky Linux.

![Rocky Linux Start up](rocky-linux-start-up.png)

3. Click your preferred language when prompted, and then click Continue.
4. In the Installation Summary window, select Installation Destination.

![Rocky Install Summary](rocky-install-summary.png)

Select your target hard drive and click Finish.
5. Select Network and Hostname, then connect to your network connection. Click Finish.

![Rocky Network](rocky-network.png)

6. Select Installation Source and select your Rocky Linux bootable USB if not already selected.
7. Select Software Selection and select the desired base environment and additional software. Click Finish when you are satisfied with your selections.

![Rocky Software Selection](rocky-software-selection.png)

8. Select the Root password from the Installation Summary window. Enter and verify your desired password. Click Finish.
9. Select Create User from the Installation Summary window. Enter your desired response. Click Finish.

![Rocky Create User](rocky-create-user.png)

10. Please be patient while installing Rocky Linux on your laptop/PC.
11. After the installation is complete, click Reboot System. Remove the Rocky Linux bootable USB drive when prompted. Press <Enter>
12. After the system restarts, click Licensing Information in the Initial Setup window
13. Read the license agreement, scroll to the bottom, and check I accept the license agreement. Click Finish.

![Rocky Installation](rocky-installation.png)

14. Click Finish Configuration.

#### Post-installation configuration steps for Rocky Linux

After Rocky Linux boots for the first time, you will need to complete some configuration steps.
1. Log in to the Rocky Linux installation using the user (and password) you created earlier.

![Rocky Login](rocky-login.png)

2. After the system restarts, click Licensing Information in the Initial Setup window
3. Read the license agreement, scroll to the bottom, and check I accept the license agreement. Click Finish.
4. Click Finish Configuration.
5. Log in to your Rocky Linux installation.
6. Select your desired language on the welcome screen. Click Next.
7. Repeat typing the screen. Click Next.
8. On the Privacy screen, choose whether you want to turn Location Services on or off. Click Next.

![Rocky Privacy](rocky-privacy.png)

9. Select the online account you want to connect to and enter your credentials, or click Skip.
10. Click Get Started with Rocky Linux.

![Rocky Ready Go](rocky-ready-go.png)

11. Congratulations, you have successfully installed Rocky Linux 8 on your laptop/PC!
Whenever you install a new release, your priority is to update your system to make sure any security updates are applied as well as software on OS updates. Fire up your terminal and update your repository with the following command (enter your root password if prompted):

```bash
sudo yum check-update
```

After the yum check-update command completes, enter the following command to update your operating system and all installed software:

```bash
sudo yum update
```

After the sudo yum update command completes, exit the terminal with the following command:

#### Improve DNF speed for Rocky Linux 8.5

Dandified YUM, better known as DNF, is a package manager for RPM-based Linux distributions that install, update, and remove packages. It was originally introduced in Fedora 18 in a testable state (i.e. Tech Preview), but it has been Fedora's default package manager since Fedora 22. The package manager in Rocky Linux is also RPM (Red Hat Package Manager). Package managers allow Linux users to install, update, and remove software.
After installing and updating and upgrading a new Rocky 8.5 installation, it is recommended that you increase the DNF speed. First backup the dnf.conf file.

```bash
sudo cp /etc/dnf/dnf.conf /etc/dnf/dnf/bak
```

Next, edit the dnf.conf file:

```bash
sudo nano /etc/dnf/dnf.conf
```

You can now increase DNF speed by adding parallel downloads by adding the following to the bottom of your dnf.conf file:

```bash
max_parallel_downloads=10
```

Note that it is recommended that users start with 10, but you can increase it to a different number, such as 15, 20, etc. It is recommended to do this with caution. To enable the fastest mirror, add this line below max_parallel_downloads=10.

```bash
fastestmirror=True
```

Save the updated /etc/dnf.conf file, then exit nano.
A restart or restart of any services is required as these changes take effect immediately.

#### Migrating to Rocky Linux 8.5

Existing CentOS 8.5, Alma Linux 8.5, RHEL 8.5, or Oracle Linux 8.5 users can easily migrate to Rocky Linux using the following procedure.
First, download the Rocky Linux migration script. The safest way is to download the migration script via git.
Type in your terminal. Install git by entering the following command (if prompted for a root password):

```bash
sudo dnf install git
```

Next, clone the rocky-tools repository with the following command:

```bash
sudo git clone https://github.com/rocky-linux/rocky-tools.git
```

You can also download the migration script via the curl command (although this is a less secure method):

```bash
sudo curl https://raw.githubusercontent.com/rocky-linux/rocky-tools/main/migrate2rocky/migrate2rocky.sh -o migrate2rocky.sh
```

Now that you have the migration script, you can execute it and start the migration. However, first, you have to give the owner of the script file execute permission via the command:

```bash
sudo chmod u+x migrate2rocky.sh
```

Finally, we will execute the script:

```bash
sudo ./migrate2rocky.sh -r
```

(Note that the '-r' option instructs the script to install everything. Please be patient while the migration script completes. When done, exit the terminal.

