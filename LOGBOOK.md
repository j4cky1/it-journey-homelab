# 📓 Project Logbook: IT-Security Home-Lab

This logbook tracks the evolution of my home-lab, focusing on infrastructure stability, automation, and security hardening.

---

## 📅 January 01, 2026 | Base OS Installation & Hardware Setup
**Objective:** Transform a legacy Acer desktop into a dedicated headless server.

### 🛠 Tasks & Implementation
* **OS Selection:** Chose **Ubuntu Server 24.04 LTS** for its long-term support and stability.
* **Boot Media Preparation:** Used my Arch Linux (ThinkPad T480) workstation to flash the image.
    ```bash
    # Identified USB device with lsblk to prevent data loss on host
    sudo cp /path/to/ubuntu.iso /dev/sdX && sync
    ```
* **Hardware Config:** Adjusted BIOS settings (Disabled Secure Boot, enabled "AC Power Loss Recovery").

### 💡 Reflection
The `sync` command is a crucial step I learned to ensure the kernel flushes all buffers to the USB hardware before removal, preventing corrupt installation media.

---

## 📅 January 05, 2026 | Networking & Remote Access (SSH)
**Objective:** Establish a secure, headless management environment.

### 🛠 Tasks & Implementation
* **Static IP:** Configured via `netplan` to ensure the NAS is always reachable at the same address.
* **SSH Hardening:** * Disabled password authentication.
    * Implemented **Ed25519 Key-based authentication** for superior security over RSA.
* **Client Setup:** Configured `~/.ssh/config` on my ThinkPad for quick access.

---

## 📅 January 10, 2026 | Storage & Backup Automation (The Borg Incident)
**Objective:** Implement a "3-2-1" backup strategy using **BorgBackup**.

### 🛠 Tasks & Implementation
* **Storage:** Mounted a **2TB WD MyPassport** as the primary backup repository.
* **Automation:** Created a Bash script to automate the backup of `/home` and `/etc`.
* **The Challenge:** Encountered a `Failed to create/acquire the lock` error.
    * **Root Cause:** Unclean shutdown during a manual test run left a lock-file in the repository.
    * **Resolution:** Used `borg break-lock` and implemented a check-logic in the script to handle stale locks safely.

### 🛡 Security Focus
I moved the Borg passphrase into a hidden, restricted file (`.borg_pw`) instead of hardcoding it into the script. This follows the **Principle of Least Privilege**.

---

## 📅 January 13, 2026 | Retention & Maintenance
**Objective:** Automate storage management to prevent the backup drive from filling up.

### 🛠 Tasks & Implementation
* **Pruning:** Implemented a **7-4-6 retention policy** (7 daily, 4 weekly, 6 monthly backups).
* **Compaction:** Added `borg compact` to the automation script to reclaim free space physically.
* **Verification:** Added `borg check` to verify archive integrity after every run.

---

## 🚀 Current System Status
| Component | Technology | Status |
| :--- | :--- | :--- |
| **Server OS** | Ubuntu Server 24.04 LTS | ✅ Running |
| **Workstation**| Arch Linux (ThinkPad T480) | ✅ Configured |
| **Backups** | BorgBackup (Automated) | ✅ Active |
| **Security** | SSH Key-Auth Only | ✅ Hardened |
