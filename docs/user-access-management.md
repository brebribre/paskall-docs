# User Access Management

Who can do what in an account: the owner, the managers they create, which screens each manager reaches, what everyone shares, and the review step that stands between a manager's save and a screen. This page is the rulebook; the last section lists where each rule lives in the code so it can be tuned later.

## Two roles

An account has exactly one kind of full user, the **owner**, and any number of **managers** the owner creates under Settings → User management. A manager is a sub account: same content, a chosen set of screens, and no say over other users.

| | Owner | Manager |
|---|---|---|
| Screens | Every screen in the account | Only the screens the owner granted them |
| Pair a new screen | Yes | Yes; it is granted to them automatically |
| Media library | Sees and deletes everything | Sees everything, deletes only their own uploads |
| Playlists | Sees and edits everything | Sees and edits everything, deletes only their own |
| Campaigns and schedules | Any screen | Only for their granted screens; other screens are dropped from the campaign, not refused |
| Changing what a screen shows | Publishes at once | Sent to the owner for review first (see below) |
| Screen settings (volume, brightness, power, rotation) | Yes | Yes, on their screens, at once |
| Users, software updates, account settings | Yes | No; the Settings page is hidden |
| Storage quota and screen limit | One of each for the whole account, counting everyone's uploads and screens. Set per account by Paskall. | Same |

!!! note
    Content is deliberately shared. A manager's upload is visible to the owner and to every other manager, and lands in the same library and the same quota. What is scoped per manager is **screens**, never files or playlists. If a customer needs two branches that cannot see each other's content, that is two accounts.

## Screen grants

A manager reaches a screen only through a grant. Grants are set by the owner from the user's row in Settings → User management, and a screen a manager pairs themselves is granted to them on the spot. Everything else follows from the grant:

1. The Screens page, the Overview and the campaign screen picker show only granted screens.
2. A campaign a manager saves keeps only its granted screens; ungranted ids are dropped and reported back as skipped, never rejected outright.
3. A screen a manager was not granted answers `404` to every request, the same as a screen that does not exist. Which screens exist beyond a manager's reach is not observable.
4. Removing a grant takes effect on the manager's next request, not their next login.

## Reviews: a manager's changes to screens wait for the owner

A manager can build content freely. The moment a save would change what a screen is showing, it is not applied. It is parked as a **review**, the owner sees it under the Reviews tab with a badge in the sidebar, and approves or rejects it. Nothing reaches a screen until the owner says so.

### What waits for review, and what does not

| A manager… | Result |
|---|---|
| Uploads media, creates a playlist, edits a playlist that no screen is playing, renames anything | Applies at once |
| Saves a playlist that is on screens, directly or through a campaign or schedule | Review |
| Turns shuffle on or off for a playlist that is on screens | Review |
| Creates, edits or deletes a campaign | Review, always: a campaign exists only to put content on screens |
| Creates, edits or deletes a schedule | Review |
| Assigns a playlist to a screen directly, or clears it | Review; the screen's other settings in the same save apply at once |
| Changes a screen's volume, brightness, power or rotation | Applies at once (a knob, see below) |

### What the manager sees

1. Before saving, the page says the save will go to the owner rather than to the screens, and the confirmation asks "Send for review?" instead of "Publish?".
2. After saving, the page shows "Sent for review" with a link to the Reviews tab. The playlist editor keeps the draft on screen, but the saved playlist is unchanged until approval; reloading the page shows the live version.
3. On the Reviews tab a manager sees only their own requests, what is waiting, and what was decided, with the owner's note on a rejection. A waiting request can be withdrawn.

### What the owner sees

1. A count on the Reviews entry in the sidebar, refreshed when the app loads and whenever a review is decided.
2. Each waiting review names who sent it, what it is (the playlist or campaign, how many scenes or rules), and every screen it would reach, with one button: **See the changes**. There is deliberately no Approve on the list, so nothing is approved unseen.
3. The review's own page shows the change itself and carries Approve and Reject. A playlist change gets the same scene list and screen preview as the playlist page, read-only, with a Proposed / Current switch so the saved playlist can be compared; a campaign change lists its rules. Reject asks for an optional note the manager will read. The manager who sent it sees the same page, with Withdraw instead of the decision buttons.
4. Approving applies the change **as the manager who sent it**: the same validation, the same screen grants. It is exactly what their save would have done, and screens pick it up within seconds, as any publish does.
5. Decided reviews stay listed, newest first, up to the last hundred.

!!! warning "Stale reviews"
    A review holds the change exactly as it was sent. If the world moved meanwhile — the playlist was deleted, the manager was deactivated, a rule no longer validates — then approving fails with the reason shown, and the review stays waiting until the owner rejects it. Nothing is marked approved for a change that did not land. Two managers editing the same playlist are last-approval-wins: the review replaces the whole scene list, as a direct save does.

!!! note "Not yet"
    There is no email or push notification: the owner finds out from the badge when they open the CMS. There is no approval by anyone but the owner, and no "trusted manager" who skips review. Both are listed below as knobs.

## Where the rules live

Every rule on this page is one place in the code. To tweak a rule, change that place and this page together.

| Rule | Where | To change it |
|---|---|---|
| Who is reviewed | `backend/app/services/reviews.py`, `needs_review()` | Return false for a trusted manager (a flag on the user), or true for the owner too if a second approver is ever wanted. |
| Which saves are reviewed | The write routes in `backend/app/api/routes/` for playlists, campaigns, schedules and devices, each calling `park()` from `api/review_gate.py` | Add the same three lines to any other route that changes a screen, for example device settings. Remove them to let a change through. |
| "On screens" for a playlist | `services/playlists.py`, `screens_reached()` | Counts direct assignment, campaigns and schedules. Also what the publish confirmation names. |
| How an approval is applied | `backend/app/api/routes/reviews.py`, `_apply()` | One branch per review kind, replaying the stored body through the same service call as a direct save. A new kind needs a branch here and a value in `models/review.py`. |
| Who may approve | `routes/reviews.py`, the `RequireOwner` dependency on approve and reject | Swap for a narrower or wider dependency. |
| History shown | `services/reviews.py`, `HISTORY_LIMIT` | Decided reviews listed on the page; older ones stay in the table. |
| What a manager can delete | `services/media.py` and `services/playlists.py`, the `NotYours` checks | "Only their own" is a created-by comparison. |
| Screen scoping | `backend/app/api/deps.py`, `device_for_user()`, and `services/devices.py`, `list_devices()` | Grant rows in `device_access`. Owners bypass them entirely. |
| Storage quota | `models/account.py`, `storage_quota_bytes`, checked in `services/operations.py` | Per account, null for unlimited. Counts every user's uploads. Set from the monitoring app's Accounts page. |
| Screen limit | `models/account.py`, `max_screens`, checked in `services/devices.py`, `claim()` | Per account, null for unlimited. Only ever stops a *new* pairing — screens already running are never touched. Set from the monitoring app's Accounts page. |
| The badge | `frontend/src/hooks/useReviews.ts`, `useReviewBadge()` | Loaded once per page load and after each decision. Poll here if a live count is ever wanted. |
