# RS485-Based DDC Cleanroom Tower Lamp Controller

An industrial-grade Direct Digital Control (DDC) system engineered for cleanroom environments. This system uses the RS485 Modbus RTU communication protocol to drive multi-tier stack lights (tower lamps) and deliver real-time visual alerts for environmental parameter deviations, pressure differentials, and system fault states.

---

## 📌 Project Overview

In pharmaceutical and high-precision manufacturing cleanrooms, immediate visual indication of room conditions is critical for regulatory compliance and contamination control. This project bridges industrial DDC controllers/gateways with cleanroom tower lamps via long-range, noise-immune RS485 communication.

### Key Features
* **RS485 / Modbus RTU Protocol:** Long-distance, multi-drop industrial communication with high noise immunity.
* **Real-Time Visual Status:** Multi-color LED tower lamp control (Green: Normal, Yellow: Warning, Red: Critical Alarm, Buzzer: Emergency).
* **Interfacing with Cleanroom DDC/PLC:** Seamless integration with standard industrial DDC systems and sensor modules.
* **Fault Tolerant & Fast Response:** Instant triggering upon cleanroom differential pressure loss or HVAC fault states.

---

## 🛠️ Hardware & Components

* **Controller / Microcontroller:** Industrial DDC Module / ESP32 / STM32
* **Communication Interface:** MAX485 / SP3485 RS485 Transceiver Module
* **Output Actuation:** 4-Channel Industrial Relay Module / Optocoupler-isolated Drivers
* **Indicators:** 24V DC Multi-layer LED Tower Lamp with Buzzer
* **Power Supply:** Industrial 24V DC DIN-Rail Power Supply

---

## ⚙️ System Architecture & Logic

1. **Data Acquisition:** DDC continuously monitors cleanroom environmental parameters (Differential Pressure, Temperature, Humidity).
2. **Command Signal (RS485):** When a parameter crosses defined thresholds, the master controller transmits Modbus RTU commands over the RS485 bus.
3. **Actuation:** The local controller processes the payload and switches corresponding output relays to activate target lamp colors and audible alarms.

| State | Tower Lamp Color | Audio Alert | Condition |
| :--- | :--- | :--- | :--- |
| **Normal** | Steady Green | Off | All parameters within acceptable range |
| **Warning** | Flashing Yellow | Off | Minor parameter drift (e.g., airflow variance) |
| **Critical** | Flashing Red | Active Buzzer | Differential pressure loss or door interlock breach |

---

## 🚀 Getting Started

### Prerequisites
* Baud Rate: `9600 bps` (Default)
* Data Bits: `8`, Stop Bits: `1`, Parity: `None`
* Modbus Slave ID: `1` (Configurable)

### Pin Configuration (Example)
```text
DDC / Microcontroller       RS485 Transceiver       Tower Lamp Relay
---------------------       -----------------       ----------------
TX Pin                -->   DI (Data In)
RX Pin                -->   RO (Receiver Out)
GPIO (DE/RE)          -->   DE & RE (Control)
                            A (+)  ----------> RS485 Bus A
                            B (-)  ----------> RS485 Bus B
                                                    Relay 1 --> Green Light
                                                    Relay 2 --> Yellow Light
                                                    Relay 3 --> Red Light + Buzzer
