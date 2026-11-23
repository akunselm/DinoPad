# DinoPad — Unity assets

This folder contains the **Unity gameplay scripts, demonstration scene, and Arduino Uno firmware** for DinoPad, a dinosaur runner driven by two physical Jump/Duck pads over USB serial.

Start at the [**repository README**](../../../README.md) for a screenshot, quick start, and complete documentation index.

## Files

| Path | Purpose |
| --- | --- |
| `Scripts/DinoPadGame.cs` | Runner gameplay, UI rendering, scoring, goal persistence, recent input history |
| `Scripts/DinoPadUnoSerial.cs` | USB serial adapter with background reading, packet validation, disconnect handling |
| `Arduino/DinoPad_Uno.ino` | Arduino firmware for two switches and `S,jump,duck` messages |
| `Scenes/DinoPadGame.unity` | Included playable demo scene |
| `Editor/DinoPadSceneTools.cs` | Optional Unity **Tools → Dino Pad** scene commands |

## Run the included demo

1. Open the inner Unity project folder `DinoPad_Unity_Project - Copy` in Unity Hub.
2. Open `Assets/DinoPadGame/Scenes/DinoPadGame.unity`.
3. To play **without** an Arduino, disable **Connect On Start** on the **Dino Pad Uno Serial** component.
4. Press **Play**. Use **Space/W/Up** to Jump and **S/Down** to Duck.

For physical pads, upload `Arduino/DinoPad_Uno.ino`, connect switches from **D2 and D3 to GND**, and set the serial port at **115200 baud**. For desktop serial support, use **.NET Framework** API compatibility. Close Arduino Serial Monitor before playing.

**Winning-score defaults:** the included demo scene has **2,000** selected, the component's script fallback is **500** for a newly created scene, and any previously saved goal overrides the scene selection.

## Technical guides

- [User guide](../../../docs/USER_GUIDE.md)
- [Hardware wiring and serial setup](../../../docs/HARDWARE_SETUP.md)
- [Architecture and protocol](../../../docs/SYSTEM_ARCHITECTURE.md)
- [Testing and troubleshooting](../../../docs/TESTING.md)
- [Standalone build setup](../../../docs/BUILDING.md)

**Important:** The project is source-only. The checked-in build scene list is empty, so add the demo scene in Unity **Build Profiles** before exporting a desktop executable. WebGL is not supported by this serial input path.
