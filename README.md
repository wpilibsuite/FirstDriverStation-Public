# FIRST Driver Station - Public

This repository contains the public portions of the FIRST Driver Station. This will include releases, along with public issue tracking.

## Installing
The latest release contains the releases for all platforms. On macOS and Windows platforms, prefer the .pkg and .exe, as those are the installers for each platform. On linux, only archives are provided.

## Per Platform Setup

### Windows
On Windows, the app has everything configured by default. Just run the installer, and then run the application.

### macOS
There are 2 permissions that macOS requires. It requires Input Monitoring and Local Network access. You will get 2 popups for this, and they must be accepted in order to work. The first time you start up the app, these prompts will cause the launch to fail, and you'll need to accept the Input Monitoring prompt, and then restart the app. Then you'll be able to accept the Local Network permission.

If Local Network access is declined, the app will still seem to function normally, as Apple does not provide a way to detect if the permission has been granted.

If you decline either of these, you can fix the settings in the `Privacy & Security` tab of System Settings.

### Linux
The following packages must be installed in order for linux to work.

```
TODO
```

Additionally, the app needs to be a part of the input group in order to have input access. That can be done with the following commands.

```
sudo chgrp input FirstDriverStation
sudo chmod g+s FirstDriverStation
```

Finally, for proper controller access, the current user needs access to hidraw. To do that, create a `/etc/udev/rules.d/72-hidraw.rules` containing

```
# Grant access to all hidraw devices for the active user
KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660", TAG+="uaccess"
```

Then reload udev rules. `sudo udevadm control --reload-rules && sudo udevadm trigger`
