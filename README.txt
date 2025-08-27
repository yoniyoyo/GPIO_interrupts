MSP430G2553 – Interrupts & Operating Modes — README

Short Summary
------------
This repository contains assembly code for the MSP430G2553 microcontroller.
The program runs directly on the MCU and demonstrates an interrupt-driven, button-controlled
state machine (FSM) with LED output and a simple square‑wave generator.

What the Project Does
---------------------
• Uses external interrupts from push buttons to switch modes 
• Implements a simple FSM with three active modes + idle (sleep mode):
  1) Binary Count on LEDs — lights an 8‑LED array on Port 1 in increasing binary values, with
     a 0.5 s step. Total run time ~10 s per activation. When re‑entered, continues from the
     last LED value written. This mode is atomic (it finishes before another mode can start).
  2) Walking LED — a single LED moves left to right repeatedly with a 0.5 s step for ~5 s. On
     next activation, the walk continues from the previous LED position. This mode is atomic (it finishes before another mode can start).
  3) Square‑Wave Output — generates a square wave on a designated output pin with 70% duty cycle,
     first at ~1 kHz for 5 s, then at ~2 kHz for 5 s, and repeats. Frequency is set with
     instruction‑cycle resolution using a cycle counter. This mode ends when another button is pressed.
  0) Idle — LEDs off; the MCU enters a low‑power/sleep mode until the next button press.


Repository Files
----------------
• main.s43  — main program, ISRs, and the FSM control flow (IAR assembly).
• api.s43   — simple API used by the application (LED control, mode handling).
• hal.s43   — hardware abstraction (timers, GPIO, interrupts).
• bsp.s43   — board support (device/clock/ports setup).
• bsp.h     — pin/port definitions and small macros used by the BSP/HAL.
(If your wiring differs, update pin definitions in bsp.h accordingly.)

Target Hardware
---------------
• MCU: MSP430G2553 
• LEDs: connected to Port 1 (see bsp.h).
• Push buttons: connected to Port 2 pins PB0..PB3 with interrupt capability (see bsp.h).
• Square‑wave output: designated pin (e.g., P2.7) as defined in the code/BSP.
• Default core clock: MCLK.

How to Build & Flash (IDE Only)
-------------------------------
IAR Embedded Workbench for MSP430 
1) Create Workspace, then create a new MSP430 assembler project and select device: MSP430G2553.
2) Add main.s43, api.s43, hal.s43, bsp.s43, and bsp.h to the project.
3) Press build. Then go to Project and then Download and Debug (Ctrl+D).
   IAR will erase, program, and start a debug session on the MCU.
4) Press Run to start execution. To reprogram, stop the session and download again.


How to Use (Runtime)
--------------------
• After flashing, the program starts in Idle (LEDs off, low‑power). Press buttons to enter modes:
  – PB0: Binary Count mode (10 s window, 0.5 s step; resumes from last value; atomic).
  – PB1: Walking LED mode (5 s window, 0.5 s step; resumes from last LED; atomic).
  – PB2: Square‑Wave mode (70% duty cycle; ~1 kHz 5 s, then ~2 kHz 5 s; ends on next mode button).
• Press a different button to switch modes according to the FSM rules in code comments.
• Exact pin mapping for PBs/LEDs is defined in bsp.h.

Notes
-----
• This code is intended to be FLASHED to the MSP430G2553 MCU.
• The design is interrupt‑driven; do not use hardware RESET during normal operation.
• Keep wiring consistent with the pin macros in bsp.h (LEDs on Port 1, PBs on Port 2).
• For other MSP430 devices, update BSP/HAL layers.

License
-------
MIT 
