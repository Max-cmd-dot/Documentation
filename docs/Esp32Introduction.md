---
sidebar_label: "Esp32 Box"
sidebar_position: 3
---

# Esp32 Box

# ESP32 Sensor Data Collection and Display System

This part implements a comprehensive system for collecting, processing, and visualizing sensor data on an ESP32 microcontroller. It utilizes various hardware components and software libraries to gather environmental data, send it to a server, and display the results on a screen. The system also includes support for Over-the-Air (OTA) firmware updates, Wi-Fi management, and the execution of commands received from a server. This Hardware part (ESP32) will syncronize his data with the Server.

## Overview of the System

The system integrates multiple sensors and peripherals, including:

- **BME680**: A sensor for measuring temperature, humidity, pressure, and gas resistance, providing a comprehensive set of environmental data.
- **TCS34725**: A color sensor that captures RGB values and calculates light intensity in lux.
- **Soil Moisture Sensors**: Two sensors for monitoring soil moisture levels at different locations.
- **TFT Display with Touch Functionality**: Visualizes sensor data and allows user interaction via touch input.

## Key Features of the System

### 1. **Sensor Data Collection and Processing**

The system continuously collects data from the connected sensors:

- **BME680 Sensor**:
  - Measures ambient temperature, humidity, barometric pressure, and gas resistance.
  - The data is processed to provide meaningful insights, such as the air quality index derived from gas resistance.
- **TCS34725 Sensor**:
  - Captures RGB color values and calculates the corresponding light intensity in lux.
  - This data can be used for applications like ambient light sensing or color detection.
- **Soil Moisture Sensors**:
  - Monitors soil moisture at two different points, providing real-time data on soil conditions, which is crucial for agricultural or gardening applications.

The collected data is periodically sent to a server for storage and further analysis. Additionally, it is displayed locally on the TFT screen for immediate visualization.

### 2. **Display Control**

The system features a graphical user interface displayed on a TFT screen:

- **LVGL (Light and Versatile Graphics Library)**:
  - Powers the display, enabling the creation of a sophisticated and user-friendly interface.
  - The interface is customizable and can display various sensor readings, status information, and alerts.
- **Touch Functionality**:
  - The system includes touch support, allowing users to interact with the interface.
  - Touch inputs can be used to navigate between different screens, start or stop data collection, or configure system settings.
  - A calibration routine is available to ensure accurate touch inputs.

### 3. **Network Communication**

The system is designed to be highly connected:

- **Wi-Fi Connectivity**:
  - Managed by the WiFiManager library, which simplifies the process of connecting to a Wi-Fi network.
  - The system can automatically connect to known networks or prompt the user to select a network if none are configured.
- **HTTP Communication**:
  - The system sends HTTP POST requests to a backend server to transmit sensor data.
  - It can also receive HTTP GET requests from the server to execute commands, such as adjusting sensor parameters or updating the display.
- **NTP (Network Time Protocol) Support**:
  - The system synchronizes its internal clock with an NTP server to ensure accurate timestamps for all collected data.

### 4. **OTA Firmware Updates**

The system supports Over-the-Air (OTA) updates:

- **ESP32 OTA Support**:
  - Enables remote firmware updates, eliminating the need to physically connect the device to a computer for updates.
  - The system regularly checks for available firmware updates on the server and automatically downloads and installs them if a new version is found.
  - OTA updates are critical for maintaining and improving the system without requiring user intervention.

### 5. **Multitasking with FreeRTOS**

The system utilizes FreeRTOS for multitasking:

- **Parallel Task Execution**:
  - The system can handle multiple tasks concurrently, such as collecting sensor data, processing HTTP requests, and updating the display.
  - Tasks are prioritized to ensure that critical operations, like maintaining network connectivity and processing sensor data, are executed without delays.
- **Task Management**:
  - Tasks can be dynamically started, stopped, or adjusted based on system needs, allowing for flexible and responsive operation.

## Libraries Used

The following libraries are integral to the system's operation:

- **Arduino.h**: The core library for Arduino-based development, providing basic functionality for the ESP32.
- **Adafruit_TCS34725.h**: Interfaces with the TCS34725 RGB color sensor, enabling color and light intensity measurements.
- **Adafruit_BME680.h**: Interfaces with the BME680 environmental sensor, enabling the collection of temperature, humidity, pressure, and gas resistance data.
- **WiFiManager.h**: Simplifies Wi-Fi configuration by managing network connections and credentials.
- **ArduinoJson.h**: Provides tools for parsing and generating JSON data, crucial for formatting data to be sent to or received from the server.
- **esp32fota.h**: Supports firmware updates via HTTP, enabling OTA functionality.
- **lvgl.h**: A versatile graphics library that powers the display and enables the creation of a sophisticated user interface.
- **TFT_eSPI.h**: Interfaces with the TFT display, handling the rendering of graphics and text on the screen.

## Installation Guide

To deploy the system on an ESP32, follow these steps:

### Prerequisites

Ensure you have the following tools and software installed:

- **PlatformIO**: A powerful, open-source ecosystem for embedded development. PlatformIO should be installed in your IDE (VSCode or Atom is recommended).
- **ESP32 Board and Drivers**: The ESP32 board should be connected to your computer, and the appropriate drivers should be installed to enable communication.
- **Git**: Installed on your system to clone the project repository. Alternatively you can just download it from Github. (https://github.com/Max-cmd-dot/NexaBoxESP32)

### Step-by-Step Instructions

1. **Clone the Project Repository**

   Begin by cloning the project repository to your local machine:

   ```bash
   git clone https://github.com/Max-cmd-dot/NexaBoxESP32.git
   ```

   Navigate to the directory where you want to store the project, then run the command above.

### Open the Project in PlatformIO

Next, open the project in your chosen IDE:

- Launch **Visual Studio Code (VSCode)**.
- Go to `File -> Open Folder...` and select the cloned project directory.
- PlatformIO should automatically detect the project and configure the environment. If not, ensure PlatformIO is properly installed and configured in your IDE.

### Install Required Libraries

The project relies on several external libraries. You can install these libraries through PlatformIO:

- In the PlatformIO sidebar, click on the "PlatformIO Home" icon.
- Navigate to "Libraries" and search for each library listed in the `platformio.ini` file. Install them one by one:
  - `Adafruit_TCS34725`
  - `Adafruit_BME680`
  - `WiFiManager`
  - `ArduinoJson`
  - `esp32fota`
  - `lvgl`
  - `TFT_eSPI`

Alternatively, you can install the libraries using the PlatformIO CLI:

```bash
pio lib install "Adafruit TCS34725" "Adafruit BME680" "WiFiManager" "ArduinoJson" "esp32fota" "lvgl" "TFT_eSPI"
```

Ensure that all dependencies are resolved and that the libraries are compatible with the ESP32.

### Configure the `platformio.ini` File

The `platformio.ini` file contains essential configurations for the project. Make sure it matches your ESP32 board's specifications:

```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps =
    adafruit/Adafruit TCS34725 @ ^2.0.3
    adafruit/Adafruit BME680 @ ^1.0.10
    tzapu/WiFiManager @ ^0.16.0
    bblanchon/ArduinoJson @ ^6.19.4
    tinusaur/esp32fota @ ^2.0.0
    lvgl/lvgl @ ^8.3.2
    bodmer/TFT_eSPI @ ^2.3.70
monitor_speed = 115200
upload_speed = 921600
```

Ensure the `board` field matches your specific ESP32 board model. Adjust the `upload_speed` and `monitor_speed` if necessary, depending on your setup.

### Upload the Code to the ESP32

Connect your ESP32 board to your computer using a USB cable. To upload the code:

- Click the `Upload` button in the PlatformIO toolbar.
- Alternatively, you can use the CLI command:

```bash
pio run --target upload
```

PlatformIO will compile the code and upload it to the ESP32. Ensure that the board is in bootloader mode if necessary (this may require holding down a button on the board).

### Monitor Serial Output

To verify that the system is running correctly and to debug any issues, you can monitor the serial output:

Click the `Monitor` button in PlatformIO.

Or run:

```bash
pio device monitor
```

Ensure the baud rate is set correctly (e.g., `115200`). The serial monitor will display system logs, sensor readings, and any error messages.

### Test the System

After uploading, test the system by interacting with the touch display, checking sensor readings, and ensuring that data is transmitted to the server. Verify Wi-Fi connectivity and the ability to perform OTA updates.

### Troubleshooting

- **Library Conflicts**: If you encounter issues with library versions, try updating or downgrading specific libraries to ensure compatibility.
- **Serial Monitor Issues**: If no output appears in the serial monitor, verify the correct COM port is selected and the baud rate matches your configuration.
- **Wi-Fi Connection Problems**: Ensure that the Wi-Fi credentials are correct, and the ESP32 is within range of the network.

By following these instructions, you should be able to successfully set up, configure, and deploy the ESP32 Sensor Data Collection and Display System. The system is designed to be modular and extensible, allowing you to add additional sensors, enhance the display interface, or integrate more complex server-side functionality as needed.
