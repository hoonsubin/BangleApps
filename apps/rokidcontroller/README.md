# Rokid Controller

Simulates the Rokid Glasses touchpad over BLE HID, so you can drive or test a Rokid
Glasses app directly from your Bangle.js 2.

## Gestures

| Bangle input | Sent as | Rokid touchpad equivalent |
|---|---|---|
| Tap (quick touch) | `KEYCODE_ENTER` | Tap (select/confirm) |
| Touch and hold | HID consumer `AC Home` → `KEYCODE_HOME` | Home |
| BTN1 click | HID consumer `AC Back` → `KEYCODE_BACK` | Back |
| Vertical drag | `KEYCODE_DPAD_DOWN` / `KEYCODE_DPAD_UP` | Swipe forward/backward |
| Horizontal drag | `KEYCODE_DPAD_RIGHT` / `KEYCODE_DPAD_LEFT` | Swipe forward/backward |

The swipe direction is decided by the dominant axis of the actual drag, so all four
directions work at all times (horizontal swipes send `Left`/`Right`, vertical swipes
send `Down`/`Up`). Tap vs hold uses the firmware's own touch classification
(`e.type`: 0 = quick, 2 = long press), and drags don't emit touch events, so the
gestures never collide.

## Pairing

1. Install this app via the Bangle.js App Loader.
2. Pair the Bangle from your phone/PC's Bluetooth settings as a keyboard.
3. Open your Rokid Glasses app (or its phone/emulator fallback) - gestures on the
   Bangle now arrive as real Android key events.

## Notes

- Runs safely in the Espruino browser emulator: HID calls are feature-detected and
  fall back to console-only simulation (a red "SIM MODE" banner appears on screen).
- HID failures are visible on the watch: the status line reads `HID OFF` when the HID
  service was never registered (Espruino cannot enable HID while a connection is
  active, so a connection at app load leaves it off), and a rejected report is shown
  in the footer instead of being swallowed.
