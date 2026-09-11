# cybersec-lab-virtualbox-kali
# Cybersecurity Lab Setup — VirtualBox & Kali Linux

A local penetration-testing / ethical hacking lab built on VirtualBox, with Kali Linux configured as the attacking machine on an isolated NAT network.

This repo documents the setup process for **WK1-PM1**, the Week 1 lab task from the Networkwalks Academy Ethical Hacking / Cybersecurity course.

## 📋 Task Overview

Set up a cybersecurity testing lab environment on a laptop/PC with the following requirements:

- VirtualBox (latest recommended version) as the hypervisor
- Kali Linux configured as the attacking/hacker machine
- A dedicated NAT Network on subnet `10.0.0.0/24`
- Clipboard sharing and file drag-and-drop enabled on the VM
- A shared folder mapping `/downloads` from the host machine
- Kali Linux assigned a static IP of `10.0.0.2/24`
- Full internet access from Kali Linux

## 🖥️ Lab Architecture

```
Host OS (Windows 10)
  └── VirtualBox
        └── NAT Network "NatNetwork" — 10.0.0.0/24
              ├── Kali Linux ........... 10.0.0.2/24  (attacker)
              ├── Windows 11 ........... 10.0.0.11/24 (optional)
              ├── Windows 10 ........... 10.0.0.10/24 (optional)
              ├── Windows 7 ............ 10.0.0.7/24  (optional)
              ├── Server 2016 .......... 10.0.0.16/24 (optional)
              └── Android 9 ............ 10.0.0.9/24  (optional)
```

**Recommended host specs:** 8GB+ RAM, 256GB+ SSD, Core i3/i5 or equivalent.

## ⚙️ Setup Steps

### Phase 1 — Core Lab

1. **Install 7-Zip** — [7-zip.org/download.html](https://7-zip.org/download.html)
2. **Install VirtualBox** — [virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
3. **Create the NAT Network**
   - VirtualBox Manager → *File → Tools → Network*
   - NAT Networks tab → Create network named `NatNetwork`
   - IPv4 Prefix: `10.0.0.0/24`
   - Enable DHCP
4. **Download & import Kali Linux** — [kali.org/get-kali](https://kali.org/get-kali) (VirtualBox pre-built image)
5. **Attach Kali to the NAT Network**
   - VM Settings → Network → Adapter 1 → Attached to: `NAT Network` → Name: `NatNetwork`
6. **Configure Kali's static IP**
   - Network icon → *Edit Connections* → Wired connection 1 → IPv4 Settings
   - Method: Manual
   - Address: `10.0.0.2` / Netmask: `24` / Gateway: `10.0.0.1`
   - DNS: `8.8.8.8` (fallback to `10.0.0.1` if internet issues occur)
7. **Enable clipboard & drag-and-drop**
   - VM Settings → General → Advanced → Shared Clipboard: `Bidirectional`, Drag'n'Drop: `Bidirectional`
8. **Enable shared folder**
   - VM Settings → Shared Folders → Add → Path: host `Downloads` folder → check Auto-mount + Make Permanent
9. **Take a snapshot** of the clean, working VM (Machine → Take Snapshot)

### Phase 2 — Optional Extensions

- Add Windows 11 / 10 / 7, Windows Server 2016, and Android VMs to the same NAT Network for a fuller target range
- Ping-test connectivity between all VMs
- Snapshot each machine once configured

## 🛠️ Troubleshooting: No Internet on Kali

A known issue on VirtualBox v7 with Kali Linux 2026.1+:

1. Confirm the NAT Network was created correctly (`10.0.0.0/24`)
2. Confirm no other VM on the network is also using `10.0.0.2`
3. Run the following, then restart Kali:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

4. Restart the VM and host machine if the issue persists

## 📚 Credits

Lab task and reference material from **Networkwalks Academy** — Ethical Hacking & Cybersecurity training.
🔗 [networkwalks.com](https://www.networkwalks.com)

## ⚠️ Disclaimer

This lab is strictly for authorized, personal learning and ethical hacking practice in an isolated virtual environment. Do not use these tools or techniques against systems you do not own or have explicit written permission to test.
