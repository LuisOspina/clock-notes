# Functional requirements

1. The Alarms module supports up to 10 shared alarms. Each alarm receives a monotonically increasing numeric identifier.
2. A primary **Add alarm** button is always placed at the bottom of the Alarms view and is disabled when 10 alarms exist.
3. Opening Add alarm shows a simple dialog. Its time defaults to the next whole local hour; for example, at 19:10 it defaults to `20:00`, and at 21:00 it defaults to `22:00`.
4. The dialog has separate `hh` and `mm` integer fields. Hours must be 0–23 and minutes must be 0–59. Invalid values prevent saving and show a clear inline message.
5. A new alarm defaults to every day of the week. The dialog presents day controls in the order `S M T W T F S`.
6. An alarm with one or more selected days is enabled. Clearing every day disables it and displays a non-blocking warning. The enabled state is not editable in the dialog.
7. The dialog may optionally collect an alarm name of up to 20 characters. When present, the name is shown in lighter text in parentheses after the day summary.
8. The dialog shows the saved time zone and provides Cancel and Add or Save actions. Editing an existing alarm also provides Delete; Delete takes effect immediately without a confirmation dialog.
9. Every alarm card displays its numeric identifier, its schedule summary, its time in `HH:MM` format, an optional name, the saved time zone, and an enabled/disabled checkbox. The schedule summary is `Every day`, `Weekdays`, `Weekends`, or a comma-separated set such as `Mon, Tue, Fri`.
10. Clicking an alarm card opens its edit dialog. Clicking the enabled checkbox must not open the dialog.
11. The enabled checkbox on the card is the manual way to enable or disable an alarm without editing its time or schedule.
12. A Dismiss control is shown only in the active alarm popup. It is not available in advance on the alarm card.
13. Alarms use the time zone in effect when they are saved. A viewer in another time zone sees the corresponding local time, including daylight-saving adjustments. The saved hour, minute, repeat days, and IANA time-zone identifier are retained so this conversion remains possible.
14. The app must not ring an alarm merely because a user enables it or saves it during the alarm's current matching minute. That already-current occurrence is treated as dismissed; the next valid occurrence may ring normally.
15. When an alarm is due, the app shows a non-browser-notification popup with the alarm time and a Dismiss button. It plays the bundled default alarm audio on a loop until dismissed. The browser must remain open for the popup and audio to occur.
16. Dismissing an alarm stops the current sound and popup only. It does not disable or delete future occurrences of the alarm.
17. Custom sound uploads are not supported. All alarms use the same bundled default sound; no audio files are stored in or served from AWS.
18. Alarm data is shared. The API supports listing, creating, updating, and deleting alarms, returns clear validation and not-found errors, and applies gateway rate limits. The public API documentation exposes only the available alarm and OpenAPI routes.
19. The Alarms module uses the application's high-contrast light theme, with readable text, visible keyboard focus indicators, and accessible labels for all controls.

## Testing approach

1. Verify a new alarm defaults to the next whole local hour and all seven days are selected.
2. Verify invalid hour and minute values cannot be saved.
3. Verify day-summary text for every day, weekdays, weekends, and an arbitrary selection.
4. Verify clearing every day disables the alarm and shows the inline warning; verify the card checkbox can re-enable it.
5. Verify an alarm saved or enabled during its matching minute does not ring until a future occurrence.
6. Verify Dismiss appears only when an alarm is actively ringing, stops the active audio and popup, and leaves later occurrences enabled.
7. Verify create, update, delete, the 10-alarm limit, and API validation/not-found responses.
8. Verify the dialog, alarm cards, and API documentation can be operated with a keyboard and remain readable in the light theme.
