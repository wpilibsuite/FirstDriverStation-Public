# Driver Station Settings

The Driver Station stores its settings in `DriverStationSettings.json`.

> [!WARNING]
> All of these settings can be configured through the DS user interface and should generally not be edited manually. Direct edits to this file while the DS is running may be overwritten.

## File Location

- **Windows**: `C:\Users\Public\Documents\FIRSTDriverStation\DriverStationSettings.json`
- **Unix (Linux/macOS)**: `~/.firstds/DriverStationSettings.json`

## Example

```json
{
  "LockedJoysticks": {
    "0": {
      "PadName": "DualSense Edge Wireless Controller",
      "PadGuid": "0300e0274c050000f20d000000016800",
      "PadSerial": "14-3a-9a-f4-0c-cd"
    }
  },
  "TeamNumber": "1",
  "TeamNumberRequired": false,
  "AllianceStation": 0,
  "TeleOpModeHash": 144115188075855873,
  "AutoOpModeHash": 72057594037927937,
  "UtilOpModeHash": 216172782113783809,
  "MatchStartDelay": 5,
  "MatchAuto": 30,
  "MatchTeleDelay": 8,
  "MatchTele": 120,
  "WindowMode": 1,
  "SelectedDashboard": 0,
  "GameData": "1341",
  "DisableSounds": false,
  "AllowJoysticksInAuto": false,
  "UseWiFiStaticAddress": false
}
```

## Fields

### `LockedJoysticks`

An object mapping joystick port numbers (as string keys `"0"`–`"5"`) to a locked joystick configuration. A locked joystick will always occupy the specified port, even if it is disconnected and reconnected.

Each entry contains:

| Field | Type | Description |
|---|---|---|
| `PadName` | string | The human-readable name of the controller. |
| `PadGuid` | string | The SDL3 GUID of the controller (32 hex characters). See [Custom Gamepad Mappings](GamepadMappings.md) for more information about GUIDs. |
| `PadSerial` | string | The serial number or hardware address of the controller (e.g. a Bluetooth MAC address). Used together with `PadGuid` to uniquely identify a specific physical device. |

### `TeamNumber`

**Type**: string

The FRC team number used by the DS to connect to the robot. This value is stored as a string.

### `TeamNumberRequired`

**Type**: boolean

When `true`, the DS requires a team number to be set before it will attempt to connect to the robot. When `false`, the DS will connect even if the team number is not configured.

### `AllianceStation`

**Type**: integer

The alliance station assignment. Valid values are:

| Value | Station |
|---|---|
| `0` | Red 1 |
| `1` | Red 2 |
| `2` | Red 3 |
| `3` | Blue 1 |
| `4` | Blue 2 |
| `5` | Blue 3 |

### `TeleOpModeHash`

**Type**: integer (64-bit)

An internal identifier for the last selected TeleOp game mode. This value is managed automatically by the DS and should not be edited manually.

### `AutoOpModeHash`

**Type**: integer (64-bit)

An internal identifier for the last selected Autonomous game mode. This value is managed automatically by the DS and should not be edited manually.

### `UtilOpModeHash`

**Type**: integer (64-bit)

An internal identifier for the last selected Test/Utility game mode. This value is managed automatically by the DS and should not be edited manually.

### `MatchStartDelay`

**Type**: integer (seconds)

The delay in seconds before the match starts after the enable command is issued.

### `MatchAuto`

**Type**: integer (seconds)

The duration in seconds of the autonomous period in a match.

### `MatchTeleDelay`

**Type**: integer (seconds)

The delay in seconds between the end of the autonomous period and the start of the teleop period.

### `MatchTele`

**Type**: integer (seconds)

The duration in seconds of the teleop period in a match.

### `WindowMode`

**Type**: integer

Controls how the DS window is displayed. Valid values are:

| Value | Mode |
|---|---|
| `0` | Windowed (floating window) |
| `1` | Docked (attached to a screen edge) |

### `SelectedDashboard`

**Type**: integer

The dashboard application to launch alongside the DS. A value of `0` means no external dashboard is launched. All other values are assigned by the DS UI and are not defined here.

### `GameData`

**Type**: string

A short string of game-specific data that is sent to the robot at the start of a match. The robot program can read this value to configure itself for a specific game variant. For details on how to read game data in your robot program, see the [WPILib documentation](https://docs.wpilib.org). The format and meaning of the game data string is defined by the game rules for each season.

### `DisableSounds`

**Type**: boolean

When `true`, the DS will not play audio notifications (e.g. match start/end sounds). When `false`, sounds are enabled.

### `AllowJoysticksInAuto`

**Type**: boolean

When `true`, joystick input from the driver station is forwarded to the robot during the autonomous period. When `false` (the default), joystick data is not sent during autonomous.

### `UseWiFiStaticAddress`

**Type**: boolean

When `true`, the DS uses a static IP address for the WiFi interface when connecting to the robot. When `false`, the DS uses the default network configuration.
