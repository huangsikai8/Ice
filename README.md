<div align="center">
    <img src="Ice/Assets.xcassets/AppIcon.appiconset/icon_256x256.png" width=200 height=200>
    <h1>Ice</h1>
</div>

> [!IMPORTANT]
> **This is an unofficial fork of [jordanbaird/Ice](https://github.com/jordanbaird/Ice).** It is not affiliated with or endorsed by the Ice project. It adds one change, described below, to the `macos-26` branch (0.11.13-dev). Everything else is the original Ice, and all credit for it goes to Jordan Baird. The original README follows this section.

## About this fork

### Covered menu bar items appear in the Ice Bar

On a MacBook with a notch, or whenever an app has a long menu, macOS runs out of room in the menu bar and quietly drops the leftmost status items. Items in Ice's **visible** section can disappear this way. Because they aren't in the hidden section either, they don't show up in the Ice Bar, which leaves no way to reach them.

With this fork, clicking the Ice icon to open the Ice Bar shows your hidden items **followed by any visible items that macOS has pushed off the menu bar**, in their usual left-to-right order. Clicking one uses Ice's existing "temporarily show" behavior: the item moves into view, opens its menu, and returns to its place after the rehide interval.

<img src="Resources/Fork/covered-items.png" width="510" alt="The Ice Bar showing hidden items followed by the covered Wi-Fi and other visible items">

In the screenshot, the Wi-Fi item and the two items after it belong to the visible section but were pushed off the menu bar by the notch and the frontmost app's menus.

An item counts as covered when the window server reports its window as off-screen, so the change handles the notch and long app menus the same way, with no geometry calculations. The change is confined to two files:

- [`Ice/MenuBar/IceBar/IceBar.swift`](Ice/MenuBar/IceBar/IceBar.swift): the hidden-section Ice Bar appends covered visible items (never the Ice icon itself).
- [`Ice/MenuBar/MenuBarItems/MenuBarItemImageCache.swift`](Ice/MenuBar/MenuBarItems/MenuBarItemImageCache.swift): images for the visible section are also captured while that Ice Bar is open.

The Ice Bar must be enabled (**Settings → General → Use Ice Bar**).

### Building

This fork is distributed as source only; there are no prebuilt releases. Building requires Xcode 26. The change has been tested on macOS 26.

The app must be signed by an Apple developer team. Ice's helper service only accepts connections from code signed by the same team, and the hardened runtime refuses to load the bundled Sparkle framework otherwise. A free Personal Team works: add your Apple ID in **Xcode → Settings → Accounts**, then build with your team ID:

```sh
xcodebuild -project Ice.xcodeproj -scheme Ice -configuration Release \
  -derivedDataPath build -allowProvisioningUpdates \
  DEVELOPMENT_TEAM=<your team ID> CODE_SIGN_STYLE=Automatic \
  CODE_SIGN_IDENTITY="Apple Development" build
```

The app is written to `build/Build/Products/Release/Ice.app`.

Before installing:

- **Quit and remove any other copy of Ice.** Every copy shares the bundle identifier `com.jordanbaird.Ice`, and macOS may launch the wrong one or apply permissions to it.
- **Grant permissions again.** A new signature invalidates earlier Accessibility and Screen Recording grants. Running `tccutil reset Accessibility com.jordanbaird.Ice` and `tccutil reset ScreenCapture com.jordanbaird.Ice` clears stale entries first. Your settings and layout are kept.
- **Decline in-app updates.** Updating installs the official build, which removes this change.

### License

Like the original, this fork is licensed under the [GNU General Public License v3.0](LICENSE). Modified by Sikai Huang on 21 September 2026, as described above.

---


Ice is a powerful menu bar management tool. While its primary function is hiding and showing menu bar items, it aims to cover a wide variety of additional features to make it one of the most versatile menu bar tools available.

![Banner](https://github.com/user-attachments/assets/4423085c-4e4b-4f3d-ad0f-90a217c03470)

[![Download](https://img.shields.io/badge/download-latest-brightgreen?style=flat-square)](https://github.com/jordanbaird/Ice/releases/latest)
![Platform](https://img.shields.io/badge/platform-macOS-blue?style=flat-square)
![Requirements](https://img.shields.io/badge/requirements-macOS%2014%2B-fa4e49?style=flat-square)
[![Sponsor](https://img.shields.io/badge/Sponsor%20%E2%9D%A4%EF%B8%8F-8A2BE2?style=flat-square)](https://github.com/sponsors/jordanbaird)
[![Website](https://img.shields.io/badge/Website-015FBA?style=flat-square)](https://icemenubar.app)
[![License](https://img.shields.io/github/license/jordanbaird/Ice?style=flat-square)](LICENSE)

> [!NOTE]
> Ice is currently in active development. Some features have not yet been implemented. Download the latest release [here](https://github.com/jordanbaird/Ice/releases/latest) and see the roadmap below for upcoming features.

<a href="https://www.buymeacoffee.com/jordanbaird" target="_blank">
    <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;">
</a>

## Install

### Manual Installation

Download the "Ice.zip" file from the [latest release](https://github.com/jordanbaird/Ice/releases/latest) and move the unzipped app into your `Applications` folder.

### Homebrew

Install Ice using the following command:

```sh
brew install --cask jordanbaird-ice
```

## Features/Roadmap

### Menu bar item management

- [x] Hide menu bar items
- [x] "Always-hidden" menu bar section
- [x] Show hidden menu bar items when hovering over the menu bar
- [x] Show hidden menu bar items when an empty area in the menu bar is clicked
- [x] Show hidden menu bar items by scrolling or swiping in the menu bar
- [x] Automatically rehide menu bar items
- [x] Hide application menus when they overlap with shown menu bar items
- [x] Drag and drop interface to arrange individual menu bar items
- [x] Display hidden menu bar items in a separate bar (e.g. for MacBooks with the notch)
- [x] Search menu bar items
- [x] Menu bar item spacing (BETA)
- [ ] Profiles for menu bar layout
- [ ] Individual spacer items
- [ ] Menu bar item groups
- [ ] Show menu bar items when trigger conditions are met

### Menu bar appearance

- [x] Menu bar tint (solid and gradient)
- [x] Menu bar shadow
- [x] Menu bar border
- [x] Custom menu bar shapes (rounded and/or split)
- [ ] Remove background behind menu bar
- [ ] Rounded screen corners
- [ ] Different settings for light/dark mode

### Hotkeys

- [x] Toggle individual menu bar sections
- [x] Show the search panel
- [x] Enable/disable the Ice Bar
- [x] Show/hide section divider icons
- [x] Toggle application menus
- [ ] Enable/disable auto rehide
- [ ] Temporarily show individual menu bar items

### Other

- [x] Launch at login
- [x] Automatic updates
- [ ] Menu bar widgets

## Why does Ice only support macOS 14 and later?

Ice uses a number of system APIs that are available starting in macOS 14. As such, there are no plans to support earlier versions of macOS.

## Gallery

#### Show hidden menu bar items below the menu bar

![Ice Bar](https://github.com/user-attachments/assets/f1429589-6186-4e1b-8aef-592219d49b9b)

#### Drag-and-drop interface to arrange menu bar items

![Menu Bar Layout](https://github.com/user-attachments/assets/095442ba-f2d0-4bb4-9632-91e26ef8d45b)

#### Customize the menu bar's appearance

![Menu Bar Appearance](https://github.com/user-attachments/assets/8c22c185-c3d2-49bb-971e-e1fc17df04b3)

#### Menu bar item search

![Menu Bar Item Search](https://github.com/user-attachments/assets/d1a7df3a-4989-4077-a0b1-8e7d5a1ba5b8)

#### Custom menu bar item spacing

![Menu Bar Item Spacing](https://github.com/user-attachments/assets/b196aa7e-184a-4d4c-b040-502f4aae40a6)

## License

Ice is available under the [GPL-3.0 license](LICENSE).
