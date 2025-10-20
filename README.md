# LFS Guide (Simplified)
A beginner-friendly, step-by-step walkthrough for building **Linux From Scratch (LFS)**.

> ⚠️ **Disclaimer:**  
> Please refer to the [official Linux From Scratch guide](https://www.linuxfromscratch.org/lfs/view/stable/index.html). This guide is just an overview, a shortened LFS guide. Refer to the LFS document for more

## Table of Contents
1. [System Setup](#1-system-setup)
2. [Preparing the Base OS](#2-preparing-the-base-os)
3. [Packages and Patches](#3-packages-and-patches)
4. [Final Preparation](#4-final-preparation)
5. [Building the Cross-Toolchain](#5-building-the-cross-toolchain)
6. [Cross-Compiling Temporary Tools](#6-cross-compiling-temporary-tools)
7. [Entering Chroot & Building Tools](#7-entering-chroot--building-tools)
8. [Installing Basic System Software](#8-installing-basic-system-software)

## STEP 1 System Setup
:Set up the environment where we will use to build LFS. 

### My VM Spec
- VMware Workstation 17
- Ubuntu 24.04.3
- RAM: 8 GB
- Processor: 1 CPU
- 80 GB of storage

## STEP 2 Preparing your base OS for building LFS

### basic requirement 
- [install required packages](https://www.linuxfromscratch.org/lfs/view/10.1-rc1/chapter02/hostreqs.html)
```shell
sudo apt install bison gawk m4 texinfo 
sudo ln -sf bash /usr/bin/sh
```
- You could also run a version check after installing all the packages using this [version-check.sh](https://www.linuxfromscratch.org/lfs/view/10.1-rc1/chapter02/hostreqs.html)

### Create new partition

We will be using the fdisk to do this. is you're not familiar with the program I recommend this [comprehensive guide](https://www.howtogeek.com/106873/how-to-use-fdisk-to-manage-partitions-on-linux/). We will start by listing the partitions we have and then start creating a new partition for building LFS.

- use -l to list the partition
```shell
sudo fdisk -l /dev/sda
```
- Create a GPT partition
```shell
fdisk /dev/sda
```
- after entering fdisk, use these commands to create a GPT partition

    - g : insert func
    - n : insert func
    - p : insert func
    - w : insert func

- Create a file system on the partition we just created 

    - format the partition to ext4 using ```mkfs```
    ```shell
    sudo mkfs -v -t ext4 <you new partition name>
    ```
- Set the $LFS Variable

```shell
export LFS=mnt/lfs
```
- mount the new partition
```shell
sudo -E mkdir -pv $LFS
sudo -E mount -v -t ext4 <you new partition name> $LFS
```

## STEP 3 Packages and Patches

- create a dir for our packages
```shell
mkdir -v $LFS/sources
chmod -v a+wt $LFS/sources #all write permission
```
- download packages for our LFS build 
```shell
wget https://www.linuxfromscratch.org/lfs/view/stable/wget-list-sysv
wget --input-file=wget-list-sysv --continue --directory-prefix=$LFS/sources
```

## STEP 4 Final Prep
After this, we can start building our LFS