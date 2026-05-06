# NullSec-WifiExfil v2

> **USB Rubber Ducky payload — Multi-Platform WiFi Credential Extractor**
> Created by: **Mr. Ender** (with AI assistance)
> **Auto-Updated:** Daily AI checks for new AV evasion techniques

## ⚠️ LEGAL DISCLAIMER

**This tool is for AUTHORIZED SECURITY TESTING ONLY.**

Unauthorized access to computer systems is ILLEGAL. The creator(s) are NOT responsible for misuse. You assume 100% responsibility for your actions.

**See DISCLAIMER.txt for complete legal terms.**

---

## Description

Extracts all saved WiFi network profiles and their passwords from Windows 10/11, macOS, Linux, and Raspberry Pi targets. Results are saved to USB drive with optional export to Dropbox, Discord, or custom locations. Includes multi-AV evasion, stealth mode, and daily AI updates.

## Features

✅ **Cross-Platform Support**
- Windows 10/11 (all editions)
- macOS (10.15+, M1/M2/Intel)
- Linux (Debian, Ubuntu, CentOS, Fedora)
- Raspberry Pi OS (32-bit & 64-bit)

✅ **Multi-Export Options**
- Save to USB drive (auto-detected per platform)
- Export to Dropbox
- Send to Discord webhook
- Save to custom location

✅ **Multi-AV Evasion**
- Windows Defender (disable/enable)
- McAfee Total Protection
- Norton 360
- Kaspersky
- Avast Premium
- Bitdefender
- Auto-detection or manual selection
- Daily AI updates for new signatures

✅ **Advanced Features**
- Timestamped vault files (`wifi_vault_YYYYMMDD_HHMMSS.txt`)
- Protection mode (checks for file on desktop before execution)
- Binary configuration system (14 bits - more control)
- Hostname and username identification
- Hidden execution (no visible windows)
- No temporary files
- PowerShell history cleanup
- Stealth mode (event log clearing)
- Anti-Forensics (free space overwrite)
- Anti-Sandbox detection
- Auto-update system (checks daily for AV bypass updates)

## OS-Specific Configurations

### Windows 10/11
**Default Config:** `11001100000000`
- Full AV evasion support
- Event log clearing
- Registry modification capability
- Admin elevation support

### macOS (Intel & Apple Silicon)
**Default Config:** `11001100100000`
- Keychain extraction
- System log clearing
- SIP bypass (if available)
- Security framework evasion

### Linux (All Distributions)
**Default Config:** `11001101000000`
- NetworkManager integration
- systemd-journal clearing
- /var/log access
- Works with sudo or root

### Raspberry Pi OS
**Default Config:** `11001101100000`
- Headless system support
- GPIO bypass (if needed)
- Lightweight operation
- Auto-start capability

## Multi-AV Evasion System

### Supported Antivirus Software

| AV Name | Version | Evasion Method | Status |
|---------|---------|-----------------|--------|
| Windows Defender | 10/11 | Disable WinDefend, stop MRT | ✅ Active |
| McAfee | Latest | Process termination | ✅ Active |
| Norton 360 | Latest | Service shutdown | ✅ Active |
| Kaspersky | Latest | Quarantine bypass | ✅ Active |
| Avast | Latest | Real-time monitoring off | ✅ Active |
| Bitdefender | Latest | Boot-time isolation off | ✅ Active |
| Trend Micro | Latest | Auto-scan disable | ✅ Staged |
| F-Secure | Latest | Signature bypass | ✅ Staged |

### Daily AI Update System

The payload includes an automatic daily update check that:
- Queries GitHub for latest AV evasion signatures
- Updates capabilities based on new detection vectors
- Maintains compatibility with latest Windows/macOS/Linux versions
- Adapts to new sandbox detection methods
- Updates anti-forensics techniques

**Update Frequency:** Every 24 hours (configurable)
**Update Source:** Secure HTTPS repository
**Fallback:** Works offline if updates unavailable

## Setup

### Quick Setup (3 Steps)

1. Choose your OS from the preset configs below
2. Edit `payload.txt` and replace placeholders
3. Flash to USB Rubber Ducky

### Full Configuration

1. Edit `payload.txt` and configure:
   - `#DRIVE` - Your USB drive letter (Windows)
   - `#DRIVE_MACOS` - macOS volume path
   - `#DRIVE_LINUX` - Linux mount point
   - `#DRIVE_RPI` - Raspberry Pi mount point
   - `#WEBHOOK` - Your Discord webhook URL
   - `#DROPBOX` - Your Dropbox API token
   - `#CUSTOM_PATH` - Custom save location
   - `#CONFIG` - Binary configuration
   - `#AV_*` - Which AVs to target

2. Flash to your USB Rubber Ducky

## Binary Configuration (14 Bits)

The `#CONFIG` setting uses 14 bits to control features:

```
Position: 0  1  2  3  4  5  6  7  8  9  10 11 12 13
Feature:  USB DRP CUS PRO ACR RSV AVO STH AFR ASB AVM ACT UPD AUT

Bit 0:  USB = Save to USB (1=yes, 0=no)
Bit 1:  DRP = Save to Dropbox (1=yes, 0=no)
Bit 2:  CUS = Save to Custom (1=yes, 0=no)
Bit 3:  PRO = Protection Mode (1=yes, 0=no)
Bit 4:  ACR = Auto-Create (1=yes, 0=no)
Bit 5:  RSV = Reserved (always 0)
Bit 6:  AVO = AV Evasion (1=enable, 0=disable)
Bit 7:  STH = Stealth Mode (1=yes, 0=no)
Bit 8:  AFR = Anti-Forensics (1=yes, 0=no)
Bit 9:  ASB = Anti-Sandbox (1=yes, 0=no)
Bit 10: AVM = Multi-AV Mode (1=all, 0=auto-detect)
Bit 11: ACT = Auto-Update Check (1=yes, 0=no)
Bit 12: UPD = Use Updates (1=yes, 0=no)
Bit 13: AUT = Auto-Restart (1=yes, 0=no)
```

## OS-Specific Preset Configurations

### Windows 10/11
```
1 1 0 0 1 1 0 0 0 0 0 = 11001100000
  │ │ │ │ │ │ │ │ │ │ └─ Auto-restart disabled
  │ │ │ │ │ │ │ │ │ └─── Updates disabled
  │ │ │ │ │ │ │ │ └───── Auto-check disabled
  │ │ │ │ │ │ │ └─────── Multi-AV disabled (auto-detect)
  │ │ │ │ │ │ └───────── Anti-Sandbox disabled
  │ │ │ │ │ └─────────── Anti-Forensics disabled
  │ │ │ │ └───────────── Stealth disabled
  │ │ │ └─────────────── AV Evasion enabled
  │ │ └───────────────── Reserved
  │ └─────────────────── Auto-Create enabled
  └───────────────────── Protection disabled
  USB + Dropbox enabled
```
**Full:** `11001101110 = Max stealth + all AV evasion`

### macOS
```
11001100100 = USB + Dropbox + AV Evasion + Multi-AV
11001101101 = ^ + Anti-Sandbox + Anti-Forensics
11001101110 = ^ + Stealth Mode (maximum)
```

### Linux / Raspberry Pi
```
11001100100 = USB + Dropbox + AV Evasion
11001101110 = ^ + All anti-detection
```

## Output Format

```
WiFi Credentials - HOSTNAME (username) - YYYYMMDD_HHMMSS
NetworkName1 | WPA2-Personal | Authentication: Open | Encryption: CCMP | Key Content: password123
NetworkName2 | WPA2-Enterprise | Authentication: 802.1X | Encryption: CCMP | Key Content: (no key)
OpenNetwork | Open | Authentication: Open | Encryption: None | Key Content: (no key)
```

## Target Systems

| OS | Privileges | Notes | AV Support |
|----|-------------|-------|------------|
| Windows 10/11 | Standard/Admin | Full WiFi + AV evasion | Defender, McAfee, Norton, Kaspersky, Avast, Bitdefender |
| macOS | Standard/Admin | Keychain + System logs | Native detection only |
| Linux | Standard/Root | NetworkManager + wpa_supplicant | Limited AV presence |
| Raspberry Pi | Standard/Root | Works headless | Limited AV presence |

## Usage Workflow

1. Choose your target OS
2. Copy appropriate config from "OS-Specific Presets"
3. Edit `payload.txt` with your settings
4. Flash to USB Rubber Ducky
5. Plug into target system
6. Payload executes automatically (2-3 seconds)
7. Check configured export location for results
8. File is named: `wifi_vault_YYYYMMDD_HHMMSS.txt`

## File Locations (After Execution)

**Windows:** `E:\wifi_vault_TIMESTAMP.txt` (or configured drive)
**macOS:** `/Volumes/[USB]/wifi_vault_TIMESTAMP.txt`
**Linux:** `/mnt/[USB]/wifi_vault_TIMESTAMP.txt`
**Raspberry Pi:** `/media/pi/[USB]/wifi_vault_TIMESTAMP.txt`

## Auto-Update System

### How It Works

1. Payload checks for daily updates (if Bit 11 = 1)
2. Connects to GitHub repository
3. Downloads latest AV evasion signatures
4. Applies new techniques automatically
5. Logs update status (if logging enabled)

### Enable Auto-Updates

1. Find `#CONFIG` in payload.txt
2. Set Bit 11 to `1` (auto-update check)
3. Set Bit 12 to `1` (use updates)
4. Save and flash

### Update Repository

**File:** `av-evasion-updates.txt` (in same directory)
**Update Frequency:** Daily
**Format:** JSON with AV signatures and bypass techniques

## Security Considerations

⚠️ **Important Notes:**
- Use only on authorized systems
- Disable AV evasion if testing on your own system
- Always have a backup USB
- Test in isolated environment first
- Understand local computer fraud laws

## vs Version 1

| Feature | v1 | v2 |
|---------|----|----|n| Platforms | Windows | Windows, macOS, Linux, RPi |
| AV Evasion | None | Multi-AV with 6 targets |
| Auto-Updates | No | Daily AI updates |
| Config Bits | 6 | 14 (7x more control) |
| Stealth Mode | Basic | Advanced (log clearing) |
| Anti-Forensics | No | Yes (free space overwrite) |
| OS-Specific Configs | None | 4 presets |
| Multi-Export | 1 | 4 methods |

## Author

**Version 2:** Mr. Ender (with AI assistance)  
**Original Concept:** NullSec (bad-antics)  
**Daily Updates:** Automated AI system  

## License

See LICENSE file in repository.

---

**Remember:** Only use this tool on systems you own or have explicit permission to test. Unauthorized access is illegal.