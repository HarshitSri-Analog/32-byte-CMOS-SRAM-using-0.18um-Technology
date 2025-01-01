# 32-byte CMOS SRAM design using 0.18µm Technology

## Overview
This repository contains the design, simulation, and analysis of a 32-byte CMOS SRAM implemented using 0.18µm CMOS technology. The project includes detailed schematics, layouts, and simulation results to provide insights into SRAM operation, design considerations, and performance metrics.

## Table of Contents
1. [Introduction](#introduction)
   - [Applications](#applications)
   - [Design Considerations](#design-considerations)
2. [Components of SRAM](#components-of-sram)
   - [6T SRAM Cell](#6t-sram-cell)
   - [Precharge Circuit](#precharge-circuit)
   - [Address Decoder](#address-decoder)
   - [Column Multiplexer/Decoder](#column-multiplexerdecoder)
   - [Data Driver](#data-driver)
   - [Control Logic Circuitry](#control-logic-circuitry)
   - [Sense Amplifier](#sense-amplifier)
3. [Working/Operation of SRAM](#workingoperation-of-sram)
   - [Read Operation](#read-operation)
   - [Write Operation](#write-operation)
4. [Test Bench of SRAM](#test-bench-of-sram)
5. [Delay Characterization](#delay-characterization)
   - [Precharge Delay](#precharge-delay)
   - [Read Delay](#read-delay)
   - [Write Delay](#write-delay)
6. [Results](#Results)
7. [Conclusion](#conclusion)
8. [Acknowledgement](#acknowledgement)


## Introduction
Static Random Access Memory (SRAM) is a critical component in modern digital systems, offering high-speed and low-power storage solutions. This project focuses on the design and simulation of a 32-byte SRAM using 0.18µm CMOS technology, emphasizing stability and efficiency.

### Applications
- Cache memory in processors
- High-speed data buffers
- Embedded memory in FPGAs and ASICs
- Networking and communication systems

### Design Considerations
- **Technology Node**: 0.18µm CMOS technology for balanced performance and power consumption.
- **Power Consumption**: Optimization for low static and dynamic power.
- **Speed**: Minimization of access and write delays.
- **Stability**: Ensuring proper operation under varying supply voltages and temperatures.
- **Area**: Efficient layout to minimize silicon real estate.

## Components of SRAM
### 6T SRAM Cell
The 6T SRAM cell is the fundamental storage unit in SRAM. It consists of:
1. **Two Cross-Coupled Inverters**: These form a bistable circuit, storing a single bit of data as either a logic '0' or '1'.
2. **Two Access Transistors**: These connect the storage node to the bitlines during read and write operations. The gates of the access transistors are controlled by the wordline signal.
   - **Key Features**:
     - Provides high-speed data access.
     - Low static power consumption due to the bistable nature of inverters.
     - Retains data as long as power is supplied.

| ![SRAM_Bitcell](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM_1bit.png) | 
| :---: | 
| Fig 1: Bitcell of 6T SRAM |

### Precharge Circuit
The precharge circuit ensures that both bitlines are precharged to a known voltage (typically VDD/2 or VDD) before a read or write operation. This is crucial for:
- **Consistency**: Reduces the influence of previous operations.
- **Efficiency**: Precharging helps in minimizing the time required for the bitlines to achieve differential voltage during read.
- **Components**:
  - PMOS transistors connected to the bitlines.
  - A control signal (Precharge Enable) to activate the precharging process.

| ![PreCharge](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/PrechargeCkt.png) | 
| :---: | 
| Fig 2: Precharge Circuit |

### Address Decoder
The address decoder is responsible for selecting the specific row and column corresponding to the memory location to be accessed. It includes:
1. **Row Decoder**:
   - Decodes the input address bits to activate a specific wordline.
   - Uses a combination of logic gates (NOT, NAND) to generate selection signals.
2. **Column Decoder**:
   - Selects the appropriate column or bitline pair based on the address bits.
   - Works in conjunction with the multiplexer for efficient column selection.

| ![Row decoder](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/Row_Decoder.png) | 
| :---: | 
| Fig 3: Row Address Decoder |

### Column Multiplexer/Decoder
The column multiplexer or decoder further narrows down the memory cell selection by:
- Connecting the selected bitline pair to the sense amplifier or data driver.
- Reducing complexity in multi-bit access.
- Ensuring correct routing of signals for read and write operations.

| ![ColDecoder](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/ColunmDecoder.png) | 
| :---: | 
| Fig 4: Column MUX/Decoder |

| ![ColDecoderArray](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/ColunmDecoderArray.png) | 
| :---: | 
| Fig 5: Column MUX/Decoder Array |

### Data Driver
The data driver is a key component during write operations, ensuring the correct data is written into the memory cell:
- **Write Operation**:
  - Drives the bitlines with the desired data.
  - Overcomes the pull-up and pull-down strengths of the storage cell.
- **Implementation**:
  - Uses strong NMOS/PMOS transistors to ensure fast data driving.

| ![DataDrive](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/Data_Driver.png) | 
| :---: | 
| Fig 6: Write Driver |

### Control Logic Circuitry
The control logic manages four key signals used in this design for synchronized operation:
- **col_a4_rd/rdb**: Reads data from the Most Significant Byte (MSB) of the memory array.
- **col_a4b_rd/rdb**: Reads data from the Least Significant Byte (LSB) of the memory array.
- **col_a4_wr**: Writes data into the MSB of the memory array.
- **col_a4b_wr**: Writes data into the LSB of the memory array.

| ![Control](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/ControlLogicCkt.png) | 
| :---: | 
| Fig 7: Control Logic Circuitory |
   
This simplified signal structure ensures efficient data access and manipulation for the memory array, streamlining read and write processes.

### Sense Amplifier
The sense amplifier is essential for read operations, amplifying the small voltage difference between the bitlines:
- **Components**:
  - A 5-transistor differential amplifier forms the core, ensuring precise detection of bitline voltage differences.
  - At the output of the differential amplifier, two CMOS inverters adjust the output logic levels to achieve complete logic '1' or '0'.
- **Operation**:
  - The differential amplifier detects even minute differences between bitlines with high sensitivity.
  - The CMOS inverters stabilize and amplify the differential output to standard logic levels.
- **Advantages**:
  - High-speed operation.
  - Robust noise immunity during reads.

| ![SenseAmpr](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/Sense_Ampr_updated_with_Voltages.png) | 
| :---: | 
| Fig 8: Sense Amplfier (with voltages at each node) |

## Working/Operation of SRAM
### Read Operation
The read operation in SRAM involves extracting stored data without altering its contents. The sequence of events is as follows:

1. **Precharging**:
   - Both bitlines are precharged to a known voltage (usually VDD) using the precharge circuit.
   - This ensures a consistent starting point for the operation.

2. **Row and Column Selection**:
   - The address decoder activates the wordline corresponding to the row containing the desired cell.
   - Simultaneously, the column multiplexer selects the appropriate bitline pair (either MSB or LSB), guided by signals like `col_a4_rd/rdb` or `col_a4b_rd/rdb`.

3. **Data Access**:
   - The activated wordline turns on the access transistors of the selected cell, connecting the cell’s storage nodes to the bitlines.
   - Depending on the stored data, one bitline discharges slightly while the other remains at the precharged level.

4. **Sensing**:
   - The differential amplifier-based sense amplifier detects the small voltage difference between the bitlines.
   - This difference is amplified by the differential amplifier, and the CMOS inverters ensure the final output reaches logic level 0 or 1.

5. **Data Output**:
   - The amplified signal is routed to the data driver, which sends it to the output bus.

### Write Operation
The write operation involves updating the data stored in a specific cell. The steps are as follows:

1. **Precharging**:
   - Similar to the read operation, the bitlines are precharged to VDD initially.

2. **Row and Column Selection**:
   - The address decoder activates the wordline of the target row.
   - The column multiplexer selects the bitline pair corresponding to the target byte (MSB or LSB), using `col_a4_wr` or `col_a4b_wr` signals.

3. **Data Driving**:
   - The data driver asserts the input data onto the selected bitlines.
   - For example, a logic '1' drives one bitline to VDD and the other to 0V.

4. **Storage Update**:
   - The access transistors connect the bitlines to the storage node of the selected cell.
   - The strong drive from the data driver overwrites the previous data in the cross-coupled inverters.

5. **Deactivation**:
   - Once the data is written, the wordline and bitlines are deactivated.
   - The cell retains the new data due to the bistable nature of the 6T cell.

| Address | D1 (LSB) | D2 (LSB) | D1 (MSB) | D2 (MSB) |
|---------|----------|----------|----------|----------|
| 07      | 04       | 07       | 09       | 03       |

| ![SRAM ReadWrite](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM_output_Read%26Write.png) | 
| :---: | 
| Fig 9: Read/Write operation in SRAM |

## Test Bench of SRAM
The test bench includes stimuli for various read and write operations, verifying functionality across different input conditions. The design utilizes the following instances:
- **busset8 and busset4 from bmslib library**: These instances are used for handling 8-bit and 4-bit inputs, respectively.
- **vpulse instance**: Used for generating the input signals to drive the operations of the SRAM.

The **SRAM_TOP instance** integrates the complete SRAM structure, including all its components, to enable a comprehensive simulation of the memory operations.

| ![SRAM_TOP_TB](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM32byte_TOP_TB.png) | 
| :---: | 
| Fig 10: SRAM Testbench |

| ![SRAM_TOP](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM32byte_TOP.png) | 
| :---: | 
| Fig 11: 32-byte SRAM |

## Delay Characterization
### Precharge Delay
The time taken to precharge the bitlines to the required voltage before any operation. This delay depends on the design of the precharge circuit and the capacitance of the bitlines (Here, its the time required by the Bitlines to go high soon after the PC signal goes low).

| ![PCdelay](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM_output_precharge_delay.png) | 
| :---: | 
| Fig 12: Precharge Delay |

### Read Delay
The time between the initiation of a read operation and the appearance of valid data at the output (Here, its the time required for Vout to detect output after the SA input goes low). This includes:
1. **Wordline Activation Delay**: The time taken to activate the wordline after the address is decoded.
2. **Bitline Discharge Delay**: The time required for the selected bitline to develop a differential voltage.
3. **Sense Amplification Delay**: The time for the sense amplifier to amplify the small voltage difference into a readable logic level.

| ![SRAM_Read delay](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM_output_Read_delay.png) | 
| :---: | 
| Fig 13: Read Delay in SRAM |

### Write Delay
The time required to overwrite the previous data with new data during a write operation (Here, its the time required for the selected bitline to go high soon after the rwn signal goes low). This includes:
1. **Bitline Driving Delay**: The time taken for the data driver to drive the bitlines to the desired logic levels.
2. **Wordline Activation Delay**: The delay for the wordline to turn on the access transistors.
3. **Storage Node Update Delay**: The time required for the cross-coupled inverters to stabilize at the new logic levels.

| ![WriteDelay](https://github.com/HarshitSri-Analog/32-byte-CMOS-SRAM-using-0.18um-Technology/blob/main/SRAM%2032%20Byte/SRAM_output_write_delay.png) | 
| :---: | 
| Fig 14: Write Delay in SRAM |

## Results
### Delay Metrics
| Metric             | Delay   |
|--------------------|---------|
| **Precharge Delay**| 1.64ns  |
| **Write Delay**    | 1.56ns  |
| **Read Delay**     | 129.7ns |

These delay metrics highlight the efficiency of the SRAM design in handling memory operations within tight timing constraints.

## Conclusion
This project demonstrates the design and simulation of a 32-byte CMOS SRAM with a focus on efficiency, stability, and performance. The detailed analysis of its components, operations, and delays ensures a comprehensive understanding of SRAM functionality.

## Acknowledgement
Special thanks to mentors, peers, and the VLSI design community for their guidance and resources throughout this project.

Feel free to explore the repository for insights into the design of CMOS based SRAM using 180nm technology. Contributions and feedback are welcome!

***If you find this repository helpful, please consider giving it a ⭐!***

