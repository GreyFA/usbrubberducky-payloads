# CAPSLOCK_SAFETY Extension

| Field       | Value              |
|-------------|--------------------|
| Author      | Mr.Ender           |
| Version     | 1.0                |
| Target      | Any                |
| DuckyScript | 3.0                |

## Description

A kill switch extension for any payload. If CapsLock is **ON** when the Ducky is plugged in, the payload stops immediately. If CapsLock is **OFF**, the payload runs normally.

Useful as a safety abort — toggle CapsLock on before plugging in to prevent accidental execution.

## Usage

Place the extension at the top of your payload and call `SAFETY_CHECK()` before any payload logic.

```ducky
REM *** TYPE "CAPSLOCK_SAFETY" IN PAYLOADSTUDIO — SELECT FROM AUTOCOMPLETE ***

SAFETY_CHECK()

REM ... rest of your payload
```

## How It Works

1. Taps CapsLock key
2. Reads `$_CAPSLOCK_ON` LED state
3. If `TRUE` — `STOP_PAYLOAD`
4. If `FALSE` — restores CapsLock state and continues

## Abort

Toggle **CapsLock ON** before plugging in the Ducky → payload will not run.
