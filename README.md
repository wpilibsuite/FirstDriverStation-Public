# *FIRST*® Driver Station - Releases & Issue Tracking

This repository contains the public portions of the FIRST Driver Station. This will include releases, along with public issue tracking.

## Notes

> [!WARNING]
> FIRST Driver Station is not supported on any current FIRST FMS. DS Alpha 7 can be supported on development versions of Cheesy Arena, but this support is considered experimental.

## Installing
The [latest release](https://github.com/wpilibsuite/FirstDriverStation-Public/releases/latest) contains the releases for all platforms. On macOS and Windows platforms, prefer the .pkg and .exe, as those are the installers for each platform. On Linux, only archives are provided.

> [!WARNING]
> These installers are not to be uploaded to any public package repositories without permission of *FIRST*® or WPILib.

## Per Platform Setup

### Windows
On Windows, the app has everything configured by default. Just run the installer, and then run the application.

### macOS
There are 3 permissions that macOS requires. It requires Input Monitoring, Local Network access, and data access from other apps. You will get popups for these, and they must be accepted in order to work. The first time you start up the app, these prompts will cause the launch to fail, and you'll need to accept the Input Monitoring prompt, and then restart the app. Then you'll be able to accept the Local Network permission.

If Local Network access is declined, the app will still seem to function normally, as Apple does not provide a way to detect if the permission has been granted.

macOS will also prompt with **"FirstDriverStation" would like to access data from other apps.** This permission is required for launching dashboards and for log file writing. You should click **Allow** when this prompt appears.

If you decline any of these, you can fix the settings in the `Privacy & Security` tab of System Settings.

If Local Network access still does not work after re-enabling it there, see [macOS Permissions](docs/macOSPermissions.md) for a terminal-based workaround.

### Linux

> [!WARNING]
> Due to missing functionality in Wayland itself, docked mode is not supported when running with the native Wayland backend.

The following packages must be installed in order for Linux to work.

```
TODO
```

Additionally, the app needs to be a part of the input group in order to have input access. That can be done with the following commands:
```sh
sudo chgrp input FirstDriverStation
sudo chmod g+s FirstDriverStation
```

Finally, for proper controller access, the current user needs access to `hidraw`. To do that, create a `/etc/udev/rules.d/72-hidraw.rules` containing:
```
# Grant access to all hidraw devices for the active user
KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660", TAG+="uaccess"
```

Then reload the `udev` rules:
```sh
sudo udevadm control --reload-rules && sudo udevadm trigger
```
