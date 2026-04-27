# Custom Gamepad Mappings

The FIRST Driver Station uses [SDL](https://www.libsdl.org/) for gamepad/joystick support. SDL includes built-in mappings for many common controllers, but you can provide custom mappings for controllers that are unrecognized or incorrectly mapped.

## Mapping File Location

Place a file named `GamepadMappings.txt` in the DS configuration directory:

- **Windows**: `C:\Users\Public\Documents\FIRSTDriverStation\GamepadMappings.txt`
- **Unix (Linux/macOS)**: `~/.firstds/GamepadMappings.txt`

The file will be loaded automatically each time the Driver Station starts.

## Mapping File Format

Each line in the file defines a mapping for one controller in SDL's gamepad mapping format:

```
<GUID>,<name>,<mapping entries>
```

- **GUID**: The SDL GUID for the controller (a 32-character hex string). You can find your controller's GUID using tools such as `sdl2-jstest` on Linux or the SDL2 gamepad tool.
- **name**: A human-readable name for the controller.
- **mapping entries**: A comma-separated list of `<button/axis>:<input>` pairs.

### Example

```
030000004c050000c405000000010000,PS4 Controller,a:b1,b:b2,back:b8,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,dpup:h0.1,guide:b12,leftshoulder:b4,leftstick:b10,lefttrigger:a3,leftx:a0,lefty:a1,rightshoulder:b5,rightstick:b11,righttrigger:a4,rightx:a2,righty:a5,start:b9,x:b0,y:b3,
```

## Further Reading

For full details on the SDL gamepad mapping format, see the official SDL documentation:

- [SDL_AddGamepadMappingsFromFile (SDL3)](https://wiki.libsdl.org/SDL3/SDL_AddGamepadMappingsFromFile)
- [SDL_GameControllerAddMappingsFromFile (SDL2)](https://wiki.libsdl.org/SDL2/SDL_GameControllerAddMappingsFromFile)

A community database of controller mappings is also available at [https://github.com/gabomdq/SDL_GameControllerDB](https://github.com/gabomdq/SDL_GameControllerDB), which can be a useful starting point for finding or contributing mappings for your controller.
