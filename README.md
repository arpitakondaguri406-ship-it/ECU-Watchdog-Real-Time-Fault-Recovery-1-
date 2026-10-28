# ECU Watchdog: Real-Time Fault Detection & Recovery

## 📌 Overview

**ECU Watchdog: Real-Time Fault Detection & Recovery** is an embedded systems project designed to monitor simulated Electronic Control Unit (ECU) processes using the **QNX Neutrino Real-Time Operating System (RTOS)**.

The system consists of three simulated ECU processes:

- **Brakes ECU** — represents a safety-critical vehicle control unit.
- **Engine ECU** — represents an engine control unit.
- **Infotainment ECU** — represents a non-safety-critical vehicle subsystem.

A watchdog supervisor monitors heartbeat messages from these ECU processes. If an ECU stops sending heartbeats and exceeds the configured timeout, the watchdog identifies the potentially unresponsive process and attempts to restart it independently.

The primary objective is to demonstrate **heartbeat-based fault detection, process monitoring, and targeted recovery** in a real-time operating system environment.

> **Project status:** Prototype under development. Features and test results should be updated as implementation and validation progress.

## 🎯 Objectives

- Monitor multiple simulated ECU processes.
- Implement heartbeat-based health monitoring.
- Detect unresponsive processes using timeout-based fault detection.
- Identify the ECU associated with a missing heartbeat.
- Attempt to restart the affected process without restarting the entire system.
- Verify recovery by checking whether heartbeat communication resumes.
- Evaluate system behavior under controlled fault scenarios.

## 🏗️ System Architecture

```text
        +----------------+   +----------------+   +----------------------+
        |   Brakes ECU   |   |   Engine ECU   |   |   Infotainment ECU   |
        +--------+-------+   +--------+-------+   +----------+-----------+
                 |                    |                      |
                 +--------------------+----------------------+
                                      |
                              Heartbeat Messages
                                      |
                                      v
                         +-------------------------+
                         |   Watchdog Supervisor   |
                         +------------+------------+
                                      |
                                      v
                         +-------------------------+
                         | Heartbeat Timeout Check |
                         +------------+------------+
                                      |
                                      v
                         +-------------------------+
                         | Identify Failed ECU     |
                         +------------+------------+
                                      |
                                      v
                         +-------------------------+
                         | Attempt Process Restart |
                         +------------+------------+
                                      |
                                      v
                         +-------------------------+
                         | Verify Heartbeat Return |
                         +-------------------------+
```

## ⚙️ Working Principle

### 1. ECU Process Initialization
The Brakes, Engine, and Infotainment ECU processes are launched as separate processes in the QNX environment.

### 2. Heartbeat Generation
Each ECU is intended to send periodic heartbeat messages to indicate that it is still communicating with the watchdog.

### 3. Heartbeat Monitoring
The watchdog tracks the most recent heartbeat received from each ECU.

### 4. Timeout Detection
The watchdog periodically checks whether an ECU's heartbeat has exceeded the configured timeout threshold.

### 5. Fault Identification
If an ECU fails to send a heartbeat within the allowed interval, the watchdog flags that ECU as potentially unresponsive.

### 6. Targeted Recovery
The watchdog attempts to restart the affected ECU process instead of restarting all ECU processes or the entire operating system.

### 7. Recovery Verification
The system should confirm recovery only after receiving a valid heartbeat from the restarted ECU.

## ⏱️ Configured Timing Parameters

| Parameter | Configured Value | Description |
|---|---:|---|
| Heartbeat interval | 100 ms | Intended interval between heartbeat messages |
| Watchdog check interval | 50 ms | Intended interval between health checks |
| Heartbeat timeout | 300 ms | Threshold for identifying a potentially unresponsive ECU |

**Note:** These values represent the configured design parameters. Actual detection latency and recovery time must be measured through testing.

## 🛠️ Technologies and Concepts

Update this list to reflect the technologies actually used in your implementation.

- QNX Neutrino RTOS
- C programming language
- Inter-Process Communication (IPC)
- Heartbeat-based monitoring
- Timeout-based fault detection
- Process management
- Fault recovery
- Real-time systems concepts
- Fault injection and recovery testing

## 📂 Suggested Repository Structure

```text
ECU-Watchdog-Real-Time-Fault-Recovery/
│
├── README.md
├── src/
│   ├── watchdog/
│   ├── brakes_ecu/
│   ├── engine_ecu/
│   └── infotainment_ecu/
│
├── docs/
│   ├── architecture/
│   └── test-results/
│
└── .gitignore
```

The directory structure is a suggestion. Keep the actual repository organized according to your source files and project configuration.

## 🚀 Build and Run

The exact build and execution commands depend on the QNX version, project configuration, target environment, and executable paths.

### General Workflow

1. Install and configure the QNX Software Development Platform.
2. Open the project in QNX Momentics IDE.
3. Build the ECU processes and watchdog application.
4. Verify that the executable files were generated successfully.
5. Confirm that the executable paths in the launch code match the files deployed to the QNX target.
6. Launch the ECU processes and watchdog in the required order.
7. Observe heartbeat communication during normal operation.
8. Introduce a controlled process failure and observe the watchdog response.
9. Check whether the affected process restarts and heartbeat communication resumes.

> Add your exact, tested build and run commands here once the project runs successfully in your QNX environment.

## 🧪 Testing and Validation

The following tests can be used to evaluate the prototype.

| Test Case | Expected Behavior |
|---|---|
| Normal operation | ECU processes send periodic heartbeats |
| ECU process termination | Watchdog detects missing heartbeats after the timeout condition is met |
| Heartbeat timeout | Watchdog identifies the potentially unresponsive ECU |
| Restart attempt | Watchdog attempts to restart the affected process |
| Recovery verification | A valid heartbeat resumes after successful recovery |
| Unaffected ECU monitoring | Other ECU processes continue operating |
| Repeated failure | Restart behavior and failure handling are evaluated |

Actual test outcomes should be documented after each test is performed.

## 📊 Performance Evaluation

The following metrics can be collected during testing:

- **Fault detection latency:** Time between fault injection and watchdog detection.
- **Restart attempt latency:** Time between detection and the restart attempt.
- **Observed recovery time:** Time between fault injection and the first valid heartbeat after recovery.
- **Process availability:** Whether unaffected ECU processes remain responsive.
- **Recovery success rate:** Number of successful recoveries divided by the total number of recovery attempts.

These metrics should be calculated from recorded test results rather than estimated from the configured timeout values.

## 🔮 Future Enhancements

- Implement repeatable fault-injection scenarios.
- Add structured event logging and automated performance measurements.
- Evaluate multiple simultaneous ECU failures.
- Implement bounded restart retries and failure handling.
- Measure fault detection and recovery latency under different workloads.
- Evaluate scheduling and resource-isolation techniques on a supported QNX target.
- Extend the prototype to an appropriate hardware platform after simulation validation.
- Develop a monitoring dashboard for visualizing ECU status and recovery events.

## ⚠️ Limitations

- This is an educational prototype, not a production-ready automotive safety system.
- A heartbeat confirms communication, but does not guarantee that an ECU is functioning correctly internally.
- Restarting a process does not guarantee that recovery will succeed.
- Simulation results do not automatically establish performance on physical automotive hardware.
- Real-time scheduling, resource isolation, and hardware integration should only be described as implemented when supported by code and test evidence.

## 🔒 Safety Disclaimer

This project is intended for educational, research, and demonstration purposes only. It must not be used to control a real vehicle or any safety-critical system. No automotive functional-safety certification or production-level reliability is implied.

## 👩‍💻 Author

**Author:** [Your Name]  
**Institution:** [Your College Name]  
**Project:** ECU Watchdog — Real-Time Fault Detection & Recovery

## 📄 License

A license has not yet been selected. If you intend to make the project publicly available for reuse or modification, choose an appropriate open-source license and add the corresponding license file to the repository.
