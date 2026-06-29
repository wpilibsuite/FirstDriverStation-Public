# Gamepad Support Matrix

The FIRST Driver Station uses SDL for gamepad support. Most common gamepads work without custom configuration, but support can vary by operating system, connection type, controller firmware, and whether the controller exposes a stable serial number. Note that only wired support is marked in this matrix. Wireless controller support is not tracked.

## Compatibility Notes

- ✅ **Supported**: The controller is expected to work normally on that platform.
- ⚠️ **Partial**: The controller is detected, but some features or connection modes may not work consistently.
- ❌ **Unsupported**: The controller is not expected to work on that platform.
- In the **SN** column, ✅ indicates that the Driver Station can identify individual controllers of the same model using a serial number or equivalent stable identifier, and ❌ indicates that it cannot. See [Gamepad Locking](GamepadLocking.md) for why this matters.
- VID/PID values can differ between firmware versions, and hardware revisions. Confirm the value shown by the Driver Station for the specific controller in use when troubleshooting.

## Matrix

| Name | VID/PID | Windows | macOS | Linux | SN | Notes |
|---|---|---|---|---|---|---|
| PlayStation 5 DualSense | `054C:0CE6` | ✅ | ✅ | ✅ | ✅ |  |
| PlayStation 5 DualSense Edge | `054C:0DF2` | ✅ | ✅ | ✅ | ✅ |  |
| Xbox Series | `045E:0B12` | ✅ | ✅ | ✅ | ❌ | Rumble support is spotty. |
| Xbox Elite Series 2 | `045E:0B00` | ⚠️ | ⚠️ | ⚠️ | ❌ | Back buttons are not supported, and rumble support is spotty. |
| Nintendo Switch Pro | `057E:2009` | ✅ | ✅ | ✅ | ✅ |  |
| Nintendo Switch 2 Pro | `057E:2069` | ✅ | ❌ | ✅ | ✅ |  |
| Nintendo NSO N64 | `057E:2019` | ✅ | ✅ | ✅ | ✅ | Mappings are odd due to the awkward button layout. |
| Nintendo NSO GameCube | `057E:2073` | ✅ | ❌ | ✅ | ✅ |  |
| Wii U GameCube Adapter (WUP-028) |  | ⚠️ | ❌ | ✅ | ❌ | On Windows, the driver needs to be changed manually to WinUSB using Zadig. Each on-device port can be used. In the DS, it will show as having an SN, however that is for the whole device, not the individual controllers. |
| Steam Controller 2026 | `28DE:1106` | ✅ | ❌ | ✅ | ✅ | Does not enumerate on macOS |
| REV USB PS4 Compatible |  | ⚠️ | ✅ | ✅ | ❌ | Shows up as a generic Xbox controller on Windows, missing much of the functionality. |
| Logitech F310 |  | ✅ | ✅ | ✅ | ❌ | Must be in D mode to show up correctly on macOS. |
| PowerA Advantage Switch 2 | `20D6:A720` | ⚠️ | ⚠️ | ⚠️ | ❌ | Shows up as a joystick without gamepad mappings. |
