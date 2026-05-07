# NullSec-WifiExfil Version 2

> **USB Rubber Ducky payload — WiFi Credential Exfiltrator with Dropbox & Discord Export**

## Overview

Version 2 improves upon the original payload with:
- **Fixed PowerShell macro substitution bug**
- **Dropbox direct upload support** (export to specific file path)
- **Better error handling & timestamps**
- **Comprehensive setup guides** for both export methods
- **Security best practices**

## Features

- ✅ Enumerates all saved WiFi profiles
- ✅ Extracts plaintext passwords (WPA/WPA2 key content)
- ✅ Captures authentication type for each network
- ✅ Reports hostname and username for identification
- ✅ Single-line PowerShell execution (no temp files)
- ✅ Hidden window execution
- ✅ **Dropbox API v2 direct upload** to specific file
- ✅ **Discord webhook exfiltration**
- ✅ **Timestamped exports**
- ✅ **Error handling & status reporting**

## Quick Start

### Option 1: Dropbox Export (Recommended)

1. Follow the **[Dropbox Setup Guide](./DROPBOX_SETUP.md)**
2. Get your **Dropbox Access Token**
3. Edit `payload-dropbox.txt`:
   ```
   DEFINE #DROPBOX_TOKEN your_access_token_here
   DEFINE #DROPBOX_PATH /payloads/wifi-logs
   ```
4. Flash to USB Rubber Ducky

### Option 2: Discord Webhook

1. Create a Discord webhook in your server
2. Edit `payload-discord.txt`:
   ```
   DEFINE #WEBHOOK_URL your_webhook_url_here
   ```
3. Flash to USB Rubber Ducky

## Output Examples

### Dropbox Output File
```
=== WiFi Credentials Exfiltration ===
Timestamp: 2026-05-07 14:23:45
Hostname: DESKTOP-ABC123
Username: john_user

Networks Found: 3

[1] HomeNetwork
    Authentication: WPA2-Personal
    Password: MySecurePassword123!

[2] OfficeWiFi
    Authentication: WPA2-Enterprise
    Password: (no key)

[3] Cafe_Guest
    Authentication: Open
    Password: (no key)

=== End Report ===
```

### Discord Output
```
**WiFi Credentials - DESKTOP-ABC123 (john_user)**
[2026-05-07 14:23:45]
\`\`\`
HomeNetwork | WPA2-Personal | MySecurePassword123!
OfficeWiFi | WPA2-Enterprise | (no key)
Cafe_Guest | Open | (no key)
\`\`\`
```

## Target Requirements

- **OS:** Windows 10/11
- **Privileges:** Standard user (for viewing profiles)
- **Note:** Admin required to see passwords on some configurations
- **Network:** Target must have internet connection for upload

## Files Included

| File | Purpose |
|------|----------|
| `payload-dropbox.txt` | Main payload - Dropbox export (recommended) |
| `payload-discord.txt` | Alternative - Discord webhook export |
| `DROPBOX_SETUP.md` | Step-by-step Dropbox app & token setup |
| `DISCORD_SETUP.md` | Step-by-step Discord webhook setup |
| `TROUBLESHOOTING.md` | Common issues & fixes |

## Payload Execution Flow

```
1. PowerShell launches hidden
2. Enumerates WiFi profiles via netsh
3. Extracts password & auth type for each
4. Formats data with timestamp
5. Uploads to Dropbox OR sends to Discord
6. Returns success/error status
7. Exits cleanly
```

## What Was Fixed in V2

### Bug Fix: Macro Substitution
**V1 Issue:**
```powershell
$wh='#WEBHOOK'  # ❌ This literally uses the string "#WEBHOOK" not the actual URL
```

**V2 Fix:**
```powershell
$wh='#WEBHOOK_URL'  # ✅ Correctly substitutes the token
DEFINE #WEBHOOK_URL your_url_here
```

### Improvements:
- Added timestamps to all exports
- Better error handling with try-catch
- Improved formatting for readability
- Support for both Dropbox and Discord
- Comprehensive setup guides

## Security Notes

⚠️ **Important:**
- Store tokens securely; treat like passwords
- Use a dedicated Dropbox app (not your main account)
- Test in a controlled environment first
- Comply with local laws & policies
- Only use for authorized security testing

## Author & Credits

- **Original:** NullSec (bad-antics)
- **Version 2 Improvements:** See Git history
- **Repository:** GreyFA/usbrubberducky-payloads

## Support

See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) for:
- Common errors & solutions
- File permission issues
- Token validation
- Network connectivity problems
