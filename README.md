<div align="center">
  <h1>Enterprise Microwave QA Automation Framework</h1>
  <h3>Industrial Hardware-in-the-Loop (HIL) Testing & Telemetry Analysis</h3>

  <p>
    <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
    <img src="https://img.shields.io/badge/PyQt5-41CD52?style=for-the-badge&logo=qt&logoColor=white" alt="PyQt5" />
    <img src="https://img.shields.io/badge/nidaqmx-0055A5?style=for-the-badge&logo=nationalinstruments&logoColor=white" alt="NI DAQmx" />
    <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy" />
    <img src="https://img.shields.io/badge/PyQtGraph-8CAAEE?style=for-the-badge&logo=python&logoColor=white" alt="PyQtGraph" />
    <img src="https://img.shields.io/badge/OpenPyXL-150458?style=for-the-badge&logo=microsoftexcel&logoColor=white" alt="Excel Data Export" />
  </p>
</div>

## Executive Summary
The **Microwave Automation Testing System** is an industrial-grade Python software infrastructure engineered to execute rigorous autonomous Quality Assurance (QA) on microwave oven hardware. Utilizing a native architecture bound directly to physical DAQ (Data Acquisition) hardware, the system dynamically ingests real-time, multi-channel telemetry—including Microwave, Grill, Lamp, Door Switch, and Buzzer signals—to enforce absolute operational compliance and safety standards.

By integrating complex deterministic State Machines and mathematically rigorous Sector-Based Defrost analytical logic, the platform completely automates Pass/Fail validation criteria and translates high-frequency raw signal data into comprehensive, enterprise-ready Excel diagnostic matrices.

---

## 1. Hardware-Software Operational Topology
The system abstracts physical hardware intricacies through a strictly partitioned codebase, ensuring UI responsiveness remains uncompromised during high-frequency DAQ polling sequences.

```mermaid
graph TD
    subgraph "External Hardware (Microwave Lab)"
        HW[Physical Relays & Sensors] -->|Analog/Digital IO| DAQ[NI DAQ Acquisition Node]
    end

    subgraph "Data Parsing Core (daq_handler.py)"
        DAQ --> BUFFER[Telemetry Timestamp Buffer]
        BUFFER --> CALC[Power Output Percentage Engine]
        BUFFER --> DEFROST[Sector-Based Defrost Logic]
        CALC --> PASS_FAIL{Industrial Pass/Fail Validator}
        DEFROST --> PASS_FAIL
    end

    subgraph "Presentation & State (main.py)"
        PASS_FAIL --> GUI[PyQt5 Command Center UI]
        PASS_FAIL --> REPORT[ExcelWriter Node]
        GUI --> STATE[state_machine.py Context Controller]
        STATE --> BUFFER
    end

    classDef hardware fill:#450a0a,stroke:#ef4444,stroke-width:2px,color:#fff;
    classDef processing fill:#172554,stroke:#3b82f6,stroke-width:2px,color:#fff;
    classDef presentation fill:#14532d,stroke:#10b981,stroke-width:2px,color:#fff;

    class HW,DAQ hardware;
    class BUFFER,CALC,DEFROST,PASS_FAIL processing;
    class GUI,REPORT,STATE presentation;
```

---

## 2. Core Architectural Components

### Presentation & UX
- `main.py`: The primary asynchronous controller bridging the user execution thread to the underlying hardware polling loops. It manages event logging, dynamic configuration of thresholds, and updates real-time `pyqtgraph` visual statistics without interface latency.
- `DefrostDialog`: A specialized computational UI class initializing user weight parameters to dynamically project and enforce rigorous chronological sector milestones.

### Acquisition & Analytics
- `daq_handler.py`: The computational nucleus. Connects via `nidaqmx` to orchestrate multi-channel parsing. It calculates strict power percentages bounded by configurable micro-windows and identifies critical industrial safety violations (e.g., catastrophic simultaneous MW+Grill signal overlaps, or Door-Open logic breaks).
- `config.py`: The centralized constants parameterization vault. Isolates device channel mappings, deterministic tolerance thresholds, sector definitions, and GUI theme directives strictly away from execution logic.

### Reporting & State Governance
- `excel_writer.py`: Programmatically interfaces with `openpyxl` to autonomously construct highly formatted Excel workbooks. It flushes raw arrays, statistical summaries, and sector-by-sector Pass/Fail matrices into immutable industrial documentation formats.
- `state_machine.py`: Enforces operational immutability. Maps transitional matrices (Idle, Running, Paused) to prevent chaotic execution cycles and hard-stops the hardware processes during safety interrupts like Child Lock engagements.

---

## 3. Sector-Based Defrost Algorithmic Execution
Due to the non-linear thermal dynamics of industrial microwave defrosting algorithms, the system divides temporal executions into three strict validation sectors.

```mermaid
sequenceDiagram
    participant Technician
    participant StateMachine
    participant DAQ_Engine
    participant ExcelWriter
    
    Technician->>StateMachine: Input Sample Mass (kg) & Initiate Defrost
    StateMachine->>DAQ_Engine: Dispatch Calculated Sector [1, 2, 3] Constraints
    
    loop Sector Validation Cycle (High-Frequency Polling)
        DAQ_Engine->>DAQ_Engine: Buffer Telemetry & Calculate Power Integration
        
        alt Actual Power Ratio == Configured Expected Power
            DAQ_Engine-->>DAQ_Engine: Append Sector Flag: PASS
        else Power Boundary Violation Detected
            DAQ_Engine-->>DAQ_Engine: Append Sector Flag: FAIL (Log Variance)
        end
    end
    
    StateMachine->>ExcelWriter: Signal Completion Matrix Flush
    ExcelWriter-->>Technician: Export Absolute Diagnostics Workbook
```

---

## ⚙️ Environment Setup & System Deployment

To deploy the DAQ automation suite securely across laboratory environments:

**1. Virtual Execution Environment Initialization**
```bash
python -m venv .venv
# Activate environment (Windows)
.venv\Scripts\activate
```

**2. Satisfy Dependency Graphs**
```bash
pip install -r requirements.txt
```

**3. Hardware Calibration & Execution**
- Physically interface channels according to mappings strictly defined in `config.py`.
- Initialize the primary control application:
```bash
python main.py
```

### Engineered & Architected by Ziad Emad
*Establishing uncompromising industrial QA standards through deterministic software architecture.*
