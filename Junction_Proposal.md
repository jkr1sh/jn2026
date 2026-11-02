I’d like to build an open-source platform for operating and testing small experimental (1T1C) DRAM arrays, with external sensing, driving, and control circuitry. 
The goal is to automate write, read, and refresh operations, and measure retention and timing limits over the entire array.

Over two weeks, I’d develop and validate the control logic using a discrete transistor dram array, and with guidance from mentors and peers with fabrication experience, work on the frontend electrical interface for sensing and driving cells that have been or will be fabricated by initiatives like HackerFab *[(Dr. Semiconductor)](https://www.youtube.com/watch?v=h6GWikWlAQA)*

The project's primary inspiration comesfrom [SoftMC’s](https://pdl.cmu.edu/PDL-FTP/NVM/17hpca_softmc_abs.shtml) approach to memory testing. While SoftMC operates commercial DRAM modules through their DDR interface, this platform would target small experimental 1T1C arrays with accessible wordlines and bitlines, that could plausibly be fabricated in hackerfabs, while providing external driving, precharge, and sensing circuitry. Users could run configurable write/read/refresh sequences and characterize retention and timing behaviour.

### Two-week Target deliverables

1. Programmable memory controller and test software
- Implement write, read, restore, and refresh sequences with adjustable timing.
- Generate test patterns, compare readback, and log failures by cell address.
- Automate hold-time and operation-timing sweeps, producing per-cell pass/fail and retention plots. Use oscilloscope measurements to validate the underlying signals, with automated instrument control (via PyVISA) as a future extension.

2. Build a discrete homebrew dram array **[George Foot](https://hackaday.io/project/175774-homebrew-dynamic-ram-with-discrete-mosfets)** 
- Build a small discrete DRAM array to validate the controller.
- Demonstrate correct pattern readback, loss of data with refresh disabled, and continued storage with refresh enabled.

3. **Memory Cell test frontend prototype**
- Build the signal chain for a test interface targeting real micron scale dram arrays with the required sense circuitry, wordline/bitline read/write drivers, precharge circuitry, and switching.
- With input from hackfab/device folks, define a target electrical range based on possible fabricated cells.
- Bench-test the frontend using selected capacitances, and known bitline loading. Document sensing margin, timing, and measurement limitations.
- Test real fabricated cells if suitable samples and connections are available during the program.

Deliverables will include the working hardware and all developed schematics, PCB design files, BoMs, source code, test scripts, and validation results. All design files, code, and documentation will be released under appropriate open-source licenses.


### Proposed Timeline (with PCB Fab)

| Period | Work and milestone |
| --- | --- |
| **Days 1–2** | Get input from team, Define array size and electrical requirements. Develop schematics, simulate critical frontend circuits, and begin controller implementation. |
| **Days 3–4** | Bench-check critical sensing and switching functions. Complete schematic review and PCB layout; submit for fabrication. |
| **Days 5–7** | While boards are in fabrication, develop and validate controller sequences, refresh, and test software using a discrete reference array. Prepare components and bring-up procedures. |
| **Days 8–10** | Assemble and bring up the PCB (is 6-7 days PCB turn around possible?). Integrate the controller and demonstrate write/read/restore/refresh across the array. |
| **Days 11–12** | Run automated pattern, retention, and timing tests. Investigate failures and make board-level fixes. |
| **Days 13–14** | Repeat validation, document measured limitations, and complete the array demonstration and open-source release. |

### Alternative Timeline (without PCB Fabrication)

| Period | Work and milestone |
| --- | --- |
| **Days 1–2** | Get input from the team, define array size and electrical requirements, and select components available in hand or on breakout boards. Develop schematics and simulate critical frontend circuits. |
| **Days 3–5** | Build the discrete reference array and frontend using soldered prototyping boards and component adapters. Keep sensing circuitry close to the array with short connections. Develop controller sequences alongside hardware bring-up. |
| **Days 6–8** | Integrate the controller and hardware. Demonstrate addressed writes, pattern readback, restoration, and refresh across the array. Resolve sensing and switching issues. |
| **Days 9–11** | Run automated pattern, retention, and timing sweeps. Log failures by cell address and investigate the effects of wiring, loading, and noise. |
| **Days 12–14** | Complete fixes and repeat validation. Document operating limits, assembly instructions, and results; demonstrate the working array and release the design files and code. |

### Hardware and facilities
- MCU or FPGA development board, selected according to timing and I/O requirements.
- Discrete transistors, capacitors, analog switches, comparators/amplifiers, and components for wordline driving, bitline write/precharge, and sensing.
- Oscilloscope, bench supply, DMM, and signal generator; access to capacitance measurement would help validate reference components.
- PCB Fab and assembly
- For fabricated-device testing: suitable samples, pad information, probe station or packaged-device carrier, and reference characterization equipment.
# Application
**Qs for folks in the HackerFab scene**
- Has anyone built, or planned, a shared-wordline/shared-bitline 1T1C array with external sensing, rather than individually probed cells?
*Future Considerations*
- Not directly relevant but possible extension: SCL 1.2u/0.8u MPW devices? (No MiM or Poly Caps in SCL's opensource PDK though)

## References
- [Dr. Semiconductor: Making RAM at Home](https://www.youtube.com/watch?v=h6GWikWlAQA)
- https://pdl.cmu.edu/PDL-FTP/NVM/17hpca_softmc_abs.shtml
- https://emrl.de/r_m_3.html
- https://hackaday.io/project/175774-homebrew-dynamic-ram-with-discrete-mosfets

---

# Getting Started

**1. [MIT OCW — DRAM (5:10)](https://ocw.mit.edu/courses/6-004-computation-structures-spring-2017/resources/dram-5-10-/)**  
Quick refresher on the access transistor, wordline, bitline, writing, precharging, charge sharing, sensing, and restoration after reading.

**2. [Vishal Saxena — Memory IC Design lectures](https://www.eecis.udel.edu/~vsaxena/courses/ece518/s15/Lectures.htm)**

(prepend ~vsaxena to the url path to access slides)

*[Video lectures](https://www.youtube.com/playlist?list=PLXGoZ2RJoEXrJ11c53y-ZQ6l5OXQvHPtk)*

| Lecture           | Contents                                                                               |
| ----------------- | -------------------------------------------------------------------------------------- |
| **1**             | 1T1C cell, array architecture, wordlines and bitlines                                  |
| **2**             | Bitline capacitance, charge sharing, equalization/precharge, sense-amplifier operation |
| **3**             | Write operation; skim fabrication/layout details initially                             |
| **4**             | Isolation devices, full sense-amplifier circuit, row/column decoding                   |
| **5, first part** | Wordline drivers                                                                       |


**3. [Bruce Jacob — High-Speed Memory Systems, Lecture B (PDF)](https://user.eng.umd.edu/~blj/CS-590.26/lectures/LectureB.pdf)**  
Useful circuit-and-timing reference: look for the diagram labelled **Access → Sense → Restore → Precharge**. It puts wordlines, bitlines, sense enables, equalization, column selection, and write drivers together.

**4. [George Foot — Homebrew Dynamic RAM with Discrete MOSFETs](https://hackaday.io/project/175774-homebrew-dynamic-ram-with-discrete-mosfets)**  
Practical discrete build: a **3 × 4-bit array**, schematic, and videos showing writing, reading, and refresh through column amplifiers.

