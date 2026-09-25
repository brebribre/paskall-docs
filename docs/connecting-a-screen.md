# Connecting a Screen

Connecting a screen takes about a minute: the screen shows a code, you type that code into Paskall, and the screen starts playing. This page walks through it with pictures.

## Before you start

**Set up the device.** The screen needs a player and must be showing a pairing code. That's covered on its own page: [Setting Up Your Device](setting-up-your-device.md), for smart TVs (web player) and Android devices (Android player).

**Set up your account.** *Coming soon.*

## Pairing a screen

**Step 1.** Open Paskall and go to **Screens** in the left menu. Click **Connect a screen**.

![The Screens page with the Connect a screen button at the top right](images/pairing/01-screens-page.png)

**Step 2.** A small window opens asking for the code.

![The Connect a screen window with fields for the code, a name and a location](images/pairing/02-connect-dialog.png)

**Step 3.** Type the code exactly as the screen shows it. Give the screen a name you'll recognise later (for example "Lobby screen") and, if you like, where it is.

![The window with the code, name and location filled in](images/pairing/03-code-typed.png)

!!! tip
    The code never contains the letters O, I or L, or the digits 0 and 1, so there is nothing to mix up. Codes stop working after 15 minutes; if yours has expired, the screen shows a new one by itself.

**Step 4.** Click **Connect**. Paskall waits for the screen to answer. This usually takes a few seconds.

![Waiting for the screen to connect](images/pairing/04-connecting.png)

**Step 5.** Once the screen has connected, Paskall asks how it is mounted. Pick the option where the top of the picture would be at the top of the panel, then click **Done**. You can change this later on the screen's page.

![The screen is connected; four buttons ask which way the panel is mounted](images/pairing/05-connected-orientation.png)

**Step 6.** You land on the screen's own page. The screen itself now shows a "waiting for content" card with its name.

![The new screen's page in Paskall](images/pairing/06-screen-page.png)

That's it. To make it play something, put a playlist on it from **Campaigns**. The screen starts playing within a few seconds.

!!! note "If a screen is removed from Paskall"
    It notices by itself and shows a new pairing code. Nothing needs doing on the hardware. Just connect it again with the new code.

## Orientation

Orientation is chosen when the screen connects, and can be changed any time on the screen's page under **Settings**. It is the rotation in degrees, clockwise from the panel's own landscape:

| Option | Meaning |
|---|---|
| 0° | Landscape |
| 90° | Portrait, top of the picture on the right |
| 180° | Landscape, upside down |
| 270° | Portrait, top of the picture on the left |

The two portrait options matter: a tall screen stood on the other side needs the other one, or everything shows upside down. A change takes effect within a few seconds. No reinstall is needed.

## Screen status

Paskall shows whether a screen is reachable from how long ago it last checked in.

| Status | Last checked in |
|---|---|
| Online | Under 2 minutes ago |
| Slow to respond | 2 to 15 minutes ago |
| Offline | Over 15 minutes ago, or never |

To check right now, open the screen's page and click **Probe** next to its status. It asks the screen to check in immediately and waits up to 40 seconds.

## Troubleshooting

**The screen is black, with no code and no content.**
This is almost always power or hardware, not pairing. A screen that has been removed or disconnected always shows a code. Check the power connection first.

**The code doesn't work.**
Open the screen's diagnostics view: hold the top-left corner of the screen for about a second, or tap that corner five times quickly. Compare the server address shown there with the Paskall you are using. If they don't match, the screen was set up for a different server.

**How do I see what a screen is doing?**
Hold the top-left corner of the screen for about a second, or tap that corner five times quickly. The five taps also work with a mouse over remote desktop, where holding often doesn't. The diagnostics view shows its name, the server it talks to, the content version, cached data, and the most recent error if there is one.

**How do I get out of the player to reach Android?**
Open the diagnostics view as above and choose **Leave player**. If the screen has an **App lock PIN** (the screen's page, under **Settings**), enter it on the number keypad that appears and choose **Leave**. The screen then shows the Android home screen. The lock comes back as soon as the player is opened again. The keypad works with a mouse over remote desktop too. The PIN is digits only.
