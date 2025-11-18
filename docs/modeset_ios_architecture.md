ModeSet iOS Architecture (v1)

This document describes the internal structure, components, and data flow of the ModeSet iOS app.
It defines how the app will implement persona switching using Focus Modes, Shortcuts, and deep links.

The architecture is intentionally simple and optimized for iPhone 15+.

1. High-Level Overview

ModeSet is a local-only, privacy-respecting app.
No remote servers are required in early versions.

The app has four core responsibilities:

Store persona definitions (local database)

Generate shortcuts and automations for each persona

Register deep links to execute persona switching

Trigger the correct Shortcut via deep link when a QR code is scanned

The iOS system (Focus + Shortcuts) handles the actual device transformation.

2. App Layers
┌───────────────────────────────┐
│        SwiftUI Frontend       │
│  Persona UI / QR Generator    │
└───────────────────────────────┘
┌───────────────────────────────┐
│        Persona Engine         │
│ Loads/saves Persona objects   │
│ Generates Shortcut metadata   │
│ Generates deep links          │
└───────────────────────────────┘
┌───────────────────────────────┐
│   Shortcut Integration Layer  │
│ Exports .shortcut files       │
│ Triggers Shortcuts via URLs   │
└───────────────────────────────┘
┌───────────────────────────────┐
│       iOS System Layer        │
│ Focus Modes / Wallpaper APIs  │
│ Shortcuts execution           │
│ URL Scheme dispatch           │
└───────────────────────────────┘

3. Data Storage

ModeSet stores all data locally using one of two options:

Option A (recommended): SwiftData

Option B: CoreData

Persona table schema:
PersonaEntity {
  id: UUID
  name: String
  description: String

  wallpaperHome: String
  wallpaperLock: String
  lockScreenStyle: String

  focusModeID: String
  allowedApps: [String]
  hiddenApps: [String]

  homeScreenPages: [String]
  widgetConfig: String

  brightness: Int
  volume: Int
  appearance: String

  automationOnID: String
  automationOffID: String

  qrDeepLink: String
}


All values remain on-device.
No network calls needed.

4. Deep Link Handler

The app registers a URL scheme:

modeset://persona/<id>


When opened:

The app reads <id>

Looks up the matching PersonaEntity

Loads the Shortcut name mapped to that persona

Calls:

shortcuts://run-shortcut?name=<ShortcutName>


This instantly triggers the persona switch.

5. Shortcut Integration Layer

ModeSet must:

5.1 Export shortcut files

Shortcuts are stored as .shortcut files.

ModeSet bundles templates:

activate_persona.shortcut
deactivate_persona.shortcut


These are duplicated for each persona with inserted variables.

The user installs them during onboarding.

5.2 Trigger Shortcuts

Shortcut execution uses deep link format:

shortcuts://run-shortcut?name=<ShortcutName>


The app only needs to open that URL.

6. Focus Mode Configuration

Focus Mode setup cannot be automated entirely.

ModeSet will:

Pre-generate instructions

Provide screens with pre-filled configuration values

Walk the user step-by-step

Detect completion via local state

Later versions may support Focus API updates if Apple expands them.

7. QR Code Generator

ModeSet creates QR codes with payload format:

https://modeset.app/switch/<personaID>


Which internally redirects to:

modeset://persona/<personaID>


This allows:

Camera → Safari → ModeSet → Shortcut → persona switch

8. SwiftUI UI Architecture

Screens:

Welcome / Setup

Persona List

Persona Editor

QR Generator

Settings

Debug (internal)

Simplify early UI as:

NavigationStack {
  PersonasView
}


Each persona cell leads to:

Edit

View QR

Trigger manually

9. Architecture Goals

Minimal OS friction

No backend required

Reliable Shortcut triggering

Local, private data

Persona switching under 1 second

Onboarding time < 90 seconds

End of iOS Architecture (v1)
