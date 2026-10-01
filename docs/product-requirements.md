# Streakly — Product Requirements (MVP)

**Platforms:** iOS and Android  
**Framework:** C# and .NET MAUI  
**Languages:** English, Turkish, German  
**Status:** Initial product scope

## 1. Product vision

Streakly is a simple, local-first habit tracker. It helps users create habits, choose the days they plan to do them, check off completions, and understand their consistency through a separate streak for each habit.

The first release prioritizes a useful, reliable habit-tracking loop over monetization, accounts, or social features. The app must work offline.

## 2. Priorities

- **P0 — MVP:** Required for the first usable release.
- **P1 — Next:** Valuable improvements after the core MVP works.
- **P2 — Later:** Defer until user feedback or product needs justify them.

| Priority | Scope |
|---|---|
| P0 | Android and iOS MAUI app; English, Turkish, and German; dark orange-and-black UI; create/edit/archive habits; choose weekdays; complete habits for the current local date; show each habit's current streak; persist locally in SQLite; test date and streak rules. |
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

### PR-04: Habit management

Users must be able to create, edit, and archive a habit. A habit has at least a name and one or more selected weekdays. Archived habits must no longer appear as due today, while their previous completion records remain stored.

### PR-05: Daily completion

The home screen must show active habits scheduled for the current local date and whether each is complete. A user can mark a scheduled habit complete for today. Repeated taps must not create duplicate completion records for that habit and date.

### PR-06: Per-habit streak

Show a separate current streak for each habit. A streak counts consecutive *scheduled occurrences* completed, not consecutive calendar days:

- A weekday on which the habit is not scheduled does not interrupt its streak.
- A missed scheduled occurrence breaks the streak after that local calendar day has ended.
- An incomplete habit scheduled for today does not break the streak before today ends.
- Completing today's occurrence extends that habit's streak by one scheduled occurrence.
- A habit with no completed scheduled occurrences has a streak of zero.

## 4. Local date, time, and streak rules

These rules are normative and must be covered by automated tests.

1. **Source of local date:** Use the device's current local calendar date and time zone. Determine the weekday and date from that local calendar, not by comparing elapsed 24-hour intervals.
2. **Completion date:** When a user completes a habit, persist the associated local date as a date-only value (`yyyy-MM-dd`) and also persist the event timestamp in UTC for audit/debugging. The saved completion date must not change when the device time zone changes later.
3. **Scheduled days:** Interpret selected weekdays in the device's current local calendar. An unselected weekday is not a due occurrence and neither adds to nor breaks a streak.
4. **End-of-day boundary:** A scheduled occurrence remains eligible to be completed until the next local midnight. A missed occurrence becomes a streak break at that midnight. Do not use a fixed 24-hour duration; local days can be shorter or longer during daylight-saving transitions.
5. **Streak evaluation:** Recalculate from the saved completion dates and the habit's scheduled weekdays whenever the app opens or resumes and after a completion or schedule edit. Do not rely on a timer that must run continuously in the background.
6. **Time-zone changes:** Keep previously stored completion dates unchanged. Apply the device's current local calendar and time zone to the current date and future due-day evaluation. A time-zone change alone must not delete or rewrite completion history.
7. **Clock changes:** Use an injectable clock/date provider in streak logic so tests can set local dates, times, and time zones deterministically. Do not build core streak behavior around sleeping background tasks.

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
- **Models:** Represent habits and completion events.
- **Services:** Apply habit rules and calculate streaks using an injectable local-date/clock provider.
- **SQLite repositories:** Load and save habits and completion records on-device.

Keep these pieces in the MAUI project initially. Add a separate backend, Clean Architecture projects, or extra abstraction layers only when concrete requirements justify the added complexity.

### P0 persistence and privacy

- The app must store core habit data locally in SQLite and work without a network connection.
- Store the database in the platform's app-private storage.
- Store completion history by habit ID and local completion date; enforce at most one completion per habit per date.
- Do not collect or transmit habit data in the MVP.
- SQLite is not encrypted by default. Do not store credentials or secrets in it. Revisit database encryption if the product later handles data requiring protection beyond the operating system's app sandbox.

### P2 backend and monetization

Accounts, cross-device sync/backup, Firebase or another backend, in-app subscriptions, paywalls, and RevenueCat are out of scope for P0 and P1. Reassess based on user feedback, privacy requirements, store policies, and operating costs before selecting services.

## 6. Verification and acceptance tests

### P0 functional tests

- Create a habit for selected weekdays; verify it appears only on those due days.
- Complete a habit; verify its status and per-habit streak update and persist after app restart.
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
