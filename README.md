# Vibration Analysis System using MSA301 & STM32F401RE

Real‑time machinery health monitoring with triaxial accelerometer, time‑domain features, FFT, and fault detection.

## Features
- High‑speed data acquisition: MSA301 @ 1 kHz over I2C
- STM32 firmware with timer‑triggered reading and binary UART streaming
- Python real‑time analysis:
  - Live time‑domain plot
  - RMS, Peak, Crest Factor, Kurtosis
  - FFT spectrum with Hanning window
  - Fault detection (unbalance, misalignment, looseness, bearing wear)

## Hardware Wiring
| MSA301 | NUCLEO‑F401RE (ARDUINO®) | STM32 Pin |
|--------|--------------------------|-----------|
| VIN    | CN6 pin 5 (+5V)          | –         |
| GND    | CN6 pin 6                | –         |
| SCL    | CN8 pin 3                | PB8       |
| SDA    | CN8 pin 5                | PB9       |
| TX     | CN8 pin 22               | PB2       |
| RX     | CN8 pin 31               | PB3       |

## Data Flow & Processing
1.	STM32 reads MSA301 every 1 ms (1000 Hz) in a timer ISR.
2.	Raw 14 bit values are converted to 16 bit signed integers and sent over UART (6 bytes per sample, little endian).
3.	Python script:
   a.	Reads binary data from serial port.
   b.	Maintains a sliding window of 512 samples.
   c.	Plots live acceleration waveform.
   d.	Computes RMS, peak, crest factor, kurtosis.
   e.	Performs FFT and displays spectrum.
   f.	Flags anomalies based on predefined thresholds.

