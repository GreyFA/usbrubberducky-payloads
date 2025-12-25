# Anti-Virus (Rubber Ducky)

This payload is an on-insert incident response helper for USB Rubber Ducky drives. It scans common user locations, scheduled tasks, user accounts, and autoruns for indicators used by malicious Rubber Ducky payloads and similar persistence mechanisms.

Features
- Logs findings to X:\Report.txt on the USB (make sure the drive letter is correct / set to storage mode)
- Detects legacy signatures (e.g. `1101`), common filename patterns, and simple content heuristics
- Detects suspicious local user accounts and scheduled tasks
- Detects registry autoruns under HKCU/HKLM
- Report-only by default; destructive actions (delete users/tasks/files/autoruns) require entering the Boss PIN

Security and Usage
- Default Boss PIN: `1337`. Change this in the payload if you need a different PIN before deployment.
- Payloads in this repository are updated monthly; pull the latest version before deployment.

License
- Use at your own risk. Modify as needed.
