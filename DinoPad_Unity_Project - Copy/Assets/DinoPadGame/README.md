# DinoPad Unity game files

This folder contains the game source and Arduino firmware for the two-pad Unity runner. For the complete project overview, installation, electrical wiring, and troubleshooting, see the [repository README](../../../README.md).

## Folder contents

| Path | Purpose |
| --- | --- |
| Scripts/DinoPadGame.cs | Runner gameplay, drawing, scoring, goal, and last-N input history |
| Scripts/DinoPadUnoSerial.cs | Arduino Uno USB serial input, connection status, and reconnect handling |
| Arduino/DinoPad_Uno.ino | Two-switch Arduino firmware |
| Scenes/DinoPadGame.unity | Preconfigured demo scene |
| Editor/DinoPadSceneTools.cs | Optional editor menu commands |

## Open the checked-in Unity project

1. Open **DinoPad_Unity_Project - Copy** (the parent Unity project folder) in Unity Hub.
2. Open **Assets/DinoPadGame/Scenes/DinoPadGame.unity**.
3. For Arduino serial, set **API Compatibility Level = .NET Framework**, and enter the Uno port in the scene object's **Dino Pad Uno Serial** component.
4. For keyboard-only play, disable **Connect On Start** and use Space/W/Up for Jump and S/Down for Duck.

The Arduino sketch uses D2 for Jump, D3 for Duck, **115200 baud**, and newline-terminated **S,jump,duck** messages (1 = pressed). The switches close to GND; internal pull-ups are enabled. Close Arduino Serial Monitor before running Unity.

This repository contains a **Unity source project**, not an exported Unity package. Open the parent Unity folder directly.
