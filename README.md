## jitter
<p align="center">
  <img width="150" height="150" alt="Logo" src="https://github.com/user-attachments/assets/4a20b352-a1eb-4546-b3a6-d6c9f9fc675a" />
</p>

# Jitter: True Random Number Generator (TRNG)

This is a custom-made Application-Specific Integrated Circuit (ASIC) True Random Number Generator (TRNG), created as part of the Hack Club ASIC program. It was made using the Tiny Tapeout service on the GlobalFoundries 180nm (GF180MCU) process.

Instead of using a software-based Pseudo-Random Number Generator (PRNG), this design uses true randomness.

## How It Works

There are three main parts to this design.

### 1. Ring Oscillator

A ring oscillator is a feedback circuit made from an odd number of inverters. As there is an odd number of stages, there is no stable state, so it will continue to oscillate.

### 2. Physical Entropy / Jitter

In real hardware, we cannot have perfect periodicity in our signal. There are minor differences in how the chip is made, along with thermal noise and voltage variations, that cause the signal edges on our oscillator to vary with time. This variability is known as jitter and cannot be predicted.

### 3. Sampling / Entropy Extraction

The oscillating signal is applied to the data input of a D flip-flop. The clock signal for the flip-flop comes from the main ASIC clock. Because the oscillator is asynchronous to our main clock, the flip-flop will capture the state of the oscillator at a random point in its cycle. This gives us a true random bit.

This continues for a total of 8 clock cycles, giving us a random byte.

## Pinout

This design can be easily integrated.

| Pin Name | Description | Direction |
| :--- | :--- | :---: |
| `clk` | Main ASIC clock, used to sample the oscillator | Input |
| `rst_n` | Reset signal (active low), sets the output register to 0 | Input |
| `ui_in[0]` | Enable TRNG. The ring oscillator is active when high; when low, the oscillator is turned off to save power | Input |
| `uo_out[7:0]` | Random byte output, gives the most recent random 8-bit number | Output |

Unused inputs and bidirectional pins are grounded/disabled.

**Tile Size:** 1x1 (approx. 340 × 160 µm)
