# ZMK Standalone Trackpad

A ZMK configuration for creating a standalone wireless trackpad using a Cirque TrackPac and Seeed Xiao nRF52840.

## Features

- ✅ Wireless Bluetooth connectivity
- ✅ Mouse movement and clicking
- ✅ Tap-to-click support
- ✅ Scroll functionality
- ✅ Low power consumption with sleep mode
- ✅ Custom sensitivity and orientation settings

## Hardware Requirements

### Components
- **Seeed Xiao nRF52840** - Main controller board
- **Cirque TrackPac** - Capacitive trackpad sensor
- Connecting wires
- Optional: 3D printed enclosure

### Wiring (SPI Connection)

| Cirque TrackPac | Xiao nRF52840 | Function |
|-----------------|---------------|----------|
| VCC             | 3.3V          | Power    |
| GND             | GND           | Ground   |
| SCLK            | D8 (P0.08)    | SPI Clock|
| MOSI            | D10 (P0.10)   | SPI Data Out |
| MISO            | D9 (P0.09)    | SPI Data In |
| SS/CS           | D7 (P0.07)    | Chip Select |
| DR              | D3 (P0.03)    | Data Ready (optional) |

## Installation

### Method 1: Download Pre-built Firmware

1. Go to the [Releases](../../releases) page
2. Download the latest `zmk.uf2` file
3. Put your Xiao nRF52840 into bootloader mode (double-press reset button)
4. Copy the `zmk.uf2` file to the USB drive that appears
5. The board will automatically reboot with the new firmware

### Method 2: Build from Source

1. Clone this repository
2. Push to GitHub (must be public for GitHub Actions)
3. GitHub Actions will automatically build the firmware
4. Download the `zmk.uf2` artifact from the Actions tab

## Configuration

The trackpad behavior can be customized by modifying the device tree overlay:

### Sensitivity
```c
sensitivity = <0x08>;  // Range: 0x01-0x1F (higher = more sensitive)
```

### Orientation
```c
rotate = <90>;    // Rotate trackpad: 0, 90, 180, 270 degrees
invert-x;         // Invert X-axis movement
invert-y;         // Invert Y-axis movement
```

### Scroll Settings
```c
scroll-divisor = <4>;  // Higher = slower scrolling
```

### Power Management
```c
sleep-timeout = <10000>;  // Sleep after 10 seconds of inactivity (ms)
```

## Usage

1. **Pairing**: After flashing firmware, the trackpad will appear as "Trackpad Xiao" in Bluetooth settings
2. **Mouse Movement**: Move finger on trackpad to control cursor
3. **Click**: Tap the trackpad surface (tap-to-click enabled by default)
4. **Scroll**: Use two-finger gestures or edge scrolling (depending on trackpad model)

## Troubleshooting

### Trackpad Not Detected
- Verify wiring connections
- Check SPI frequency setting (try reducing from 1MHz to 500kHz)
- Enable logging and check serial output

### Cursor Movement Issues
- Adjust `sensitivity` value
- Try different `rotate` values (0, 90, 180, 270)
- Toggle `invert-x` and `invert-y` options

### Connection Problems
- Ensure device is in pairing mode
- Clear Bluetooth cache on host device
- Check power management settings

### Build Failures
- Ensure repository is public (required for GitHub Actions)
- Check Actions logs for specific error messages
- Verify all configuration files are properly formatted

## Customization

### Adding Physical Buttons
If you want to add physical buttons (left/right click, middle click), modify the keymap:

```c
default_layer {
    bindings = <
        &mkp LCLK  &mkp RCLK  &mkp MCLK
    >;
};
```

### Custom Gestures
Add custom behaviors for multi-finger gestures or special actions:

```c
behaviors {
    custom_scroll: custom_scroll {
        compatible = "zmk,behavior-mouse-scroll";
        mode = "smooth";
        acceleration = <2>;
    };
};
```

## Development

### Local Building
```bash
# Install ZMK development environment
west init -l config
west update
west zephyr-export

# Build firmware
west build -s zmk/app -b seeeduino_xiao_ble -- -DSHIELD=trackpad_xiao
```

### Debug Mode
Enable logging by uncommenting in `trackpad_xiao.conf`:
```conf
CONFIG_ZMK_USB_LOGGING=y
```

Then monitor serial output at 115200 baud.

## Contributing

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [ZMK Firmware](https://zmk.dev/) - The amazing mechanical keyboard firmware
- [Cirque](https://www.cirque.com/) - For the excellent trackpad sensors
- [Seeed Studio](https://www.seeedstudio.com/) - For the Xiao nRF52840 board

## Support

- 📖 [ZMK Documentation](https://zmk.dev/docs)
- 💬 [ZMK Discord Server](https://zmk.dev/community/discord/invite)
- 🐛 [Issue Tracker](../../issues)

---

**Note**: This is an experimental configuration. Test thoroughly before relying on it for daily use.