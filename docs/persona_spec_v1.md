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
