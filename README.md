# CMM Measurement Arm

A professional desktop application for 4-axis arm with real-time 3D visualization, advanced geometry analysis, and Arduino-based hardware control.

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## Overview

The CMM Measurement Arm system consists of two main components:

1. **[Desktop Application](App/)** - Electron-based software for data capture and analysis
2. **[Hardware & Firmware](Hardware_Firmware/)** - Arduino firmware for the arm

## Key Features

- 🎯 **Real-time 3D Tracking** - Live position updates with forward kinematics
- 📐 **Geometry Analysis** - Circle, plane, and line fitting with best-fit calculations
- 🧊 **3D Visualization** - Interactive Three.js viewer with orbit controls
- 📊 **Live Recording** - Automated point capture at 1-20 Hz
- � **Tool Library** - Multiple probe tools with offset compensation
- 💾 **CSV Export** - Export captured points and geometry results
- ↩️ **Undo/Redo** - Full operation history
- 🎮 **Simulator Mode** - Test without hardware

## Quick Start

### Desktop Application

```bash
cd App
npm install
npm start
```

See [App/README.md](App/README.md) for detailed installation and usage instructions.

### Hardware & Firmware

Upload the Arduino firmware to your Arduino Mega 2560:

1. Open `Hardware_Firmware/Arduino/CMM_Arm_Arduino.ino` in Arduino IDE
2. Select **Board: Arduino Mega 2560**
3. Select your serial port
4. Click **Upload**

See [Hardware_Firmware/docs/README.md](Hardware_Firmware/docs/README.md) for complete hardware setup, wiring diagrams, and configuration.

## System Requirements

### Desktop Application
- **Node.js** v18.0.0 or higher
- **npm** v9.0.0 or higher
- **OS**: Windows, macOS, or Linux

### Hardware
- **Arduino Mega 2560**
- **4x Quadrature Encoders** (600 PPR recommended)
- **USB Cable** for serial communication

## Tech Stack

- **[Electron](https://www.electronjs.org/)** - Desktop application framework
- **[Three.js](https://threejs.org/)** - 3D visualization
- **[SerialPort](https://serialport.io/)** - Hardware communication
- **[Node.js](https://nodejs.org/)** - Runtime environment
- **Arduino** - Firmware platform

## Project Structure

```
CMM-Arm/
├── App/                      # Desktop application
│   ├── src/                  # Core modules
│   ├── assets/               # Icons and images
│   ├── docs/                 # App documentation
│   └── README.md             # App installation guide
│
├── Hardware_Firmware/        # Arduino firmware
│   ├── docs/                 # Firmware documentation
│   └── Arduino/              # Firmware source
│
├── LICENSE                   # MIT License
└── README.md                 # This file
```

## Documentation

- **[App Documentation](App/README.md)** - Desktop application setup and usage
- **[Firmware Documentation](Hardware_Firmware/docs/README.md)** - Complete firmware guide with wiring, commands, and troubleshooting

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Built with ❤️ for the maker community.

---

**Version 1.0.0** | [Report Issues](https://github.com/Toy-Department/CMM-Arm/issues)
