ModeSet Persona Specification (v1)

Purpose:
Defines the structure, data fields, and behavior of a ModeSet persona for iOS-first devices (iPhone 15+).
This spec captures how a persona maps to Focus Modes, Wallpapers, Lock Screens, Widgets, and Automations.

1. Persona Object Structure

Each persona is stored as a JSON-like object:

Persona {
  id: string (UUID)
  name: string
  description: string

  wallpaperHome: ImageID
  wallpaperLock: ImageID
  lockScreenStyle: LockScreenPresetID

  focusModeID: string
  allowedApps: [AppBundleID]
  hiddenApps: [AppBundleID]

  homeScreenPages: [HomeScreenPageID]
  widgetConfig: WidgetPresetID

  brightness: number (0–100)
  volume: number (0–100)
  appearance: "light" | "dark" | "auto"

  automationOnID: ShortcutID
  automationOffID: ShortcutID

  qrDeepLink: string
}

2. Persona Transitions

Switching to a persona triggers:

Focus Mode Activation

Wallpaper swap (home + lock)

Home Screen page filtering

Allowed apps + notification filters

Brightness/appearance/volume changes

Widget stack changes (if supported)

All executed via pre-authorized Shortcuts actions.

3. Deep Link Structure

Format:

modeset://persona/<id>


Examples:

modeset://persona/study
modeset://persona/social
modeset://persona/private

4. QR Code Encoding

QR codes embed:

https://modeset.app/switch/<personaID>


This redirects internally to:

modeset://persona/<personaID>


Camera → Safari → ModeSet app → Shortcut → Persona transition.

5. Focus Mode Mapping

Each persona requires:

focus:
  allowedPeople: []
  allowedApps: allowedApps[]
  silencedApps: all others
  lockScreenID: preset
  homeScreenPages: [pageIDs]
  filters:
    appearance
    lowPowerMode
    calendar filter (optional)

6. Automation Actions

When persona ON:

Set Focus Mode → <focusModeID>

Set Wallpaper (home/lock)

Set Brightness

Set Volume → 0 or preset

Set Appearance → dark/light

Optionally: set Low Power mode

When persona OFF:

Restore normal settings

7. Future Persona Extensions

Time-based persona triggers

Location-based persona triggers

App-driven persona triggers

Multi-layer persona packs

End of v1 spec
