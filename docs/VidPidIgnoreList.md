# VID/PID Ignore List

The FIRST Driver Station allows you to exclude specific USB devices from being detected as joysticks/gamepads. This is useful for keyboards, mice, or other HID devices that the DS picks up as joystick inputs.

Every USB device is identified by two 16-bit numbers assigned by the USB Implementers Forum:

- **VID (Vendor ID)**: Identifies the manufacturer of the device (e.g., `05AC` is Apple).
- **PID (Product ID)**: Identifies the specific product from that manufacturer (e.g., `E3D8` for a particular keyboard model).

Together, a VID + PID pair uniquely identifies a specific product from a specific vendor.

## Finding a Device's VID and PID

You can look up the VID and PID of any connected USB device directly in the Driver Station. In the  gamepad view, right-click on the device and the VID and PID will be displayed.

## File Location

Place a file named `VidPidIgnoreList.json` in the DS configuration directory:

- **Windows**: `C:\Users\Public\Documents\FIRSTDriverStation\VidPidIgnoreList.json`
- **Unix (Linux/macOS)**: `~/.firstds/VidPidIgnoreList.json`

The file will be loaded automatically each time the Driver Station starts.

## File Format

The file must contain a valid JSON object with up to two optional keys:

- **`VidIgnoreList`**: An array of entries. Any USB device whose VID matches an entry in this list will be ignored entirely.
- **`IgnoreList`**: An array of entries. Only devices whose VID **and** PID both match an entry will be ignored.

Each entry contains:

| Field  | Required | Description |
|--------|----------|-------------|
| `Name` | Yes      | A human-readable label for the entry (for documentation purposes only). |
| `Vid`  | Yes      | The USB Vendor ID as a hexadecimal string (without the `0x` prefix). |
| `Pid`  | `IgnoreList` only | The USB Product ID as a hexadecimal string (without the `0x` prefix). |

> **Note:** VID and PID values must be hexadecimal strings and must **not** include a `0x` prefix (e.g., use `"05AC"`, not `"0x05AC"`).

## Example

```json
{
  "VidIgnoreList": [
    {
      "Name": "Apple USB Devices",
      "Vid": "05AC"
    }
  ],
  "IgnoreList": [
    {
      "Name": "GMMK 3 PRO HE Wireless (75%)",
      "Vid": "342D",
      "Pid": "E3D8"
    }
  ]
}
```

In this example:

- All USB devices with Vendor ID `05AC` (Apple) will be ignored.
- The specific device with Vendor ID `342D` and Product ID `E3D8` (GMMK 3 PRO HE Wireless) will be ignored.
