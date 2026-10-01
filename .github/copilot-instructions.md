# Copilot instructions for Streakly App

## Project intent

This repository is for a habit and streak tracking application. Favor simplicity, maintainability, and user value over over-engineering.

## General coding guidance

- Keep code clear, typed, and easy to follow
- Prefer small, focused components and functions
- Favor conventional naming and consistent file organization
- Avoid introducing unnecessary dependencies or abstraction layers
- Don’t add placeholders or dead code that are not related to the current task

## .NET MAUI and C# guidance

- Build the mobile app with .NET MAUI and C#, targeting both Android and iOS.
- Prefer MAUI controls and platform APIs over adding a separate web front end.
- Keep UI code, view models, and habit-domain logic separated according to existing project patterns.
- Keep platform-specific behavior behind clear platform abstractions or platform-specific files.
- Use dependency injection and asynchronous APIs for I/O-bound work.
- Validate user input and provide useful, accessible feedback.
- Ensure screens work across supported device sizes and respect platform accessibility conventions.
- Do not assume iOS can be built or signed on Windows; iOS builds require macOS and Xcode.

## Data and services

- Prefer local-first behavior for core habit tracking unless the requirements say otherwise.
- Use the persistence technology already established by the project; do not introduce a database or remote service without a concrete need.
- If a backend is added, keep domain logic independent of HTTP and use established ASP.NET Core patterns for the service.

## Testing expectations

- Add or update tests when behavior changes
- Prefer targeted tests for the exact behavior under change
- Validate edge cases and failure paths, not just happy paths

## Documentation expectations

- Update documentation when behavior, setup, or conventions change
- Keep setup instructions practical and current
- Document any assumptions that affect architecture or delivery decisions

## Security and quality

- Never commit secrets, API keys, or local environment values
- Sanitize and validate user input before using it in APIs or queries
- Avoid unsafe logging of sensitive information
- Follow the least-privilege principle in permissions and configuration

## Preferred workflow

- Start with the smallest working implementation
- Keep changes scoped to the task at hand
- Verify with the smallest relevant commands or tests
- If a design decision is unclear, prefer the simplest path that preserves future extensibility
