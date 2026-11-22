# DinoPad hardware and serial setup

DinoPad uses two digital inputs on an **Arduino Uno R3** to control a Unity game. No additional Arduino libraries are required by the supplied sketch.

## Wiring the switches

Use **normally-open** switches or pressure pads. Each switch closes a connection between its Arduino pin and **GND** when pressed.

~~~text
                            Arduino Uno
                         ┌────────────────┐
          Jump contact ──┤ D2             │
          Duck contact ──┤ D3             │
         Shared ground ──┤ GND            │
                         │            USB ├──── computer
                         └────────────────┘

    Jump: D2 ─────[normally-open switch]───── GND
    Duck: D3 ─────[normally-open switch]───── GND
~~~

A common ground can be shared between the two switches. The sketch configures **INPUT_PULLUP**, so the pins are internally held high at rest and read **LOW when pressed**. External resistors aren't needed for this arrangement.

**Electrical check:** Do not connect the switches across 5 V and a digital input; this wiring expects each switch to connect its input to ground. Make sure your pads do not create a short between 5 V and GND.

## Upload firmware

1. Connect the Arduino Uno using a **data-capable USB cable**.
2. Open [DinoPad_Uno.ino](../DinoPad_Unity_Project%20-%20Copy/Assets/DinoPadGame/Arduino/DinoPad_Uno.ino) in Arduino IDE 2.
3. Select **Arduino Uno** and the correct serial port under Tools.
4. Upload the sketch.
5. Open Serial Monitor and set **115200 baud**.

## Verify the serial protocol

The firmware prints a line when the pressed states change and sends a heartbeat roughly every **120 ms**.

| Serial line | Jump (D2) | Duck (D3) |
| --- | --- | --- |
| S,0,0 | Released | Released |
| S,1,0 | Pressed | Released |
| S,0,1 | Released | Pressed |
| S,1,1 | Pressed | Pressed |

The sketch uses approximately **25 ms debounce** to reject brief contact bounce. A single pressed switch should produce a stable 1 for its corresponding field.

**Close the Arduino Serial Monitor before entering Play mode in Unity.** Most operating systems allow only one program to open the same port at a time.

## Connect Unity

1. Open the Unity project folder **DinoPad_Unity_Project - Copy** in Unity Hub.
2. Set **Player → Other Settings → API Compatibility Level** to **.NET Framework**.
3. Open **Assets/DinoPadGame/Scenes/DinoPadGame.unity**.
4. Select the **Dino Pad Game** object, then find the **Dino Pad Uno Serial** component.
5. Set **Port Name** to your Uno's port (for example, COM4), keep **Baud Rate = 115200**, and enable **Connect On Start**.
6. Press **Play**. The on-screen **UNO USB** label reports the connection state.

**AUTO** works only if exactly one serial port is available. Enter the exact port when multiple devices are connected.

## Troubleshooting checklist

- **No serial output?** Check board model, firmware upload, selected port, and USB data cable.
- **Pad stuck pressed?** Check for an unintended short to GND or physically stuck contact.
- **Intermittent signal?** Inspect contacts/wiring and verify a stable input in Serial Monitor.
- **Arduino works in Serial Monitor but not Unity?** Close Serial Monitor and double-check the port and .NET Framework compatibility.
- **Unity appears connected but ignores a pad?** Briefly jumper D2 or D3 to GND to test the Arduino input independently of the physical pad.
- **Can't use hardware?** Turn off **Connect On Start** and use Space/W/Up to jump or S/Down to duck.

The repository does not yet include 3D-printable enclosure files or a documented pad mechanical assembly. The electrical interface above is sufficient for a basic switch-based prototype.
