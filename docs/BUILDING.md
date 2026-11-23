# Building DinoPad for desktop

DinoPad is distributed as **Unity 6 source code**, not as a downloadable executable. The supported serial-input implementation is intended for the Unity Editor and desktop standalone builds, **not WebGL**.

> **Important:** The checked-in `ProjectSettings/EditorBuildSettings.asset` currently contains an **empty scene list**. You must add the game scene before building a standalone executable.

## Prerequisites

- Unity Hub and a compatible Unity 6 Editor (the project records **6000.6.6f1**)
- Desktop build support for your target platform
- A clone/download of this repository
- Optional: Arduino Uno and two normally-open switches for hardware input

## Run the game in the Unity Editor

1. Open the inner project folder `DinoPad_Unity_Project - Copy` in Unity Hub.
2. Open `Assets/DinoPadGame/Scenes/DinoPadGame.unity`.
3. For keyboard-only play, disable **Connect On Start** on the **Dino Pad Uno Serial** component.
4. Press **Play**.

## Prepare a standalone build

1. In Unity, open **File → Build Profiles** (Unity 6). Depending on Unity version, scene management can also appear under **Build Settings**.
2. Choose your desktop build target and ensure its build-support module is installed.
3. Add `Assets/DinoPadGame/Scenes/DinoPadGame.unity` to the list of included scenes. Keep it enabled.
4. If using the Uno over USB, set **Project Settings → Player → Other Settings → API Compatibility Level** to **.NET Framework** for desktop `System.IO.Ports` support.
5. Select the **Dino Pad Game** object. Set **Port Name** to the device name on the target computer, or use `AUTO` only when exactly one serial port is detected. Verify **Baud Rate = 115200**.
6. For a keyboard-only build, disable **Connect On Start** before building.
7. Choose **Build**, select a destination, and run the resulting program.

**Do not expect the default project to produce a playable build without step 3.** No standalone build is included or independently verified with this documentation.

## Hardware notes

The Arduino Uno is a **USB serial device**, not a keyboard emulator. Desktop hardware play requires the Uno sketch, a working serial port, and the corresponding device path. A Windows `COM` port name won't carry over to another computer or operating system.

If the build works with the keyboard but not the controller, follow the [hardware setup](HARDWARE_SETUP.md) and [manual test checklist](TESTING.md).

## Current limitations

- No included `.exe`, macOS application, Linux binary, or Unity package.
- No WebGL/browser serial input through the supplied `System.IO.Ports` path.
- No 3D-printable pad enclosure or mechanical files in this repository yet.
- No automated release/build pipeline.
- Actual output builds need testing on the platform and hardware where they will run.
