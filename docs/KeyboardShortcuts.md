# Keyboard Shortcuts

The FIRST DS supports the following keyboard shortcuts. These shortcuts are monitored even when the app is in the background.

- `Spacebar`: E-Stops the robot. This only will take effect if the robot is enabled.
- `Backspace`: A-Stops the robot. In match mode, this will disable the robot for the rest of autonomous. In auto mode, this will disable the robot.
- `Enter`: Disables the robot. If in match mode, will stop the match. Otherwise will just disable the robot.
- `[ + ] + \`: Enables the robot. In match mode, will start the match.
- `Left Control`: Will refresh the currently connected joystick lists. Newly connected joysticks are not enumerated unless the robot is disabled or the DS is on the gamepad screen. This is because enumerating a newly connected joystick can hang the joystick thread, which we don't want to occur while enabled. This shortcut allows you to force a reenumeration on any screen on the DS. If on the gamepad screen, enumeration will always occur.
- `Esc + I`: When held for 1 second, will reset an E-Stop.
