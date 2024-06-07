# $${\color{grey}DIGITAL \space VLSI \space SOC \space DESIGN \space AND \space PLANNING \space}$$

$${\color{blue}This  \space is  \space a \space  \space two  \space week  \space RTL2GDSII  \space flow  \space workshop  \space organized  \space by  \space VSD  \space in  \space collaboration  \space with  \space NASSCOM.}$$
$${\color{blue}This \space workshop \space provides \space a \space theoretical \space and \space practical \space insights \space of \space the \space design \space flow \space using \space open \space source \space EDA \space tools}$$. 
<p align="center" width="100%"><img src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e62a2dc3-fbc5-4dcc-a1b3-f4d48e177d4a" width="400" height="300" title="ASICFlow" /></p>

## INDEX
- [`Inception of open source EDA, OpenLANE and SKY130PDK`](#day1)
- [`Goodfloorplan vs bad floorplan and introduction to library cells`](#day2)
- [`Design library cell using Magic Layout and ngspice characterization`](#day3)
- [`Pre-layout timing analysis and importance of good clock tree`](#day4)
- [`Final steps for RTLGDS using tritonRoute and openSTA`](#day5)  

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

Taking “Inverter”  as an example.  The inverters .magic file is obtained from the GitHub repository and we are doing the post layout simulation in ngspice and post characterizing the cell. This cell will be plugged into the picorv32 core. 

#### LabWork
Clone the following Git repository to obtain the inverter layout:
Github link: https://github.com/nickson-jose/vsdstdcelldesign

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/334928cb-06db-44d9-9b23-0de416b9f531"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c758e962-ef18-4a62-9def-ba146fbdeb81"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/08d23485-577b-4b8a-9060-716154b42d2e"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a71651eb-091f-4487-a439-12dcde785798"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a71651eb-091f-4487-a439-12dcde785798"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d5c3c433-61a9-4b59-af6e-26f833b6a80e"></p>
To check the logical function of this inverter, extracting the layout into spice netlist.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/3a13a2a1-4dbb-49c7-9041-a34f06a6c28f"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2a7786d1-46f2-488e-ad11-5f1f392b1d91"></p>

The spice file contains:
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/56074265-7b11-404d-ba03-017eb2d841e8"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/58d154fa-c6a7-4efc-83ff-11e210ab1cb5"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d7cc402b-81c6-42b6-a851-948d7fd30db6"></p>
Make the required changes to the converted spice code model.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/017b63c7-fefd-4016-8c0a-54f4b519f675"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2d4c4d49-e79f-4994-a0fc-e72d890a5c52"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/af24fe3b-46e4-48d6-8d1d-99afde63f80b"></p>

**Calculation:**\
**output voltage**= 3.3v\
20% of 3.3 = 0.66v\
80% of 3.3 = 2.64v\
50% of 3.3 = 1.65v\
**Rise transition**(20% - 80%): 2.24601(80%) - 2.1825(20%)  = 0.064ns\
**Fall transition**(80% - 20%): 4.09492(20%) – 4.05304(80%) = 0.041ns\
**Rise cell delay**: 2.21167 – 2.14956 = 0.062ns\
**Fall cell delay**:  4.07797 – 4.05 = 0.02797ns

The following exercise is about learning DRC  error and fix.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/cbfd1c27-886d-460a-b5e0-d7c33d4d8c59"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a133b568-9de7-4e73-9a60-6d1a62b4a0e1"></p>
.magicrc file is loading the local version of tech file.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0ccbe29f-5c7b-429c-a209-3686d624af30"></p>

Magic opens after giving the ‘magic -d xr” command. To open the met3 example, File-> open -> select “Met3.mag”. The independent geometries (m3.1,m3.2,m3.5,m3.6, and m3.3d) displayed shows some kind of DRC error (white patches in the geometry). The rules for each metal is given in “skywater pdk” document.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8dfefa2f-ca70-4fe3-80c7-92f2bc7bb7ee"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/1afc6f17-bc36-4a6c-9178-9af060e6f6b6"></p>

Select the required metal and type “ drc why” to see errors.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/33d061ee-8848-4a19-8e96-11db5feec8a0"></p>

To paint any layer, use tcl window or paint options on the right corner of the layout screen. First, select the area by creating a box. In tcl window, type “paint <layer>” or select the required option from the right corner and press the middle button of the mouse. Can measure the width of the selected area by typing “box” in the tcl window.” Snap int” will help in moving the box and aligning it with the required area.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/ee448eea-9b64-46d8-a4a2-f6d475d7419d"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e900846d-dc6b-4397-84d3-503240c3e95f"></p>
The following is “poly.mag”.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c73a3344-cad3-4326-871b-877b4dfc795d"></p>
Need to check the drc rules in the tech file. Open the file and search for “poly.9”. See the available rules. Add the required rules and save it.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/952c3229-c046-4787-95a0-4c425632f6fe"></p>

**Before:** 

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8fe51900-74ef-4916-8436-600cca01a507"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/15c9e0e4-300c-4b4e-a341-174f324b7784"></p>

**After:**
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/6aa339ca-635b-497e-9415-f5aff2b9ea39"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/16caacf1-e1a6-47e9-aee0-701cb2354ef4"></p>

We can see the error flagged by showing white patches.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5688d1ba-d189-4b2f-a386-fcf60ff32857"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/39a38e52-b63c-4362-990d-0d7708a6afbf"></p>

**Before:**
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/23424f6f-8ece-4169-b74f-74dd9ff9490f"></p>

**After:**
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/11aebb28-af7f-4a95-8515-cbd0d4bc108b"></p>

After the changes, load the file again and the error patches are now detectable.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/98870d80-06c7-4aa2-9ec7-c9f0ed64ed0a"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/7be6bdd8-83ae-4fe4-989b-d277134a3425"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b76442fd-9b86-4915-a885-47b1ed037b01"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8d6d3ae0-b288-4a32-824b-b9a065b56af4"></p>
Added the highlighted part.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b293c5aa-7fc5-4a32-b3d5-f03f8b915ba6"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/85f79ee8-7b4b-4671-b681-11130dabeb36"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/89c09ddc-a30a-422c-bade-f41fc7dc9676"></p>
Save it and now check. The error seen as white patches.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b353bfe3-2c5d-4419-8733-7485fc31ccfa"></p>
Add contact and error will disappear.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/7f9e2fab-bab5-4ca1-a7f3-0dec03e3ae92"></p>

## Pre-layout timing analysis and importance of good clock tree
We have characterized the inverter. Next using the layout create lef file and use it in picorv32 design.

#### LabWork

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8ab42fe8-ada0-4e3e-93fe-17cf36471c34"></p>
Press “g” to activate the grids in the layout.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/257d11b0-5ed0-4714-8ea8-b7a67c31c3ea"></p>
We can confirm that the port “A” and “Y” are on the intersection of horizontal and vertical tracks of Li1. 
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/13796f30-c55e-4cb1-b502-28a278cba464"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/bb8926fd-7ea4-47bc-8030-ec96b66b9aee"></p>
To define the port, select the port than go to Edit -> text.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a87c6e5f-c41e-4976-bd24-61073b705202"></p>
Define the purpose of the port by selecting each port and setting it as input, output, signal, power, and ground. After that save the file and extract the lef file.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c49d7da5-d922-445d-8344-732359685a18"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0c69c6fb-3490-4b70-b9ed-ce1516e16105"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8144756f-151a-49cf-9d9d-4e4c24f96a52"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/97c76f4a-0ea7-4994-9f75-5ca9f1e73241"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d440bd45-4959-4ef0-8477-af96e5b74181"></p>

Following steps will be how to incorporate this lef file to picorv32:
- Copy the lef file and tech library to design src folder, this makes file accessing easy.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/3add2028-c0e1-4968-911a-e1805114102b"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0f2e7305-1396-45bd-8086-1e2fec1e70b3"></p>

- Modify “config.tcl” file.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/486f28b9-20d9-4668-940c-48bc6c5cb2bb"></p>

**Before:**
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/403c1048-40d9-430c-8e7b-ad16928a189d"></p>

**After:**
  <p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/bfdab522-eedb-4bd5-8f8b-10f7654db382"></p>

- Design preparation and merge the lef file to the openlane flow

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/11e3ef0a-3781-4deb-9d3d-085aa3f3378f"></p>

- Synthesis
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/f1aa5487-d2a5-44c8-ae07-47f416e15146"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/27565dbe-042f-424f-a039-d619762d7a7f"></p>

- To reduce slack violation do some modification.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8878c655-3411-456a-8e3d-8869d3dc88b8"></p>

Observe the timing violation.
Wns is maximum slack
Tns is total negative slack
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2ac18213-5571-4efc-86b1-b80777c43b34"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/9e36d001-e22a-49b6-b17d-e84bedd5828f"></p>
Try balancing between delay and area by making few changes. Start from prep design.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c58348c4-2ac8-4132-a7f4-328fef8995ac"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/3ba63c57-896c-42c8-84e7-76a152d6874a"></p>

As floor plan is not completely executed using “run_floorplan” command, use the alternative command: 
- init_floorplan
- Place_io
- Tap_decap_or
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5646c199-6390-42be-b34d-cf304017288d"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b25a1e9c-79ec-49b5-9b8d-20513f42734b"></p>

- Check  “merged.lef” has the newly added inverter file.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/061d5e5c-a342-4e22-babb-f1255138b270"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/9979653e-edbd-4123-93b0-ea3ad00541d2"></p>

Now, continue with placement
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a6814973-908e-4de0-aa7a-9b6aa4d9e8a4"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/af7ed805-14c4-4507-b973-b37358a6d02b"></p>

- Check the layout
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8fae4912-170d-4fe6-ac91-2953515b3cc9"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a36bec62-0e5e-4502-bfc6-a98adf96feae"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/40a47b4a-67ce-496e-a78a-9450fd95ef89"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/40bb6beb-9004-44c2-afcb-d2a0044a6342"></p>

- Timing Analysis carried out from a STA.
Create “pre_sta.conf”
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/30d51f90-494c-4116-8a2b-263e0a6e117f"></p>
 <p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/f24d32d8-d9a7-49ca-b6c4-f4f7d3c8bebb"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/c314fa37-2cd8-44b1-8d7b-e7e40cb5d429"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8104f854-d1ac-4ba3-a59a-dff62f60490b"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a7a8ae55-65ea-4fe2-9c93-74de13173857"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/58e0c3d9-d1c1-4819-af31-f6fd29c69bf8"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/927e7e3b-dd56-4682-aa21-201c56e6a2d7"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d8ee6257-d95b-4ca3-aac1-1cdba44bdebf"></p>

- Delay of any cell is dependent on the input transition and output load. Tryout to reduce Fanout.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5c3e3756-65ff-49d2-8ae2-1cf1eda65460"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/dbb39a72-be02-480d-9360-2c343cf40c4e"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e90ffc1c-b663-47ac-8ec6-151e9d22831c"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2165ce29-a575-41e6-bccb-0b100b253f63"></p>
In the above method the slack is stable not violated. Trying the method shown in the training material.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/1d18d1ba-2795-4de5-9c3c-a6888ec03708"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/1e5b1cc6-a65b-4b64-8ae1-23dccc1b5dad"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2fe53d1b-2a3f-4082-9d20-c07587bf6412"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/cfa09cf1-f800-43ab-a9f6-7629f5ab847f"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/58a213a1-9edb-4a83-b833-a42b0d3d698c"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/9a3a063a-9bf4-49ba-a6ac-c51fe8594675"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5993a1cb-f980-4fa4-ad1b-099cf76caa17"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/eef8d6c4-138d-4bc3-9e13-0623bd238a0c"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/09942641-9f63-4264-9a8b-e4a2c5ac0e84"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/efce228b-a9eb-4555-8e17-f95b53a8e201"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/3132c10b-b57c-4edd-adcc-9dabaa96289d"></p>

Type the below command to generate custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/87128044-57af-4fcd-ae68-0a46e3988389"></p>
The slack is reduced from -23.90 to -22.6173
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/8b498762-7c9d-4fac-af37-ac0e479cd319"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e2d3e805-9a26-4b35-840d-70497d63f3cd"></p>

Will continue with floorplan after synthesis. 
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b4da1a78-4373-4486-977c-8bf0ac3965ae"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0cf079cc-6e8e-47fc-87a2-557b92004028"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b39be286-7c5f-45fe-a32b-835637278cdf"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e45c5aca-f560-463c-9b16-12275001e0c0"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0e48a5a8-1061-4895-b7fe-91a07be33635"></p>
Openlane has all eda tool and openroad does the floorplanning, placement, CTS, Optimization ,and  Global Routing.

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e238984c-cdd4-4fd5-9260-5d3ed378eed5"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/fa3c4de1-ca55-46c4-b85f-fbad2e25ba28"></p>
Enter into openroad and do the timing analysis as open STA is already integrated inside openroad. 
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e2891673-4057-4afd-96e9-924ad1a2d7c1"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a80e3f2b-a1ef-4514-b3cc-0c70121cd0fe"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/59e77a87-4570-4028-8061-9b44aaeb6a48"></p>
Set the current def path before “run_cts”.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d4de6d3f-ff84-45d2-bad6-c606a6848b40"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/515ec5c4-fa91-4524-a4a1-f8d582383869"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/470e00a4-f2fd-4856-a5bb-7e6e76dd6680"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/86d4f865-525e-4fe3-9eb5-39a2cdb7c9fc"></p>

Including the larger size clock buffer in the clock path has improved the result.
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/e19c8abf-81de-4e9e-8b31-b4ebd65a9304"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/de67eb93-37de-49da-8704-364f806bc037"></p>


## Final steps for RTLGDS using tritonRoute and openSTA




#### LabWork

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/5baed693-0c98-4a18-ba2b-9948399d66a3"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/89d001b4-75a8-46ed-8257-f303f187df08"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/2f03adfa-7d2a-46ae-975c-09689a3feea4"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/6222e66c-c9a2-43da-af83-232ecae64344"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/4baad3a3-2074-41fa-9bb4-67072287cb36"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/bb53da6b-ce85-4663-b715-95c72776b878"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/30688fb7-c1fc-4cce-a443-20b8abaa3d9b"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/71df2fb9-560c-4235-9460-529fc9b7da66"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/130a459d-489d-4205-99ed-4f4b8a9108fd"></p>
The number of the cell can be found in 
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/23da9258-756d-438b-b83b-efaecea8cf1a"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/0d5037f7-ebe6-4fdf-9e78-994155c49b1c"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/541ddc94-2a45-44b3-8b0d-c386a3d61c83"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/02de8bfd-ae6e-41c1-89ae-fcbe397b755f"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/7aa1189c-fc44-4c04-894d-0cb4aa5c4aab"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/762bd073-afd3-4dc0-8357-9b9520af903c"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/4789daf1-5745-4b90-8c9b-6769a47eae03"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/f199f7f8-9a04-4314-ab79-88a11dc18ffb"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/cf25c7b5-2db8-4491-bb9d-46ec61f5a635"></p>
<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/a9f995b4-e405-4551-b37b-d1d1e3f3cc78"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/49ae4d4e-3ee8-421f-950c-eab2c546acb8"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/19af0851-8fdc-4d2b-bad1-ffef92db00c0"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/942a3f27-1526-48fc-8649-7edee9348bf1"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/4eaca097-2903-47f5-99d1-6b0f347d7de4"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/92d48843-bf33-49d1-889d-09cb5090c19e"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d62315d2-f8c3-479d-8f99-f346bfa9d65c"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/d5f1e065-9e46-4860-9f5e-3d13c84d98f8"></p>



<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/41db76c6-2604-45fd-879f-e434d70e9f6a"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/af0d6bbd-397f-4d8c-98b0-8aa228e1b349"></p>

<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/b5a8f9e4-83ea-42f0-a555-beba327d8490"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/42e8f414-d6d8-4b69-bcfc-bc866c9450f4"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/30868f29-c823-40d6-ab6b-f90ba3294de8"></p>


<p align="center" width="100%"><img width="425" alt="image" src="https://github.com/Meghashree-H-S/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING/assets/44599861/6974c2c1-e5c8-44e7-9341-e25bf7198503"></p>


**Reference:**
1)	https://openlane2.readthedocs.io/en/latest/getting_started/newcomers/index.html#what-is-openlane 
2)	https://www.tomshardware.com/reviews/glossary-soc-system-on-chip-definition,5890.html 
3)	https://riscv.org/about/ 
4)	https://github.com/google/skywater-pdk 
5)  https://github.com/efabless/openlane2
6)	[Open Magic](http://opencircuitdesign.com/magic/)
7) [ngspice](https://ngspice.sourceforge.io/)
8) [efabless](https://github.com/efabless/caravel)
9) [Google Skywater pdk](https://github.com/google/skywater-pdk/tree/main/libraries)
10) [openLane](https://github.com/The-OpenROAD-Project/OpenLane/tree/master)
11) [Welcome to SkyWater SKY130 PDK’s documentation!—SkyWater SKY130 PDK 0.0.0-369-g7198cf6 documentation (skywater-pdk.readthedocs.io)](https://skywater-pdk.readthedocs.io/en/main/))




**Acknowledgements:**
- Kunal Ghosh, co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.
- Nickson P Jose, Physical Design Engineer, Intel Corporation.
- R. Timothy Edwards, Senior Vice President of Analog and Design, efabless Corporation.
- Mohamed Shalan, Head of the EDA/IC Design Team at Efabless.
  









