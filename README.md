# SPI Master Verification Project

A UVM-based SystemVerilog verification environment for an **SPI Master DUT** connected through an **APB interface**. Built for the CSE313s (UG2018) Verification course at Ain Shams University, Faculty of Engineering — Team 16.

## Overview

This project verifies an SPI Master design against its specification using a mix of directed tests, constrained-random tests, functional coverage, SystemVerilog Assertions (SVA), Bus Functional Models (BFMs), and a reference model / scoreboard. The goal is to validate:

- SPI protocol correctness (all modes, widths, loopback)
- APB register read/write operations and reset behavior
- FIFO functionality under stress
- Interrupt generation, masking, and W1C clearing
- Clock divider and timing behavior
- Corner cases and illegal/error scenarios
- Stability under long randomized regressions

## Verification Flow

1. Configure the DUT via the APB interface.
2. Generate SPI transactions.
3. Drive transactions using BFMs.
4. Monitor SPI activity (MOSI, MISO, SCLK, CS).
5. Compare DUT outputs against the reference model.
6. Collect functional coverage.
7. Validate timing/protocol correctness via assertions.

## Testcases

| Category | Test | Description |
|---|---|---|
| Sanity | `sanity_test.sv` | Basic end-to-end SPI transfer validation |
| Register | `reg_access_test.sv` | APB register read/write, permissions, reset values |
| Register | `randomized_reg_access_test.sv` | Randomized APB register access, illegal addresses |
| Register | `ral_hw_reset_test.sv` | RAL-based hardware reset value checks |
| Protocol | `width_coverage_test.sv` | 8/16/32-bit transfer width coverage |
| Protocol | `randomized_width_coverage_test.sv` | Randomized widths + payloads |
| Protocol | `mode_coverage_test.sv` | All SPI CPOL/CPHA mode combinations |
| Protocol | `loopback_test.sv` | Internal TX→RX loopback data integrity |
| Protocol | `delay_transfer_test.sv` | Inter-transfer delay / idle stability |
| Stress | `fifo_stress_test.sv` | FIFO full/empty/overflow/underflow under heavy traffic |
| Interrupt | `interrupt_test.sv` | TX_EMPTY, RX_FULL, TX_OVF, RX_OVF, TRANSFER_DONE, IRQ, W1C, race conditions |
| Corner Case | `clk_div_corner_test.sv` | Clock divider min/max corner cases |
| Corner Case | `error_injection_test.sv` | Illegal configs, reserved addresses, FIFO error scenarios |
| Regression | `randomized_sanity_test.sv` | Long randomized end-to-end regression |

## Coverage & Assertions

**Functional coverage** (`coverage.sv`, `coverage2_regfile.sv`, `coverage_spi_core.sv`) tracks SPI modes, widths, bit ordering, loopback, FIFO states, interrupts, APB register accesses, clock divider/delay configurations, and key cross-coverage combinations.

**SVA** (`spi_sva.sv`) checks reset values, enable/disable behavior, FIFO status, SS_n timing, APB protocol timing, interrupt/W1C behavior, overflow flags, reserved-address behavior, SCLK polarity/divider timing, BUSY signal behavior, loopback routing, and transfer-gap insertion.

## Running the Regression

```bash
# from MSYS2
make clean
make compile
make regress
make cov
```

## Results

The environment combined directed and randomized testing, SVA, functional coverage, and reference-model checking to achieve strong confidence in SPI protocol compliance, register functionality, FIFO robustness, interrupt handling, timing correctness, and overall DUT stability.

## Team 16

| Name | ID |
|---|---|
| Ahmed Tarek Hassanien | 2200772 |
| Mahmoud Ahmed Gamal | 2200433 |
| Sherif Ahmed Abdelfatah | 2200176 |
| Ahmed Emad Mohamed | 2201374 |
| Nour Ahmed Khalaf | 2200176 |
| Mariam Tarek Samir | 2200167 |
| Rana Gamal Reda | 2200749 |

*Course: CSE313s — Verification (UG2018), Ain Shams University Faculty of Engineering*
