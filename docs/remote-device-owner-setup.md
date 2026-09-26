# Device Owner over Wi-Fi

Turn on Device Owner for a screen that already runs Marien Player, without a cable or a reset.

1. Install adb on your computer: `brew install --cask android-platform-tools`
2. Connect to the device (same Wi-Fi):
    ```bash
    adb connect <device-ip>:5555
    ```
    If that fails (Android 11+): on the device, **Developer options → Wireless debugging → Pair device with pairing code**, then run `adb pair <ip>:<port>` with that code, and `adb connect` to the address on the main Wireless debugging screen.
    Android 10 or older needs a USB cable once: `adb tcpip 5555`, then unplug.
3. Remove every account on the device (**Settings → Accounts**). Check with:
    ```bash
    adb shell dumpsys account | grep "Account {"
    ```
4. Grant Device Owner and restart the app:
    ```bash
    adb shell dpm set-device-owner \
      com.fortu.player/com.fortu.player.kiosk.DeviceAdminReceiver
    adb shell am force-stop com.fortu.player
    adb shell monkey -p com.fortu.player -c android.intent.category.LAUNCHER 1
    ```
5. Check: open diagnostics (hold the top-left corner) and look for `device owner (full kiosk)`.

!!! note
    Device Owner can only be removed with a factory reset.
