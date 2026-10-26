# Real-Time Clock and Stopwatch Using 8051 Microcontroller

**Course Project | Experiment 3 — Real-Time Clock with Stopwatch**  
**Department of Electronics and Electrical Communication Engineering (E&ECE)**  
**Indian Institute of Technology Kharagpur**

**Duration:** February 2025 – April 2025  
**Supervisor:** Prof. Indrajit Chakrabarti

---

## 1. Project Overview

This project focuses on interfacing common-cathode seven-segment displays with an 8051 microcontroller through the 8255 Programmable Peripheral Interface (PPI). The experiment progresses from displaying decimal digits to implementing a digital clock and stopwatch with timer-interrupt-based timekeeping.

The final application is designed to display time in `HH:MM:SS` format using six seven-segment digits. The normal clock maintains the current time, while the stopwatch provides an independent elapsed-time counter. A mode switch selects which time is displayed, and dedicated controls operate the stopwatch.

The project explores fundamental concepts in microcontroller programming, peripheral interfacing, timer configuration, interrupt service routines, time management, and multiplexed display control.

## 2. Objectives

- Interface common-cathode seven-segment displays with the 8051 system using the 8255 PPI.
- Generate the segment patterns required to display decimal digits.
- Implement single-digit and two-digit display routines.
- Configure and program the 8255 for input/output operations.
- Configure Timer 1 and develop an interrupt service routine (ISR) for periodic timekeeping.
- Implement a digital clock with hours, minutes, and seconds.
- Implement an independent stopwatch with start and stop controls.
- Use display multiplexing to drive multiple seven-segment digits through shared segment-data lines.
- Handle user inputs to select the clock or stopwatch display mode.

## 3. Experiment Tasks

### Part A — Single Seven-Segment Display

Display the decimal digits `0–9` continuously on a common-cathode seven-segment display, with a nominal one-second digit display interval as specified in the experiment manual.

**Key concepts:**
- Seven-segment display pin configuration
- Common-cathode operation
- Segment encoding and lookup tables
- 8255 output-port interfacing
- Current-limiting resistors for individual segments

### Part B — Two Seven-Segment Displays

Display the specified number sequences continuously using two seven-segment displays, with a nominal 0.5-second digit display interval.

The required sequences are:

- `00–06`
- `15–23`
- `29`
- `47–54`

The segment lines of the two displays are connected in parallel and driven through a 74245 bus transceiver. The common-cathode selection lines are controlled through another 8255 port and a separate 74245.

**Key concepts:**
- Two-digit number representation
- Decimal digit extraction
- Display multiplexing
- Digit-selection logic
- Interfacing multiple displays using shared segment lines

### Part C — Real-Time Clock and Stopwatch

Develop a digital clock and stopwatch displaying time in `HH:MM:SS` format using six common-cathode seven-segment digits.

The intended functionality includes:

- **Real-time clock:** Maintains hours, minutes, and seconds in 24-hour format.
- **Automatic timekeeping:** Uses a timer interrupt and an ISR to update the clock periodically.
- **Mode selection:** Switches the display between the normal clock and stopwatch.
- **Stopwatch start:** Starts the stopwatch from zero.
- **Stopwatch stop:** Pauses the stopwatch while retaining its elapsed time.
- **Independent counters:** Maintains the normal clock and stopwatch separately, allowing both to be updated while the stopwatch is running.
- **Multiplexed display:** Refreshes the six digits to display the selected time.

## 4. Hardware Requirements

The following components are specified or referenced in the experiment manual and implementation.

| Component | Purpose |
|---|---|
| 8051 microcontroller kit | Executes the Assembly program |
| 8255 Programmable Peripheral Interface | Provides programmable digital I/O ports |
| Common-cathode seven-segment displays | Display decimal digits |
| 74245 bus transceivers | Buffer and drive display signals |
| Current-limiting resistors | Limit current through individual LED segments |
| Mode switch | Selects the clock or stopwatch display |
| Start and stop switches | Control stopwatch operation |
| Interconnecting wires | Connect the display, switches, and interface circuitry |

The actual kit wiring and available peripheral connections should
