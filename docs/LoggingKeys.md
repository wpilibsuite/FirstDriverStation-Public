# Logging Keys

This document describes the NT keys used for Driver Station logging. In Driver Station logs, NetworkTables entries are written with the `DS:` log prefix, so the topic `/Dscomm/Status/Power/Battery` appears in the log as `DS:/Dscomm/Status/Power/Battery`.

Topic names are case-sensitive. A local NetworkTables server on port `6767` exposes these values for live log viewers such as AdvantageScope. Protobuf schemas are available through NetworkTables to decode raw payloads.

## Payload Notes

Several topics use protobuf payloads. They appear as raw NetworkTables values, but decode to the named message structures below.

| Payload | Important fields |
|---|---|
| `mrc.proto.ProtobufControlData` | `control_word`, `match_time`, `current_op_mode`, `game_data`, `joysticks[]` |
| `mrc.proto.ProtobufMatchInfo` | `event_name`, `match_type`, `match_number`, `replay_number` |
| `mrc.proto.ProtobufJoystickDescriptors` | `descriptors[]`: `joystick_name`, `is_gamepad`, `gamepad_type`, `supported_outputs` |
| `mrc.proto.ProtobufJoystickOutputs` | `outputs[]`: packed `leds`, `rumble`, `trigger_rumble` values for each joystick |
| `mrc.proto.ProtobufAvailableOpModes` | `modes[]`: `hash`, `name`, `group`, `description`, `text_color`, `background_color` |
| `mrc.proto.ProtobufConsoleLineTimestamp` | `console_line`, `sequence_number`, `timestamp` |
| `mrc.proto.ProtobufErrorInfoTimestamp` | `error_info`, `sequence_number`, `timestamp`, `num_occurrences` |
| `mrc.proto.ProtobufProgramCrashInfoTimestamp` | `program_crash_info`, `timestamp` |
| `mrc.proto.ProtobufResolvedTeamNumberSet` | `entries[]`: `team_number`, `host_name`, `ip_address` |
| `mrc.proto.ProtobufVersionInfoSet` | `entries[]`: component `device_id`, `name`, and `version` |

`control_word` is a packed integer. The currently used bits are:

| Bits | Meaning |
|---|---|
| 0 | Enabled |
| 1-2 | Robot mode: `0` unknown, `1` autonomous, `2` teleoperated, `3` utility |
| 3 | E-stop |
| 4 | FMS connected |
| 5 | Driver Station connected |
| 8-11 | Alliance station index |
| 13 | Timed match |

`current_op_mode` and `CurrentOpModeTrace` values are packed op-mode hashes. Bits 0-55 are the op-mode hash, bits 56-57 are the robot mode, and bit 58 is the enabled-state bit.

## Driver Station Topics

### `/Dscomm/Control/Robot`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Control/Robot/ControlData` | `mrc.proto.ProtobufControlData` | Main DS-to-robot control packet. Published every control update, including duplicate values. |
| `/Dscomm/Control/Robot/IsCurrentOpModeTraceCorrect` | Boolean | Whether the robot-reported current op-mode trace matches the DS-selected op mode and enabled state. |
| `/Dscomm/Control/Robot/JoystickDescriptors` | `mrc.proto.ProtobufJoystickDescriptors` | Connected joystick metadata, published when joystick order or descriptors change. |
| `/Dscomm/Control/Robot/MatchInfo` | `mrc.proto.ProtobufMatchInfo` | Event and match metadata sent to the robot. |

### `/Dscomm/Control/Settings`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Control/Settings/TeamNumber` | String | Selected team number or robot address. |
| `/Dscomm/Control/Settings/RequireTeamNumberMatch` | Boolean | Whether robot connections must match the configured team number. |
| `/Dscomm/Control/Settings/UseWiFiStaticAddress` | Boolean | Whether to use the robot Wi-Fi static-address behavior. |
| `/Dscomm/Control/Settings/TimeZone` | String | Local DS time zone as an IANA time-zone ID, for example `America/New_York`. Falls back to `UTC`. |
| `/Dscomm/Control/Settings/DockedHeight` | Integer | Docked Driver Station window height. This key may be absent from logs; dashboard docked height is also available through the dashboard interface. |

### `/Dscomm/Control/Requests`

Integer request values are incrementing counters so repeated requests remain observable; the value is not a request argument. `ResetEStop` is a Boolean reset signal instead.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Control/Requests/RebootRobot` | Integer | User requested robot reboot. |
| `/Dscomm/Control/Requests/RestartRobotCode` | Integer | User requested robot-code restart. |
| `/Dscomm/Control/Requests/StopRobotCode` | Integer | User requested robot-code stop. |
| `/Dscomm/Control/Requests/ResetEStop` | Boolean | User requested E-stop reset. |
| `/Dscomm/Control/Requests/NewVersionRequest` | Integer | User requested robot version information. |

### `/Dscomm/Status/Connection`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Connection/UdpConnAddr` | Integer | Robot UDP connection address. `0` means disconnected; nonzero is the robot IP address as an integer. |
| `/Dscomm/Status/Connection/HasTcpConn` | Boolean | Whether the TCP robot connection is active. |
| `/Dscomm/Status/Connection/FmsTimes` | Integer | Packed FMS timing summary: bits 0-7 are trip time in milliseconds, bits 8-23 are lost packet count, and bits 24-39 are sent packet count. |

### `/Dscomm/Status/Robot`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Robot/HasUserCode` | Boolean | Whether robot code is present/running. |
| `/Dscomm/Status/Robot/HasUserCodeReady` | Boolean | Whether robot code has reported that it is ready. |
| `/Dscomm/Status/Robot/RequestDisable` | Boolean | Robot requested that the DS disable it. |
| `/Dscomm/Status/Robot/StatusWord` | Integer | Packed robot-reported status; see the bit layout below. This is distinct from the DS-requested `control_word` and the displayed `RobotState`. |
| `/Dscomm/Status/Robot/CurrentOpModeTrace` | Integer | Robot-reported current op-mode trace. Published every loop, including duplicates. |
| `/Dscomm/Status/Robot/RobotUpTime` | Double | Robot system uptime in seconds. |
| `/Dscomm/Status/Robot/RobotCodeUpTime` | Double | Robot-code uptime in seconds. |
| `/Dscomm/Status/Robot/IsSimulation` | Boolean | Whether the connection is to a simulated robot. |
| `/Dscomm/Status/Robot/IsUsb` | Boolean | Whether the connection is over USB. |
| `/Dscomm/Status/Robot/WatchdogNotFed` | Boolean | Whether the robot watchdog was not fed. |
| `/Dscomm/Status/Robot/ControlDataNotUpdating` | Boolean | Whether robot control data is not updating. |

#### `StatusWord` bit layout

Decode `/Dscomm/Status/Robot/StatusWord` with masks, not as an enum. Bits 0-7 contain the robot status byte; bits 8-9 contain the OnBot override flags.

| Bits | Mask | Meaning |
|---|---|---|
| 0-1 | `0x003` | Robot mode: `0` unknown, `1` autonomous, `2` teleoperated, `3` utility. |
| 2 | `0x004` | Robot reports enabled. |
| 3 | `0x008` | Program-start flag (`ProgStart`). |
| 4 | `0x010` | Brownout prevention active. |
| 5 | `0x020` | High-voltage battery. |
| 6 | `0x040` | Unassigned by the current protocol. |
| 7 | `0x080` | Robot reports E-stop. |
| 8 | `0x100` | OnBot override active. |
| 9 | `0x200` | OnBot override enabled. |
| 10-15 | | Unassigned by the current protocol. |

For example, `(value & 0x004) != 0` reads enabled, and `(value & 0x003) == 1` reads autonomous. A-stop has no bit in this word. `StatusWord` resets to `0` on robot disconnect; check connection topics when interpreting it.

### `/Dscomm/Status/Timing`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Timing/AveragePacketTime` | Float | Average robot communication packet time in microseconds. |
| `/Dscomm/Status/Timing/LostPackets` | Integer | Count of lost communication packets. |
| `/Dscomm/Status/Timing/LoopDelta` | Integer | Native control-loop period/delta in microseconds. |
| `/Dscomm/Status/Timing/LoopTime` | Integer | Native control-loop runtime in microseconds. |
| `/Dscomm/Status/Timing/UiLoopDelta` | Integer | UI loop period/delta in microseconds. |
| `/Dscomm/Status/Timing/UiLoopTime` | Integer | UI loop runtime in microseconds. |
| `/Dscomm/Status/Timing/PacketTime` | Integer | Last packet timing sample in microseconds. |
| `/Dscomm/Status/Timing/Rtt` | Integer | Minimum observed timesync round-trip time in microseconds for the current connection. The unsigned all-ones sentinel is exposed as `-1` in the signed NT integer until measured and on disconnect. |
| `/Dscomm/Status/Timing/DirectRtt` | Integer | Latest matching timesync round-trip time measurement in microseconds. |
| `/Dscomm/Status/Timing/ComputedDelay` | Integer | Robot-reported computed communication delay in microseconds (signed). |

### `/Dscomm/Status/Power`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Power/Battery` | Double | Robot battery voltage. |
| `/Dscomm/Status/Power/HighVoltageBattery` | Boolean | Whether the robot is reporting a high-voltage battery. |

### `/Dscomm/Status/Resources`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Resources/CPU` | Double | Robot CPU utilization percentage. |
| `/Dscomm/Status/Resources/FreeMemory` | Integer | Robot free memory in bytes. |
| `/Dscomm/Status/Resources/FreeStorage` | Integer | Robot free storage in bytes. |
| `/Dscomm/Status/Resources/CanBusUtilizations` | Float Array | CAN bus utilization percentages. |

### `/Dscomm/Status/Network`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Network/UdpErrorCode` | Integer | Packed UDP error data: bits 0-15 are the packet index (low byte first), bits 16-23 are the error code. Decode the code with `(value >> 16) & 0xFF`. |
| `/Dscomm/Status/Network/UdpSendErrorCode` | Integer | Latest UDP send error code. |
| `/Dscomm/Status/Network/OptimizedWlanError` | Integer | Latest optimized WLAN error code. |

### `/Dscomm/Status/Reporting`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Reporting/VersionInfo` | `mrc.proto.ProtobufVersionInfoSet` | Robot component versions. |
| `/Dscomm/Status/Reporting/ResolvedTeamNumbers` | `mrc.proto.ProtobufResolvedTeamNumberSet` | Team-number resolution results containing team number, hostname, and IP address entries. |

### `/Dscomm/Status/DriverStation`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/DriverStation/DsCpuUtilization` | Double | DS computer CPU utilization percentage. |
| `/Dscomm/Status/DriverStation/DsMemoryUtilization` | Double | DS computer memory utilization percentage. |
| `/Dscomm/Status/DriverStation/DsBattery` | Integer | DS computer battery percentage, or `-1` when not available. |
| `/Dscomm/Status/DriverStation/RobotState` | Integer | Robot state shown in the DS status area; see the values below. Published initially and when the state changes. |

#### `RobotState` values

These values describe the displayed Driver Station state, including connection, selection, and match conditions. They are separate from the robot-reported `StatusWord`.

| Value | State |
|---|---|
| 0 | `PressSpacebar` |
| 1 | `RequestingFmsLite` |
| 2 | `NoRadioConnection` |
| 3 | `NoRobotConnection` |
| 4 | `NoRobotCommunication` |
| 5 | `UdpError` |
| 6 | `NoRobotCode` |
| 7 | `OnBotOverrideEnabledDsEnabled` |
| 8 | `OnBotOverrideEnabledDsDisabled` |
| 9 | `OnBotOverrideEnabledDsEStopped` |
| 10 | `OnBotOverrideDisabledDsEnabled` |
| 11 | `OnBotOverrideDisabledDsDisabled` |
| 12 | `OnBotOverrideDisabledDsEStopped` |
| 13 | `OnBotOverrideUtilityEnabledDsEnabled` |
| 14 | `OnBotOverrideUtilityEnabledDsDisabled` |
| 15 | `OnBotOverrideUtilityDisabledDsEnabled` |
| 16 | `OnBotOverrideUtilityDisabledDsDisabled` |
| 17 | `WaitingForRobotCode` |
| 18 | `WaitingForOpModes` |
| 19 | `HeadRefEStopped` |
| 20 | `EmergencyStopped` |
| 21 | `RobotShowingEmergencyStopped` |
| 22 | `BrownedOut` |
| 23 | `WatchdogNotFed` |
| 24 | `SelectOpModes` |
| 25 | `SelectMatchTimes` |
| 26 | `SelectUtilityOpMode` |
| 27 | `SelectAutoOpMode` |
| 28 | `SelectTeleopOpMode` |
| 29 | `UnknownState` |
| 30 | `TeleoperatedEnabled` |
| 31 | `TeleoperatedDisabled` |
| 32 | `TeleoperatedCountdown` |
| 33 | `TeleoperatedWaiting` |
| 34 | `AutonomousEnabled` |
| 35 | `AutonomousDisabled` |
| 36 | `AutonomousBypassed` |
| 37 | `AutonomousCountdown` |
| 38 | `AutonomousStopped` |
| 39 | `UtilityEnabled` |
| 40 | `UtilityDisabled` |
| 41 | `UtilityCountdown` |

### `/Dscomm/Status/DriverStation/GarbageCollection`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/DriverStation/GarbageCollection/GcCount` | Integer | Total managed garbage-collection count. |
| `/Dscomm/Status/DriverStation/GarbageCollection/GcDeltaTimeUs` | Integer | Time spent in the latest garbage-collection interval, in microseconds. |
| `/Dscomm/Status/DriverStation/GarbageCollection/GcReason` | Integer | Runtime garbage-collection reason code. |
| `/Dscomm/Status/DriverStation/GarbageCollection/GcType` | Integer | Runtime garbage-collection type code. |

### `/Dscomm/Outputs`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Outputs/JoystickOutputs` | `mrc.proto.ProtobufJoystickOutputs` | Robot-to-DS joystick output requests such as LEDs and rumble. |

### `/Dscomm/Console`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Console/ConsoleLine` | `mrc.proto.ProtobufConsoleLineTimestamp` | Robot console output line with timestamp and sequence number. Published with duplicate values preserved. |
| `/Dscomm/Console/ErrorInfo` | `mrc.proto.ProtobufErrorInfoTimestamp` | Robot error or warning details with timestamp, sequence number, occurrence count, location, and call stack. |
| `/Dscomm/Console/ProgramCrashInfo` | `mrc.proto.ProtobufProgramCrashInfoTimestamp` | Robot program crash details, location, call stack, and timestamp. |

### `/Dscomm/Display`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Display/AnsiText` | String | ANSI display text from the robot. |

### `/Dscomm/Keyboard`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Keyboard/LostKeyboardPermissions` | Boolean | Indicates that keyboard permissions were lost and the DS should exit or request permissions. |
| `/Dscomm/Keyboard/EStopButtonPressed` | Boolean | E-stop keyboard shortcut state. |
| `/Dscomm/Keyboard/DisableButtonPressed` | Boolean | Disable keyboard shortcut state. |
| `/Dscomm/Keyboard/EnableButtonPressed` | Boolean | Enable keyboard shortcut state. |
| `/Dscomm/Keyboard/ReloadJoysticksButtonPressed` | Boolean | Joystick reload shortcut state. |
| `/Dscomm/Keyboard/ResetEstopButtonPressed` | Boolean | Reset E-stop shortcut state. |
| `/Dscomm/Keyboard/AStopButtonPressed` | Boolean | A-stop keyboard shortcut state. |

### `/Dscomm/Modes`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Modes/OpModeOptions` | `mrc.proto.ProtobufAvailableOpModes` | All available robot op modes. Each mode's hash metadata identifies it as autonomous, teleoperated, or utility. |

### `/Dscomm/Alerts`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Alerts/{group}/{level}/{id}/text` | String | Alert text. `group` is the alert group, `level` is an integer severity level, and `id` is the alert identifier. |
| `/Dscomm/Alerts/{group}/{level}/{id}/active` | Integer | Alert active timestamp. `0` means inactive; a nonzero value is the activation timestamp. |
