# Set Up Your Device

Pick a player. Both end on a pairing code; then [connect the screen](connecting-a-screen.md).

| | For | Install |
|---|---|---|
| [Web player](#web-player) | Smart TVs (Samsung, LG), any browser | Nothing |
| [Android player](#android-player) | Android TV boxes, sticks, tablets | The app. Adds silent updates, screen off on schedule, lock-down. |

![The pairing code on a Marien screen](images/pairing/player-pairing-code.png){ width="300" }

## Web player

1. Open **https://player.marien.co.id** in the TV's browser.
2. Press **OK** on the remote once to hide the browser bar.
3. Set that address as the browser's home page, and turn off the TV's screensaver.

!!! note
    Videos stay muted until you allow autoplay with sound in the browser's settings.

## Android player

1. On the device: open the **Play Store** → profile icon → **Play Protect** → gear → turn off **Scan apps with Play Protect**.
2. Install the app. Either:
    - **On the device:** open [Player Releases](player-releases.md) in its browser, tap **Download**, open the file, allow unknown sources.
    - **From a computer (USB debugging on):**
      ```bash
      curl -L -o marien-player.apk https://api.marien.co.id/player/download
      adb install -r marien-player.apk
      ```
3. Open **Marien Player**. It shows a pairing code.

To stop anyone leaving the app, see [Device Owner over Wi-Fi](remote-device-owner-setup.md).
