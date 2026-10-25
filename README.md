# ArduPilot Support for a Staggered Octocopter

Custom motor mixing and ArduPilot firmware modification for an eight-motor staggered UAV configuration.

## Project Overview

This project explored a staggered rotor arrangement as an alternative to a coaxial configuration. My work included propulsion testing, developing a custom motor-mixing configuration, modifying ArduPilot firmware, and conducting ground and flight tests.

## My Contributions

- Compared staggered and coaxial rotor configurations through thrust-stand testing.
- Measured 12% higher propulsion efficiency for the staggered configuration under the tested conditions.
- Developed a custom motor-mixing configuration for the eight-motor layout.
- Modified ArduPilot firmware to incorporate the configuration.
- Validated stable flight through ground and flight testing.

## Firmware Modification

The modification was made in:

`libraries/AP_Motors/AP_MotorsMatrix.cpp`

The implementation defines the motor-mixing entries for the staggered configuration and registers them with ArduPilot.

## Project Documentation

This repository will document:

- Aircraft geometry and motor layout
- Motor-mixing implementation
- Firmware version and setup
- Propulsion test methods and results
- Ground and flight validation

## Project Status

Ground and flight testing completed. Supporting documentation and test results are being added.

## Acknowledgement

This project builds on the ArduPilot open-source autopilot software.

ArduPilot: https://github.com/ArduPilot/ardupilot
