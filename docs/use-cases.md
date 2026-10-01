# Streakly — Use Cases

These MVP use cases cover recurring habits and one-time tasks. Completing an item is the primary action; short notes add context but are optional. Structured metrics such as minutes, pages, or kilometers are deferred.

## Core concepts

- **Recurring habit:** An activity with selected weekdays, such as “Read” every day or “Yoga” on Monday, Wednesday, and Friday. It has a per-habit streak.
- **One-time task:** A task planned for a particular date, with an optional time, such as “Clean the lawn tomorrow morning.” It does not have a streak in the MVP.
- **Completion note:** Optional free text attached to a completion. Examples: “Read The Hobbit, 20 pages, 30 minutes”; “Yoga — gentle flow”; “Cleared weeds along the fence.”
- **Completion time:** Automatically recorded when the user marks the item complete. Editing or backdating this timestamp is out of scope for the MVP.

## UC-01 — Create a recurring habit

**Primary actor:** User  
**Precondition:** The app is open.

**Main flow:**

1. The user chooses to add a habit.
2. The user enters a name, for example “Read” or “Yoga”.
3. The user selects one or more weekdays.
4. The user optionally adds a short general note.
5. The user saves the habit.
6. The app lists it on the selected days in the device's local calendar.

**Acceptance criteria:**

- A habit can be saved without a note.
- It is not due on unselected weekdays.
- The habit's streak counts completed scheduled occurrences only.

## UC-02 — Create a one-time dated task

**Primary actor:** User

**Main flow:**

1. The user chooses to add a one-time task, for example “Clean the lawn”.
2. The user selects a due date, such as tomorrow.
3. The user may select a due time, such as 09:00, and may add a short note.
4. The user saves the task.
5. The app shows it in the due/coming-up task list for that local date and time.

**Acceptance criteria:**

- Due date is required; due time and note are optional.
- “Tomorrow morning” is represented by choosing tomorrow's date and, if desired, a specific time; reminders are not part of the MVP.
- The task is one-off and is not automatically repeated.
- One-time tasks do not have streaks in the MVP.

## UC-03 — View today's items

**Primary actor:** User

**Main flow:**

1. The user opens or resumes the app.
2. The app determines today's date and time using the device's current local time zone.
3. The app shows recurring habits scheduled for today and one-time tasks due today.
4. Each item shows its title and completion status; any note is available as supporting context.
5. Future tasks may appear in a separate upcoming list.

**Acceptance criteria:**

- Unscheduled habits do not appear in today's due-habit list.
- An incomplete habit due today remains eligible until local midnight.
- Due and overdue one-time tasks are distinguishable.

## UC-04 — Complete an item and optionally add a note

**Primary actor:** User  
**Precondition:** The item is due or active.

**Main flow:**

1. The user taps the completion control.
2. The app marks the item complete for the local date and automatically records the completion timestamp.
3. If the item is a recurring habit, its streak is recalculated.
4. The user may add or edit a short completion note, immediately or later.

**Examples:**

- Habit “Read”: note “The Hobbit, 20 pages, about 30 minutes.”
- Habit “Yoga”: note “Gentle flow, 25 minutes.”
- One-time task “Clean the lawn”: note “Cleared grass and weeds near the garden beds.”

**Acceptance criteria:**

- A note is never required for completion.
- The user can write any useful context in the note; the MVP does not parse it into structured metrics.
- Editing or removing a note does not change completion state or a habit's streak.
- A one-time task completion never changes any habit streak.
- Repeated taps do not create duplicate completion records.

## UC-05 — Review a recurring habit's streak

**Primary actor:** User

**Main flow:**

1. The app evaluates the habit's scheduled occurrences and saved completion dates.
2. It shows the number of consecutive scheduled occurrences completed.
3. A missed scheduled occurrence breaks the streak after that local day ends.
4. Unscheduled days are skipped.

**Acceptance criteria:**

- A due habit left incomplete today does not break its streak before local midnight.
- A missed due habit breaks its streak starting at the next local calendar day.
- Completion notes and their contents do not affect streak calculation.
- Date-boundary and daylight-saving cases follow the rules in the [Product Requirements](product-requirements.md#4-local-date-time-and-streak-rules).

## Data captured in the MVP

| Record | Required fields | Optional fields |
|---|---|---|
| Recurring habit | Name; selected weekdays | General note |
| One-time task | Title; local due date | Local due time; note |
| Completion | Item ID/type; local completion date; automatically captured UTC timestamp | Free-text completion note |

Store completion dates as local date-only values so they remain stable if the device time zone changes. Task due times are interpreted in the current local time zone. Notes are plain text; the app does not infer duration, page count, distance, or completion from them.

## Out of scope for the MVP

- Streaks for one-time tasks
- Structured metrics and targets (duration, pages, distance, repetitions)
- Required notes or measurements
- Manual entry/backdating of completion timestamps
- Reminder notifications
- Arbitrary custom measurement types, health-platform integrations, or advanced analytics
