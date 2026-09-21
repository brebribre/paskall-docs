# Connecting a Screen

This guide covers two separate tasks: pairing a screen that is already running Fortu Player, and installing Fortu Player on a device for the first time. Most of the time you only need the pairing steps.

| | |
|---|---|
| **[Pair a screen](#pair-a-screen)** — most common | The device is already running Fortu Player and is showing a pairing code. Takes under a minute. |
| **[Set up new hardware](#set-up-new-hardware)** — first time only | A fresh Android device with nothing installed. About 10 minutes. Done once per screen. |

## Pair a screen

Use this procedure when the screen is already running Fortu Player and is showing a pairing code.

1. Power on the screen. It displays a 6 character pairing code.

    *The code uses only unambiguous characters. It never contains O, 0, I, 1, or L, because it is read from across a room.*

2. Note the server address printed below the code.

    *If pairing does not work, check this address first. It must match the CMS you are pairing from. This is the most common cause of a pairing code that will not work.*

3. In the CMS, go to **Screens → Connect a screen**.
4. Enter the pairing code.
5. Enter a name and location for the screen, then submit the form.
6. The screen receives its credentials within about 5 seconds and shows an idle card.

    *Assign a playlist to start playback. Playback starts within 30 seconds, usually sooner.*

!!! note
    If a screen is deleted from the CMS, it detects this automatically and shows a new pairing code on its own. No action is required on the hardware. Repeat the steps above using the new code.

## Set up new hardware

*About 10 minutes.* Use this procedure the first time Fortu Player is installed on a device. Once complete, the screen behaves exactly like the pairing flow above. This is done once per screen.

### Step 1: Allow the APK to install

Google Play Protect blocks installing an APK that did not come from the Play Store by default. Turn this off once per device before attempting either install method below — otherwise the install is silently blocked or flagged as harmful.

!!! info "Skip if"
    You're going straight to Device Owner setup (Step 4). That flow factory resets the device and installs over `adb install`, which isn't gated by Play Protect's unknown-sources restriction the way a browser or file-manager install is — and a freshly reset device has no signed-in Google account to reach this setting with anyway.

1. Open the **Google Play Store** app on the device.
2. Tap the profile icon, top right.
3. Tap **Play Protect**.
4. Tap the gear/settings icon, top right of the Play Protect screen.
5. Turn off **Scan apps with Play Protect**.

    *Wording and navigation vary slightly by Play Store version — look for anything mentioning scanning or blocking apps installed from outside the Play Store.*

### Step 2: Install the app

[Download latest APK](https://signage-cms-production.up.railway.app/player/download){ .md-button .md-button--primary }

Always the current build. Open this link on the device's own browser for Method B below, or on your computer for Method A. [Looking for an older build?](https://signage-cms-production.up.railway.app/player/versions)

Choose one of the following two methods.

#### Method A: USB, from a computer

1. On the device, go to Settings → About phone.
2. Tap Build number 7 times to enable Developer options.
3. Go to Settings → Developer options and enable **USB debugging**.
4. Connect the device to your computer with a USB cable.
5. Accept the "Allow USB debugging?" prompt on the device.
6. Run:

    ```bash
    adb install -r fortu-player.apk
    ```

    *The file downloaded above, wherever you saved it.*

#### Method B: No cable

1. On the device itself, open this page in its browser and tap **Download latest APK** above.

    *No browser, or already locked into another app? Copy the APK to a USB drive or cloud storage from another computer instead, then open it from this device's file manager.*

2. Open the downloaded file from the device's notification shade or file manager.
3. Allow "Install from unknown sources" when prompted.

### Step 3: Choose a kiosk level

A plain install keeps the screen awake and hides the system bars, but the Home button still exits the app. Three levels are available, in order of strength.

| Level | Setup required | Behavior |
|---|---|---|
| Screen pinning | None | Settings → Security → App pinning. Prevents accidental exits only. Any user can unpin the app. |
| Launcher mode | Edit AndroidManifest.xml, rebuild | The screen boots directly into Fortu Player. Not recommended on a personal device, since it will also request to become that device's launcher. |
| Device Owner mode | See Step 4 below | **Recommended.** Cannot be exited. Required for silent updates and for switching the display fully off on a power schedule (other installs show a black screen and let the panel sleep). After a reboot the device shows its home screen; open Fortu Player from there. |

### Step 4: Set up Device Owner mode (recommended)

!!! warning "Prerequisite"
    The device must be factory reset, and no Google account can be signed in during setup. Android will not grant Device Owner status otherwise, and there is no workaround. This step must happen before anything else in this section.

1. Factory reset the device: Settings → System → Reset → Erase all data.
2. During setup, skip the Google account step.

    *Skip Wi-Fi during setup too, if the option is offered. Add Wi-Fi afterward from Settings. Some setup wizards add a Google account automatically as soon as the device is online.*

3. Enable Developer options: Settings → About → tap Build number 7 times.
4. Enable USB debugging: Settings → Developer options → USB debugging.
5. Install the app and grant Device Owner status:

    ```bash
    adb install -r fortu-player.apk
    adb shell dpm set-device-owner \
      com.fortu.player/com.fortu.player.kiosk.DeviceAdminReceiver
    ```

    *Expect the output `Success: Device owner set to package com.fortu.player`. If the output instead says "there are already some accounts on the device," an account was added during setup. Factory reset the device and repeat from step 2.*

6. Launch the app. On first launch it automatically locks itself to the foreground, disables the lock screen, and keeps the screen on. No further configuration is needed.
7. Pair the screen using the code it displays. See [Pair a screen](#pair-a-screen) above.
8. Verify the setup. Long press anywhere on the screen to open the diagnostics overlay. Confirm it reads `device owner (full kiosk)`. In the CMS, the screen's page shows **Setup: Managed** once it has checked in.

    *If it reads `not owner (screen pinning only)`, or the CMS shows **Setup: Basic**, step 5 did not complete successfully. Repeat step 5.*

9. Reboot the device once. It returns to the Android home screen. Open Fortu Player from there and confirm the content loop resumes.

!!! note
    Device Owner mode cannot be removed with an adb command. To reuse a device for another purpose later, factory reset it. Confirm you will not need the device back before completing this setup.

## Orientation

Orientation is set per screen in the CMS, not during installation. The same installed app supports every way a panel can be mounted.

It is a rotation in degrees, clockwise from the panel's own landscape: 0° is landscape, 90° portrait with the top on the right, 180° landscape upside down, 270° portrait with the top on the left. The two portrait values matter: a totem stood on the other side needs the other one, or everything shows upside down.

The CMS asks for it the moment a screen connects. To change it later, use the screen's Settings tab. The change takes effect on the screen's next check-in. No reinstall is required.

## Screen status

Based on how long ago a screen last checked in — not a live connection, since a screen only ever polls the CMS, it isn't polled by it.

| Status | Last checked in |
|---|---|
| Online | Under 2 minutes ago |
| Slow to respond | 2–15 minutes ago |
| Offline | Over 15 minutes ago, or never |

To check right now instead of waiting: open the screen's page and click **Probe**, next to its status. It asks the screen to check in immediately and waits up to 40 seconds for it to do so.

## Troubleshooting

**The screen is black. It does not show a code or content.**
This should not happen. A screen that is deleted, revoked, or disconnected is designed to always show a visible pairing code, specifically so it can be diagnosed from across a room. A fully black screen usually indicates a power or hardware problem, not a pairing problem. Check the power connection first.

**Pairing does not work.**
Compare the server address shown below the code on the screen with the CMS you are pairing from. If they do not match, the code is valid but cannot be claimed from that CMS.

**How do I check a screen's current status?**
Long press anywhere on the screen to open the diagnostics overlay. It shows the screen's name, the server it is connected to, the current content version, the amount of cached data, and the most recent error, if any.
