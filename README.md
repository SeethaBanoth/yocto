# yocto
# Yocto Project – Interview Questions & Answers

This document covers commonly asked Yocto Project and BitBake interview questions with simple and clear explanations.

---

## 1️⃣ What is BitBake and how does it work?

**Answer:**

BitBake is the build engine used by the Yocto Project.

It reads build instructions written in recipes (`.bb` files), resolves dependencies, and executes tasks to build packages, images, and SDKs.

### How BitBake works:
1. Parses configuration files (`local.conf`, `bblayers.conf`)
2. Parses recipes and classes
3. Resolves build-time and run-time dependencies
4. Creates a task dependency graph
5. Executes tasks in the correct order
6. Produces packages and final Linux image

👉 BitBake is similar to `make`, but designed for building **entire Linux systems**.

---

## 2️⃣ What is Yocto and what is the difference between Yocto and Buildroot?

**Answer:**

Yocto is a framework used to build **custom embedded Linux distributions** for specific hardware.

### Difference between Yocto and Buildroot:

| Yocto | Buildroot |
|-----|----------|
| Framework, not a distro | Build system |
| Uses BitBake | Uses Make |
| Supports package management | No package manager |
| Suitable for large projects | Suitable for small projects |
| Reproducible builds | Faster but less flexible |

👉 **Yocto is preferred for complex and production-level systems**, while Buildroot is good for quick prototypes.

---

## 3️⃣ Why is Yocto preferred in production systems?

**Answer:**

Yocto is preferred in production because:
- Highly customizable
- Reproducible builds
- Strong dependency management
- Supports long-term maintenance (LTS releases)
- Scales well for large and complex products
- Widely used by semiconductor vendors

👉 It provides **full control over the Linux distribution**.

---

## 4️⃣ What is a layer and how do you create a custom layer?

**Answer:**

A layer is a logical collection of:
- Recipes
- Configuration files
- Classes

Layers help organize and isolate functionality.

### Create a custom layer:
```bash
bitbake-layers create-layer meta-custom
bitbake-layers add-layer meta-custom

---

## 5️⃣ What is Poky? What does it contain and what do you modify in it?

**Answer:**

Poky is the **reference distribution of the Yocto Project**.

It provides a complete working setup to start building custom embedded Linux images.

### Poky contains:
- **BitBake** → build engine
- **OpenEmbedded-Core (OE-Core)** → core recipes and classes
- **Meta layers** → `meta`, `meta-poky`, `meta-yocto-bsp`
- **Sample images** → `core-image-minimal`, `core-image-full-cmdline`
- **Build environment scripts**

### What you modify in Poky:
- `build/conf/local.conf` → machine, image type, features
- `build/conf/bblayers.conf` → add/remove layers

👉 You **do not modify Poky source directly** for product changes.  
Customizations are done using **custom layers**.

---

## 7️⃣ What is BitBake execution pipeline?

**Answer:**

BitBake follows a **task-based execution pipeline** to build software and images in Yocto.  
Each task represents one stage of the build process and is executed in a defined order.

### BitBake execution pipeline:
do_fetch
↓
do_unpack
↓
do_patch
↓
do_configure
↓
do_compile
↓
do_install
↓
do_package
↓
do_rootfs

### Task explanation:
- **do_fetch** → Downloads source code
- **do_unpack** → Extracts source archive
- **do_patch** → Applies patches
- **do_configure** → Configures build system
- **do_compile** → Compiles source code
- **do_install** → Installs files into staging area
- **do_package** → Creates binary packages
- **do_rootfs** → Builds final root filesystem

👉 BitBake executes only required tasks using **dependency tracking and shared state cache (sstate)**.

---

## 8️⃣ What are the common errors faced while building Yocto projects?

**Answer:**

During Yocto builds, several common errors are encountered.

### Common errors and causes:

- **Nothing PROVIDES 'xyz'**
  - Required recipe or layer is missing
  - Fix: Add the correct layer to `bblayers.conf`

- **Failed to fetch URL**
  - Network issue or incorrect `SRC_URI`
  - Fix: Check internet or source URL

- **Task failed: do_compile**
  - Compilation error or missing dependency
  - Fix: Check log file in `tmp/work/`

- **Layer compatibility error**
  - Layer branch does not match Yocto release
  - Fix: Use correct branch for all layers

- **Disk space error**
  - Insufficient storage for build
  - Fix: Ensure at least 100GB free space

- **Permission denied**
  - Incorrect directory permissions
  - Fix: Fix ownership of build directory

👉 Most Yocto errors can be debugged by checking task logs and BitBake error messages.

---

## 📌 One-line interview summary

**BitBake executes a structured task pipeline to build Yocto images, and most build errors are related to missing layers, dependencies, or configuration issues.**
