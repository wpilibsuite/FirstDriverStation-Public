# Gamepad Locking

The FIRST Driver Station allows you to lock a gamepad to a specific slot using the **Locked** checkbox next to each joystick slot. When a gamepad is locked, the DS will remember the slot assignment and automatically reassign the gamepad to that same slot the next time it is connected or the Driver Station is relaunched.

## How to Lock a Gamepad

Check the **Locked** checkbox next to the joystick slot where you want to keep the gamepad. The DS will save this assignment and use it on subsequent connections.

## Unique vs. Non-Unique Gamepads

Whether locking works reliably depends on whether the gamepad is *uniquely identifiable*.

- **Uniquely identifiable gamepad**: The DS can tell it apart from all other connected controllers (for example, because it has a unique serial number). If a gamepad is uniquely identifiable, a **fingerprint icon** will appear next to the Locked checkbox. For these gamepads, the correct slot assignment is guaranteed—even if multiple controllers of the same model are connected at the same time.

- **Non-unique gamepad**: The DS cannot distinguish this controller from other controllers of the same type. No fingerprint icon will be shown. Locking still works when only **one** gamepad of that type is connected; the DS will match it to the locked slot as the sole controller of that type. However, if **two or more** non-unique gamepads of the same type are connected, the order in which they are assigned to locked slots is random, so you may not get the expected slot assignment.

## Summary

| Scenario | Locked slot behavior |
|---|---|
| Uniquely identifiable gamepad | Always assigned to the correct locked slot |
| Only one gamepad of its type connected (non-unique) | Assigned to the correct locked slot |
| Multiple gamepads of the same type connected (non-unique) | Slot assignment order is random between locked slots |
