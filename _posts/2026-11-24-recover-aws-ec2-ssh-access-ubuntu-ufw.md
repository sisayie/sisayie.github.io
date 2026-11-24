---
title: "How to Recover SSH Access to an AWS EC2 Ubuntu Instance After Enabling Firewal?"
seo_title: "AWS EC2 SSH Connection Timed Out? Recover Ubuntu Using EBS and chroot"
description: "Learn how to recover SSH access to an AWS EC2 Ubuntu instance after accidentally enabling UFW."
date: 2026-11-24
permalink: /posts/2026/11/2026-11-24-recover-aws-ec2-ssh-access-ubuntu-ufw/
tags:
    - AWS
    - EC2
    - Ubuntu
    - Linux
    - UFW
    - SSH
    - EBS
    - DevOps
    - Cloud Infrastructure
---

## Introduction

I recently ran into an interesting problem with one of my AWS EC2 instances. I had a small Ubuntu server running on an EC2 `t3.micro` instance with an 8 GB EBS volume. Everything was working perfectly, and I could connect to the server using SSH and my private key.

I decided to increase the EBS storage from 8 GB to 16 GB. This seemed like a straightforward operation, and I didn't expect it to cause any problems.

However, shortly afterward, I discovered that I could no longer connect to my server.

Initially, I suspected that increasing the disk size had somehow broken the instance. After investigating, I discovered something much simpler: I had enabled the firewall, i.e., UFW (Uncomplicated Firewall) without allowing SSH connections first.

This blog post documents how I diagnosed the problem and recovered access to my original Ubuntu installation without losing data or rebuilding the EC2 instance.

## 1\. The Initial Problem

My original EC2 configuration was:

| Component | Configuration |
| --- | --- |
| Cloud provider | AWS |
| Instance type | t3.micro |
| Operating system | Ubuntu |
| Original EBS volume | 8 GB |
| Updated EBS volume | 16 GB |
| Connection method | SSH with private key |
| SSH username | ubuntu |

After increasing the EBS volume size, I attempted to connect as usual:


```bash
ssh -i your-key.pem ubuntu@EC2_PUBLIC_IP
```

But the connection never completed.

I checked the AWS EC2 console and found that:

- The instance was running.
- Both EC2 status checks passed (3/3).
- The EBS volume had been successfully increased to 16 GB.

This suggested that the instance itself was healthy from AWS's perspective.

## 2\. Diagnosing the SSH Connection Problem

The next step was to enable verbose SSH logging.

I ran:

bash

```bash
ssh -vvv -i your-key.pem ubuntu@EC2_PUBLIC_IP
```

The output ended with:

```text
OpenSSH_9.6p1 Ubuntu-3ubuntu13.20, OpenSSL 3.0.13
debug1: Connecting to EC2_PUBLIC_IP port 22.
debug3: set_sock_tos: setting socket options
```

The important detail was that SSH was hanging while attempting to establish a TCP connection to port 22.

It hadn't even reached the authentication stage.

This suggested a networking or firewall problem rather than an incorrect SSH key.

I also checked the AWS security group to ensure that inbound TCP traffic on port 22 was allowed from my public IP address. That was fine. 

But then  I remembered something important.

**I had enabled UFW before I terminated my earlier SSH session\!**

## 3\. Understanding How UFW Can Block SSH

UFW stands for Uncomplicated Firewall. It is a frontend for managing Linux firewall rules and is commonly used on Ubuntu servers.

One important thing to understand is that enabling UFW without first allowing SSH can block incoming SSH connections.

For example, this command enables the firewall:

```bash
sudo ufw enable
```

But if SSH hasn't been allowed, incoming connections on port 22 may be blocked.

The correct sequence should be:

```bash
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

For better security, restrict access to your own IP address:

```bash
sudo ufw allow from YOUR_PUBLIC_IP to any port 22 proto tcp
```

Unfortunately, I had already enabled UFW and lost SSH access.

Hmmmm. I needed another way to access the server.

## 4\. Attempting to Recover Access Through EC2 Serial Console

AWS provides a feature called EC2 Serial Console.

It allows administrators to access an instance through a serial connection, independently of its normal network connectivity.

I tried:

1. Opening the EC2 console.
2. Selecting my instance.
3. Clicking Connect.
4. Choosing EC2 Serial Console.

However, I received this error:

```text
Failed to connect to your instance

SendSerialConsoleSSHPublicKey failed.
Try again later.
```

This was a separate problem: AWS couldn't establish the serial console session.

EC2 Serial Console requires several prerequisites:

- Serial Console access must be enabled at the AWS account level.
- The IAM user or role must have the required permissions.
- Any applicable AWS Organizations restrictions must permit access.
- The AWS Region and instance must support the feature.

I cleared all the above prerequisites. But then stumbled on another issue. 

## 5\. Understanding the Ubuntu EC2 Default Credentials

Even if the serial connection succeeds, Ubuntu may still require an operating-system username and password. 

A common question when using EC2 Serial Console is: What are the default Ubuntu credentials?

For standard Ubuntu EC2 images:

**Username:**

```text
ubuntu
```

**Password:**

There is no default password.

Normally, Ubuntu EC2 instances use SSH key authentication rather than password authentication.

This was exactly how I had configured my server.

I had never created a password for the `ubuntu` user.

Therefore, even if I managed to establish a serial console connection, I would potentially need to recover or configure a password before logging in. That is, default username is known but password was not set and I could not login.

I needed a different recovery method.

## 6\. Recovering the Instance Using a Temporary EC2 Instance

The solution was to use another EC2 instance as a recovery machine.

This approach allows you to access the original Ubuntu filesystem by attaching its EBS volume to another Linux instance.

The advantage is that you don't need to reinstall Ubuntu or delete your existing data.

### Step 6.1: Create a temporary Ubuntu EC2 instance

I created a new EC2 instance with the following configuration:

| Setting | Value |
| --- | --- |
| Name | ubuntu-recovery |
| AMI | Ubuntu Server |
| Instance type | t3.micro |
| Availability Zone | Same as original instance |
| SSH access | Restricted to my public IP |

The recovery instance was used only to access and repair the original disk.

### Step 6.2: Stop the original instance

Before detaching its root EBS volume, I stopped the original EC2 instance.

I then opened:

AWS Console → EC2 → Volumes.

I identified the original 16 GB EBS volume.

As an additional precaution, I created an EBS snapshot before modifying anything.

This provided a recovery point in case something went wrong.

### Step 6.3: Detach the original EBS volume

I selected the original volume and chose:

Actions → Detach volume.

Once the volume was detached, I attached it to the temporary Ubuntu recovery instance.

Both instances needed to be in the same Availability Zone for this attachment.

I selected a secondary device name, such as `/dev/sdf`.

An important detail: AWS device names and Linux device names don't always match. On Nitro-based instances, an attached EBS volume commonly appears as an NVMe device.

### Step 6.4: Identify the original disk

After connecting to the temporary instance through SSH, I executed:


```bash
lsblk -f
```

The output looked like this:

text

```text
nvme0n1
├─nvme0n1p1  ext4  cloudimg-rootfs  ...  /
├─nvme0n1p13 ext4  BOOT             ...  /boot
├─nvme0n1p14
└─nvme0n1p15 vfat  UEFI             ...  /boot/efi

nvme1n1
├─nvme1n1p1  ext4  cloudimg-rootfs
├─nvme1n1p13 ext4  BOOT
├─nvme1n1p14
└─nvme1n1p15 vfat  UEFI
```

The two disks were:

- `/dev/nvme0n1`: The temporary recovery instance's own disk.
- `/dev/nvme1n1`: The original EC2 instance's disk.

The original root filesystem was located on:


```text
/dev/nvme1n1p1
```

### Step 6.5: Mount the original filesystem

I tried mounting the disk:


```bash
sudo mkdir -p /mnt/recovery

sudo mount /dev/nvme1n1p1 /mnt/recovery
```

I then checked the mounted filesystem:

```bash
ls /mnt/recovery
```

The expected output included directories such as:

```text
bin
boot
dev
etc
home
lib
root
usr
var
```

This confirmed that I had successfully accessed the original Ubuntu installation.

## 7\. Using chroot to Access the Original Ubuntu Installation

Another useful Linux feature is `chroot`.

It allows you to change the apparent root directory of a process.

In this situation, it allowed me to execute commands within the original Ubuntu filesystem while running the recovery instance.

Before entering the chroot environment, I mounted the necessary virtual filesystems:


```bash
sudo mount --bind /dev /mnt/recovery/dev
sudo mount --bind /proc /mnt/recovery/proc
sudo mount --bind /sys /mnt/recovery/sys
sudo mount --bind /run /mnt/recovery/run
```

Then I entered the original installation:

```bash
sudo chroot /mnt/recovery
```

At this point, commands such as:

```bash
cat /etc/ufw/ufw.conf
```

were accessing the original installation's configuration rather than the recovery instance's configuration.

This distinction is very important.

Without chroot, commands must reference the original filesystem explicitly, for example:

```bash
cat /mnt/recovery/etc/ufw/ufw.conf
```

**Note that**: inside chroot, the original filesystem becomes `/`.

## 8\. Disabling UFW on the Original Installation

Once inside the chroot environment, I checked the UFW configuration:

```bash
cat /etc/ufw/ufw.conf
```

The configuration contained:

```text
ENABLED=yes
```

I have disabled it using:

```bash
ufw disable
```

Alternatively, I could have modified the configuration file directly:

```bash
sed -i 's/^ENABLED=yes/ENABLED=no/' /etc/ufw/ufw.conf
```

This ensures that UFW is disabled when the original system boots again.

### An additional step: Set an Ubuntu password

Since I was already inside the original Ubuntu installation, I also had the opportunity to set a password for the Ubuntu user:

```bash
passwd ubuntu
```

This is optional for restoring SSH access.

It can, however, be useful if password-based console login is needed later.

## 9\. Exiting chroot and Unmounting the Original Filesystem

After completing the changes, I exited the chroot environment:

```bash
exit
```

I then attempted to unmount the original filesystem:


```bash
sudo umount /mnt/recovery
```

However, I received another error:

text

```text
umount: /mnt/recovery: target is busy
```

This is a common Linux issue.

It means that some process is still accessing the mounted filesystem.

Possible causes include:

- A terminal whose current working directory is inside the mounted filesystem.
- A process running inside the mounted filesystem.
- Filesystems such as `/dev`, `/proc`, `/sys`, and `/run` still mounted beneath `/mnt/recovery`.

The correct approach is to inspect the mount hierarchy:

```bash
findmnt -R /mnt/recovery
```

And identify processes using the mount:

```bash
sudo fuser -vm /mnt/recovery
```

If the terminal is still inside the mounted directory, move outside it:

```bash
cd /
```

If virtual filesystems were mounted separately, unmount them first, in reverse order:

bash

```bash
sudo umount /mnt/recovery/run
sudo umount /mnt/recovery/sys
sudo umount /mnt/recovery/proc
sudo umount /mnt/recovery/dev
```

Finally:

```bash
sudo sync
sudo umount /mnt/recovery
```

**Note**: The volume should only be detached after the unmount succeeds.

## 10\. Reattaching the Original Volume

Once the original filesystem is safely unmounted:

1. Open EC2 → Volumes.
2. Select the original 16 GB EBS volume.
3. Detach it from the temporary recovery instance.
4. Attach it to the original EC2 instance.
5. Restore its original root device mapping.
6. Start the original EC2 instance.

**Note**: The root device mapping is important. Make sure the volume is attached as the original root volume, not as an additional data disk.

## 11\. Testing SSH Again

After the original instance starts, check its current public IPv4 address.

If the instance was stopped and started, its public IP may have changed unless it uses an Elastic IP. That was the case.

Then try:

```bash
ssh -i your-key.pem ubuntu@EC2_PUBLIC_IP
```

Assumption is that if UFW was the only problem and the AWS security group and networking are configured correctly, SSH should work again. That was the case and it worked!

**Security Note**: For a public-facing server, restricting SSH to trusted IP addresses is preferable.

## 12\. Lessons Learned

This experience taught me several useful lessons about AWS EC2 and Linux server administration. Let me mention a few here:

### A) Always allow SSH before enabling UFW

This is probably the most important lesson.

Before enabling UFW again, explicitly allow SSH. No, I am not talking about enabling SSH in SecurityGroup. I am talking about UFW. It is like making sure, when you leave home, you have your house keys before you slam the door behind you.

To enable UFW, use:

```bash
sudo ufw allow from YOUR_PUBLIC_IP to any port 22 proto tcp
sudo ufw enable
```

### B) EBS volume resizing and filesystem resizing are different operations

Increasing an EBS volume from 8 GB to 16 GB does not automatically mean that Ubuntu will use all the additional space.

<!-- Depending on the partition and filesystem configuration, you may also need to expand the partition and filesystem.

Useful commands include:

```bash
lsblk
df -h
```

For an ext4 filesystem, after the partition has been expanded, you can use:

```bash
sudo resize2fs /dev/YOUR_ROOT_PARTITION
```

The correct partition must be identified first. -->

### C) AWS status checks don't guarantee SSH connectivity

This sounds obvious but important and I overlooked it. An instance can pass all AWS status checks and still be inaccessible over SSH.

Status checks primarily monitor the health of the instance and its underlying infrastructure. They don't guarantee that your firewall, SSH daemon, or network configuration is correct.

### D) Keep an alternative recovery method available

AWS provides several ways to recover an inaccessible EC2 instance:

- EC2 Serial Console.
- AWS Systems Manager Session Manager.
- EBS volume attachment to another instance.

Knowing how to use these tools can save considerable time and, most importantly, your data! ...... and may be your job too :D

### E) Never modify an EBS volume without a backup

This is also somewhat obvious but important. Before making significant changes to an instance's root disk, create an EBS snapshot.

It provides an additional layer of protection if a recovery operation goes wrong.

## Conclusion

What initially looked like a problem caused by increasing an EBS volume turned out to be a firewall configuration issue.

The recovery process involved creating a temporary Ubuntu instance, attaching the original EBS volume, mounting the original root partition, entering the installation using chroot, and checking the UFW configuration.

Even when the original operating system is inaccessible, AWS's EBS architecture gives you a practical way to recover the filesystem and repair the installation without rebuilding the server.

And, most importantly, always remember:

**Allow SSH before enabling firewall on a remote Ubuntu server\!**