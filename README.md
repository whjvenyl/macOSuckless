# macOSuckless

This document is a list of scripts, commands, hacks, and tweaks to make macOS suck less.

- [Finder Tweaks](#finder-tweaks)
  - [Disable .DS_Store files](#disable-ds_store-files)
  - [Delete .DS_Store files](#delete-ds_store-files)
  - [Use column view as default](#use-column-view-as-default)
  - [Show file extensions](#show-file-extensions)
  - [Show hidden files](#show-hidden-files)
  - [Unhide Library folder](#unhide-library-folder)
  - [Open new Finder windows in home directory](#open-new-finder-windows-in-home-directory)
  - [Search current folder by default](#search-current-folder-by-default)
  - [Show directory info](#show-directory-info)
  - [Show tab bar](#show-tab-bar)
  - [Don't show mounted volumes on Desktop](#dont-show-mounted-volumes-on-desktop)
  - [Show preview pane](#show-preview-pane)
  - [Set sidebar width](#set-sidebar-width)
  - [Better sidebar defaults](#better-sidebar-defaults)
  - [Show connected servers in sidebar](#show-connected-servers-in-sidebar)
  - [Restart Finder and Dock](#restart-finder-and-dock)
- [Usability Tweaks](#usability-tweaks)
  - [Expand Save Panel by default](#expand-save-panel-by-default)
  - [Set Dock size and make it unchangable](#set-dock-size-and-make-it-unchangable)
  - [Dim Dock icon when application is hidden](#dim-dock-icon-when-application-is-hidden)
  - [Make Dock empty (removes all current icons)](#make-dock-empty-removes-all-current-icons)
  - [Add a blank space to the end of Dock](#add-a-blank-space-to-the-end-of-dock)
  - [Speed up LaunchPad animations](#speed-up-launchpad-animations)
  - [Change layout of LaunchPad](#change-layout-of-launchpad)
  - [Don't sort Mission Control Spaces by most recent use](#dont-sort-mission-control-spaces-by-most-recent-use)
- [Input Tweaks](#input-tweaks)
  - [Disable natural scroll direction](#disable-natural-scroll-direction)
  - [Use keyrepeat instead of special character insert dialog on press-and-hold](#use-keyrepeat-instead-of-special-character-insert-dialog-on-press-and-hold)
- [System Tweaks](#system-tweaks)
  - [Allow Applications to be installed from anywhere](#allow-applications-to-be-installed-from-anywhere)
  - [Make crash reporter appear as a notification instead of a floating window (less annoying)](#make-crash-reporter-appear-as-a-notification-instead-of-a-floating-window-less-annoying)
  - [Disable shadows on window screenshots](#disable-shadows-on-window-screenshots)
- [Performance Optimizations](#performance-optimizations)
  - [Disable smooth scrolling](#disable-smooth-scrolling)
  - [Disable animations when opening and closing windows](#disable-animations-when-opening-and-closing-windows)
  - [Disable animations for Quick Look windows](#disable-animations-for-quick-look-windows)
  - [Speed up window resize animations (fullscreen, etc.)](#speed-up-window-resize-animations-fullscreen-etc)
  - [Disable animation when opening the Info window in Finder](#disable-animation-when-opening-the-info-window-in-finder)
  - [Disable animations when you open an application from the Dock.](#disable-animations-when-you-open-an-application-from-the-dock)
  - [Make all animations faster that are used by Mission Control.](#make-all-animations-faster-that-are-used-by-mission-control)
  - [Disable animation on resizing windows before and after showing the version browser (also disabled by NSWindowResizeTime -float 0.001)](#disable-animation-on-resizing-windows-before-and-after-showing-the-version-browser-also-disabled-by-nswindowresizetime--float-0001)
  - [Disable rubberband scrolling (doesn't affect web views)](#disable-rubberband-scrolling-doesnt-affect-web-views)
  - [Disable Dock show/hide animations](#disable-dock-showhide-animations)
  - [Reduce Dock show/hide delay](#reduce-dock-showhide-delay)
  - [Disable animation on column views](#disable-animation-on-column-views)
  - [Disable menubar show animation during fullscreen mode](#disable-menubar-show-animation-during-fullscreen-mode)
- [Compatibility Tweaks](#compatibility-tweaks)
  - [Disable Non-Retina Font Smoothing](#disable-non-retina-font-smoothing)

## Finder Tweaks

### Disable .DS_Store files

_macOS 10.8-10.10_
```shell
#!/usr/bin/env bash
if [[ -n `csrutil status | grep "disabled"` ]]; then
  brew cask install asepsis
else
  echo "You must disable System Integrety Protection to install Asepsis.\nReboot into recovery mode (Command+R during startup) and run 'csrutil disable' from Terminal while in recovery mode." && exit 1
fi
```

### Delete .DS_Store files

_macOS 10.12+_
```shell
#!/usr/bin/env bash
sudo find / -name *.DS_Store -depth -exec rm {} \;
```

### Use column view as default

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.Finder FXPreferredViewStyle Nlsv
```

### Show file extensions

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g AppleShowAllExtensions -bool true
```

### Show hidden files

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder AppleShowAllFiles true
```

### Unhide Library folder

_macOS 10.12+_
```shell
#!/usr/bin/env bash
chflags nohidden ~/Library
```

### Open new Finder windows in home directory

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults read com.apple.finder NewWindowTargetPath -string file:///Users/`whoami`
```

### Search current folder by default

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder FXDefaultSearchScope -string SCcf
```

### Show directory info

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder ShowPathbar -bool true
defaults write com.apple.finder ShowStatusBar -bool true
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
```

### Show tab bar

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder ShowTabView -bool true
```

### Don't show mounted volumes on Desktop

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool false
defaults write com.apple.finder ShowHardDrivesOnDesktop -bool false
defaults write com.apple.finder ShowMountedServersOnDesktop -bool false
defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool false
```

### Show preview pane

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder ShowPreviewPane -bool true
defaults write com.apple.finder PreviewPaneWidth -int 172
```

### Set sidebar width

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder SidebarWidth -int 172
```

### Better sidebar defaults

SFL storage is located in `~/Librarsysy/Application\ Support/com.apple.sharedfilelist/`

`sfltool` (included with macOS) and `mysides` (https://github.com/mosen/mysides) can by used to a limited extent on macOS 10.12, however 10.13 will require more advanced scripting via the XPC interface (see: https://stackoverflow.com/questions/1062856/how-do-you-programmatically-put-folder-icons-on-the-finder-sidebar-given-that-y/16769092#16769092).

macOS 10.12
```shell
#!/usr/bin/env bash
user=`whoami`
mysides remove All\ My\ Files file:///System/Library/CoreServices/Finder.app/Contents/Resources/MyLibraries/myDocuments.cannedSearch/
mysides remove iCloud x-apple-finder:icloud
mysides remove domain-AirDrop nwnode://domain-AirDrop
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Desktop
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Documents
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Downloads
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Movies
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Music
sfltool add-item com.apple.LSSharedFileList.FavoriteItems file:///Users/$user/Pictures
PlistBuddy -c "Add :networkbrowser:CustomListProperties:com.apple.NetworkBrowser.backToMyMacEnabled bool False" /Users/$loggedInUser/Library/Preferences/com.apple.sidebarlists.plist
PlistBuddy -c "Add :networkbrowser:CustomListProperties:com.apple.NetworkBrowser.bonjourEnabled bool False" /Users/$loggedInUser/Library/Preferences/com.apple.sidebarlists.plist
killall cfprefsd
killall Finder
```

### Show connected servers in sidebar

_macOS 10.11_
```shell
#!/usr/bin/env bash
defaults delete com.apple.sidebarlists networkbrowser
defaults write com.apple.sidebarlists networkbrowser -array-add '<dict><key>CustomListItems</key><array/><key>CustomListProperties</key><dict><key>com.apple.NetworkBrowser.connectedEnabled</key><true/><key>com.apple.NetworkBrowser.bonjourEnabled</key><false/><key>com.apple.NetworkBrowser.backToMyMacEnabled</key><true/></dict><key>Controller</key><string>CustomListItems</string></dict>'
```

### Restart Finder and Dock

_macOS 10.12+_
```shell
#!/usr/bin/env bash
killall Finder
rm ~/Library/Application\ Support/Dock/*.db
killall Dock
```

## Usability Tweaks

### Expand Save Panel by default

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSNavPanelExpandedStateForSaveMode -bool true
defaults write -g NSNavPanelExpandedStateForSaveMode2 -bool true
```

### Set Dock size and make it unchangeable

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock tilesize -int 48
defaults write com.apple.dock size-immutable -bool true
killall Dock
```

### Dim Dock icon when application is hidden

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock showhidden -boolean true
killall Dock
```

### Make Dock empty (removes all current icons)

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults delete com.apple.dock persistent-apps
killall Dock
```

### Add a blank space to the end of Dock

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock persistent-apps -array-add '{tile-data={}; tile-type="spacer-tile";}'
killall Dock
```

### Speed up LaunchPad animations

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock springboard-show-duration -float 0.1
defaults write com.apple.dock springboard-hide-duration -float 0.11
defaults write com.apple.dock springboard-page-duration -float 0.1
```

### Change layout of LaunchPad

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock springboard-columns -int 8
defaults write com.apple.dock springboard-rows -int 12
defaults write com.apple.dock ResetLaunchPad -bool true
```

### Don't sort Mission Control Spaces by most recent use

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock mru-spaces -bool false
```

## Input Tweaks

### Disable natural scroll direction

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write NSGlobalDomain com.apple.swipescrolldirection -bool false
echo "You may have to logout and log back in for this change to take effect."
```

### Use keyrepeat instead of special character insert dialog on press-and-hold

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g ApplePressAndHoldEnabled -bool false
defaults write -g InitialKeyRepeat -float 50
defaults write -g KeyRepeat -float 1
```

## System Tweaks

Make macOS less intrusive/annoying.

### Allow Applications to be installed from anywhere

_macOS 10.12+_
```shell
#!/usr/bin/env bash
sudo spctl --master-disable
```

### Make crash reporter appear as a notification instead of a floating window (less annoying)

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.CrashReporter UseUNC 1
```

### Disable shadows on window screenshots

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.screencapture disable-shadow -bool true
killall SystemUIServer
```

## Performance Optimizations

Speed up some aspects of OS X.

Links:

- http://www.defaults-write.com/10-terminal-commands-to-speed-up-macos-sierra-on-your-mac

### Disable smooth scrolling

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSScrollAnimationEnabled -bool false
```

### Disable animations when opening and closing windows

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSAutomaticWindowAnimationsEnabled -bool false
```

### Disable animations for Quick Look windows

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g QLPanelAnimationDuration -float 0
```

### Speed up window resize animations (fullscreen, etc.)

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSWindowResizeTime -float 0.001
```

### Disable animation when opening the Info window in Finder

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.finder DisableAllAnimations -bool true
```

### Disable animations when you open an application from the Dock.

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock launchanim -bool false
```

### Make all animations faster that are used by Mission Control.

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock expose-animation-duration -float 0.1
```

### Disable animation on resizing windows before and after showing the version browser (also disabled by NSWindowResizeTime -float 0.001)

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSDocumentRevisionsWindowTransformAnimation -bool false
```

### Disable rubberband scrolling (doesn't affect web views)

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSScrollViewRubberbanding -bool false
```

### Disable Dock show/hide animations

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock autohide-time-modifier -float 0
```

### Reduce Dock show/hide delay

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write com.apple.dock autohide-delay -float 0.1
```

### Disable animation on column views

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSBrowserColumnAnimationSpeedMultiplier -float 0.001
```

### Disable menubar show animation during fullscreen mode

_macOS 10.12+_
```shell
#!/usr/bin/env bash
defaults write -g NSToolbarFullScreenAnimationDuration -float 0.001
```

## Compatibility Tweaks



### Disable Non-Retina Font Smoothing

Fixes an issue with some applications that have poor smoothing in Mojave when not running on Retina monitors.

_macOS 10.14_
```shell
defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO
```
