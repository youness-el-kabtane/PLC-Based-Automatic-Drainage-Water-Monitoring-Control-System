# PLC-Based Automatic Drainage Water Monitoring & Control System

## Project Overview
> The **PLC-Based Automatic Drainage Water Monitoring & Control System**, utilizes a **Siemens S7-300 PLC** and a **SIMATIC MP 177 Touch HMI** to automate water drainage. The system monitors **high and low-level sensors** to control a **drainage pump** and **valve** through both **manual and automatic modes**. It provides real-time status updates via **visual indicators** for equipment operation and water level thresholds

## Main Objectives
• **Automate water drainage** using a **Siemens S7-300 PLC**.
• **Monitor water levels** in real-time through **high and low-level sensors**.
• **Control hardware** (drainage pump and valve) using both **manual and automatic modes**.
• **Provide a user interface** for system interaction and visualization via a **Touch HMI**.
• **Display system status** through visual **indicators** for water levels and active equipment.

## System Components / Architecture
![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/b5ea8c631262b871750e6b592fcff2d1a35cbf38/images/image1.png)

**1. Central Controller**

• **Siemens SIMATIC S7-300 PLC:** The core of the system is a **CPU314C-2 PN/DP**, which handles all processing logic, input monitoring, and output commands.

**2. Human-Machine Interface (HMI)**

![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/02ee5d285b4abc1c5a050930d5cfa2a8c55b71b0/images/image2.png)

• **SIMATIC Multi Panel MP 177 6" Touch:** This device provides the visual interface for the operator. It features a **Dashboard** for real-time monitoring and an **About** screen for project information.

**3. Input Components (Sensing)**

• **High-Level Sensor :** Detects when the water reaches the maximum threshold.
• **Low-Level Sensor :** Detects when the water is at the minimum threshold.
• **HMI Controls:** Virtual buttons for **Manual**, **Automatic**, and **Stop** commands.

**4. Output Components (Actuators)**

• **Drainage Pump:** Driven by a motor (M1) to remove water from the system.
• **Control Valve:** Manages the flow of water during the drainage process.

**5. Visual Indicators**

The system provides real-time status feedback through dedicated light indicators:

• **Pump Indicator**.
• **Valve Indicator**.
• **High-Level Indicator**.
• **Low-Level Indicator**.

## Variables Tables
![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/b0a13ab6f3fd4d5163b3e6ac6ed0a0a6b8aca347/images/image3.png)

## Control Logic
![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/edc7624e5301062cc05b6eaca76fa0c54bc9b60c/images/image4.png)

• **Network 1 (Manual Option):** This network establishes the **Manual mode (M0.6)**. It is activated when the **Stop button (M0.0)** is not pressed and the system is not in **Automatic mode (M0.2)**. It latches the manual state when the operator selects it on the HMI.

![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/a7515eafe211b152a1f18f72ab1c96268bc516da/images/image5.png)

• **Network 2 (Automatic Option):** This network establishes the **Automatic mode (M0.7)**. Similar to Network 1, it requires the **Stop button (M0.0)** to be inactive and the system not to be in **Manual mode (M0.1)**. It latches the automatic state upon user selection.

**Valve and Pump Control Logic**
![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/f2815b90ec3f275ea3add45e39e6b2e60569d405/images/image6.png)

• **Network 3 (Valve Manual/Automatic Control):** This network controls the **Valve (Q0.1)** using an internal marker (M1.1). In **Manual mode**, the valve opens if the **Valve_Open (M0.4)** command is active and the **Low_level_Sensor (I0.1)** detects water. In **Automatic mode**, the valve opens once both the **Second (M1.2)** and **Minute (M1.3)** timers have elapsed. It is reset (closed) by the **Stop command**, the **Valve_Close** manual command, or if level conditions are no longer met.

![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/37185bc67fed13fe4364fc37968d46dd5439578d/images/image7.png)

• **Network 4 (Pump Manual Control):** In **Manual mode (M0.6)**, this network allows direct operation of the **Pump (Q0.0)** via the **Pump_Control (M0.3)** HMI command. The pump is automatically reset if the **High_level_Sensor (I0.0)** is triggered, the **Stop** command is given, or if the **Valve (Q0.1)** is currently open.

![enter image description here](https://github.com/youness-el-kabtane/PLC-Based-Automatic-Drainage-Water-Monitoring-Control-System/blob/f8e80a7a73bd7e575ec13f6e7347c9d0d878fdba/images/image8.png)

• **Network 5 (Pump Automatic Control):** In **Automatic mode (M0.7)**, this network manages the pump autonomously. It sets the pump state based on specific water level thresholds detected by the sensors. The pump is reset if the **High_level_Sensor** is reached, **Stop** is pressed, or the **Valve** is active.

**Timing & Data Conversion**
![enter image description here](#)
• **Network 6 & 7 (Second and Minute Timers):** These networks utilize **S5 Timers (T0 and T1)** to manage timed operations during **Automatic mode**. They start when the mode is active and the pump is off. **T0** generates the **Second (M1.2)** signal, while **T1** generates the **Minute (M1.3)** signal.

![enter image description here](#)
• **Network 8 & 9 (Get Second/Minute Value):** These networks handle the conversion of user-defined time settings from the HMI. **Network 8** converts seconds (**MD10**) and **Network 9** converts minutes (**MD20**) into milliseconds. They both call **FC40 (TIM_S5TI)** to transform the **IEC Time** format into **S5 Time** values (stored in **MW40** and **MW60**) that the PLC timers can process.

**Status Indicators**
![enter image description here](#)
• **Network 10 (Valve_Indicator):** Directly maps the status of the **Valve (Q0.1)** to the physical **Valve_Indicator (Q0.3)**.

• **Network 11 (Pump_Indicator):** Directly maps the status of the **Pump (Q0.0)** to the physical **Pump_Indicator (Q0.2)**.

• **Network 12 (Low_level_Indicator):** Maps the status of the **Low_level_Sensor (I0.1)** to the **Low_level_Indicator (Q0.5)**.

• **Network 13 (High_level_Indicator):** Maps the status of the **High_level_Sensor (I0.0)** to the **High_level_Indicator (Q0.4)**.

---
**Author:**  Youness El Kabtane

**Website:**  [younesselkabtane](https://youness-el-kabtane.github.io/site/)

**Version:**  1.0.0

**Made with 💗**

