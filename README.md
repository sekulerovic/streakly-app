# Streakly App

**Languages:** English | [Türkçe](README.tr.md) | [Deutsch](README.de.md)

[Project checklist (Turkish)](TODO.md)

[Use cases (Turkish)](docs/use-cases.md)

Streakly App is a productivity and habit-tracking application designed to help people build consistency by tracking daily actions, streaks, and momentum over time.

## Project goal

The app should help users:

- Create and manage recurring habits
- Log daily check-ins and completions
- View streaks, progress, and history
- Receive lightweight motivation and accountability cues
- Stay focused on sustainable routines rather than perfection

## Technology

Streakly is a cross-platform mobile app for iOS and Android, built with:

- C# and .NET
- .NET MAUI for the shared mobile UI and native platform integrations
- SQLite for on-device persistence where local storage is needed

An ASP.NET Core service may be added later if features require a shared backend, accounts, or synchronization. The mobile app should not depend on a backend for core habit tracking unless the product requirements call for it.

## Repository status

The repository contains the .NET MAUI Android/iOS starter app and initial product documentation. The habit and task features are not implemented yet.

## Planned structure

- `src/StreaklyApp/` — .NET MAUI application, platform entry points, and shared UI
- `tests/` — automated tests
- `docs/` — design and product documentation
- `.github/` — repository automation and Copilot instructions

## Local setup

Install the supported .NET SDK, the .NET MAUI workload, and Git. Use Visual Studio with the .NET MAUI workload on Windows, or the .NET tooling and Xcode on macOS.

To develop and run the app:

1. Open `StreaklyApp.slnx` in Visual Studio.
2. Restore the .NET dependencies.
3. Select `StreaklyApp` as the startup project and run it on an Android emulator or device.
4. To build, run, or sign for iOS, use a Mac with Xcode. Visual Studio on Windows can connect to a paired Mac for iOS development.

## Development principles

- Prefer simple, readable code over clever abstractions
- Keep features small and testable
- Document assumptions and decisions in code or docs
- Validate behavior before merging changes
- Keep secrets and environment-specific values out of source control

## Roadmap

- Define product requirements and user flows
- Implement the recurring habit and one-time task flows
- Define the habit and check-in domain models
- Implement habit creation and tracking
- Add streak calculations and progress views
- Persist data locally, then evaluate whether cloud sync is needed

## Contributing

Use short, focused branch names and keep pull requests easy to review. Favor clear commit messages and add tests for behavior changes.

## License

This project is currently unlicensed until a final decision is made for distribution and commercial use.
