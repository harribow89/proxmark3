<a id="Top"></a>

# Proxmark3 GUI Testing Guide

## Table of Contents
- [Proxmark3 GUI Testing Guide](#proxmark3-gui-testing-guide)
- [Table of Contents](#table-of-contents)
- [Overview](#overview)
- [GUI Status](#gui-status)
- [Built-in Qt GUI Features](#built-in-qt-gui-features)
- [GUI Testing on Kali Linux](#gui-testing-on-kali-linux)
  - [Prerequisites](#prerequisites)
  - [Installation on Kali](#installation-on-kali)
  - [Verifying Qt GUI Support](#verifying-qt-gui-support)
  - [Testing the GUI](#testing-the-gui)
- [Known Issues and Limitations](#known-issues-and-limitations)
- [External GUI Applications](#external-gui-applications)
- [Troubleshooting](#troubleshooting)

## Overview
^[Top](#top)

This guide answers the question: **Is the Proxmark3 GUI finished and ready for testing on Kali?**

**Short Answer:** Yes, the built-in Qt-based GUI is functional and ready for testing on Kali Linux. It provides graphical visualization features for various Proxmark3 operations.

## GUI Status
^[Top](#top)

The Proxmark3 Iceman fork includes a built-in Qt-based GUI that provides:
- ✅ **Graph Window**: Displays signal graphs for RF analysis (e.g., during `hw tune`, LF/HF operations)
- ✅ **Picture Viewer**: Displays images from certain operations (e.g., `hf emrtd info`)
- ✅ **Interactive Controls**: Zoom, pan, and analyze captured signals
- ✅ **Cross-platform Support**: Works on Linux (including Kali), macOS, and Windows

The GUI has been actively maintained with recent updates including:
- Picture viewer functionality
- Enhanced keyboard controls (home, end, pageup, pagedown)
- Qt5 compatibility
- Thread handling improvements

## Built-in Qt GUI Features
^[Top](#top)

The integrated GUI provides the following features:

**Graph Window Features:**
- Real-time signal plotting
- Interactive zoom and pan controls
- Grid overlay with configurable spacing
- Cursor measurements with scale factors
- Multiple graph overlay support
- Operation controls (autocorr, ASK edge detection, thresholds)
- Keyboard shortcuts for navigation

**Picture Viewer Features:**
- Display images from eMRTD and other operations
- Base64 image support
- Integrated with Qt framework

**Usage:**
The GUI automatically appears when running commands that produce graphical output, such as:
```
hw tune
lf read
hf 14a sniff
hf emrtd info
```

## GUI Testing on Kali Linux
^[Top](#top)

### Prerequisites
^[Top](#top)

Kali Linux is officially supported by the Proxmark3 Iceman fork. The GUI works on Kali as long as Qt5 dependencies are installed.

### Installation on Kali
^[Top](#top)

1. **Update your system:**
```sh
sudo apt-get update
sudo apt-get upgrade -y
```

2. **Install all dependencies including Qt5:**
```sh
sudo apt-get install --no-install-recommends git ca-certificates build-essential pkg-config \
libreadline-dev gcc-arm-none-eabi libnewlib-dev qtbase5-dev \
libbz2-dev liblz4-dev libbluetooth-dev libpython3-dev libssl-dev libgd-dev
```

**Note:** The `qtbase5-dev` package is required for GUI support.

3. **Clone and build:**
```sh
git clone https://github.com/RfidResearchGroup/proxmark3.git
cd proxmark3
make clean && make -j
```

### Verifying Qt GUI Support
^[Top](#top)

When you build the client, check the build output for the GUI support status:

```
GUI support:       QT5 found, enabled (Qt version X.X.X)
```

If you see:
```
GUI support:       QT not found, disabled
```

Then Qt5 is not installed properly. Install `qtbase5-dev` and rebuild.

**To explicitly skip GUI support** (for headless systems), build with:
```sh
make SKIPQT=1
```

### Testing the GUI
^[Top](#top)

1. **Connect your Proxmark3 device** (or run without hardware for testing)

2. **Start the client:**
```sh
./pm3
```

3. **Test the graph window:**
```
[usb] pm3 --> hw tune
```
This should open a graphical window showing the antenna tuning graph.

4. **Test interactive features:**
   - Use mouse wheel to zoom
   - Click and drag to pan
   - Press `Home`, `End`, `PageUp`, `PageDown` for navigation
   - Adjust controls in the overlay panel

5. **Test with LF operations:**
```
[usb] pm3 --> lf read
[usb] pm3 --> data plot
```

6. **Test picture viewer** (if you have compatible tags):
```
[usb] pm3 --> hf emrtd info
```

## Known Issues and Limitations
^[Top](#top)

**Working:**
- ✅ Qt5 GUI on Kali Linux
- ✅ Graph window displays
- ✅ Interactive controls
- ✅ Picture viewer
- ✅ Multi-threading support

**Limitations:**
- The GUI requires an X server (won't work on headless systems without X forwarding)
- WSL1/WSL2 users may need to configure X server (see [WSL Installation Guide](/doc/md/Installation_Instructions/Windows-WSL2-Installation-Instructions.md))
- Some older distributions may still have Qt4; Qt5 is recommended

**Not related to built-in GUI:**
- ⚠️ External GUI from Gaucho (old, not maintained, incompatible)

## External GUI Applications
^[Top](#top)

In addition to the built-in Qt GUI, there are third-party GUI applications available:

1. **[Proxmark3 Universal GUI](https://github.com/burma69/PM3UniversalGUI)** - Works more or less
2. **[Proxmark3 GUI (cross-compiled)](https://github.com/wh201906/Proxmark3GUI/)** - Recently updated, claims to support latest source
3. **[Proxmark3_GUI](https://github.com/Phreak87/Proxmark3_GUI)** - Simple GUI in VB.NET

These are separate applications and not part of this repository.

## Troubleshooting
^[Top](#top)

**Problem:** GUI window doesn't appear
- **Solution:** Verify Qt5 is installed: `dpkg -l | grep qtbase5`
- **Solution:** Check build output for "GUI support: QT5 found, enabled"
- **Solution:** Ensure X server is running (for SSH: use X forwarding with `ssh -X`)

**Problem:** "Gtk-Message: Failed to load module 'canberra-gtk-module'"
- **Solution:** Install the module: `sudo apt-get install libcanberra-gtk-module`
- **Note:** This is a non-blocking warning

**Problem:** libQt5Core.so.5 not found (common on WSL1)
- **Solution:** See the [Troubleshooting guide](/doc/md/Installation_Instructions/Troubleshooting.md#libQt5Coreso5-not-found)

**Problem:** Want to run without GUI
- **Solution:** Build with `make SKIPQT=1` for a smaller binary without Qt dependencies

---

## Conclusion

**The Proxmark3 built-in Qt GUI is finished and ready for testing on Kali Linux.** It provides essential visualization features for RF analysis and is actively maintained. As long as Qt5 dependencies are installed, the GUI works reliably on Kali and other Debian-based distributions.

For the latest updates and features, see the [CHANGELOG.md](/CHANGELOG.md).
