# NullSec-WifiExfil v2

> **USB Rubber Ducky payload — Multi-Platform WiFi Credential Extractor**
> Created by: **Mr. Ender** (with AI assistance)

## ⚠️ LEGAL DISCLAIMER

**This tool is for AUTHORIZED SECURITY TESTING ONLY.**

Unauthorized access to computer systems is ILLEGAL. The creator(s) are NOT responsible for misuse. You assume 100% responsibility for your actions.

**See DISCLAIMER.txt for complete legal terms.**

---

## Description

Extracts all saved WiFi network profiles and their passwords from Windows 10/11, macOS, Linux, and Raspberry Pi targets. Results are saved to USB drive with optional export to Dropbox, Discord, or custom locations.

## Features

✅ **Cross-Platform Support**
- Windows 10/11
- macOS (10.15+)
- Linux (Debian/Ubuntu)
- Raspberry Pi OS

✅ **Multi-Export Options**
- Save to USB drive (auto-detected)
- Export to Dropbox
- Send to Discord webhook
- Save to custom location

✅ **Advanced Features**
- Timestamped vault files (`wifi_vault_YYYYMMDD_HHMMSS.txt`)
- Protection mode (checks for file on desktop before execution)
- Binary configuration system (control each feature)
- Hostname and username identification
- Hidden execution (no visible windows)
- No temporary files
- PowerShell history cleanup

## Setup

1. Edit `payload.txt` and configure:
   - `#WEBHOOK` - Your Discord webhook URL
   - `#DROPBOX` - Your Dropbox API token
   - `#CUSTOM_PATH` - Custom save location
   - `#CONFIG` - Binary configuration (see below)

2. Flash to your USB Rubber Ducky

## Binary Configuration

The `#CONFIG` setting uses 6 bits to control features:

```
Position: 0 1 2 3 4 5
Feature:  U D C P A X

U = Save to USB (1=yes, 0=no)
D = Save to Dropbox (1=yes, 0=no)
C = Save to Custom Location (1=yes, 0=no)
P = Protection Mode (1=yes, 0=no)
A = Auto-Create Vault (1=yes, 0=no)
X = Reserved (0)
```

### Configuration Examples

- `110000` - USB + Dropbox only
- `100100` - USB + Protection mode
- `111111` - All features enabled
- `100000` - USB only (basic)
- `110011` - USB + Dropbox + Protection + Auto-Create

## Output Format

```
WiFi Credentials - HOSTNAME (username)
NetworkName1 | WPA2-Personal | password123
NetworkName2 | WPA2-Enterprise | (no key)
OpenNetwork | Open | (no key)
```

## Target Systems

| OS | Privileges | Notes |
|----|-------------|-------|
| Windows 10/11 | Standard User | Admin needed for some passwords |
| macOS | Standard User | May require password prompt |
| Linux | Standard User | Root needed for full WiFi data |
| Raspberry Pi | Standard User | Works on headless systems |

## Usage

1. Configure `payload.txt` with your settings
2. Flash to USB Rubber Ducky
3. Plug into target system
4. Payloade executes automatically
5. Check configured export locations for results

## File Locations

**Windows:** `E:\wifi_vault_TIMESTAMP.txt` (or configured drive)
**macOS:** `/Volumes/[USB]/wifi_vault_TIMESTAMP.txt`
**Linux:** `/mnt/[USB]/wifi_vault_TIMESTAMP.txt`
**Raspberry Pi:** `/media/pi/[USB]/wifi_vault_TIMESTAMP.txt`

## Protection Mode

If enabled, the payload checks for a file named `my_password` on the Desktop. If found, execution stops.

To activate protection:
1. Create empty file: `C:\Users\Public\Desktop\my_password`
2. Set `#CONFIG` bit 4 to `1`

## vs Version 1

| Feature | v1 | v2 |
|---------|----|----|n| Platforms | Windows only | Windows, macOS, Linux, RPi |
| Export Methods | Discord only | USB, Dropbox, Discord, Custom |
| Configuration | Webhook only | Binary config (6 features) |
| Protection Mode | No | Yes |
| Timestamping | No | Yes |
| Auto USB detect | No | Yes |
| Stealthiness | Basic | Enhanced |

## Author

**Version 2:** Mr. Ender (with AI assistance)  
**Original Concept:** NullSec (bad-antics)

## License

See LICENSE file in repository.

---

**Remember:** Only use this tool on systems you own or have explicit permission to test. Unauthorized access is illegal.