# Rbook — v2 deploy

## Files
| File | Where it goes |
|---|---|
| `index.html` | Replace `index.html` in the `fc-internal-tools/rebook` GitHub repo (GitHub Pages redeploys in ~1 min) |
| `bookmarklet.txt` | Chrome → right-click the Rbook bookmark → Edit → paste the whole line into **URL** |
| `bookmarklet.src.js` | Readable source of the bookmarklet (commit to repo for reference) |

The new page still works with the old bookmarklet, so the order doesn't matter.

## What changed
- **Invites emailed**: bookings send `sendUpdates=all`, so guests get the invite email. The success screen lists who got it.
- **Day picker**: you choose weeks ahead plus a weekday (Mon–Fri). The target is the original call date + N weeks, moved to that weekday in the same Mon–Fri week. It defaults to the original call's weekday. A blue preview line shows the exact date and time before you book.
- **Conflicts**: events marked Free, working-location events, declined events and cancelled events no longer count as clashes. All-day events marked Busy (e.g. leave) do count. Suggestions use the same rules.
- **Book anyway** (v2.1): when there's a clash, a red button under the suggestions books the time you asked for regardless, e.g. over lunch or a hold you'll skip. The button shows the date, time and what it overlaps. If you change any field after a clash, the conflict box clears, so the button can't book a stale time.
- **Suggestions**: weekdays only. Starts on the target day, then moves to the next weekday(s), searching up to 5 weekdays and showing 2 days. Shows the 6 slots closest to your requested time, within 9:00am–3:30pm.
- **Sign-in**: re-auth uses `prompt:''` plus your email as a hint, so after the hourly token expiry the Google popup should flash and close without the account picker.
- **Bookmarklet**: reads both "Thursday, 29 October" and "Thursday, October 29". It works out the year across Dec/Jan and handles "9 – 10am" and 24-hour times. Skip-words now match whole words only, so a title like "Noosa…" is no longer skipped. If the date can't be read, the popup shows a red warning instead of silently using today.
- Your own email is no longer added to the attendees twice. The January (month 0) bug is fixed.

## Smoke test after deploy
1. Open a call in Google Calendar → click the bookmark. Check that "Original call:" shows the right date.
2. Change Day → check that the preview date moves within the same week.
3. Book a test call with your own second address as a guest → check the invite email arrives.
4. Pick a time over an existing meeting → check the suggestions skip Sat/Sun.

## Config (top of the `<script>` block in index.html)
`BOOKABLE_START/END`, `SUGGEST_MAX_DAYS`, `SUGGEST_SEARCH_DAYS`, `SUGGEST_PER_DAY`, `COLOR_ID`.
