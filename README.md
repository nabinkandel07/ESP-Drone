
## ESP-Drone Overview
ESP-Drone is a flying development board developed by Espressif, based on the ESP32/ESP32-S2/ESP32-S3 microcontrollers. Equipped with core Wi-Fi capabilities, this drone can connect to and be controlled by a mobile app or gamepad via a Wi-Fi network. It features a simplified hardware structure, clear and readable code, and supports functional expansion—making it an ideal choice for STEAM education. A portion of its code is derived from the Crazyflie open-source project, which is licensed under GPL3.0.
<img width="1460" height="526" alt="image" src="https://github.com/user-attachments/assets/ffccb746-0280-496c-9c7b-af5ab1f201d2" />

## Main Features
ESP-Drone offers the following key functionalities:
- **Stabilize mode**: Maintains the drone’s attitude stability for smooth and steady flight.
- **Height-hold mode**: Regulates thrust output to keep the drone flying at a fixed altitude.
- **Position-hold mode**: Locks the drone’s position to ensure it stays at a specific location during flight.
- **PC debugging**: Supports static and dynamic debugging via the cfclient tool.
- **App control**: Enables easy control through a mobile app over Wi-Fi.
- **Gamepad control**: Allows intuitive control using a gamepad connected via cfclient.

## Main Components
ESP-Drone 2.0 consists of a main board and multiple extension boards, each designed for specific functions:
- **Main board**: Integrates an ESP32-S2 module, core sensors required for basic flight, and provides hardware expansion interfaces for connecting additional modules.
- **Extension boards**: Connect to the main board’s expansion interfaces to add specialized sensors, enabling advanced flight capabilities.

| No. | Modules | Main Components | Function | Interfaces | Mount Location |
|-----|---------|-----------------|----------|------------|----------------|
| 1 | Main board - ESP32-S2 | ESP32-S2-WROVER + MPU6050 | Basic flight operations | I2C, SPI, GPIO, Expansion interfaces | - |
| 2 | Extension board - Position-hold module | PMW3901 (optical flow sensor) + VL53L1X (TOF sensor) | Indoor position-hold flight | SPI + I2C | Mount at the bottom, facing the ground |
| 3 | Extension board - Pressure module | MS5611 pressure sensor | Height-hold flight | I2C or MPU6050 slave interface | Top or bottom of the drone |
| 4 | Extension board - Compass module | HMC5883 compass sensor | Advanced flight modes (e.g., head-free mode) | I2C or MPU6050 slave interface | Top or bottom of the drone |

For more detailed hardware specifications, refer to the [Hardware Reference](https://docs.espressif.com/projects/esp-drone/en/latest/hardware-reference/index.html).

## ESP-IDF Overview
ESP-IDF (Espressif IoT Development Framework) is the official development framework for ESP32/ESP32-S2/ESP32-S3 microcontrollers, provided by Espressif. It includes a comprehensive set of libraries, header files, and core software components necessary for building IoT projects on these platforms. Additionally, ESP-IDF offers essential tools for development and production workflows, such as build systems, flashing utilities, debuggers, and performance measurement tools.

For detailed instructions on using ESP-IDF, see the [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s2/get-started/index.html).

## Crazyflie Overview
Crazyflie is an open-source quadcopter developed by Bitcraze, with the following key features:
- Supports combinations of various sensors to enable advanced flight modes (e.g., height-hold and position-hold).
- Built on FreeRTOS, allowing users to decompose complex drone systems into multiple software tasks with configurable priorities.
- Includes a full-featured custom client (cfclient) and the CRTP (Crazyflie Real-Time Protocol) for debugging, data logging, and control.

![A swarm of drones exploring the environment, avoiding obstacles and each other. (Guus Schoonewille, TU Delft)](crazyflie-overview)
<img width="1016" height="469" alt="image" src="https://github.com/user-attachments/assets/8f4a4b8b-0adb-45a9-9269-cdde1d38148b" />


For more information, visit the [Crazyflie official website](https://www.bitcraze.io/crazyflie/).

# Preparations
## Assemble Hardware
Follow the steps below to assemble the ESP32-S2-Drone V1.2. For a detailed hardware overview and pinout diagram, refer to the [Hardware Reference](https://docs.espressif.com/projects/esp-drone/en/latest/hardware-reference/index.html).
<img width="1762" height="732" alt="image" src="https://github.com/user-attachments/assets/60204aa0-ab71-44da-8b0c-441fca31c886" />


### ESP32-S2-Drone V1.2 Assembly Flow
1. Attach the motors to the drone frame, ensuring they are securely fastened.
2. Connect the motor wires to the corresponding ports on the main board (M1, M2, M3, M4).
3. Install the main board onto the frame, aligning the mounting holes.
4. Attach any required extension boards (e.g., position-hold, pressure, or compass modules) to the main board’s expansion interfaces.
5. Secure the battery connector to the main board, ensuring proper polarity.
6. Install the propellers (A-type and B-type) according to the direction guidelines (see the "Propeller Direction" section below).

## Download and Install ESP-Drone App
The ESP-Drone app is available for both Android and iOS devices:
- **Android**: Scan the QR code below to download the app directly.  
  <img width="280" height="280" alt="image" src="https://github.com/user-attachments/assets/9defa966-3e3a-4eb1-bb02-2cafac528bcb" />

- **iOS**: Search for "ESP-Drone" in the App Store and download the application.

### App Source Code
- iOS: [ESP-Drone-iOS](https://github.com/espressif/esp-drone-ios)
- Android: [ESP-Drone-Android](https://github.com/espressif/esp-drone-android)

## Install cfclient (Optional)
This step is only required for advanced debugging and configuration. cfclient is a PC-based client tool for controlling and debugging the drone.

<img width="644" height="497" alt="image" src="https://github.com/user-attachments/assets/b50eb8f3-d5e9-40e5-b14f-253a998a2c41" />


### 1. Install cfclient
#### 1.1 Download the source code
```bash
git clone https://github.com/qljz1993/crazyflie-clients-python.git
```
#### 1.2 Navigate to the project directory
```bash
cd crazyflie-clients-python
```
#### 1.3 Install dependencies and cfclient
```bash
pip3 install -e .
```
#### 1.4 Launch cfclient
```bash
cfclient
```

### 2. Configure Controllers
<img width="959" height="539" alt="image" src="https://github.com/user-attachments/assets/0990d384-cc52-4a52-b8c4-7e96637f560b" />

#### 2.1 Map the four core control channels:
- **Roll**: Controls the drone’s left/right tilt.
- **Pitch**: Controls the drone’s forward/backward tilt.
- **Yaw**: Controls the drone’s rotation (left/right turn).
- **Thrust**: Controls the drone’s altitude (up/down movement).
#### 2.2 Assign buttons for flight mode switching (e.g., "Assisted control" for toggling height-hold/position-hold modes).

# ESP-Drone App Guide
## Establish Wi-Fi Connection
1. Power on the ESP-Drone. It will act as a Wi-Fi Access Point (AP) with the following default credentials:
   - **SSID**: ESP-DRONE_XXXX (XXXX is a unique identifier derived from the drone’s MAC address)
   - **PASSWORD**: 12345678
2. On your mobile device, enable Wi-Fi and search for the ESP-DRONE_XXXX network.
3. Select the network and enter the password to establish a connection. Once connected, your mobile device and drone will communicate via Wi-Fi.

## Customize Settings
You can adjust flight parameters based on your use case, or use the default configuration provided below:

```
Default Configuration:
Flight Control Settings
    1. Mode: Mode2 (American-style control layout)
    2. Deadzone: 0.2 (sensitivity threshold for joystick input)
    3. Roll trim: 0.0 (compensation for left/right tilt deviation)
    4. Pitch trim: 0.0 (compensation for forward/backward tilt deviation)
    5. Advanced flight control: true (enables advanced features)
    6. Advanced Flight Control Preferences
        1. Max roll/pitch angle: 15° (maximum tilt angle for stability)
        2. Max yaw angle: 90° (maximum rotation angle)
        3. Max thrust: 90 (upper limit for motor power)
        4. Min thrust: 25 (lower limit for motor power to prevent stalling)
        5. X-Mode: true (enables X-shaped flight configuration for agility)
Controller Settings
    7. Use full travel for thrust: false (limits thrust range for beginner safety)
    8. Virtual joystick size: 100 (adjusts the size of on-screen joysticks)
App Settings
    9. Screen rotation lock: true (prevents screen rotation during flight)
    10. Full screen mode: true (uses the entire screen for controls)
    11. Show console: true (displays real-time flight data and logs)
```

## Flight Control
1. Open the ESP-Drone app and tap the "Connect" button/icon. When the connection is successful, the drone’s LED will blink **green**.
2. To take off, gently slide the "Thrust" joystick (left side of the screen) upward. The drone will lift off and hover at a low altitude.
3. Control the drone’s movement using the on-screen joysticks:
   - **Left joystick**: Adjusts thrust (up/down) and yaw (left/right turn).
   - **Right joystick**: Controls roll (left/right) and pitch (forward/backward).

<img width="1879" height="970" alt="image" src="https://github.com/user-attachments/assets/797b7404-061d-4534-90cd-1b7bbc4c9e1e" />


# PC cfclient Guide
cfclient is a PC-based client tool originally developed for the Crazyflie project. It fully implements the CRTP protocol, enabling fast debugging and control of the drone. ESP-Drone has customized cfclient to align with its functional requirements.

<img width="700" height="392" alt="image" src="https://github.com/user-attachments/assets/77596a3a-587e-4ff8-a109-f944c317b210" />

<img width="644" height="497" alt="image" src="https://github.com/user-attachments/assets/6b0f87e7-ec21-467f-85dc-514477d30c6e" />


The project uses JSON files to store configuration and cache data. For detailed configuration instructions, refer to the [User Configuration File](https://docs.espressif.com/projects/esp-drone/en/latest/software-reference/user-configuration-file.html).

## Flight Settings
### Basic Flight Control
#### Flight Modes
- **Normal mode**: Designed for beginners, with limited maximum angle and thrust to ensure stability.
- **Advanced mode**: Unlocks the full range of maximum angles and thrust for experienced users.

#### Assisted Modes
- **Altitude-hold mode**: Maintains a constant flight altitude. Requires a barometric pressure sensor (e.g., MS5611).
- **Position-hold mode**: Locks the drone’s position. Requires an optical flow sensor (PMW3901) and a Time-of-Flight (TOF) sensor (VL53L1X).
- **Height-hold mode**: Maintains a fixed height. Note: The drone must be flying at least 40 cm above the ground, and a TOF sensor is required.
- **Hover mode**: Hovers at 40 cm or higher above the takeoff point. Requires an optical flow sensor and a TOF sensor.

#### Trim Adjustments
- **Roll Trim**: Compensates for sensor installation deviations. Adjusts the drone’s rotation around the front-back horizontal axis (left/right movement).
- **Pitch Trim**: Compensates for sensor installation deviations. Adjusts the drone’s rotation around the left-right horizontal axis (forward/backward movement).

> Note: In assisted modes, the thrust controller functions as a height controller to maintain stability.

### Advanced Flight Control
- **Max angle**: Sets the maximum allowable pitch and roll angles.
- **Max yaw rate**: Defines the maximum rotation speed (yaw) of the drone.
- **Max thrust**: Configures the upper limit for motor power output.
- **Min thrust**: Configures the lower limit for motor power output (prevents motor stalling).
- **Slew limit**: Prevents sudden drops in thrust. If thrust falls below this value, the drone will restrict thrust reduction to the "Slew rate".
- **Slew rate**: The maximum rate at which thrust can decrease when below the "Slew limit".

## Configure Input Device
Follow the on-screen prompts in cfclient to map your controller’s channels to the drone’s control functions (Roll, Pitch, Yaw, Thrust).

<img width="956" height="542" alt="image" src="https://github.com/user-attachments/assets/847dcbf3-a088-4c18-8fa5-d3f625ac07d2" />


## Flight Data Monitoring
In the "Flight Control" tab of cfclient, you can view real-time drone status. Detailed data is displayed in the bottom-right corner, including:
- **Target**: The desired angle (set by the controller).
- **Actual**: The measured angle (from the drone’s sensors).
- **Thrust**: The current thrust value (0–100).
- **M1/M2/M3/M4**: The actual power output of each motor.

## Tune Online Parameters
cfclient allows real-time adjustment of PID (Proportional-Integral-Derivative) parameters for flight optimization.

<img width="1379" height="1050" alt="image" src="https://github.com/user-attachments/assets/c0b69769-99c3-4c5a-8b24-c9eb5b0ab9fa" />


### Notes
1. Modified parameters take effect immediately, eliminating the need for frequent firmware flashing.
2. You can define which parameters are editable in real time via the drone’s code.
3. Online parameter adjustments are for debugging purposes only—changes are not saved when the drone is powered off.

## Monitor Flight Data
1. Configure log parameters in the "Log Configuration" and "Log Blocks" tabs to select which data to record (e.g., gyroscope, accelerometer, or sensor readings).
  <img width="929" height="736" alt="image" src="https://github.com/user-attachments/assets/a9e8516b-e47f-4f91-a929-b63c53531f55" />

  <img width="1060" height="594" alt="image" src="https://github.com/user-attachments/assets/208f1a61-6f29-44d8-bf0c-a3b9e56c09f7" />

2. Use the "Plotter" tab to visualize real-time data as waveforms, enabling you to monitor gyroscope and accelerometer performance.

<img width="1375" height="1015" alt="image" src="https://github.com/user-attachments/assets/d34bb528-d627-4388-96d1-ee60fb785815" />


# Propeller Direction
Install A-type and B-type propellers according to the diagram below. Ensure the propellers are securely attached and rotate in the correct direction. During power-on self-test, verify that all propellers spin smoothly and in the correct orientation (as indicated by the drone’s documentation).

<img width="500" height="498" alt="image" src="https://github.com/user-attachments/assets/c2e16d37-66e4-4324-a360-f9bb4f7b0335" />


> Note: Incorrect propeller installation will cause the drone to lose stability or fail to take off.

# Preflight Check
Before launching the drone, perform the following checks to ensure safety and functionality:
1. Place the drone with its **head (front)** facing forward and its **tail (antenna end)** facing backward.
2. Position the drone on a flat, level surface. Power it on only when it is stationary.
3. Use cfclient to confirm that the drone’s sensors detect a level orientation.
4. After establishing a connection (via app or cfclient), check that the drone’s tail LED blinks **fast green** (indicates normal operation).
5. Verify that the drone’s head LED is not blinking **red** (red blinking indicates low battery—recharge before flight).
6. Test thrust response: Gently slide the thrust controller (left side of the app or cfclient) upward to ensure the motors activate smoothly.
7. Test direction control: Move the roll/pitch joystick (right side of the app or cfclient) to confirm the drone’s motors adjust correctly to change direction.
8. Once all checks are passed, the drone is ready for flight. Enjoy!


## sources 
**https://github.com/leeebo/crazyflie-clients-python**

**https://docs.espressif.com/projects/espressif-esp-drone/en/latest/gettingstarted.html#main-features**

**https://github.com/jobitjoseph/ESP32-Flight-controller-**

**https://www.youtube.com/watch?v=V_mZsiZcy7s**
