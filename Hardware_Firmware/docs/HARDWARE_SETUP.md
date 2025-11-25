# Hardware Setup Guide

**CMM Arm - Firmware v1.0.2**

Complete hardware assembly, wiring, and bill of materials for building a 4-axis coordinate measuring machine (CMM) arm with Arduino control.

---

## 📋 Table of Contents

- [Bill of Materials (BOM)](#bill-of-materials-bom)
- [Tools Required](#tools-required)
- [Pin Assignment Reference](#pin-assignment-reference)
- [Step-by-Step Wiring](#step-by-step-wiring)
- [Power Supply Setup](#power-supply-setup)
- [Encoder Mounting](#encoder-mounting)
- [Cable Management](#cable-management)
- [Testing Procedure](#testing-procedure)
- [Calibration](#calibration)

---

## 🛒 Bill of Materials (BOM)

### Core Components

| Qty | Item | Specification | Notes | Est. Cost |
|-----|------|--------------|-------|-----------|
| 1 | Arduino Mega 2560 | Genuine or compatible | Must be Mega, not Uno | $15-40 |
| 4 | Rotary Encoders | 600 PPR, quadrature (A/B), 5V | Incremental, not absolute | $10-25 each |
| 1 | USB Cable | Type-B to Type-A | For programming & communication | $5-10 |
| 1 | Power Supply | 9-12V DC, 1A+ | External power recommended | $10-15 |

**Total Core Cost: ~$90-175**

### Encoder Specifications (Critical)

Your encoders MUST have:
- ✅ **Quadrature output** (channels A and B, 90° phase shift)
- ✅ **5V compatible** (or use level shifters for 3.3V)
- ✅ **Incremental type** (not absolute)
- ✅ **Minimum 100 PPR** (600 PPR recommended, up to 4096 supported)

**Recommended Encoder Models:**
- CUI AMT103-V (600 CPR)
- E6B2-CWZ series (various PPR)
- Omron E6B2-CWZ series
- TRD-J series (various PPR)

### Wiring Components

| Qty | Item | Specification | Notes |
|-----|------|--------------|-------|
| 8 | Encoder Cables | 4-conductor, 22-24 AWG | Shielded recommended for >1m runs |
| 1 | Breadboard | Half-size or larger | Optional - for testing/prototyping |
| 20+ | Jumper Wires | Male-to-male, male-to-female | For breadboard connections |
| 1 | Hookup Wire | 22 AWG solid/stranded | For permanent connections |
| 1 | Heat Shrink Tubing | Assorted sizes | For connection insulation |

### Optional Components

| Qty | Item | Purpose |
|-----|------|---------|
| 4 | 0.1µF Capacitors | Noise filtering across encoder power |
| 1 | Project Enclosure | Protect Arduino and connections |
| 4 | DB9 or M12 Connectors | Professional encoder connections |
| 1 | Panel Mount USB Extension | Extend USB port to enclosure |

---

## 🔧 Tools Required

### Essential Tools

- **Soldering iron** and solder (for permanent connections)
- **Wire strippers** (22-24 AWG)
- **Multimeter** (continuity testing, voltage measurement)
- **Small screwdrivers** (Phillips and flathead)
- **Diagonal cutters** (wire cutting)

### Recommended Tools

- **Heat gun** (for heat shrink tubing)
- **Crimping tool** (for connector pins)
- **Oscilloscope** (encoder signal verification - optional but helpful)
- **Label maker** (wire identification)

---

## 📍 Pin Assignment Reference

### Complete Pin Map

```
╔════════════════════════════════════════════════════════════╗
║        Arduino Mega 2560 - CMM Arm Pins        ║
╠════════════════════════════════════════════════════════════╣
║                                                            ║
║  ENCODER 1 (Base Rotation)                                 ║
║  ├─ Channel A → Pin 2  (INT0 - Hardware Interrupt)        ║
║  └─ Channel B → Pin 3  (INT1 - Hardware Interrupt)        ║
║                                                            ║
║  ENCODER 2 (Shoulder Pitch)                                ║
║  ├─ Channel A → Pin 18 (INT5 - Hardware Interrupt)        ║
║  └─ Channel B → Pin 19 (INT4 - Hardware Interrupt)        ║
║                                                            ║
║  ENCODER 3 (Elbow Pitch)                                   ║
║  ├─ Channel A → Pin 20 (INT3 - Hardware Interrupt)        ║
║  └─ Channel B → Pin 21 (INT2 - Hardware Interrupt)        ║
║                                                            ║
║  ENCODER 4 (Wrist Pitch)                                   ║
║  ├─ Channel A → Pin 22 (PCINT - Pin Change Interrupt)     ║
║  └─ Channel B → Pin 23 (PCINT - Pin Change Interrupt)     ║
║                                                            ║
║  POWER (All Encoders)                                      ║
║  ├─ Vcc → 5V                                               ║
║  └─ GND → GND                                              ║
║                                                            ║
║  COMMUNICATION                                             ║
║  └─ USB Type-B → PC (programming & serial data)           ║
║                                                            ║
║  EXTERNAL POWER (Optional but recommended)                 ║
║  └─ Barrel Jack → 9-12V DC, 1A+                           ║
║                                                            ║
╚════════════════════════════════════════════════════════════╝
```

### Why These Specific Pins?

**Hardware Interrupts (Pins 2, 3, 18, 19, 20, 21):**
- Provide instant response to encoder edges
- No polling delay = no missed counts
- Critical for accurate position tracking at high speeds

**Pin Change Interrupts (Pins 22, 23):**
- Used for 4th encoder (Mega has only 6 hardware interrupts)
- Still very responsive, minimal difference from hardware interrupts

**Cannot Use Arduino Uno:**
- Uno has only 2 hardware interrupt pins (pins 2 & 3)
- Would require software polling for encoders 2-4 = missed counts
- Mega 2560 is required for reliable 4-encoder operation

---

## 🔌 Step-by-Step Wiring

### Encoder Pin Identification

First, identify your encoder pins. Typical pinouts:

**Common 5-Pin Encoder:**
```
Encoder Connector (Front View)
┌─────────────────┐
│  ●   ●   ●   ●  │
│ Vcc  A   B  GND │
└─────────────────┘
```

**Some encoders include:**
- **Index/Z channel** (not used in this firmware, leave disconnected)
- **Shield** (connect to GND if present)

Check your encoder datasheet for exact pinout!

### Wiring Encoder 1 (Base Rotation)

**Target Pins:** 2 (A), 3 (B)

1. **Strip 5mm of insulation** from encoder cable wires
2. **Tin wire ends** with solder for solid connection
3. **Connect to Arduino:**
   - Encoder Vcc → Arduino 5V
   - Encoder GND → Arduino GND
   - Encoder Channel A → Arduino Pin 2
   - Encoder Channel B → Arduino Pin 3
4. **Verify continuity** with multimeter (each connection should beep)
5. **Add heat shrink** over exposed connections

**Connection Diagram:**
```
Encoder 1                    Arduino Mega 2560
┌──────────┐                ┌─────────────────┐
│          │                │                 │
│  Vcc  ●──┼────[red]───────┤ 5V              │
│          │                │                 │
│  GND  ●──┼───[black]──────┤ GND             │
│          │                │                 │
│  Ch A ●──┼───[green]──────┤ Pin 2 (INT0)    │
│          │                │                 │
│  Ch B ●──┼───[blue]───────┤ Pin 3 (INT1)    │
│          │                │                 │
└──────────┘                └─────────────────┘
```

### Wiring Encoder 2 (Shoulder Pitch)

**Target Pins:** 18 (A), 19 (B)

Follow same procedure as Encoder 1, connecting to:
- Channel A → Pin 18
- Channel B → Pin 19
- Vcc → 5V (can share with other encoders)
- GND → GND (can share with other encoders)

### Wiring Encoder 3 (Elbow Pitch)

**Target Pins:** 20 (A), 21 (B)

Follow same procedure, connecting to:
- Channel A → Pin 20
- Channel B → Pin 21
- Vcc → 5V
- GND → GND

### Wiring Encoder 4 (Wrist Pitch)

**Target Pins:** 22 (A), 23 (B)

Follow same procedure, connecting to:
- Channel A → Pin 22
- Channel B → Pin 23
- Vcc → 5V
- GND → GND

### Common Ground Connection

**CRITICAL:** All encoders must share common ground with Arduino!

**Recommended approach:**
1. Connect all encoder GND wires together
2. Connect this common GND to Arduino GND with single wire
3. Use a terminal block or breadboard bus for clean distribution

**Star Ground Configuration:**
```
        Arduino GND
             │
        ┌────┴────┐
        │ Terminal│
        │  Block  │
        └────┬────┘
    ┌────┼────┼────┐
    │    │    │    │
  Enc1 Enc2 Enc3 Enc4
```

---

## ⚡ Power Supply Setup

### Option 1: USB Power Only

**Suitable for:**
- Testing and development
- 1-2 low-current encoders
- Short cable runs

**Limitations:**
- USB current limit: ~500mA total
- May cause voltage drop with 4 encoders
- Can cause erratic encoder behavior

**Setup:**
1. Connect USB cable only
2. No external power supply needed
3. Monitor for stability issues

### Option 2: External Power (Recommended)

**Suitable for:**
- Production use
- All 4 encoders
- Maximum stability

**Advantages:**
- More current capacity (1A+)
- Cleaner power = fewer errors
- USB still available for communication

**Setup:**
1. **Select appropriate supply:** 9-12V DC, 1A minimum
2. **Verify polarity:** Center-positive (standard)
3. **Connect to barrel jack** on Arduino
4. **USB remains connected** for serial communication

**Power Distribution:**
```
External Supply (9-12V DC)
        │
        ▼
    Barrel Jack
        │
        ▼
  Arduino Mega 2560
        │
  ┌─────┴──────┐
  │  5V  Reg   │  (On-board voltage regulator)
  └─────┬──────┘
        │
        ▼
     5V Pin ────┬────┬────┬──── To all encoders
                │    │    │
              Enc1 Enc2 Enc3 Enc4
```

### Adding Noise Filtering (Optional but Recommended)

For cleanest operation, add 0.1µF ceramic capacitors:

**Placement:**
- Across each encoder's Vcc and GND pins
- As close to encoder as possible
- Reduces electrical noise and false counts

```
Encoder Power Connection
        5V ──┬───● Encoder Vcc
             │
          [0.1µF]  (Ceramic capacitor)
             │
       GND ──┴───● Encoder GND
```

---

## 🔩 Encoder Mounting

### Mechanical Considerations

**Shaft Coupling:**
- Use flexible couplers to accommodate minor misalignment
- Avoid rigid couplers (causes binding and encoder damage)
- Ensure zero backlash for accurate measurements

**Bearing Support:**
- Encoders should NOT support mechanical loads
- Use bearings to support arm segments
- Encoder only senses rotation

**Alignment:**
- Encoder shaft must be concentric with rotation axis
- Misalignment causes oscillating readings
- Use alignment tools or dial indicators

### Mounting Best Practices

1. **Secure firmly** to prevent movement relative to arm segment
2. **Protect from contaminants** (dust, oil, coolant)
3. **Route cables** with strain relief to prevent wire fatigue
4. **Allow thermal expansion** (don't over-constrain)
5. **Protect from impacts** (encoders are fragile)

---

## 🧵 Cable Management

### Cable Routing

**Goals:**
- Prevent cable fatigue and breakage
- Minimize electrical noise pickup
- Allow full range of motion
- Maintain professional appearance

**Best Practices:**

1. **Use Shielded Cable** for runs >1 meter
   - Connect shield to GND at Arduino end only
   - Prevents ground loops

2. **Provide Strain Relief**
   - Cable ties every 150mm
   - Avoid tight bends (min radius: 10x cable diameter)
   - Use spiral cable wrap or cable chain

3. **Separate Power and Signal**
   - Keep encoder cables away from motors/high voltage
   - Use separate wire bundles if possible

4. **Label Everything**
   - Label both ends of each wire
   - Use: "ENC1-A", "ENC1-B", "ENC1-5V", "ENC1-GND"
   - Saves hours during troubleshooting

### Professional Cable Assembly

**For permanent installation:**

1. **Use Connectors**
   - DB9, M12, or aviation connectors
   - Allows easy encoder replacement
   - Professional appearance

2. **Crimp, Don't Solder** (for connectors)
   - More reliable long-term
   - Prevents cold solder joints
   - Use proper crimping tool

3. **Environmental Protection**
   - Use cord grips for enclosure entry
   - Seal unused connector pins
   - Consider IP65+ connectors for harsh environments

---

## 🧪 Testing Procedure

### Step 1: Visual Inspection

Before powering on:

- ✅ Verify all connections are solid (no loose wires)
- ✅ Check for shorts (especially Vcc to GND)
- ✅ Ensure proper polarity (Vcc to 5V, not GND)
- ✅ Verify no bare wires touching metal parts

### Step 2: Power-On Test

1. **Connect USB only** (no external power yet)
2. **Observe Arduino power LED** (should be solid, not flickering)
3. **Measure voltage at encoder Vcc pins:** Should read 5.0V ± 0.25V
4. If voltage low (<4.75V): Too much current draw, use external power

### Step 3: Firmware Upload Test

1. Open Arduino IDE
2. Load CMM_Arm_Arduino.ino
3. **Tools → Board → Arduino Mega 2560**
4. **Tools → Port → [Your COM port]**
5. **Upload** firmware
6. Wait for "Done uploading"

### Step 4: Serial Communication Test

1. **Tools → Serial Monitor**
2. Set to **115200 baud**, **Newline**
3. Should see startup message:
   ```
   =====================================
   4-Axis CMM Arm
   Firmware Version: 1.0.2
   Date: 2025-11-20
   =====================================
   Ready for commands
   ```
4. Type `VERSION` → Should respond: `VERSION,1.0.2,2025-11-20`

✅ **Success**: Communication working

### Step 5: Individual Encoder Test

Enable debug mode to test encoders one at a time:

1. **Edit config.h:**
   ```cpp
   #define DEBUG_ENCODERS true
   ```

2. **Re-upload firmware**

3. **Open Serial Monitor**

4. **Manually rotate Encoder 1** (slowly, one direction)
   - Should see count increasing or decreasing
   - Numbers should change smoothly, no jumps

5. **Test each encoder** individually

6. **Verify direction:**
   - Rotate clockwise → count should increase
   - If decreases, flip `ENCODER_X_DIRECTION` in config.h

7. **Disable debug mode** when done:
   ```cpp
   #define DEBUG_ENCODERS false
   ```

### Step 6: Full System Test

1. **Disable all debug modes** in config.h
2. **Re-upload firmware**
3. **Position arm at known reference point**
4. Type `ZERO` → Should respond: `ACK,ENCODERS_ZEROED`
5. Type `GETPOS` → Should show position
6. **Move arm to different position**
7. Type `GETPOS` → Position should change
8. **Return to reference point**
9. Position should read close to 0,0,0

✅ **Success**: System fully operational

---

## 📐 Calibration

### Physical Calibration

**1. Measure Link Lengths**

Critical for accurate coordinates. Measure center-to-center:

- **Link 1**: Base rotation center to shoulder pivot center
- **Link 2**: Shoulder pivot center to elbow pivot center  
- **Link 3**: Elbow pivot center to wrist pivot center
- **Link 4**: Wrist pivot center to probe tip

**Tools needed:**
- Calipers or precision ruler
- 0.1mm accuracy minimum

**Update config.h:**
```cpp
#define LINK_1_LENGTH 254.3    // Your actual measurement
#define LINK_2_LENGTH 253.8
#define LINK_3_LENGTH 254.1
#define LINK_4_LENGTH 36.2
```

**2. Set Encoder Directions**

If any encoder counts backwards:

```cpp
// In config.h:
#define ENCODER_2_DIRECTION -1  // Flip direction
```

Test by rotating each joint in positive direction and verifying angle increases.

**3. Verify Encoder PPR**

Confirm your encoder resolution:

```cpp
#define ENCODER_PPR 600  // Match your encoder datasheet
```

### Software Calibration

**1. Zero Position Setup**

Establish reference point:

1. Position arm at known, repeatable location
2. Send `ZERO` command
3. This position is now coordinate origin (0, 0, 0)

**2. Validate Accuracy**

Test coordinate accuracy:

1. Position arm at several known points (use gauge blocks or fixtures)
2. Use `GETPOS` to read coordinates
3. Compare to known positions
4. Accuracy should be within:
   - ±0.5mm for typical 600 PPR encoders
   - ±0.2mm for 2400+ PPR encoders

**3. Repeatability Test**

Verify system consistency:

1. Move arm to position A
2. Record coordinates with `GETPOS`
3. Move to position B
4. Return to position A
5. Record coordinates again
6. Difference should be <0.1mm for good repeatability

If repeatability poor:
- Check for mechanical backlash
- Verify encoder mounting is rigid
- Ensure no loose connections
- Verify cable isn't fatiguing

---

## ⚠️ Common Issues and Solutions

### Encoder Not Counting

**Symptoms**: Zero counts, or counts not changing

**Check:**
1. Wiring connections (continuity test each wire)
2. Power at encoder (should be 5V)
3. Encoder shaft actually rotating (coupling may be slipping)
4. Swap with known-good encoder to isolate problem

### Erratic Counting

**Symptoms**: Jumps, missing counts, random changes

**Causes & Solutions:**

| Cause | Solution |
|-------|----------|
| Electrical noise | Add 0.1µF capacitors, use shielded cable |
| Loose connections | Check and re-secure all connections |
| Insufficient power | Use external power supply |
| Speed too high | Slow rotation or upgrade to higher-frequency encoders |
| Bad encoder | Replace encoder |

### Wrong Coordinates

**Symptoms**: XYZ values don't match physical position

**Check:**
1. Link lengths in config.h match actual measurements
2. All encoder directions correct (should increase when rotating positive)
3. Encoder PPR setting matches encoder specification
4. Use `ZERO` command at known reference point

### Arduino Not Responding

**Symptoms**: No startup message, commands ignored

**Solutions:**
1. Press Reset button on Arduino
2. Close and reopen Serial Monitor
3. Verify 115200 baud rate setting
4. Check USB cable (try different cable)
5. Verify correct COM port selected

---

## 📞 Support

For hardware setup questions:
- See the Troubleshooting section in [README.md](README.md)
- Post in [GitHub Discussions](https://github.com/Toy-Department/CMM-Arm/discussions)
- File issue on [GitHub Issues](https://github.com/Toy-Department/CMM-Arm/issues)

---

**Document Version:** 1.0.2  
**Last Updated:** November 20, 2025  
**Firmware Version:** 1.0.2
