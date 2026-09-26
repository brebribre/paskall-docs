# Setting Up Your Device

A screen needs a player before it can be connected. There are two, and they behave the same once connected. Pick the one that fits your hardware:

| | Use it for | Needs |
|---|---|---|
| **[Web player](#option-1-web-player)** | Smart TVs (Samsung, LG), or any device with a modern browser | Nothing to install. Open a web page. |
| **[Android player](#option-2-android-player)** | Android TVs, TV sticks, tablets, signage boxes | An app to install. Stronger: silent updates, the panel can switch fully off on a schedule, and the device can be locked down. |

Whichever you choose, the device ends up showing a pairing code. Then continue with [Connecting a Screen](connecting-a-screen.md).

![The pairing screen: a six-letter code on a blue background](images/pairing/player-pairing-code.png)

## Option 1: Web player

1. On the TV or device, open the browser and go to:

    **https://player.paskall.co.id**

2. A pairing code appears. Connect the screen from Marien with that code. See [Connecting a Screen](connecting-a-screen.md).
3. Press **OK** on the remote once. That hides the browser bar; the player stays full screen after that, including through its own updates.
4. Make it stay there: set that address as the browser's home page, turn on the browser's kiosk or full-screen mode if it has one, and turn off the TV's own screensaver or eco sleep.

!!! tip "Sound"
    Browsers keep videos muted until someone presses a key. If a video should have sound, allow autoplay with sound in the TV browser's settings.

!!! note "What the web player can't do"
    It can't install updates (it reloads itself instead), can't switch the panel off on a schedule (it shows black instead), and can't show websites that refuse to be embedded. For a screen that must be fully managed, use the Android player.

## Option 2: Android player

*About 10 minutes, once per screen.*

### Step 1: Allow the app to install

Google Play Protect blocks apps that don't come from the Play Store. Turn that off once on the device.

!!! info "Skip this if"
    You are going straight to Device Owner setup (Step 4). That path installs over a cable and isn't blocked by Play Protect.

1. Open the **Google Play Store** app on the device.
2. Tap the profile icon, top right, then **Play Protect**.
3. Tap the gear icon, top right.
4. Turn off **Scan apps with Play Protect**.

### Step 2: Install the app

Get the app from the [Player Releases](player-releases.md) page. Open it on the device's own browser for Method B, or on your computer for Method A.

#### Method A: USB, from a computer

1. On the device, go to Settings → About phone and tap **Build number** 7 times.
2. Go to Settings → Developer options and turn on **USB debugging**.
3. Connect the device to your computer with a USB cable and accept the "Allow USB debugging?" prompt on the device.
4. On your computer, download the current build and install it over the cable. The first line saves the file as `marien-player.apk` in the folder you run it from; the second sends that file to the device:

    ```bash
    curl -L -o marien-player.apk https://api.paskall.co.id/player/download
    adb install -r marien-player.apk
    ```

    Already downloaded it from the [Player Releases](player-releases.md) page? Skip the first line and put your file's name in the second.

#### Method B: No cable

1. On the device, open the [Player Releases](player-releases.md) page in its browser and tap **Download** on the current version.
2. Open the downloaded file from the notification shade or file manager.
3. Allow "Install from unknown sources" when asked.

Open the app. It shows a pairing code. You can [connect the screen](connecting-a-screen.md) now, or lock the device down first (below).

### Step 3: Choose how locked down the screen should be

A plain install keeps the screen awake and hides the system bars, but the Home button still exits the app. Three levels, weakest first:

| Level | Setup | What it does |
|---|---|---|
| Screen pinning | None. Settings → Security → App pinning. | Stops accidental exits only. Anyone can unpin. |
| Launcher mode | Edit AndroidManifest.xml and rebuild. | The device boots straight into the player. Not for a personal device. |
| Device Owner | Step 4 below. | **Recommended.** Can't be exited. Needed for silent updates and for switching the display fully off on a schedule. |

### Step 4: Device Owner mode (recommended)

!!! warning "Before you begin"
    The device must be factory reset, with no Google account signed in. Android won't grant Device Owner otherwise, and there is no way around it.

1. Factory reset the device: Settings → System → Reset → Erase all data.
2. During setup, skip the Google account step. Skip Wi-Fi too if offered, and add it afterwards from Settings.
3. Turn on Developer options: Settings → About → tap Build number 7 times.
4. Turn on USB debugging: Settings → Developer options → USB debugging.
5. On your computer, download the current build, install it over the cable, and make it Device Owner. The first line saves the file as `marien-player.apk` in the folder you run it from (skip it if you already have the file, and use its name instead):

    ```bash
    curl -L -o marien-player.apk https://api.paskall.co.id/player/download
    adb install -r marien-player.apk
    adb shell dpm set-device-owner \
      com.fortu.player/com.fortu.player.kiosk.DeviceAdminReceiver
    ```

    You should see `Success: Device owner set to package com.fortu.player`. If it says there are already accounts on the device, an account was added during setup. Factory reset and start again from step 2.

6. Open the app. It locks itself to the foreground, disables the lock screen and keeps the screen on. Nothing else to configure.
7. Connect the screen with the code it shows. See [Connecting a Screen](connecting-a-screen.md).
8. Check it worked: hold the top-left corner of the screen for about a second (or tap it five times quickly) to open diagnostics, and confirm it says `device owner (full kiosk)`. In Marien, the screen's page shows **Setup: Managed** once it has checked in. If it says **Basic**, repeat step 5.
9. Reboot once. The device comes back to the Android home screen; open Marien Player from there and confirm it resumes.

!!! note
    Device Owner can't be removed with a command. To reuse the device for something else later, factory reset it.

Already running the app and want Device Owner without a cable or a reset? See [Remote Device Owner Setup](remote-device-owner-setup.md).
