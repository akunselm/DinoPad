# DinoPad manual testing and troubleshooting

This is a **manual test plan**, not a report of tests already executed. It helps verify the included source code, firmware, and the real controller step by step.

Refer to the [hardware setup guide](HARDWARE_SETUP.md) for wiring and the [user guide](USER_GUIDE.md) for gameplay.

## A. Keyboard-only smoke test

- [ ] Open `DinoPad_Unity_Project - Copy` in Unity Hub.
- [ ] Load `Assets/DinoPadGame/Scenes/DinoPadGame.unity`.
- [ ] Disable **Connect On Start** in the **Dino Pad Uno Serial** component.
- [ ] Press **Play**; the runner field, controls, winning-score settings, and history panel appear.
- [ ] Press **Space** or **Up arrow** to start and jump.
- [ ] Hold **S** or **Down arrow** while grounded; the dinosaur ducks until released.
- [ ] Hold Jump; one press should not cause automatic repeated jumps.
- [ ] Release Jump, land, and press again; a new jump should occur.
- [ ] Use the on-screen **+ / −** buttons; the winning score should change in steps of 50.
- [ ] Press **CLEAR** to empty the recent input history.
- [ ] Hit an obstacle; confirm **GAME OVER** appears.
- [ ] Click **PLAY AGAIN**; confirm a new run starts.
- [ ] Confirm the history lists button, source, score, and elapsed time.

## B. Arduino serial test (without physical pads)

You need an Uno-compatible board, USB data cable, and Arduino IDE. Use a **jumper between the specific input pin and GND** to simulate each normally-open switch.

- [ ] Upload `DinoPad_Uno.ino` to the Uno.
- [ ] Open Serial Monitor at **115200 baud**.
- [ ] With D2 and D3 unconnected, expect `S,0,0` approximately every 120 ms.
- [ ] Jumper **D2 to GND**; after the 25 ms debounce interval, expect `S,1,0`.
- [ ] Release D2; expect `S,0,0`.
- [ ] Jumper **D3 to GND**; expect `S,0,1`.
- [ ] Jumper both inputs to GND; expect `S,1,1`.
- [ ] Release both inputs; expect `S,0,0`.
- [ ] Close Arduino Serial Monitor before using Unity.

**Safety:** Never connect the input pins to 5 V to simulate a press. The code enables internal pull-up resistors; a press is a connection from the input to GND. Avoid shorting 5 V directly to GND.

## C. Arduino-to-Unity integration

- [ ] Open the demo Unity scene and select **Dino Pad Game**.
- [ ] Confirm desktop **API Compatibility Level** is **.NET Framework**.
- [ ] Set **Port Name** to the Uno's real port (e.g., `COM4`); keep baud rate at 115200.
- [ ] Enable **Connect On Start**. Ensure Serial Monitor and Serial Plotter are closed.
- [ ] Enter Play mode and allow a few seconds for a possible Uno reset on port open.
- [ ] Wait for **UNO USB: Connected...**.
- [ ] Press the physical Jump pad or jumper D2 to GND. Check game reaction and `Arduino D2` history source.
- [ ] Hold Duck or jumper D3 to GND. Check held duck state and `Arduino D3` history source.
- [ ] Release D3. The dinosaur should stop ducking.
- [ ] Unplug the USB cable during Play mode. The serial status should change and any held pad state should release.
- [ ] Reconnect the board; the adapter should attempt to reconnect.

## D. UI and persistence checks

- [ ] Set a custom winning-score target using + / −.
- [ ] Stop and re-enter Play mode. Confirm the winning-score value is retained via Unity `PlayerPrefs`.
- [ ] Reach a win or collision state, record the best score, and start a new run.
- [ ] Confirm the personal best is retained once a run ends.
- [ ] Change **Last N Presses** in the Inspector to a small number, e.g., 3, and verify that only the newest three presses remain.
- [ ] Clear history; confirm the list empties.
- [ ] Check keyboard, mouse, and (where available) touch as independent input sources.

## Troubleshooting by symptom

| Symptom | What to check |
| --- | --- |
| Serial Monitor shows nothing | Correct board/port selected, correct firmware uploaded, data-capable USB cable, 115200 baud |
| `S,0,0` never changes | D2/D3 wiring, GND, actual switch contact; test with a safe jumper to GND |
| Input always shows pressed | Shorted or stuck switch, wrong wiring |
| Unity says port access denied | Close Arduino Serial Monitor, Plotter, and other serial programs |
| Unity says multiple ports | Replace `AUTO` with the exact device port in the Inspector |
| Unity opens port, no valid packets | Confirm matching firmware, 115200 baud, and the `S,0,0` packet format |
| `System.IO.Ports` errors | Check desktop platform and **.NET Framework** API Compatibility Level |
| Serial works but gameplay is unresponsive | Confirm the Dino Pad Uno Serial component's **Game** reference points to the DinoPadGame component |
| Keyboard works but Arduino doesn't | Isolate Arduino serial test (B), then integration test (C) |
| Game compiles but doesn't run in standalone build | Add the `DinoPadGame.unity` scene to Unity's build scene list; see [building](BUILDING.md) |
| Need to play without hardware | Disable **Connect On Start**; use keyboard, mouse, or touch |

## Test environment and results

| Test | Result |
| --- | --- |
| Source-level documentation review | Done; source and configuration inspected |
| Unity Editor gameplay smoke test | Not yet independently verified for this documentation |
| Arduino switch / serial test | Not yet independently verified for this documentation |
| End-to-end physical controller test | Not yet independently verified for this documentation |
| Standalone desktop build | Not yet independently verified for this documentation |

Update this table after testing on your hardware. Avoid marking unrun tests as passed.
