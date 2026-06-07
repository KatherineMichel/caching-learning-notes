# Linux Filesystem Hierarchy Standard

ChatGPT sumnary of [Linux Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)

**Filesystem Hierarchy Standard (FHS) 3.0 — Summary**

The **Filesystem Hierarchy Standard (FHS)** is a specification from the Linux Foundation that defines how directories and files should be organized in UNIX-like operating systems. Its goal is to ensure **consistency and interoperability** across systems so that software, tools, and users can reliably find files in expected locations.

---

## 🔑 Core Idea

FHS standardizes **where things live in the filesystem**—for example:

* Programs
* Configuration files
* Libraries
* Logs
* User data

This makes systems easier to manage, develop for, and document.

---

## 📁 Main Filesystem Structure

### Root (`/`)

* The top-level directory that contains everything.
* Must include essential directories needed for booting and system recovery.

---

## ⚙️ Key Directories in `/`

* **`/bin`** – Essential user commands (e.g., basic shell utilities)
* **`/sbin`** – System administration binaries
* **`/boot`** – Bootloader files (kernel, initrd)
* **`/dev`** – Device files (hardware interfaces)
* **`/etc`** – System-wide configuration files
* **`/home`** – User home directories
* **`/lib`** – Essential shared libraries and kernel modules
* **`/media`** – Mount points for removable media (USB, CDs)
* **`/mnt`** – Temporary mount points
* **`/opt`** – Optional/add-on software packages
* **`/root`** – Root user’s home directory
* **`/run`** – Runtime system data (since boot)
* **`/srv`** – Data served by system services (e.g., web content)
* **`/tmp`** – Temporary files (often cleared on reboot)

---

## 📦 `/usr` Hierarchy (User System Resources)

Contains **read-only, shareable data and programs**:

* **`/usr/bin`** – Most user commands
* **`/usr/sbin`** – Non-essential system binaries
* **`/usr/lib`** – Libraries for programs
* **`/usr/share`** – Architecture-independent data (docs, man pages)
* **`/usr/local`** – Locally installed software (safe from system updates)

---

## 🔄 `/var` Hierarchy (Variable Data)

Stores data that **changes frequently**:

* **`/var/log`** – Log files
* **`/var/tmp`** – Temp files preserved across reboots
* **`/var/lib`** – Application state data
* **`/var/cache`** – Cached data
* **`/var/spool`** – Queued tasks (e.g., printing, mail)

---

## 🧠 Key Principles

* **Separation of concerns**:
  Static vs. variable data, user vs. system data
* **Predictability**:
  Programs know where to find configs, logs, binaries
* **Portability**:
  Software works across different Linux distributions
* **Modularity**:
  Supports mounting different parts of the filesystem separately

---

## 🐧 Linux-Specific Additions

Includes virtual filesystems like:

* **`/proc`** – Process and kernel info
* **`/sys`** – Hardware and kernel interface

---

## 📌 Bottom Line

FHS is essentially a **blueprint for organizing a Linux filesystem**, ensuring that:

* Developers know where to install files
* Sysadmins know where to manage them
* Users experience consistency across systems

It’s foundational for how Linux systems stay organized and interoperable.
