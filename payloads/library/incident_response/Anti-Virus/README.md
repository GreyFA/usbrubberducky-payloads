```markdown
# Anti-Virus (Rubber Ducky) - ENDER RESPECT v2025

This payload is an on-insert incident response helper for USB Rubber Ducky drives. It scans common user locations, scheduled tasks, user accounts, and autoruns for indicators used by malicious Rubber Ducky payloads and similar persistence mechanisms.

Features
- Logs findings to X:\Report.txt on the USB (make sure the drive letter is correct / set to storage mode)
- Detects legacy signatures (e.g. `1101`), common filename patterns, and simple content heuristics
- Detects suspicious local user accounts and scheduled tasks
- Detects registry autoruns under HKCU/HKLM
- Report-only by default; destructive actions (delete users/tasks/files/autoruns) require entering the Boss PIN

Security and Usage
- Default Boss PIN: `1337`. Change this in the payload if you need a different PIN before deployment.
- The payload runs PowerShell with `-ExecutionPolicy Bypass` to perform scanning and (optionally) removal.
- The script writes a human-readable log to `X:\Report.txt`. Ensure the USB is mounted and that `X:` matches the USB drive letter on target systems.
- Files marked with the first line `#IMMUNE#` will be skipped (useful for shielding critical files when testing).

Customization
- Edit the arrays `$userPatterns`, `$tasks`, `$searchPaths`, and `$patterns` in the payload to add or remove detection rules.
- Replace the PIN in `ConfirmDelete` if you want a different authorization code.

Testing
1. Put the payload on a Rubber Ducky (or compatible emulator) and insert on a test VM.
2. Ensure the USB exposes the drive letter `X:` and the device is in storage mode for the log to be written.
3. Review `X:\Report.txt` after execution.

Caveats
- This payload attempts to be cautious: destructive actions only occur after PIN authorization. Use caution and test in a controlled environment.
- Detection is heuristic-based and may produce false positives.

License
- Use at your own risk. Modify as needed.
```
