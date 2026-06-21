# 📱 Something OS

Welcome to the official **Something OS** developer organization. Something OS is a production-ready, modular, AOSP-inspired Linux distribution built for mobile devices, scaling a hardware-accelerated Qt6/QML desktop shell on top of a modern mainline kernel and custom Ubuntu base.

---

## 🗺️ Workspace Architecture

We adopt an AOSP-style decoupled directory structure to keep core code, device trees, and proprietary firmware cleanly separated:

| Repository | Path in Source Tree | Description |
| :--- | :--- | :--- |
| [**manifests**](https://github.com/Something-OS/manifests) | `.repo/manifests` | XML configuration mapping out the entire OS workspace tree. |
| [**build**](https://github.com/Something-OS/build) | `build/` | Standardized, colorized build toolchain scripts and environmental setup. |
| [**device_oneplus_fajita**](https://github.com/Something-OS/device_oneplus_fajita) | `device/oneplus/fajita` | Board configurations, system configurations, and device-specific overlays. |
| [**vendor_oneplus_fajita**](https://github.com/Something-OS/vendor_oneplus_fajita) | `vendor/oneplus/fajita` | Proprietary modem and graphics firmware blobs, decoupled from open-source code. |
| [**androidshell**](https://github.com/Something-OS/androidshell) | `androidshell/` | Premium Qt6/QML mobile desktop shell featuring status bar, navigation, and power controls. |
| [**releases**](https://github.com/Something-OS/releases) | *(External Release)* | Pre-configured base Ubuntu rootfs tarballs and system flashable zips. |

---

## ⚡ Quick Start Guide

To synchronize the workspace and compile Something OS on your host machine, follow these steps:

### 1. Synchronize the Workspace
Ensure you have the `repo` tool installed, then initialize and sync the workspace:
```bash
# Initialize workspace pointing to our manifests on the main branch
repo init -u https://github.com/Something-OS/manifests.git -b main

# Sync all repositories in parallel
repo sync -j$(nproc)
```

### 2. Set Up Prerequisites
Install the required cross-compilers and build dependencies on your Ubuntu/Debian host:
```bash
sudo apt-get install gcc-aarch64-linux-gnu g++-aarch64-linux-gnu android-tools-fsutils cpio sshpass wget
```

### 3. Bootstrap the Rootfs
Initialize your build shell environment and fetch/provision the base target filesystem:
```bash
# Load build commands
source build/envsetup.sh

# Select target combo
lunch fajita

# Download the base rootfs release dynamically
m setup-rootfs

# Chroot and install essential tools, Qt6 libraries, systemd configurations, and users
m bootstrap-rootfs
```

### 4. Build System Images
Compile all targets (bootloader image, linux kernel, desktop shell, and system rootfs image):
```bash
# Build everything at once
m all

# Or compile individual targets:
m bootimg          # Build ramdisk & kernel boot image
m kernel           # Compile kernel sources and DTB nodes
m desktop          # Compile AndroidShell inside chroot
m rootfs           # Package Ubuntu partition raw/sparse image
```

---

## 🤝 Contributing & Porting
Want to bring up Something OS on a new device? Refer to our detailed **Device Bringup Guide** located in the [manifests repository README](https://github.com/Something-OS/manifests#device-bringup-guide).
