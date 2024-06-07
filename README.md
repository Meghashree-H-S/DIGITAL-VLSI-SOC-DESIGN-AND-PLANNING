# $${\color{red} Work \space under \space progress \space...............!!!!!}$$


# $${\color{grey}DIGITAL \space VLSI \space SOC \space DESIGN \space AND \space PLANNING \space}$$

$${\color{blue}This  \space is  \space a \space  \space two  \space week  \space RTL2GDSII  \space flow  \space workshop  \space organized  \space by  \space VSD  \space in  \space collaboration  \space with  \space NASSCOM.}$$
$${\color{blue}This \space workshop \space provides \space a \space theoretical \space and \space practical \space insights \space of \space the \space design \space flow \space using \space open \space source \space EDA \space tools}$$. 
<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e62a2dc3-fbc5-4dcc-a1b3-f4d48e177d4a" width="400" height="300" title="ASICFlow" /></p>

## INDEX
- ['Inception of open source EDA, OpenLANE and SKY130PDK'](#day1)
- ['Goodfloorplan vs bad floorplan and introduction to library cells'](#day2)
- ['Design library cell using Magic Layout and ngspice characterization'](#day3)
- ['Pre-layout timing analysis and importance of good clock tree'](#day4)
- ['Final steps for RTLGDS using tritonRoute and openSTA'](#day5)  

**Languages**: Verilog, Tcl, Bash, Linux commands\
**EDA Tools**: OpenLANE, magic, OpenROAD, ngspice\
**Operating system**: Ubuntu

## Inception of open source EDA, OpenLANE and SKY130PDK
SoC (System on chip) are used in many electronic systems or components such as smart phone, smart TV, smart watches, etc.. SoC can use Microcontroller, Microprocessor or Application specific Integrated circuit(ASIC) [2].Arduino board can be taken as one example which has the chip. The chip needs to be designed and planned which involves several steps that includes RTL to GDSII flow, later the chip will be fabricated. The main components of chip are Pads (used for signal transmission), Core(has logic gates), and Die (Size of the chip). 
<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/884aacb1-7123-43bb-9433-5e6d8a1d3964" width="425"></p>

<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c619b15f-aaeb-4f7f-beff-ad9acf76fca9" width="425"></p>

<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/3e796fb1-53fe-40f8-a542-d7590588bd4d" width="425"></p>

The Fig2 shows the Arduino board” which has a chip. The Fig3 can provide a blue print of how a board with the processor and other peripherals are accommodated. The Fig4 is the package of size 7mm, which have pins and chip. The Fig5 is the enlarged version of the chip which has Pads ( helps to transmit the signal), Core (Digital logic like and , nor…. Is placed), and Die (it is provides the size of the entire chip).  “RISCV SOC” with other components like  SRAM, PLL, ADC, DAC, and SPI is shown in Fig5. ADC, DAC, SRAM, and PLL are Foundry IP’s. Foundry is a factory where the chip gets manufactured. Intellectual property consist of certain design logic, specifications, or any information that will be helpful to manufacture specific logic. For example, DAC IP is used to convert digital to analog signals. Macros are digital logics.

RISC-V ISA ( Instruction set Architecture) is the language of the computer which can be used to talk to the computer. In Fig6, you can see that the Layout is shown which will be used to execute the swap program of C. For example, a c program is compiled to Assembly program (RISC-V) and then covert to machine or binary (0 or 1). Then the bits get executed in the particular layout.  Actually, RISCV specification are implemented using RTL (picorv32 cpu core) and then to Layout. The RISC-V is an open standard [3].

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/4f07e99b-d378-43ef-81ce-227f8e92b5ca" ></p>

Applications are used in day to day life and understanding how these apps are internally executing with the help of Hardware can be seen in the Fig7. System Software has major components such as Operating System(OS), Compiler, and Assembler. The programming language(C,C++,VB, Java)  is converted to instructions(.exe). Assembler will help in converting the Instructions to binary digits. The binary digits are fed to hardware. See the  example of “Stop watch application” in Fig8.

<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/ec084ec4-5911-4166-ad24-db06153d7828" width="425"></p>

Architecture of the computer (For example: RISC-V) helps in communicating with the computer. Hardware description language like Verilog and vhdl can be used to write the RTL. RTL is synthesized into netlist, which is represented in gates (and, or, flipflops…..) 

The Open Source EDA Tools and PDK are used to design. PDK stands for Process Design Kit, which is a collection of files used to model a fabrication process for the EDA tools used to design an IC  that includes Device Models, Digital Standard Cell Libraries, I/O Libraries, Process Design Rules: DRC, LVS, PEX, and so on. The open source foundry PDK is provided by sky water technology which is partnered with Google helps in manufacturing the custom ASICs.  Sky130 pdk  is a hybrid 180nm – 130nm technology[4].

<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/52aefe11-0dd6-4ea9-a2dd-520223ec58b7" width="425"></p>

OpenLane is a flow that uses open source tools to achieve a tapeout of a chip. The Fig 11 shows the OpenLane flow. The flow can be Automatic or Interactive. 

<p align="center" width="100%"><img width="438" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a56905bb-fb98-405f-a666-0ebc60dec93c"></p>

#### LabWork
In this section, we prepare design, run synthesis, and calculate the Flip ratio.  We can check the clock period values in the  “config.tcl” and “sky130A_sky130_fd_sc_hd_config.tcl”. The values can be changed in these folders but as you see there are two different clock period values (5ns and 24.73). Each file, sets the value differently, but the values set in the “sky130A_sky130_fd_sc_hd_config.tcl” file is considered first. “picorv32a” design is used to run the openLane flow.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/7da11655-534c-46c2-b7a2-852c1b8467d6"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/334b6c8b-8f68-4e04-aaca-9847c5de749e"></p>

**Commands used for design setup and synthesis :**

1)	cd  Desktop/work/tools/openlane_working_dir/openlane

2)	docker

3)	./flow.tcl -interactive

4)	package require openlane 0.9

5)	prep -design picorv32a

6)	run_synthesis

7)	exit


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/43f0157a-ff74-421a-9db1-0898120f7743"></p>

<p align="center" width="100%"><img width="430" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a107e903-3d68-4734-979f-a2076a2c6ec1"></p>

**Calculation:**
Total number of Cells (T) = 14876\
Number of D Flip Flops (DF) = 1613\
**Flop Ratio**  =  DF/T =1613/14876 = 0.108\
**Flop percentage** = Flop Ratio * 100 = 0.108 * 100 = 10.84%

## Goodfloorplan vs bad floorplan and introduction to library cells
After Synthesis, we need to do Floor planning, where we set the die area, core area, aspect ratio, utilization factor, place in/op cells, and macro placement. Standard cells are placed in placement stage.

#### LabWork

To find the parameters available for each stage (synthesis, floor plan, placement…), open “Readme.md” and this can be used to set the required values for that specific parameter.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/ee6e2e05-9e40-462a-af52-3c4640db7077"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b0b6f4a3-64e4-4b1d-a0ea-e297a1a09dc0"></p>

We can check the values set in three different configuration folder, “floorplan.tcl”, the  “config.tcl” and “sky130A_sky130_fd_sc_hd_config.tcl”. The highest precedence is given to “sky130A_sky130_fd_sc_hd_config.tcl”, then “config.tcl”, and later “floorplan.tcl”.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/34f224b2-0242-4b24-90ad-bbfa3147c220"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/28ba7577-a1f1-4e95-a4eb-1bf627356545"></p>

Follow the commands used in previous section from 1 to 6, then follow below commands.
8)	run_floorplan 

<p align="center" width="100%"> <img width="500" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/9768c127-fa5f-4ef4-9e64-2ead8d702641"></p>

The current updated version of open lane does not print any information in “ioplacer.log” and also provides two extra files “pdn.log” and pdn_runtime.txt”. pdn stands for Power delivery Network.

<p align="center" width="100%"><img width="581" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c2eca489-d277-47ea-a4c7-fd735386b2c3"></p>

The design prep stage creates a folder and the configuration file will show the parameter values that is considered for the run. As we already know, The values (utilization factor, vmetal, hmetal..) for the run can be set in three files and the values can overwritten based on the hierarchy of the files.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8800f7eb-f40a-47d5-b62d-fcc7ab9bdc76"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/ed378606-667c-4235-811f-0be2169948c7"></p>

After Floorplan, The def file is created in the results path. def file stands for design exchange format, which gives information of how the components are placed (orientation and location) and DIE area information.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/68d5d809-9871-44fd-95a5-8fe17aba7737"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/cf5ceb65-3636-4835-b77a-f73565631fd8"></p>

**Calculation:**
1 Micron = 1000 database units\
Dimensions of the chip in Micrometer: 660.685, 671.405 \
**Area of Die** = Die Height * Die width = 671.405 * 660.685 = 443,587.212425 square Micron

**To view the layout,**
- cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-05_16-25/results/floorplan/
- magic -T  /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

<p align="center" width="100%"><img width="584" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a6a7fb19-ffbc-4f72-b5f3-a5af4d74e4dc"></p>

Select the entire layout by pressing “s” and move the layout to the center press “v”.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/36ff5939-ed06-4aa8-8f31-75e5e7222a2d"></p>

Select an area and press “z” to zoom in. IO_mode was set to 1, so the io pins are equi distant.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e4c00a69-ca8b-4727-adf3-390bf106ad6b"></p>
By selecting the required iopin, you can see the metal information on tkcon.tcl window by typing “what”. 
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c9f34dec-7f6d-4ebf-8d78-316e73045a33"></p>
We can see decap cell, vertical and horizontal pins, tap cells and the standard cells which is not placed yet. Tap cells help in avoiding the latchup condition.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/625df688-1ce0-4d72-a235-00ddd705d5d5"></p>
**Placement:**
1)	Global placement – reducing the wire length. We are focusing on reduction of Half parameter wire length(HPWL)
2)	Detailed placement(legalization) more required from timing point of view and takes care of not over lapping the design.
**After “run_floorplan”:**
9)	Run_placement

<p align="center" width="100%"><img width="587" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5234c851-66e7-4c56-a2b6-d6661ff9ce22"></p>

Placement is where the standard cell position get fixed. As you can see the layout after the placement, the standard cells which were initially at the lower left corner of the floor plan is now placed in the rows.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2b76ea21-24aa-4ffc-9539-4276fa8e90b1"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d470252c-8c56-4e44-bf03-a1314bcd92dc"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/45d27823-bd9e-4076-99c9-a27779c2ae1e"></p>


## Design library cell using Magic Layout and ngspice characterization

#### LabWork

## Pre-layout timing analysis and importance of good clock tree


#### LabWork

## Final steps for RTLGDS using tritonRoute and openSTA


#### LabWork
