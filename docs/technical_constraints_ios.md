iOS Technical Constraints (ModeSet)

This document outlines what iOS (iOS 17+, iPhone 15+) allows and does not allow for device-level persona switching. ModeSet must operate strictly within the allowed APIs to remain App-Store compliant.

1. Allowed Capabilities (Confirmed)
1.1 Wallpaper Switching

iOS Shortcuts allow:

Set Home Screen wallpaper

Set Lock Screen wallpaper

(Shortcuts → “Set Wallpaper”)

1.2 Focus Mode Switching

Shortcuts allow:

Turn a Focus Mode ON

Turn a Focus Mode OFF

Focus Modes can:

Hide Home Screen pages

Link to Lock Screens

Filter notifications

Dim Lock Screen

Change appearance (dark/light)

1.3 Lock Screen Linking

iOS 16+ lets each Focus Mode have its own Lock Screen style.

1.4 Home Screen Page Visibility

Focus Modes can show specific Home Screen pages and hide others.
This is how ModeSet simulates “hiding apps” indirectly.

1.5 Notification Filtering

Focus Modes allow:

Allowed apps

Allowed people

Everything else silenced

1.6 Appearance Control

Shortcuts allow toggling:

Light mode

Dark mode

Auto

1.7 Brightness + Volume Control

Shortcuts allow setting brightness and volume directly.

1.8 Low Power Mode

Shortcuts allow enabling/disabling Low Power Mode.

1.9 Deep Links + URL Schemes

Apps can register custom URL schemes like:

modeset://persona/<id>

They can be opened via:

QR scan → Safari → app link

Direct app trigger

Shortcut trigger

1.10 QR Code Flow

iOS Camera opens URLs from QR codes.
If those URLs deep link to ModeSet, the app can immediately trigger a Shortcut → Focus → persona switch.

2. Not Allowed (Non-negotiable)
2.1 Rearranging App Icons Automatically

iOS does not allow:

Moving icons

Creating/deleting pages

Re-ordering apps

Only entire pages can be shown/hidden.

2.2 Hiding/Unhiding Individual Apps

Apple prohibits hiding individual apps programmatically.
Focus page filtering is the only acceptable workaround.

2.3 True System Themes

No custom:

icon packs

system colors

fonts

full visual skins

2.4 Running Shortcuts Completely Silently

Some Shortcuts require user trust/approval.
Automations can run silently only after the user toggles “Allow Running Without Asking.”

2.5 Altering Notification UI

ModeSet cannot redesign notification banners or system UI.
It can only silence/allow notifications via Focus.

3. Acceptable Workarounds
3.1 Use Focus Modes as the Persona Engine

Focus handles:

Notifications

Screen visibility

Lock Screen style

Allowed apps

Home Screen pages

This is ModeSet’s backbone.

3.2 Use Shortcuts for Visual + Behavioral Changes

Shortcuts handle:

Wallpaper switching

Brightness

Volume

Low Power Mode

Appearance

Triggering specific Focus modes

3.3 QR Codes as Instant Triggers

QR codes → URL → ModeSet app → Shortcut → Focus → persona transition.
To the user, this feels like instant “shape-shifting.”

4. App Store Compliance Notes

ModeSet MUST NOT:

Mimic MDM / supervised device control

Hide apps beyond Focus page filtering

Interfere with Screen Time / parental controls

Auto-run Shortcuts without explicit user opt-in

ModeSet IS SAFE if all changes are:

Pre-authorized via Shortcuts + Focus setup

Clearly explained to the user

Reversible at any time

End of iOS Technical Constraints (v1)
