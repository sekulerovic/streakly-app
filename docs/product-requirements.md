# Streakly — Product Requirements (MVP)

**Platforms:** iOS and Android  
**Framework:** C# and .NET MAUI  
**Languages:** English, Turkish, German  
**Status:** Initial product scope

## 1. Product vision

Streakly is a simple, local-first activity tracker. Users can manage recurring habits and one-time dated tasks, check off completions, and add a short note with context about what they did.

The first release prioritizes a useful, reliable habit-tracking loop over monetization, accounts, or social features. The app must work offline.

## 2. Priorities

- **P0 — MVP:** Required for the first usable release.
- **P1 — Next:** Valuable improvements after the core MVP works.
- **P2 — Later:** Defer until user feedback or product needs justify them.

| Priority | Scope |
|---|---|
| P0 | Android and iOS MAUI app; English, Turkish, and German; dark orange-and-black UI; recurring habits with selected weekdays and per-habit streaks; one-time tasks with due date and optional due time; one-tap completion and optional short notes; local SQLite; test date and streak rules. |
| P1 | Reminders; completion history and calendar; summaries and progress views; accessibility and usability refinements from testing. |
| P2 | Accounts, cloud backup/sync, social features, subscriptions, paywall, and other monetization. Consider only after validating the core app. |

**Explicitly out of P0:** daily focus timer, "1% better" progress framing, mandatory subscription, free trial/paywall, RevenueCat, Firebase, and server-side user profiles.

## 3. P0 product requirements

### PR-01: Supported platforms

The app must be built with .NET MAUI and C# and support Android and iOS from a shared codebase. iOS builds and signing require macOS and Xcode.

### PR-02: Localization

The app must provide English, Turkish, and German UI text. It should use the device language when supported and fall back to English when it is not. All user-facing strings must use the app's localization resources rather than being hard-coded in views.

### PR-03: Visual design and accessibility

Use a simple dark interface with orange accents:

- Background: `#121212`
- Surface/card: `#1E1E1E`
- Accent: `#FF8A00`
- Primary text: `#FFFFFF`
- Secondary text: `#B3B3B3`

Use orange for emphasis and primary actions, not as the background for large text areas. Maintain readable contrast, scalable text, semantic labels, and keyboard/screen-reader accessibility where supported.

### PR-04: Recurring habit management

Users must be able to create, edit, and archive a recurring habit. A habit has a name and one or more selected weekdays. It may also have an optional short note. Archived habits must no longer appear as due today, while their previous completion records remain stored.

### PR-05: Daily completion

The home screen must show active habits scheduled for the current local date and whether each is complete. A user can mark a scheduled habit complete for today with one simple action. A user may attach an optional short note to the completion, such as the book title and pages read, or what yoga they practiced. Notes are not required and do not determine completion state or streak. Repeated taps must not create duplicate completion records for that habit and date.

### PR-06: One-time dated task

Users must also be able to create a one-time task with a title, a required local due date, and an optional local due time and short note. Example: “Clean the garden lawn”, due tomorrow at a chosen morning time. The app shows due and overdue tasks separately from recurring habits. Completing a one-time task closes it; one-time tasks do not have streaks in the MVP.

### PR-07: Per-habit streak

Show a separate current streak for each habit. A streak counts consecutive *scheduled occurrences* completed, not consecutive calendar days:

- A weekday on which the habit is not scheduled does not interrupt its streak.
- A missed scheduled occurrence breaks the streak after that local calendar day has ended.
- An incomplete habit scheduled for today does not break the streak before today ends.
- Completing today's occurrence extends that habit's streak by one scheduled occurrence.
- A habit with no completed scheduled occurrences has a streak of zero.

Only recurring habits have streaks in the MVP. Completing, missing, or editing a one-time task never changes any habit streak.

## 4. Local date, time, and streak rules

These rules are normative and must be covered by automated tests.

1. **Source of local date:** Use the device's current local calendar date and time zone. Determine the weekday and date from that local calendar, not by comparing elapsed 24-hour intervals.
2. **Completion date:** When a user completes a habit, persist the associated local date as a date-only value (`yyyy-MM-dd`) and also persist the event timestamp in UTC for audit/debugging. The saved completion date must not change when the device time zone changes later.
3. **Scheduled days:** Interpret selected weekdays in the device's current local calendar. An unselected weekday is not a due occurrence and neither adds to nor breaks a streak.
4. **End-of-day boundary:** A scheduled occurrence remains eligible to be completed until the next local midnight. A missed occurrence becomes a streak break at that midnight. Do not use a fixed 24-hour duration; local days can be shorter or longer during daylight-saving transitions.
5. **Task due time:** Store a task's due date as a local date-only value. If a due time is provided, interpret it as local wall-clock time in the device's current time zone; the task becomes overdue after that local time. Without a due time, the task is due on that local date and becomes overdue after that date ends. Due times organize tasks in the MVP; reminder notifications are deferred.
6. **Streak evaluation:** Recalculate from the saved completion dates and the habit's scheduled weekdays whenever the app opens or resumes and after a completion or schedule edit. Do not rely on a timer that must run continuously in the background.
7. **Time-zone changes:** Keep previously stored completion dates and task due dates unchanged. Apply the device's current local calendar and time zone to the current date and future due-day evaluation. A time-zone change alone must not delete or rewrite stored dates.
8. **Clock changes:** Use an injectable clock/date provider in date-sensitive logic so tests can set local dates, times, and time zones deterministically. Do not build core streak behavior around sleeping background tasks.

## 5. Data and architecture

### P0 architecture

Start with one .NET MAUI application and a simple MVVM structure:

```text
Views (MAUI / XAML)
    -> ViewModels
        -> Habit and streak services
            -> SQLite repositories
```

- **Views:** Render screens and bind to view-model state.
- **ViewModels:** Coordinate user actions and screen state; avoid embedding persistence or streak rules in UI code.
- **Models:** Represent recurring habits, one-time tasks, and completion events with optional notes.
- **Services:** Apply habit rules and calculate streaks using an injectable local-date/clock provider.
- **SQLite repositories:** Load and save habits and completion records on-device.

Keep these pieces in the MAUI project initially. Add a separate backend, Clean Architecture projects, or extra abstraction layers only when concrete requirements justify the added complexity.

### P0 persistence and privacy

- The app must store core habit data locally in SQLite and work without a network connection.
- Store the database in the platform's app-private storage.
- Store recurring-habit completion history by habit ID and local completion date; enforce at most one completion per habit per date. Store one-time tasks with their local due date and optional due time. Completion events may include an optional note and automatically captured UTC timestamp.
- Do not collect or transmit habit data in the MVP.
- SQLite is not encrypted by default. Do not store credentials or secrets in it. Revisit database encryption if the product later handles data requiring protection beyond the operating system's app sandbox.

### P2 backend and monetization

Accounts, cross-device sync/backup, Firebase or another backend, in-app subscriptions, paywalls, and RevenueCat are out of scope for P0 and P1. Reassess based on user feedback, privacy requirements, store policies, and operating costs before selecting services.

## 6. Verification and acceptance tests

### P0 UI test environments

- **Primary development environment:** Visual Studio on Windows with the .NET MAUI workload and an Android emulator created through Android Device Manager. Run the app on the emulator during UI development; use XAML Hot Reload where supported to inspect visual changes without restarting for every edit.
- **Device sanity check:** Before the MVP release, install and exercise the main flows on at least one physical Android phone. Emulator testing does not replace checks for touch behavior, text scaling, and real-device layout.
- **iOS validation:** Before the MVP release, build and test on an iOS Simulator using a Mac with Xcode, or on a physical iPhone through the supported Mac-paired workflow. Windows alone cannot run the iOS Simulator or sign iOS builds.
- Use the same core acceptance flows on both platforms: create/edit/archive a habit, select weekdays, complete today's habit, restart the app, and verify persisted state and streak.

### P0 functional tests

- Create a habit for selected weekdays; verify it appears only on those due days.
- Complete a habit; verify its status and per-habit streak update and persist after app restart.
- Add or edit an optional note on a completion; verify it is saved and does not alter completion state or streak.
- Create a one-time task with a due date and optional due time; verify it appears in the task list and can be completed.
- Verify a one-time task becomes overdue after its selected local due time, or after local midnight when no time was selected.
- Verify free-text completion notes persist across app restart and do not affect streak calculations.
- Verify completing, missing, or editing a one-time task never affects a recurring habit's streak.
- Verify that changing the device time zone preserves the saved task due date and that the due time is interpreted as local wall-clock time.
- Tap complete repeatedly; verify only one completion exists for that habit/date.
- Verify one habit's completion or missed day does not alter another habit's streak.
- Verify archived habits disappear from the active list without deleting their history.

### P0 local-time and streak tests

Use a controllable clock/date provider; cover at minimum:

- Complete just before local midnight: completion belongs to that local date.
- At local midnight: the current date changes and a missed previous scheduled occurrence breaks the streak.
- A scheduled occurrence left incomplete during its day does not break the streak before midnight.
- An unselected weekday between two completed scheduled days does not break the streak.
- A missed selected weekday breaks the streak once its local day ends.
- Daylight-saving spring-forward and fall-back: evaluation follows calendar dates, not 24-hour durations.
- Change the device time zone: stored completion dates and history remain unchanged; current/future schedule evaluation follows the new local calendar.
- App closed over midnight and reopened: streak is recalculated correctly without background execution.
- Leap day and month/year boundaries: date comparisons remain correct.

## 7. Deferred roadmap

- **P1:** Reminders and notifications; history/calendar; richer progress summaries.
- **P2:** Backup and multi-device synchronization; accounts; subscriptions and paywall; social or coaching features.
- **P2:** Reassess architecture and data protection before adding any remote service.
