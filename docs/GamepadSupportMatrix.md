# Gamepad Support Matrix

The FIRST Driver Station uses SDL for gamepad support. Most common gamepads work without custom configuration, but support can vary by operating system, connection type, controller firmware, and whether the controller exposes a stable serial number. Note that only wired support is marked in this matrix. Wireless controller support is not tracked.

## Compatibility Notes

- ✅ **Supported**: The controller is expected to work normally on that platform.
- ⚠️ **Partial**: The controller is detected, but some features or connection modes may not work consistently.
- ❌ **Unsupported**: The controller is not expected to work on that platform.
- In parenthesis, ✅ indicates that the Driver Station can identify individual controllers of the same model using a serial number or equivalent stable identifier, and ❌ indicates that it cannot. See [Gamepad Locking](GamepadLocking.md) for why this matters.
- VID/PID values can differ between firmware versions, and hardware revisions. Confirm the value shown by the Driver Station for the specific controller in use when troubleshooting.
- All controller test results are for the controller in wired mode, unless otherwise listed.  Compatibility/features may vary when using a wireless connection (Bluetooth, wireless receiver, etc).

## Matrix

| Name                               | VID/PID     | Windows | macOS  | &nbsp;&nbsp;Linux&nbsp;&nbsp; | Driver&nbsp;Hub | Notes |
|------------------------------------|-------------|:-------:|:------:|:------:|:----------:|-------|
| Playstation 3 Dualshock 3/Sixaxis  | `054C:0268` | ❓ (❓) | ⚠️ (❌) | ⚠️ (❌) | ⚠️ (❌)    | Need to press PS button for controller to activate.  Sticks read as being at the top left until controller activated and stick moved on Linux/Driver Hub.  Controller shows up as 2 controllers on macOS.  Select and PS button mapping is swapped on macOS. |
| Playstation 4 Dualshock 4          | `054C:05C4` CUH-ZCT1x<br>`054C:09CC` CUH-ZCT2x | ❓ (❓) | ✅ (✅) | ✅ (✅) | ✅ (✅)    |  |
| Etpark Wired Controller for PS4    |             | ❓ (❓) | ❓ (❓) | ❓ (❓) | ❓ (❓)    | From CM: "Newer versions of this device may not support all functionality provided by the FTC SDK", so there may be multiple variants? |
| REV USB PS4 Compatible             |             | ⚠️ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    | Shows up as a generic Xbox controller on Windows, missing much of the functionality. |
| PlayStation 5 DualSense            | `054C:0CE6` | ✅ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    |  |
| PlayStation 5 DualSense Edge       | `054C:0DF2` | ✅ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    |  |
| Xbox 360 Wired Controller          |             | ❓ (❓) | ❓ (❓) | ❓ (❓) | ❓ (❓)    |  |
| PDP/GameStop Xbox 360 Controller   | `0E6F:0401` | ❓ (❓) | ⚠️ (✅) | ✅ (✅) | ✅ (✅)    | More than 1 Xinput controller makes the DS bug out on macOS (https://github.com/wpilibsuite/FirstDriverStation-Public/issues/62) |
| Xbox One Controller (Model 1537)   | `045E:02D1` | ❓ (❓) | ✅ (✅) | ✅ (✅) | ✅ (✅)    | No "impulse trigger" (trigger rumble) support on Linux/Driver Hub.  No controller battery reporting on macOS/Linux/Driver Hub. |
| Xbox One S Controller (Model 1708) | `045E:02EA` | ❓ (❓) | ✅ (❌) | ✅ (✅) | ✅ (✅)    | No "impulse trigger" (trigger rumble) support on Linux/Driver Hub.  Broken controller battery reporting on macOS.  No controller battery reporting on Linux/Driver Hub. |
| Xbox Series                        | `045E:0B12` | ✅ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    | Rumble support is spotty. |
| Xbox Elite Series 2                | `045E:0B00` | ⚠️ (❓) | ⚠️ (❓) | ⚠️ (❓) | ❓ (❓)    | Back buttons are not supported, and rumble support is spotty. |
| Logitech F310                      | `046D:C21D` | ❓ (❓) | ⚠️ (✅) | ✅ (✅) | ✅ (✅)    | More than 1 Xinput controller makes the DS bug out on macOS (https://github.com/wpilibsuite/FirstDriverStation-Public/issues/62) |
| Steam Controller (2026)            | `28DE:1106` | ✅ (❓) | ❌ (❓) | ✅ (❓) | ❓ (❓)    | Does not enumerate on macOS |
| Nintendo Switch Pro                | `057E:2009` | ✅ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    |  |
| Nintendo Switch 2 Pro              | `057E:2069` | ✅ (❓) | ❌ (❓) | ✅ (❓) | ❓ (❓)    |  |
| Nintendo NSO N64                   | `057E:2019` | ✅ (❓) | ✅ (❓) | ✅ (❓) | ❓ (❓)    | Mappings are odd due to the awkward button layout. |
| Nintendo NSO GameCube              | `057E:2073` | ✅ (❓) | ❌ (❓) | ✅ (❓) | ❓ (❓)    |  |
| Wii U GameCube Adapter (WUP-028)   |             | ⚠️ (❓) | ❌ (❓) | ✅ (❓) | ❓ (❓)    | On Windows, the driver needs to be changed manually to WinUSB using Zadig. Each on-device port can be used. In the DS, it will show as having an SN, however that is for the whole device, not the individual controllers. |
| PowerA Advantage Switch 2          | `20D6:A720` | ⚠️ (❓) | ⚠️ (❓) | ⚠️ (❓) | ❓ (❓)    | Shows up as a joystick without gamepad mappings. |

## Notes:
- Linux/Driver Hub do not support the "impulse trigger" (trigger rumble) feature on Xbox One controllers, see:
  - https://github.com/dlundqvist/xone/pull/189#issuecomment-4181948852
  - https://github.com/libsdl-org/SDL/blob/main/src/joystick/linux/SDL_sysjoystick.c#L1685-L1688
  - https://github.com/paroj/xpad/issues/236
  - https://lore.kernel.org/linux-input/?q=xbox-gip
