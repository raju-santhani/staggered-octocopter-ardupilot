# Firmware Modification for a Staggered Octocopter

## Objective

Modify ArduPilot's motor-mixing configuration to support an eight-motor staggered aircraft configuration.

## Original Firmware

- **Project:** ArduPilot
- **Upstream repository:** https://github.com/ArduPilot/ardupilot
- **Version or commit used:** [To be added]
- **Flight controller:** Orange Cube

## Modified Source File

`libraries/AP_Motors/AP_MotorsMatrix.cpp`

## Location of the Change

- **Function:** `AP_MotorsMatrix::setup_octaquad_matrix(motor_frame_type frame_type)`
- **Frame class:** `OCTAQUAD`
- **Modified case:** `MOTOR_FRAME_TYPE_X`
- **Compilation guard:** `AP_MOTORS_FRAME_OCTAQUAD_ENABLED`
- **Frame settings used during testing:** Choose Octaquad as frame type

The existing OCTAQUAD X motor-definition block was replaced with custom definitions for the staggered eight-motor configuration.

The modification retains the existing `"X"` frame label and selection. In this modified firmware, selecting OCTAQUAD X uses the custom motor definitions; a separate selectable frame type was not added.

## Original Code

The original OCTAQUAD X implementation used `MotorDef` entries and registered the motors through `add_motors()`.

```cpp
case MOTOR_FRAME_TYPE_X: {
    _frame_type_string = "X";

    static const AP_MotorsMatrix::MotorDef motors[] {
        {   45, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 1 },
        {  -45, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  7 },
        { -135, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 5 },
        {  135, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  3 },
        {  -45, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 8 },
        {   45, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  2 },
        {  135, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 4 },
        { -135, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  6 },
    };

    add_motors(motors, ARRAY_SIZE(motors));
    break;
}
```

## Modified Code

The modified implementation uses `MotorDefRaw` entries and registers the motors through `add_motors_raw()`.

```cpp
case MOTOR_FRAME_TYPE_X: {
    _frame_type_string = "X";

    static const AP_MotorsMatrix::MotorDefRaw motors[] {
        {   26.565,  -63.435, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 1 }, // Motor 1
        {  -63.435, -153.435, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  7 }, // Motor 2
        { -153.435,  116.565, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 5 }, // Motor 3
        {  116.565,   26.565, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  3 }, // Motor 4
        {  -26.565, -116.565, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 8 }, // Motor 5
        {   63.435,  -26.565, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  2 }, // Motor 6
        {  153.435,   63.435, AP_MOTORS_MATRIX_YAW_FACTOR_CCW, 4 }, // Motor 7
        { -116.565,  153.435, AP_MOTORS_MATRIX_YAW_FACTOR_CW,  6 }, // Motor 8
    };

    add_motors_raw(motors, ARRAY_SIZE(motors));
    break;
}
```

## Scope of the Change

The modification changes the motor-definition structure, numerical entries, and motor-registration call within the existing OCTAQUAD X case.

The code above documents the replacement block. It is not a complete firmware file or a standalone program.

The meaning and scaling of the numerical fields should be documented against the `MotorDefRaw` definition and `add_motors_raw()` implementation in the firmware revision used.

## Motor Layout and Mixing Derivation

Refer to `Motor_layount.jpg` for the aircraft's motor layout.

A detailed explanation of the mixing derivation, coordinate convention, and relationship between the diagram numbering and code entries will be added.

## Validation

Ground and flight testing demonstrated stable flight with the modified firmware under the tested conditions.

Refer to the repository's `references` folder for available flight logs or video.

## Acknowledgement

This modification builds on ArduPilot. The underlying firmware was developed by the ArduPilot contributors.

Upstream project: https://github.com/ArduPilot/ardupilot
