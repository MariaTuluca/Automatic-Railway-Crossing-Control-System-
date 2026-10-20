# Automatic-Railway-Crossing-Control-System-
Developed a safety-critical real-time control system for an automated railway level crossing using FreeRTOS on Arduino Mega (AVR).

# Automated Railway Crossing Barrier Control System (FreeRTOS)

A deterministic, real-time embedded system developed for an automated railway level crossing barrier, built on **Arduino Mega 2560** utilizing **FreeRTOS**.

---

## Overview

The system monitors train arrivals and departures across railway tracks using ultrasonic sensors and controls a motorized barrier (servomotor) while strictly adhering to safety and utility constraints:
* **Safety (Priority 1):** The barrier must remain completely closed whenever a train is present or in the guard zone.
* **Utility / Throughput:** The barrier opens as soon as the control zone is clear to minimize road traffic interruption.
* **Time Constraints:** Enforces safety guard windows ($\tau_{min} = 1s$), maximum stationary clearance timeouts ($\tau_{max} = 5s$), and smooth physical transit ($\tau_{open}/\tau_{close} = 30ms$).

---

## System Architecture

The firmware relies on a preemptive, priority-based FreeRTOS design split into three distinct tasks:

| Task | Priority | Responsibility |
| :--- | :--- | :--- |
| **`taskControl`** | High (3) | Acts as the central Finite State Machine (FSM). Processes events and coordinates safety/utility logic. |
| **`taskServo`** | Medium (2) | Executes smooth, linear angular transitions for the barrier in $1\,\text{ms}$ steps. |
| **`taskSensors`** | Low (1) | Samples ultrasonic sensors (HC-SR04) periodically ($60\,\text{ms}$) with hysteresis filtering to eliminate edge jitter. |

### Inter-Task Communication & Timers
* **Queues (`qEvents`, `qServo`):** Decouple asynchronous sensor triggers and timer expirations from state decisions and actuation.
* **Software Timers (`TimerHandle_t`):** Non-blocking one-shot timers handle guard delays (`tTauMin`), stationary limits (`tTauMax`), and movement confirmations (`tOpenDone`, `tCloseDone`).

---

## Hardware Configuration

* **Microcontroller:** Arduino Mega 2560 (ATmega2560)
* **Actuator:** Standard Micro Servo (PWM Pin `9`)
* **Sensors:**
  * **S1 (Approach):** TRIG: Pin `8`, ECHO: Pin `7`
  * **S2 (Departure/Control Zone):** TRIG: Pin `10`, ECHO: Pin `6`

---

## Repository Structure

* `src/`: Complete Arduino/FreeRTOS source code (`.ino`).
* `docs/`: Full academic design report and verification logs.

---

## Contributors

* **Maria-Laura Țuluca**
* **Mădălina-Ioana Palade**
