# Building and Publishing the Player

Building a new release of the Android player, uploading it to R2, and rolling it out to screens from the CMS. Three separate steps, on purpose — uploading a build never installs it anywhere by itself.

## Prerequisites

- **The release signing key.** The build signs with the key in `~/.paskall/keystore.properties` (it names the keystore file, its password and the key alias). Only a computer that has this file can make a build that screens will accept as an update: Android refuses to update an app over one signed with a different key. Without the file the build still succeeds, but signed with a throwaway debug key, and Gradle prints a warning saying so. Never commit the key, and keep a backup of the `~/.paskall` folder somewhere safe. Losing it means reinstalling the app by hand on every screen.
- The Android SDK's **build-tools**, for `aapt2` — used to read the version straight out of the built APK so it can never disagree with what's uploaded. Not required: pass `--version` explicitly to the publish script instead if you don't have it on your `PATH`.
- The backend's Python virtualenv, with R2 credentials already in its `.env` — the publish script uploads through the same `app.infra.storage` module the CMS itself uses.

## Step 1: Bump the version

In `player/app/build.gradle.kts`, bump both fields in `defaultConfig`:

```kotlin
versionCode = 8        // integer, always higher than the last release
versionName = "1.0.7"  // what the CMS and the on-device debug overlay show
```

!!! warning "Emulator testing"
    If you've been testing on an emulator with a throwaway version like `1.0.999-emu-test` and a `-PapiBaseUrl=http://localhost:8001` build, put the real values back before doing a real release — including reverting any temporary `android:usesCleartextTraffic="true"` added to `AndroidManifest.xml` for local HTTP testing. A production build should never carry either.

## Step 2: Build the release APK

From `player/`:

```bash
./gradlew clean assembleRelease
```

No `-PapiBaseUrl` flag — that override exists only for pointing a local build at `localhost` during emulator testing. Left unset, the build bakes in the real production API URL. Check the build is signed with the real key before uploading it (the `DN` line must read `CN=Paskall Player, O=Fortu Digital, C=ID`, not `Android Debug`):

```bash
apksigner verify --print-certs app/build/outputs/apk/release/app-release.apk | grep DN
```

The signed APK lands at:

```
app/build/outputs/apk/release/app-release.apk
```

Worth running the unit tests in the same breath, since nothing else in this flow will catch a regression before it reaches a screen:

```bash
./gradlew testDebugUnitTest
```

## Step 3: Upload to R2

From `backend/`, with the virtualenv active:

```bash
.venv/bin/python -m scripts.publish_player_apk \
  ../player/app/build/outputs/apk/release/app-release.apk
```

The script reads `versionName` back out of the APK with `aapt2` and refuses to guess if it can't — pass it explicitly instead if needed:

```bash
.venv/bin/python -m scripts.publish_player_apk <apk> --version 1.0.7
```

!!! note "Nothing is live yet"
    This uploads the build to `s3://<bucket>/apks/fortu-player-<version>.apk` and verifies it landed intact. No screen installs anything as a result — uploading and publishing are deliberately separate acts, so a build can sit in R2 for review before anything runs it.

## Step 4: Roll it out

### Try it on one screen first

Open the screen's page in the CMS, **Manage** tab, **Software update**. Pick the version you just uploaded and choose **Update this screen**. That one screen installs it on its next check-in — within a few seconds — and nothing else changes. Once you're happy, roll it out to everyone.

*Only an owner can do this. The pending update clears itself once the screen reports it is running that version, or you can cancel it before then.*

### Roll it out to every screen

Open the CMS as an owner — **Settings → Software updates**. Pick the version and choose **Now** or a future date and time.

Every screen is told right away (or on its next check-in, if it can't be reached by push) and installs it silently if it's provisioned as Device Owner. A screen that isn't Device Owner can't self-install; see [Remote Device Owner Setup](remote-device-owner-setup.md).

!!! warning "This one is fleet-wide"
    Scheduling "now" here pushes to every live screen at once. For anything riskier than a small fix, verify on the emulator or on one real screen (the section above) before scheduling it for the fleet.

!!! note "Rolling back"
    Schedule an older version the same way. The most recently *scheduled* rollout whose time has already passed is always what's live, so an earlier build scheduled for now simply takes back over — there's no separate "undo."

## Quick reference

```bash
# from player/ — bump versionCode/versionName in build.gradle.kts first
./gradlew clean assembleRelease testDebugUnitTest

# from backend/
.venv/bin/python -m scripts.publish_player_apk \
  ../player/app/build/outputs/apk/release/app-release.apk

# then, in the CMS as owner:
#   one screen first → its page → Manage → Software update
#   everyone        → Settings → Software updates → pick the version → Now / schedule
```
