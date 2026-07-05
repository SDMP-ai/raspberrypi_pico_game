# Pico Arcade System

A retro arcade cabinet console built using the **Raspberry Pi Pico (RP2040)**, driving an **ST7789 SPI LCD Display** (240x320) using **LVGL v8.3** for retro-themed graphics and **FreeRTOS** for concurrent real-time scheduling.

Controls are managed using a 2-axis analog joystick and dual hardware button switches.

---

## 🎮 Games Included

1. **Pico Snake**
   * Classic snake game rendered on an indigo-bordered grid.
   * Features a multi-colored neon tail (alternating sky blue and purple) and a hot pink fruit target.
   * Wall collisions and self-collisions trigger the Game Over sequence.

2. **Brick Breaker (Breakout)**
   * Action-arcade brick demolition game.
   * Features 4 layers of color-coded bricks:
     * **Top**: Rose Pink (+20 pts)
     * **Upper**: Orange (+20 pts)
     * **Lower**: Yellow (+20 pts)
     * **Bottom**: Neon Green (+20 pts)
   * The ball bounces off walls, bricks, and the player's sliding yellow paddle.
   * Automatically respawns a fresh set of bricks upon clearance.

3. **Space Invaders**
   * Action shooter with auto-firing laser weaponry.
   * Features 3 rows of marching invaders (colored Rose, Violet, and Sky Blue).
   * A player-controlled green starship slides left and right, auto-firing yellow bullets upwards.
   * If any invader reaches the bottom line or touches the ship, a Game Over is triggered.

---

## 🛠️ Hardware Requirements & Pinout

### 1. Display (ST7789 240x320 SPI LCD)
| ST7789 Pin | Pico GP Pin | Description |
|---|---|---|
| **DIN (MOSI)** | `GP19` | SPI0 TX |
| **CLK (SCK)** | `GP18` | SPI0 Clock |
| **CS** | `GP17` | SPI0 Chip Select |
| **DC** | `GP16` | Data / Command Select |
| **RST** | `GP21` | Hardware Reset |
| **BL (Backlight)** | `GP22` | Backlight Enable |

### 2. Input Controls
| Input Component | Pico GP Pin | Description |
|---|---|---|
| **Joystick X-Axis** | `GP26` | Analog input (ADC0) |
| **Joystick Y-Axis** | `GP27` | Analog input (ADC1) |
| **Button 1** | `GP1` | Switch Input (Pull-up) for Select & Exit |
| **Button 2** | `GP2` | Switch Input (Pull-up) for Pause / Play |

---

## 🕹️ Control Schemes

* **In the Arcade Menu**:
  * **Joystick Y-Axis (Up/Down)**: Cycles selection between Snake, Brick Breaker, and Space Invaders.
  * **Button 1 (GPIO 1)**: Single click launches the highlighted game.
* **In-Game**:
  * **Joystick**: Controls game movement (Snake direction, Brick Breaker paddle sliding, Space Invaders ship sliding).
  * **Button 1 (GPIO 1)**: Double-click to instantly quit the current game, delete assets, and return to the main selection menu.
  * **Button 2 (GPIO 2)**: Single-click to toggle Pause / Play status. Pausing halts the game logic and shows a translucent pause menu overlay.

---

## 📂 Project Structure

```
├── CMakeLists.txt              # CMake build configuration
├── pico_sdk_import.cmake       # Pico SDK importer script
├── inc/
│   ├── FreeRTOSConfig.h        # FreeRTOS scheduler properties
│   ├── lv_conf.h               # LVGL graphical library settings
│   └── st7789.h                # ST7789 driver header
└── src/
    ├── main.c                  # Core state machine, input loop, and game engines
    └── st7789.c                # Hardware SPI driver implementation for ST7789
```

---

## 🚀 Building and Flashing

### Prerequisites
* Raspberry Pi Pico SDK installed and configured (`PICO_SDK_PATH` environment variable set).
* Arm GNU Toolchain (`arm-none-eabi-gcc`).
* CMake (v3.13+) and Ninja or Make.

### Build Steps

1. Create a build directory:
   ```bash
   mkdir build
   cd build
   ```

2. Generate Make/Ninja build files:
   ```bash
   cmake -G Ninja ..
   ```

3. Compile the binaries:
   ```bash
   ninja
   ```

4. Flash to Pico:
   * Connect the Raspberry Pi Pico to your computer in Bootloader mode (hold the BOOTSEL button while plugging in the USB cable).
   * Copy the generated `.uf2` binary to the mounted Pico USB drive.
