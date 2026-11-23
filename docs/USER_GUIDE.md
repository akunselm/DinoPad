# DinoPad user guide

DinoPad is a Unity desktop dinosaur runner with **two controls: Jump and Duck**. It works with an Arduino two-pad controller or with keyboard, mouse, and touch inputs.

**First time here?** Start at the [project README](../README.md) for a short setup guide.

## Start playing

1. Clone or download the repository.
2. In Unity Hub, select **Add project from disk** and open the inner `DinoPad_Unity_Project - Copy` folder (the folder containing `Assets`, `Packages`, and `ProjectSettings`).
3. Open with Unity **6000.6.6f1**, the version recorded by the project, or a compatible Unity 6 Editor.
4. Open `Assets/DinoPadGame/Scenes/DinoPadGame.unity`.
5. For keyboard-only play, select the **Dino Pad Game** object and disable **Connect On Start** on its **Dino Pad Uno Serial** component.
6. Press **Play** in the Unity Editor.

No physical controller is needed to try the game. The repository does not currently include a pre-built executable.

## Controls

| Action | Keyboard | On-screen | Arduino |
| --- | --- | --- | --- |
| Start / jump | Space, W, Up arrow | JUMP pad or START button | Jump switch on D2 |
| Duck | S, Down arrow (hold) | DUCK pad (hold) | Duck switch on D3 (hold) |
| Jump while running | Same keys | JUMP pad, or click the play field | Jump switch on D2 |
| Retry | Jump or PLAY AGAIN button | PLAY AGAIN | Jump switch on D2 |
| Adjust win target | — | − / + buttons beside WINNING SCORE | — |
| Clear press history | — | CLEAR in history panel | — |

Jump fires on a **new press**. Holding it does not repeat jumps automatically. Jumping is possible only while on the ground and not ducking. Duck remains active while held, lowering the dinosaur while it is on the ground.

Mouse and touch inputs work with the on-screen pads; keep the pointer/finger on DUCK to hold it.

## Gameplay and score

Avoid cacti and birds as the game gradually increases speed. The run ends when you hit an obstacle (**GAME OVER**) or reach the winning score (**YOU WIN!**). Use **PLAY AGAIN** or Jump to start another run.

The game tracks your score and a personal best. Best score is written to Unity `PlayerPrefs` when a run ends.

### Winning score: why you may see 2,000 instead of 500

The **checked-in demo scene sets Winning Score to 2,000**. The C# component's initial value of **500** is the fallback for a freshly added component without that scene override.

A *previously saved* goal overrides the scene value on startup. The **+ / −** controls change the goal in increments of **50** (unless `Goal Step` was changed in the Inspector); valid goals range from **50 to 100,000**. New choices are saved with `PlayerPrefs`.

### Recent inputs

The demonstration scene keeps the **last 10 button presses**; the Inspector allows a history size between **1 and 100**. Every entry records:

- JUMP or DUCK
- Input source, such as `Arduino D2` or `Keyboard / Touch`
- Score when the button was pressed
- Seconds since the **Unity scene loaded**, not since the most recent run began

The list stores new presses, not repeated entries for a held button. It persists across in-scene restarts until cleared or the scene is reloaded. Enable **Print Presses To Console** in the Inspector to log events.

## Unity Inspector reference

Select **Dino Pad Game** in the Hierarchy:

| Component | Property | Included scene value |
| --- | --- | --- |
| DinoPad Game | Winning Score | 2,000 |
| DinoPad Game | Goal Step | 50 |
| DinoPad Game | Last N Presses | 10 |
| DinoPad Game | Show Button History | Enabled |
| DinoPad Game | Starting / Maximum Speed | 380 / 680 |
| Dino Pad Uno Serial | Port Name | `AUTO` |
| Dino Pad Uno Serial | Baud Rate | 115200 |
| Dino Pad Uno Serial | Connect On Start | Enabled |

The code targets 60 frames per second, but actual performance depends on the computer.

## Arduino connection indicators

The `UNO USB` status reflects the serial reader:

- **Connected - jump=0 duck=0:** valid packets are arriving and neither button is pressed.
- **Waiting for Uno...:** the port opened, but the reader hasn't yet received a valid packet; opening the port can reset the Uno.
- **No serial ports found:** check the board and USB data cable.
- **Multiple ports:** choose the Uno's actual device name in **Port Name** instead of using `AUTO`.
- **Serial issue:** check the port, close Arduino Serial Monitor, and verify the firmware and baud rate.
- **Serial disabled:** keyboard, mouse, and touch remain playable.

The serial input reconnects on errors and releases held button state when communication is lost to avoid a stuck Duck command.

## More information

- [Hardware wiring and serial setup](HARDWARE_SETUP.md)
- [Technical architecture](SYSTEM_ARCHITECTURE.md)
- [Manual test checklist and troubleshooting](TESTING.md)
- [Making a standalone desktop build](BUILDING.md)
