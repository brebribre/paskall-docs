# Remote Device Owner Setup

Granting Device Owner status to a screen over Wi-Fi, with no USB cable and no factory reset. Use this for a screen that's already running Fortu Player and is reachable on the network, but was never provisioned as Device Owner.

!!! warning "When to use this"
    Setting up a brand new device? Use [Set up new hardware](connecting-a-screen.md#set-up-new-hardware) instead — a factory reset plus USB is simpler and more reliable when you're starting from nothing. This guide is for a screen you already have running, in place, that you now want to upgrade to full kiosk mode remotely.

## Step 1: Install adb

On your computer, not the device. One-time setup.

```bash
brew install --cask android-platform-tools
```

Confirm it installed:

```bash
adb version
```

## Step 2: Connect to the device over Wi-Fi

The device must already be network-debuggable. If it was ever connected over USB before with `adb tcpip 5555` run against it, it may already be listening — try connecting directly first:

```bash
adb connect <device-ip>:5555
```

!!! note
    Get the device's IP from Settings → Wi-Fi → tap the connected network → Details. If the connect above succeeds immediately, skip to Step 3.

If that fails, the device needs Wireless debugging turned on first — **Android 11 or newer only**.

1. On the device: Settings → About → tap Build number 7 times to enable Developer options.
2. Settings → Developer options → **Wireless debugging** — turn it on.

    *Tap the words "Wireless debugging" itself, not just its toggle, to open the detail screen — the pairing option below only appears there.*

3. Tap **Pair device with pairing code**. It shows a 6-digit code and an IP:port.
4. From your computer, on the same Wi-Fi network:

    ```bash
    adb pair <ip>:<pairing-port>
    ```

    Enter the 6-digit code when prompted.

    *Using zsh? Quote any glob-like argument (`"FortuPlayer:*"` later in this guide, for example) — zsh expands an unquoted `*` itself and fails with "no matches found" before adb ever sees it.*

5. Go back to the main Wireless debugging screen — it now shows a second, different IP:port. Use that one to actually connect:

    ```bash
    adb connect <ip>:<connect-port>
    ```

!!! warning "Android 10 or older"
    Wireless debugging doesn't exist on these versions — there's no way to enable network adb without touching a cable at least once. Connect the device over USB and run `adb tcpip 5555` once, then unplug. It listens over the network on port 5555 until the next reboot. From there, connect with `adb connect <device-ip>:5555` as in the first box above.

## Step 3: Remove any accounts on the device

Device Owner can only be granted to a device with zero accounts — Google, Samsung, work, anything. This is the single most common failure at the next step.

Check what's there:

```bash
adb shell dumpsys account | grep "Account {"
```

If anything is listed, remove it on the device: Settings → Accounts → tap each account → Remove account.

## Step 4: Grant Device Owner

```bash
adb shell dpm set-device-owner \
  com.fortu.player/com.fortu.player.kiosk.DeviceAdminReceiver
```

!!! warning "If this fails"
    `IllegalStateException: Not allowed to set the device owner because there are already some accounts on the device` means Step 3 wasn't complete. Remove the remaining account and retry — no reboot needed.

## Step 5: Restart the app

Kiosk features check Device Owner status at startup, so an already-running instance won't pick up the change on its own.

```bash
adb shell am force-stop com.fortu.player
adb shell monkey -p com.fortu.player -c android.intent.category.LAUNCHER 1
```

A reboot works just as well, if that's easier.

## Step 6: Verify

Long press anywhere on the screen to open the diagnostics overlay. Confirm it reads `device owner (full kiosk)`, not `not owner (screen pinning only)`.

!!! note
    From here on, silent self-updates and automatic recovery after a power loss both work the same as a device provisioned via factory reset + USB. Device Owner status cannot be removed with an adb command — to reuse this device for something else later, factory reset it.
