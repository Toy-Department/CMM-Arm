# Contributing to CMM Arm Firmware

Thank you for your interest in contributing to the CMM Arm Arduino firmware! This open-source project welcomes contributions from the maker, metrology, and reverse engineering communities.

---

## 🤝 How to Contribute

### Reporting Bugs

1. Check [existing issues](https://github.com/Toy-Department/CMM-Arm/issues) to avoid duplicates
2. Create a new issue with the `bug` label and `firmware` tag
3. Include:
   - **Firmware version** (use `VERSION` command)
   - **Arduino board** (Mega 2560, etc.)
   - **Encoder specifications** (PPR, model)
   - **config.h settings**
   - **Serial Monitor output** showing the problem
   - **Steps to reproduce**

### Suggesting Features

1. Open an issue with the `enhancement` label and `firmware` tag
2. Describe:
   - **Use case**: Why is this feature needed?
   - **Proposed solution**: How should it work?
   - **Hardware impact**: Does it require additional components?
   - **Alternatives considered**: Other approaches you've thought about

### Submitting Pull Requests

1. **Fork** the repository
2. **Create a branch** for your feature: `git checkout -b firmware/my-new-feature`
3. **Make your changes** following the code style guidelines below
4. **Test thoroughly** on actual hardware (see Testing section)
5. **Update documentation** in `docs/` if needed
6. **Commit** with clear messages: `git commit -m "Add EEPROM configuration storage"`
7. **Push** to your fork: `git push origin firmware/my-new-feature`
8. **Open a Pull Request** with a detailed description

---

## 📝 Code Style Guidelines

### Arduino C++

- Follow **Arduino coding standards**
- **Indent** with 2 spaces (not tabs)
- **Naming conventions**:
  - Variables/functions: `camelCase`
  - Constants: `UPPER_SNAKE_CASE`
  - Classes: `PascalCase`
- **Comments**: Document all functions with purpose and parameters
- **Keep config.h separate** from code logic

**Example:**
```cpp
/**
 * Calculate forward kinematics for 4-axis arm
 * @param theta1-4: Joint angles in degrees
 * @return XYZ coordinates in millimeters
 */
void calculatePosition(float theta1, float theta2, float theta3, float theta4) {
    // Calculate link positions using forward kinematics
    float x = link1 * cos(theta1) + link2 * cos(theta1 + theta2);
    // ...
}
```

### Configuration Files

- All user-configurable settings go in `config.h`
- Use `#define` for constants
- Group related settings together
- Add clear comments explaining each setting
- Include units in comments (mm, degrees, Hz, etc.)

---

## 🧪 Testing

### Before Submitting a PR

Test on **actual hardware** (Arduino Mega 2560 + encoders):

- [ ] Firmware compiles without errors or warnings
- [ ] Uploads successfully to Arduino Mega 2560
- [ ] Serial communication works (startup message appears at 115200 baud)
- [ ] All commands respond correctly:
  - [ ] `INFO` - Shows system information
  - [ ] `VERSION` - Returns firmware version
  - [ ] `GETPOS` - Returns current position
  - [ ] `ZERO` - Zeros encoders correctly
  - [ ] `START` / `STOP` - Recording works
  - [ ] `SETPPR`, `SETDIM`, `SETTOOL` - Configuration commands work
- [ ] Encoders count correctly (test with `DEBUG_ENCODERS`)
- [ ] Position calculations are accurate (verify with known positions)
- [ ] No memory leaks during extended operation (24+ hours)
- [ ] No crashes or lockups under normal use

### Testing Checklist for Specific Changes

**If you modified encoder reading:**
- Test all 4 encoders individually
- Test at various rotation speeds
- Verify no missed counts
- Check for noise immunity

**If you modified kinematics:**
- Verify XYZ coordinates at known positions
- Test full range of motion
- Check for singularities or edge cases
- Validate against manual calculations

**If you modified serial protocol:**
- Test all commands
- Verify response formats
- Test error handling (invalid commands, parameters)
- Check buffer overflow protection

---

## 🏗️ Project Structure

### Firmware Files (`Hardware_Firmware/Arduino/`)

| File | Purpose |
|------|---------|
| `CMM_Arm_Arduino.ino` | Main sketch, setup() and loop() |
| `config.h` | User-configurable settings |
| `encoder.h` / `encoder.cpp` | Encoder reading with interrupts |
| `kinematics.h` / `kinematics.cpp` | Forward kinematics calculations |
| `serial_protocol.h` / `serial_protocol.cpp` | Command parsing and responses |

### Documentation (`Hardware_Firmware/docs/`)

| File | Purpose |
|------|---------|
| `README.md` | Complete firmware guide |
| `QUICK_START.md` | 15-minute setup guide |
| `HARDWARE_SETUP.md` | Wiring and BOM |
| `CHANGELOG.md` | Version history |

---

## 🎯 Good First Issues

Look for issues labeled `good first issue` and `firmware` - these are beginner-friendly tasks like:

- Adding new serial commands
- Improving error messages
- Adding debug output options
- Optimizing calculations
- Adding configuration options
- Improving documentation
- Testing on different encoder types

---

## 📚 Documentation Contributions

Documentation improvements are always welcome! This includes:

- Fixing typos or unclear explanations
- Adding wiring diagrams or photos
- Improving troubleshooting guides
- Adding example configurations
- Creating tutorials for specific use cases
- Translating documentation

---

## 🔬 Development Tips

### Debugging

Enable debug modes in `config.h`:
```cpp
#define DEBUG_MODE true
#define DEBUG_ENCODERS true
#define DEBUG_KINEMATICS true
```

**Remember:** Disable all debug modes before submitting PR!

### Memory Usage

Arduino Mega 2560 has limited resources:
- **Flash:** 256KB (program storage)
- **SRAM:** 8KB (runtime memory)
- **EEPROM:** 4KB (persistent storage)

Monitor memory usage during compilation. Keep SRAM usage below 80%.

### Interrupt Safety

When modifying encoder code:
- Keep interrupt handlers SHORT and FAST
- Don't use `Serial.print()` in interrupts
- Use `volatile` for variables shared with interrupts
- Disable interrupts when reading multi-byte values

---

## 📜 License

By contributing, you agree that your contributions will be licensed under the **MIT License**.

---

## 💬 Questions?

- **GitHub Discussions**: [Ask the community](https://github.com/Toy-Department/CMM-Arm/discussions)
- **GitHub Issues**: [Report bugs or request features](https://github.com/Toy-Department/CMM-Arm/issues)
- **Email**: toydepartment2025@gmail.com

---

**Thank you for helping make CMM Arm firmware better!** 🎉
