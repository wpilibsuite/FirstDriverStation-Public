# Logging Keys

This document describes the NT keys used for Driver Station logging. In Driver Station logs, NetworkTables entries are written with the `DS:` log prefix, so the topic `/Dscomm/Status/Battery` appears in the log as `DS:/Dscomm/Status/Battery`.

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

### `/Dscomm/Control`

These topics are published by the Driver Station UI and consumed by the native Driver Station communication layer.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Control/TeamNumber` | String | Selected team number or robot address. |
| `/Dscomm/Control/RequireTeamNumberMatch` | Boolean | Whether robot connections must match the configured team number. |
| `/Dscomm/Control/UseWiFiStaticAddress` | Boolean | Whether to use the robot Wi-Fi static-address behavior. |
| `/Dscomm/Control/ControlData` | `mrc.proto.ProtobufControlData` | Main DS-to-robot control packet. Published every control update, including duplicate values. |
| `/Dscomm/Control/JoystickDescriptors` | `mrc.proto.ProtobufJoystickDescriptors` | Connected joystick metadata, published when joystick order or descriptors change. |
| `/Dscomm/Control/MatchInfo` | `mrc.proto.ProtobufMatchInfo` | Event and match metadata sent to the robot. |
| `/Dscomm/Control/TimeZone` | String | Local DS time zone as an IANA time-zone ID, for example `America/New_York`. Falls back to `UTC`. |
| `/Dscomm/Control/RebootRobot` | Boolean | User requested robot reboot. |
| `/Dscomm/Control/RestartRobotCode` | Boolean | User requested robot-code restart. |
| `/Dscomm/Control/ResetEStop` | Boolean | User requested E-stop reset. |
| `/Dscomm/Control/IsCurrentOpModeTraceCorrect` | Boolean | Whether the robot-reported current op-mode trace matches the DS-selected op mode and enabled state. |
| `/Dscomm/Control/DockedHeight` | Integer | Docked Driver Station window height. Subscribed by the native backend. |

### `/Dscomm/Status`

These topics are published by the native Driver Station communication layer, except the DS utilization and UI loop timing topics, which are published by the UI.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Status/UdpConnAddr` | Integer | Robot UDP connection address. `0` means disconnected; nonzero is the robot IP address as an integer. |
| `/Dscomm/Status/HasTcpConn` | Boolean | Whether the TCP robot connection is active. |
| `/Dscomm/Status/HasUserCode` | Boolean | Whether robot code is present/running. |
| `/Dscomm/Status/HasUserCodeReady` | Boolean | Whether robot code has reported that it is ready. |
| `/Dscomm/Status/RequestDisable` | Boolean | Robot requested that the DS disable it. |
| `/Dscomm/Status/StatusByte` | Integer | Packed robot status byte. Bit `0x80` is E-stop, bit `0x40` is enabled, low mode bits identify autonomous/other mode state. |
| `/Dscomm/Status/CurrentOpModeTrace` | Integer | Robot-reported current op-mode trace. Published every loop, including duplicates. |
| `/Dscomm/Status/AveragePacketTime` | Float | Average robot communication packet time in microseconds. |
| `/Dscomm/Status/LostPackets` | Integer | Count of lost communication packets. |
| `/Dscomm/Status/LoopDelta` | Integer | Native control-loop period/delta in microseconds. |
| `/Dscomm/Status/LoopTime` | Integer | Native control-loop runtime in microseconds. |
| `/Dscomm/Status/UiLoopDelta` | Integer | UI loop period/delta in microseconds. |
| `/Dscomm/Status/UiLoopTime` | Integer | UI loop runtime in microseconds. |
| `/Dscomm/Status/PacketTime` | Integer | Last packet round-trip or packet timing sample in microseconds. |
| `/Dscomm/Status/Rtt` | Integer | Robot communication round-trip time. Defaults to the maximum unsigned 64-bit value until measured. |
| `/Dscomm/Status/DirectRtt` | Integer | Direct round-trip time measurement. |
| `/Dscomm/Status/ComputedDelay` | Integer | Computed communication delay. |
| `/Dscomm/Status/Battery` | Double | Robot battery voltage. |
| `/Dscomm/Status/HighVoltageBattery` | Boolean | Whether the robot is reporting a high-voltage battery. |
| `/Dscomm/Status/CPU` | Double | Robot CPU utilization percentage. |
| `/Dscomm/Status/FreeMemory` | Integer | Robot free memory in bytes. |
| `/Dscomm/Status/FreeStorage` | Integer | Robot free storage in bytes. |
| `/Dscomm/Status/RobotUpTime` | Double | Robot system uptime in seconds. |
| `/Dscomm/Status/RobotCodeUpTime` | Double | Robot-code uptime in seconds. |
| `/Dscomm/Status/IsSimulation` | Boolean | Whether the connection is to a simulated robot. |
| `/Dscomm/Status/IsUsb` | Boolean | Whether the connection is over USB. |
| `/Dscomm/Status/JoystickOutputs` | `mrc.proto.ProtobufJoystickOutputs` | Robot-to-DS joystick output requests such as LEDs and rumble. |
| `/Dscomm/Status/FmsTimes` | Integer | Packed FMS timing summary: sent packet count, lost packet count, and trip time in milliseconds. |
| `/Dscomm/Status/UdpErrorCode` | Integer | Packed UDP error data: low byte packet index, next byte packet index high, next byte error code. |
| `/Dscomm/Status/WatchdogNotFed` | Boolean | Whether the robot watchdog is active/not fed. |
| `/Dscomm/Status/ResolvedTeamNumbers` | `mrc.proto.ProtobufResolvedTeamNumberSet` | Team-number resolution results containing team number, hostname, and IP address entries. |
| `/Dscomm/Status/CanBusUtilizations` | Float Array | CAN bus utilization percentages. The DS UI currently reads up to five values. |
| `/Dscomm/Status/DsCpuUtilization` | Double | DS computer CPU utilization percentage. |
| `/Dscomm/Status/DsMemoryUtilization` | Double | DS computer memory utilization percentage. |
| `/Dscomm/Status/DsBattery` | Integer | DS computer battery percentage, or `-1` when not available. |

### `/Dscomm/Console`

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Console/ConsoleLine` | `mrc.proto.ProtobufConsoleLineTimestamp` | Robot console output line with timestamp and sequence number. Published with duplicate values preserved. |
| `/Dscomm/Console/ErrorInfo` | `mrc.proto.ProtobufErrorInfoTimestamp` | Robot error or warning details with timestamp, sequence number, occurrence count, location, and call stack. |
| `/Dscomm/Console/ProgramCrashInfo` | `mrc.proto.ProtobufProgramCrashInfoTimestamp` | Robot program crash details, location, call stack, and timestamp. |

### `/Dscomm/Keyboard`

These topics are published by the native keyboard/input layer and are consumed by the Driver Station UI. They are published with duplicate values preserved.

| Key | Type | Description |
|---|---|---|
| `/Dscomm/Keyboard/LostKeyboardPermissions` | Boolean | Indicates that keyboard permissions were lost and the DS should exit or request permissions. |
| `/Dscomm/Keyboard/EStopButtonPressed` | Boolean | E-stop keyboard shortcut state. |
| `/Dscomm/Keyboard/DisableButtonPressed` | Boolean | Disable keyboard shortcut state. |
| `/Dscomm/Keyboard/EnableButtonPressed` | Boolean | Enable keyboard shortcut state. |
| `/Dscomm/Keyboard/ReloadJoysticksButtonPressed` | Boolean | Joystick reload shortcut state. |
| `/Dscomm/Keyboard/ResetEstopButtonPressed` | Boolean | Reset E-stop shortcut state. |
| `/Dscomm/Keyboard/AStopButtonPressed` | Boolean | A-stop keyboard shortcut state. |

### Op Modes And Alerts

| Key | Type | Description |
|---|---|---|
| `/Dscomm/OpModeOptions` | `mrc.proto.ProtobufAvailableOpModes` | All available robot op modes. The DS separates them into autonomous, teleoperated, and utility modes by each mode's hash metadata. |
| `/Dscomm/Alerts/{group}/{level}/{id}/text` | String | Alert text. `group` is the alert group, `level` is an integer severity level, and `id` is the alert identifier. |
| `/Dscomm/Alerts/{group}/{level}/{id}/active` | Integer | Alert active timestamp. `0` means inactive; a nonzero value is the activation timestamp. |
