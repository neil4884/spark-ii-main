# spark-ii-main
An Arduino-framework-based Teensy 4.0 software for SPARK-II rocket electronics for receivin and data logging telemetry of sensor data.

## Features
This software utilizes pseudo-asynchronous process to operate each type of operation in different phases and intervals. It also features Operational Mode, Simulation Mode (*Coming soon*), On-board SD Card Reading and DFU Mode (EEPROM Read/Write) via Serial command line.

## System States
There are three integers specifying the state of software:

#### 1. OS_STATE
`os_state` is for controlling software operation mode. Available states are:
1. `0` Operational State
2. `1` SD Directory listing
3. `2` Wait for User Input -> Filename
4. `3` Serial log Filename's File content
5. `253` Simulation mode (*Coming soon*)
6. `254` DFU Mode
7. `255` End of operation

#### 2. SS_STATE (*Coming soon*)
`ss_state` is for controlling overall stages and phases of suboperation. Available states are:
1. `0` Prelaunch Phase
2. `1` Ascending Phase
3. `2` At-Apogee Phase
4. `3` Deployment 1 Phase
5. `4` Deployment 2 Phase
6. `5` Final Descent Phase
7. `6` Touchdown Phase (End of operation)

#### 3. DFU_STATE
`dfu_state` is for controlling substates in DFU mode. Available states are:
1. `0` DFU-off
2. `1` DFU-on from off
3. `2` Serial log device_id
4. `4` Read EEPROM memory at memory index
5. `6` Write-update EEPROM memory at memory index with new value
6. `8` Clear EEPROM memory to ZERO (0)

## Commands
Commands can be written via Serial command line.
1. `START_OP` triggers update `os_state` to `0` in all conditions other than in SD Read Mode or DFU Mode (`os_state` is `2` or `254`).
2. `READ_SD` triggers update `os_state` to `1` in all conditions other than in SD Read Mode or DFU Mode (`os_state` is `2` or `254`). After `READ_SD`, user can input Filename indefinitely until triggered by `END_READ`.
3. `ENTER_DFU` triggers update `os_state` to `254` in all conditions other than in SD Read Mode or DFU Mode (`os_state` is `2` or `254`).
4. `END_OP` triggers update `os_state` to `255` in all conditions other than in SD Read Mode or DFU Mode (`os_state` is `2` or `254`).
5. `END_READ` triggers update `os_state` to `255` only when in SD Read Mode (`os_state` is `2`).
6. `SYS_CMD_REQUEST_DEVICE_ID` triggers update `dfu_state` to `2` only when enabled DFU Mode (`os_state` is `254` and `dfu_state` is not `0`).
7. `READ_EEPROM` triggers update `dfu_state` to `4` only when enabled DFU Mode (`os_state` is `254` and `dfu_state` is not `0`). After `READ_EEPROM`
