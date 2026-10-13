# Urbus

A citizen-facing Flutter app for tracking municipal fleet vehicles (buses) in real time, built on top of a [Traccar](https://www.traccar.org/) GPS-tracking backend.

## Live Demo

[TODO: add live link] — no published build or store listing found in the repository.

## Screenshots

> Add screenshots to `/screenshots` and reference them below.

| Login | Live Tracking | History |
| --- | --- | --- |
| ![Login](screenshots/login.png) | ![Tracking](screenshots/tracking.png) | ![History](screenshots/history.png) |

[TODO: capture and add real screenshots — `assets/images/logo.png` is the only image currently in the repo]

## The Problem It Solves

Riders of a municipal transit/fleet system have no visibility into where their bus is or when it will arrive. Urbus connects to a Traccar server and gives citizens a live map of the fleet, an ETA to their stop, ride history, and service alerts/announcements — replacing guesswork with real-time location data.

## Features

- **Authentication** against a Traccar server (`/api/session`), with the session persisted via a cookie jar (`cookie_jar` + `dio_cookie_manager`) so login survives app restarts.
- **Live tracking map** (`flutter_map` + OpenStreetMap/CARTO tiles) showing vehicle position via a WebSocket connection to Traccar, with automatic reconnect (exponential backoff) and ETA calculation to the next checkpoint.
- **Checkpoint / route progress** display with arrival detection based on a configurable arrival radius.
- **Ride history** screen backed by Traccar's stop/reports endpoints.
- **Alerts** screen (requires authentication).
- **Announcements** screen with banner notifications.
- **Local push notifications** (arrival, delay, announcement, visit) via `flutter_local_notifications`.
- **Connectivity awareness** — a banner reacts to network state via `connectivity_plus`.
- **Mock-data fallback** — when `TRACCAR_BASE_URL` is not configured, services fall back to mock data instead of silently failing, and this is surfaced explicitly in the UI rather than presented as real data.
- Spanish-language date/time formatting and a dark, map-first custom theme.

## Tech Stack

- **Framework:** Flutter (Dart SDK `>=3.3.0 <4.0.0`)
- **State management:** `flutter_riverpod`
- **Routing:** `go_router`
- **Networking:** `dio`, `cookie_jar`, `dio_cookie_manager`, `web_socket_channel`
- **Maps:** `flutter_map`, `latlong2`
- **Persistence:** `flutter_secure_storage` (secure, not `shared_preferences`, specifically for auth data), `path_provider`
- **Config:** `flutter_dotenv` (`.env` file)
- **Notifications:** `flutter_local_notifications`, `timezone`
- **Connectivity:** `connectivity_plus`
- **UI:** `google_fonts`, `flutter_svg`, `shimmer`, `sliding_up_panel`, `intl`
- **Testing:** `flutter_test`, `mockito`, `build_runner`
- **Backend (external):** [Traccar](https://www.traccar.org/) GPS tracking server — the app is a client only; no backend code lives in this repo.
- **Platforms:** Android, iOS, Web (platform folders present in the repo)

## Project Structure

```
lib/
├── main.dart                     # App bootstrap: env, locale, notifications, cookie jar
├── app/
│   ├── app.dart                  # Root MaterialApp widget
│   ├── routes.dart                # go_router routes + auth-aware redirects
│   └── theme/                     # Colors, typography, theme data
├── core/
│   ├── constants/                 # App-wide constants and API endpoint definitions
│   ├── models/                    # User, unit, checkpoint, visit, announcement models
│   ├── providers/                 # Riverpod providers (auth state)
│   ├── services/                  # Auth, tracking (WS), location, notifications, connectivity
│   └── utils/                     # ETA calculator, formatters, validators
├── features/
│   ├── auth/                      # Splash + login screens
│   ├── tracking/                  # Live map screen, bottom sheet, markers, ETA widgets
│   ├── history/                   # Ride/visit history
│   ├── alerts/                    # Alerts screen (authenticated)
│   ├── announcements/             # Announcements screen + banner
│   └── profile/                   # User profile screen
└── shared/
    ├── extensions/                # Context/String extensions
    └── widgets/                   # Reusable buttons, fields, overlays, skeletons

test/
└── unit/                          # Auth service, ETA calculator, tracking controller tests
```

## Getting Started

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install) (Dart `>=3.3.0 <4.0.0`)
- A running [Traccar](https://www.traccar.org/) server (or use the public demo instance for evaluation)
- Android Studio / Xcode (for device/emulator builds) or a web browser (for the Flutter web target)

### Installation

```bash
git clone https://github.com/jmcuaor1/User_profile-.git
cd User_profile-
flutter pub get
```

### Environment Variables

Copy `.env.example` to `.env` and fill in your Traccar server URLs:

```bash
cp .env.example .env
```

| Variable | Purpose |
| --- | --- |
| `TRACCAR_BASE_URL` | Base HTTP(S) URL of the Traccar REST API |
| `TRACCAR_WS_URL` | WebSocket URL of the same Traccar server, used for live position updates |

If `TRACCAR_BASE_URL` is left empty, the app falls back to mock data instead of a real backend.

### Running

```bash
flutter run
```

## Tests

Unit tests cover the auth service, ETA calculator, and tracking controller:

```bash
flutter test
```

## Author

**Juan Miguel Cuao Ríos**

Portfolio: [TODO: add portfolio link]

LinkedIn: [https://www.linkedin.com/in/juan-miguel-cuao-rios-b888081a9](https://www.linkedin.com/in/juan-miguel-cuao-rios-b888081a9)
