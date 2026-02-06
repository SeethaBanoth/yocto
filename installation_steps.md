# Commands to build a Yocto Project

## 🧱 STEP 0: System Requirements (One Time)

👉 Use Ubuntu 20.04 or 22.04 (64-bit)
👉 At least 100 GB free disk
👉 Internet required

## 🧩 STEP 1: Install Required Packages
```c
sudo apt update
sudo apt install gawk wget git diffstat unzip texinfo gcc \
build-essential chrpath socat cpio python3 python3-pip \
python3-pexpect xz-utils debianutils iputils-ping \
python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
pylint xterm
```
**🔍 What is the use?**

These tools are needed for:

- Downloading source code

- Compiling software

- Running BitBake

Without these, Yocto will fail.

## 🧩 STEP 2: Download Yocto (Poky)
```c
git clone -b kirkstone git://git.yoctoproject.org/poky.git
```
**🔍 What is the use?**

Downloads Poky (Yocto reference build system)

kirkstone = stable LTS version

**📁 New folder created:**

poky/

## 🧩 STEP 3: Download Raspberry Pi Layer
```c
cd poky
git clone -b kirkstone https://github.com/agherzan/meta-raspberrypi.git
```
**🔍 What is the use?**

- Adds hardware support for Raspberry Pi boards

- Without this, Yocto doesn’t know how to build for RPi4

## 🧩 STEP 4: Initialize Build Environment
```c
source oe-init-build-env
```
**🔍 What is the use?**

- Creates build/ directory

- Sets Yocto environment variables

Moves you into:

poky/build/

## 🧩 STEP 5: Edit bblayers.conf using vi
```c
vi conf/bblayers.conf
```
**🔍 What is the use?**

Tells BitBake which layers to use

**✍️ Inside vi, add this line:**
```c
${TOPDIR}/../meta-raspberrypi \
```

📌 Add it inside BBLAYERS.

## 🧩 STEP 6: Edit local.conf using vi
```c
vi conf/local.conf
```
**🔍 What is the use?**

Main configuration file

**Controls:**

- Target machine

- Image features

- Packages

**✍️ Change these lines:**

1️⃣ Set Raspberry Pi 4
```c
MACHINE = "raspberrypi4-64"
```
2️⃣ Enable WiFi & SSH
```c
DISTRO_FEATURES:append = " wifi ssh"
```
3️⃣ Accept licenses
```c
LICENSE_FLAGS_ACCEPTED = "commercial"
```
4️⃣ Increase build speed (optional)
```c
BB_NUMBER_THREADS = "8"
PARALLEL_MAKE = "-j8"
```

Save and exit (:wq).

## 🧩 STEP 7: Build Image for Raspberry Pi 4
```c
bitbake core-image-base
```
**🔍 What is the use?**

Starts full Yocto build

Builds:

- Kernel

- Root filesystem

- Drivers

- Bootloader

⏳ First build takes 2–4 hours

## 🧩 STEP 8: Locate Output Image
```c
cd tmp/deploy/images/raspberrypi4-64/
ls
```
**🔍 What is the use?**

Shows generated files

You will see:
```c
core-image-base-raspberrypi4-64.wic.bz2
```
## 🧩 STEP 9: Flash Image to SD Card
```c
no
bunzip2 core-image-base-raspberrypi4-64.wic.bz2
sudo dd if=core-image-base-raspberrypi4-64.wic of=/dev/sdX bs=4M status=progress
sync
```
**🔍 What is the use?**

Writes Yocto image to SD card

/dev/sdX = your SD card (be careful!)

## 🧩 STEP 10: Boot Raspberry Pi 4

1️⃣ Insert SD card
2️⃣ Power ON RPi
3️⃣ Login:

username: root
(no password)


🎉 Yocto Linux is running!
