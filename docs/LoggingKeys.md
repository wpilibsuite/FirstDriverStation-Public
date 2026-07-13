# Logging Keys

This document describes the NT keys used for Driver Station logging. In Driver Station logs, NetworkTables entries are written with the `DS:` log prefix, so the topic `/Dscomm/Status/Power/Battery` appears in the log as `DS:/Dscomm/Status/Power/Battery`.

This document is scoped to DS comms: the C# Driver Station NetworkTables bridge and the native `DsServer` side. A local forwarding server mirrors the Driver Station instance on port `6767` for tools such as AdvantageScope.

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

These topics are published by the Driver Station UI and consumed by the native Driver Station communication layer.

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
| `/Dscomm/Control/Settings/DockedHeight` | Integer | Docked Driver Station window height. Subscribed by the native backend. |

### `/Dscomm/Control/Requests`

Request values are monotonically incremented by the UI so repeated requests remain observable.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Control/Requests/RebootRobot` | Integer | User requested robot reboot. |
| `/Dscomm/Control/Requests/RestartRobotCode` | Integer | User requested robot-code restart. |
| `/Dscomm/Control/Requests/ResetEStop` | Boolean | User requested E-stop reset. |
| `/Dscomm/Control/Requests/NewVersionRequest` | Integer | User requested robot version information. |

### `/Dscomm/Status/Connection`

These topics are published by the native Driver Station communication layer.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Connection/UdpConnAddr` | Integer | Robot UDP connection address. `0` means disconnected; nonzero is the robot IP address as an integer. |
| `/Dscomm/Status/Connection/HasTcpConn` | Boolean | Whether the TCP robot connection is active. |
| `/Dscomm/Status/Connection/FmsTimes` | Integer | Packed FMS timing summary: sent packet count, lost packet count, and trip time in milliseconds. |

### `/Dscomm/Status/Robot`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Robot/HasUserCode` | Boolean | Whether robot code is present/running. |
| `/Dscomm/Status/Robot/HasUserCodeReady` | Boolean | Whether robot code has reported that it is ready. |
| `/Dscomm/Status/Robot/RequestDisable` | Boolean | Robot requested that the DS disable it. |
| `/Dscomm/Status/Robot/StatusWord` | Integer | Robot status word. Bits 0-7 are the robot status byte, bit 8 is OnBot override active, and bit 9 is OnBot override enabled. |
| `/Dscomm/Status/Robot/CurrentOpModeTrace` | Integer | Robot-reported current op-mode trace. Published every loop, including duplicates. |
| `/Dscomm/Status/Robot/RobotUpTime` | Double | Robot system uptime in seconds. |
| `/Dscomm/Status/Robot/RobotCodeUpTime` | Double | Robot-code uptime in seconds. |
| `/Dscomm/Status/Robot/IsSimulation` | Boolean | Whether the connection is to a simulated robot. |
| `/Dscomm/Status/Robot/IsUsb` | Boolean | Whether the connection is over USB. |
| `/Dscomm/Status/Robot/WatchdogNotFed` | Boolean | Whether the robot watchdog was not fed. |
| `/Dscomm/Status/Robot/ControlDataNotUpdating` | Boolean | Whether robot control data is not updating. |

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
| `/Dscomm/Status/Timing/Rtt` | Integer | Robot communication round-trip time. Defaults to the maximum unsigned 64-bit value until measured. |
| `/Dscomm/Status/Timing/DirectRtt` | Integer | Direct round-trip time measurement. |
| `/Dscomm/Status/Timing/ComputedDelay` | Integer | Computed communication delay. |

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
| `/Dscomm/Status/Resources/CanBusUtilizations` | Float Array | CAN bus utilization percentages. The DS UI currently reads up to five values. |

### `/Dscomm/Status/Network`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Network/UdpErrorCode` | Integer | Packed UDP error data: low byte packet index, next byte packet index high, next byte error code. |
| `/Dscomm/Status/Network/UdpSendErrorCode` | Integer | Latest UDP send error code. |
| `/Dscomm/Status/Network/OptimizedWlanError` | Integer | Latest optimized WLAN error code. |

### `/Dscomm/Status/Reporting`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/Reporting/VersionInfo` | `mrc.proto.ProtobufVersionInfoSet` | Robot component versions. |
| `/Dscomm/Status/Reporting/ResolvedTeamNumbers` | `mrc.proto.ProtobufResolvedTeamNumberSet` | Team-number resolution results containing team number, hostname, and IP address entries. |

### `/Dscomm/Status/DriverStation`

These topics are published by the Driver Station UI.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/DriverStation/DsCpuUtilization` | Double | DS computer CPU utilization percentage. |
| `/Dscomm/Status/DriverStation/DsMemoryUtilization` | Double | DS computer memory utilization percentage. |
| `/Dscomm/Status/DriverStation/DsBattery` | Integer | DS computer battery percentage, or `-1` when not available. |

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

These topics are published by the native keyboard/input layer and consumed by the Driver Station UI. They are published with duplicate values preserved.

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
| `/Dscomm/Modes/OpModeOptions` | `mrc.proto.ProtobufAvailableOpModes` | All available robot op modes. The DS separates them into autonomous, teleoperated, and utility modes by each mode's hash metadata. |

### `/Dscomm/Alerts`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Alerts/{group}/{level}/{id}/text` | String | Alert text. `group` is the alert group, `level` is an integer severity level, and `id` is the alert identifier. |
| `/Dscomm/Alerts/{group}/{level}/{id}/active` | Integer | Alert active timestamp. `0` means inactive; a nonzero value is the activation timestamp. |
