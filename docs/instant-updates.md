# Instant Updates

Changes made in the CMS now reach most screens within a few seconds instead of within 30 seconds. *Rolling out gradually · No setup required.*

## Summary

Screens check in with the CMS every 30 seconds to look for changes. This poll interval has not changed and will not change. It is what keeps a screen working even on a bad connection.

The CMS can now also notify a screen directly when a change is made, instead of waiting for the screen's next check-in. When this notification arrives, the screen updates immediately rather than waiting up to 30 seconds.

| | Change saved → screen updates | How |
|---|---|---|
| **Before** | Up to 30 seconds | Screen updates on its next check-in |
| **Now** | A few seconds | Screen is notified directly |

## What triggers an instant update

- **Assigning a playlist** — from a screen's Manage tab, or the quick assign dropdown on the screens list.
- **Renaming or relocating a screen** — name, location, orientation, and timezone changes all reach the screen immediately.
- **Editing playlist contents** — reordering, adding, or removing media updates every screen that plays the playlist.
- **Changing a daypart schedule** — adding, editing, or removing a scheduled override for a screen.

## Rollout status

**Rolling out screen by screen.** This feature reaches each screen the next time it reconnects. No action is required to enable it. A screen that has not picked it up yet continues to work as before: changes reach it within its normal 30 second check-in.

This is a one-way improvement. No screen becomes slower during the rollout.

## FAQ

**Do I need to change any settings?**
No. This works automatically. There is nothing to enable or configure.

**What happens if a screen is offline when I make a change?**
The screen picks up the change as soon as it reconnects, the same as before. Offline screens do not lose changes. They apply them later, on reconnect.

**Will every screen get this at the same time?**
No. Screens adopt this individually as they reconnect. It is normal to see some screens update instantly while others still take up to 30 seconds for a period of time.

**Does this change how playlists or schedules work?**
No. This only changes how quickly a screen learns about a change that was already made. Playlist and schedule behavior is unchanged.
