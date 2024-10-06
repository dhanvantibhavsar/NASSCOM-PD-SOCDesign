# Digital VLSI SoC Design and planning Training
## Overview
This repository contains my work on System-on-Chip (SoC) design, following the methodologies used in the NASSCOM VSD SoC Design Program.

Internship provided by :- VLSI System Design \
Internship Date:- 17 Sept 2024 to 2 Oct 2024

## Acknowledgements
[kunal Ghosh](https://github.com/kunalg123),Co-founder, VSD Corp. Pvt. Ltd. \
[Nickson P Jose](https://github.com/nickson-jose) Technical Lead @ HCLTech | Ex-Intel \
[R. Timothy Edwards](https://github.com/RTimothyEdwards) Senior Vice President of Analog and Design, efabless Corporation.

## Tools Used
- Verilog for RTL design.

- OpenROAD for physical design.

- Magic for layout.

- Sky130 PDK.

## Workflow
- RTL Design.

- Synthesis.

- Floorplanning.

- Placement & Routing.

- GDSII Generation.

## Physical Design Flow
   In the RTL to GDS flow, physical design plays a crucial role. During this stage, the synthesized netlist, design constraints, and standard cell library are used as inputs to generate a layout (GDS file), adhering to the design rules specified by the foundry. This layout is then sent to the foundry for chip fabrication. The entire process of converting a gate-level netlist into a layout is referred to as physical design.

   Physical design consists of multiple stages, each with its own mandatory checks, analyses, and verifications. Here is a detailed information.

   ![PD FLOW](./images/rtltogds.png)

## Import Design or NetlistIn
The first step in the physical design flow is the Import Design or NetlistIn phase. Here, all relevant design files and constraint files are imported, including the netlist, SDC (Synopsys Design Constraints), UPF (Unified Power Format), DEF (Design Exchange Format), technology files, logical and physical libraries, and TLU+ files.

## Floorplanning or Chip Planning
Floorplanning is a crucial stage in the physical design process. At this point, the netlist, which describes the design and its various blocks along with their interconnections, is utilized. While the netlist provides a logical representation of the ASIC design, floorplanning translates this into a physical layout. This involves mapping the logical description to the physical description by placing blocks or macros within the chip or core area. Key tasks in this phase include:

Determining the width and height of the core and die.
Identifying the locations of predefined cells/macros.
Planning the placement of I/O pins.
Creating the pad ring for the chip.
The main objectives of floorplanning are to minimize both area and delay.

## Placement
Placement involves arranging the standard cells within the design. During this stage, the tool determines the optimal location for each standard cell on the die using internal algorithms. Placement is not merely about positioning the cells from the synthesized netlist; it also involves design optimization and assessing the routability of the layout. Various criteria can drive placement decisions, including timing, congestion, and power optimization. The goals of placement include:

Optimizing area, timing, and power.
Minimizing timing DRCs (Design Rule Checks) and cell/pin density.
Ensuring that the placement is routable.
## Clock Tree Synthesis (CTS)
Clock Tree Synthesis (CTS) ensures that clock signals are uniformly distributed to all sequential elements within the design. This process involves inserting buffers or inverters along the clock paths to minimize skew. The objectives of CTS are to satisfy clock tree design rule constraints—such as maximum transition, maximum load capacitance, and maximum fanout—while achieving targets like minimal skew and insertion delay.

## Routing
Routing occurs after CTS and determines the precise paths for interconnections within the layout. This process connects various blocks in the chip. With information from the placed cells, blockages, clock tree buffers/inverters, and I/O pins available post-CTS, the routing tool can finalize all connections specified in the netlist. The routing stage employs metal and vias to establish the electrical connections defined by the logical relationships in the netlist. The main objectives of routing include:

Meeting timing constraints.
Avoiding LVS (Layout vs. Schematic) errors.
Ensuring no DRC errors exist.
Minimizing total wire length.
Routing consists of several stages:

a) Global Routing \
b) Detailed Routing \
b) Track Assignment \
c) Detailed Routing \
d) Search and Repair 

## Physical Verification and Signoff
After routing, the layout is prepared for the Physical Verification and Signoff stage. Here, various signoff checks are performed, including physical verification, timing analysis, and logical equivalence checking. Key physical verification checks include:

Layout vs. Schematic (LVS): This compares the layout with the schematic to confirm functional matching. A clean LVS report indicates that they match.

Design Rule Check (DRC): This verifies that the layout adheres to the design rules established by the fabrication team, assessing spacing rules between metals, minimum width rules, via rules, and more.

Electrical Rule Check (ERC) and Antenna Check: Additional checks to ensure electrical integrity.

The equivalence check compares the pre-layout/synthesis netlist with the post-layout netlist generated by the tool after place and route (PnR) to ensure functional consistency.

## DAY 1
 ### Task - To run Openlane flow using 'picorv32a' design and calculate the flop ratio.
  To run the Openlane flow with the picorv32a design and calculate the flop ratio, follow these steps:
  1. Change the directory to Openlane
  2. Create an alias for running Docker
  3. Invoke OpenLane in interactive mode
  4.  Write the Package require

``` 
     cd Desktop/work/tools/openlane_working_dir/openlane
     docker
     ./flow.tcl -interactive
     package require openlane 0.9 
     
```
Now run synthesis using following command


`
 run_synthesis
`

![openlane](./images/openlane.png)
![synthesis](./images/runsynthesis.png)

 Calculate the flop ratio

 Flop ratio= Number of D Flip flops/ Total number of cells

 percentage of DFF's = flop ratio x 100

 Here 
 Number of DFF is 1613
 and total number of cells is 14876.
 My flop ratio is 10.842

![flop ratio](./images/flopratio.png)

## Day 2
### Good floorplan VS Bad floorplan and Introduction to library cells 

#### Chip Floorplanning Considerations:

#### Utilization Factor and Aspect Ratio
To determine the utilization factor and aspect ratio in chip design, we first need to understand how the height and width of the core and die areas are defined.

Core Area: The section of the chip where all logic cells and components are placed. This is the functional area of the chip where logic operations occur.

Die Area: The outer region surrounding the core, primarily used for placing input/output (I/O) related components.
The core's height and width are defined by the netlist, which determines the number of components needed to implement the logic. The die's dimensions depend on those of the core.

For instance, consider a netlist with two logic gates and two flip-flops, each occupying 1 square unit. The total area required for the core is 4 square units.

*Utilization Factor:*
This is defined as the ratio of the area occupied by the netlist to the total core area. A good floorplan ensures that the utilization factor is less than 1, leaving room for any additional logic that may be required later.

`Utilization Factor = (Area occupied by netlist / Total core area)`
​
 
*Aspect Ratio:*
The aspect ratio is the ratio of the core’s height to its width. If the aspect ratio is 1, the core is square-shaped; otherwise, it will be rectangular.

`Aspect Ratio = (Height of the core / Width of the core)`
​
### Task-1 
#### Run 'picorv32a' Design Floorplan Using OpenLANE.
Steps for running floorplan
1. Open the terminal and navigate to the directory where the design is saved.
2. Run the command ` run_floorplan` to generate the floorplan.
3. The floorplan will be saved in the `floorplan` directory.
4. Open the floorplan in the Magic tool to visualize the design.
![floorplan](./images/floorplan.png)
### Task-2
#### Calculate Die area in microns
```Die area = (Die width x Die height)```
```math
Die\ width\ in\ microns = \frac{550064}{1000} = 550.064\ Microns
```
```math
Die\ height\ in\ microns = \frac{567120}{1000} = 567.120\ Microns
```
```math
Area\ of\ die\ in\ microns = 550.064 * 567.120 = 311952.29568\ Square\ Microns
```
![DIE AREA](./images/Screenshot%202024-09-29%20161038.png)


### Task-3
#### Load the floorplan in Magic tool
Use the following command to launch MAGIC with the required technology file, LEF, and DEF:
```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-09_14-14/results/floorplan/
```
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

```
![Magic tool](./images/openmajic.png)

 Once the MAGIC GUI opens, you should see the floorplan layout displayed in the tool.

We can now use the visualization tools in MAGIC (zoom, center, select cells, etc.) to inspect and analyze the floorplan.

- Center the design: Press `S` to select the entire design and `V` to align it vertically on the screen.

- Zoom in: Left-click and drag to select a specific area, then press `Z` to zoom in on that region.

![Magic tool](./images/Screenshot%202024-09-29%20162625.png)

### Task-4
#### Run placement and run plcement def in magic tool
#### Placement in VLSI Design
Placement involves deciding where to physically place standard cells (logic elements) within a chip. Placement can be broken down into two stages:

*Global Placement:* This assigns rough locations to cells, allowing some overlap. The goal is to create an initial layout that adheres to area constraints.

*Detailed Placement:* This refines the locations, ensuring there is no overlap and that cells are placed on legal sites, which directly impacts the routing stage.
Use the following comamand to run the placement
```
run_placement
```
![placement](./images/runfloorplan.png)
![placement](./images/resultplacement.png)

Use the following command to run the placement def in magic tool

1. Navigate to the Directory
```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-09_14-14/results/placement/

```
2. Load the Placement DEF File in MAGIC
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```
![placement](./images/magicTplacement.png)

#### Inputs and Outputs in the Design Process
- Process Design Kits (PDKs): Technology-specific information for chip design.
- Design Rule Check (DRC) and Layout vs. Schematic (LVS) Rules: Ensure compliance with layout and functional requirements.
- SPICE Models: Used for circuit simulation.
- Library & Custom Specifications: Specific design libraries and user-defined parameters.

Outputs:

- CDL (Circuit Description Language): A text-based representation of the circuit.
- GDSII: The layout file for fabrication.
- LEF (Library Exchange Format): Contains cell size, pin location, and other details.
- SPICE Netlist: Extracted parasitic information for circuit simulation.
- Timing, Noise, and Power Libraries: Generated during characterization.

## Day 3
### Design library cell using Magic Layout and Ngspice characterization
To view the CMOS inverter layout in MAGIC and refer to the necessary cell files, follow these steps:

1. Clone the Custom Inverter Layout
``` 
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
![image](./images/clonerepo.png)
![image](./images/repo.png)

2. Navigate to the Inverter Layout Directory
Change to the directory where the layout files are located:

```
cd vsdstdcelldesign
cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```
![alt text](cplibfilestosrc-1.png)
3. Open the Inverter Layout in MAGIC
To open the inverter layout in MAGIC, use the following command:
```
magic -T sky130A.tech sky130_inv.mag &
```
![image](./images/magicinverter.png)
![image](./images/selctnmosregion.png)

4. Perform SPICE Extraction in MAGIC
To extract the SPICE netlist from the layout, follow these commands inside the MAGIC tool:

```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
![image](./images/extractall.png)

![image](./images/Screenshot%202024-09-29%20233428.png)

![alttext](./images/spicefile.png)

### loading spice file for ngspice simulation and make a plot
```
ngspice sky130_inv.spice
plot y vs a
```
![alt text](./images/fatalerror.png)

![alt text](./images/plot.png)

![image](./images/Screenshot%202024-09-30%20131732.png)

## Create a LEF file
### Here are some examples of DRC errors

M3.1 - Metal Width DRC Violation:
Violation: The width of the metal trace in this layout does not meet the minimum width requirement as specified by the design rules.
Error: Metal width is too narrow, which can lead to issues like higher resistance or failure during fabrication.

M3.2 - Metal Spacing DRC Violation:
Violation: The distance between adjacent metal traces in M3.2 is below the required spacing, violating the metal spacing design rule.
Error: Metal traces are placed too close together, which can lead to short circuits or reliability issues.

M3.5 - Via Overlapping DRC Violation:
Violation: Vias in M3.5 overlap with each other, breaking the rule that mandates minimum spacing between vias.
Error: Overlapping vias can cause connectivity issues and structural problems in the layout.

M3.6 - Minimum Area DRC Violation:
Violation: The enclosed area in M3.6 does not meet the required minimum area threshold as per design rules.
Error: Minimum area violation, which may lead to insufficient current-carrying capacity and reliability concerns.

These types of DRC violations must be addressed during physical verification to ensure the design is manufacturable and reliable.

To open the Magic Tool use this command
```
magic -d XR &
```
![openmagictool](./images/openmagictool.png)

Open the met3.mag file in magic tool
![metal3](./images/openm3magfile.png) 
![alt text](./images/Screenshot%202024-09-30%20152355.png)

#### Steps for Poly.9 Rule Correction:
1. Check Poly Spacing:
Ensure that the spacing between polysilicon structures meets the minimum distance specified by the design rules. For example, if the minimum poly spacing is defined as 0.15 microns, make sure there is no violation of this minimum distance.

2. Adjust Poly Width:
The width of the polysilicon lines must meet the minimum width requirement. Ensure the poly lines are wide enough as per the rule, typically in the range of 0.15 microns or larger (depending on technology node).

3. Correct Poly-to-Diffusion Overlap:
Ensure that there is proper overlap between the polysilicon and diffusion regions (source/drain). Incorrect overlap can lead to improper transistor operation or manufacturing defects.

4. Check Poly over Gate Region:
In the gate regions of transistors, make sure that the poly layer extends appropriately over the active area to ensure proper gate formation. Ensure the poly is aligned and does not violate the minimum overlap rules for the gate oxide.
5. Run DRC:
After making the corrections, run the DRC again to check if the poly.9 rule violation has been resolved.

5. Adjust Layout as Needed:
If there are still violations after initial corrections, further tweak the layout by either increasing the poly spacing, adjusting alignment, or modifying the dimensions to meet the specified design rules.

![alt text](./images/Screenshot%202024-09-30%20154232.png) 
![alt text](./images/loadpoly.png) 
![alt text](./Screenshot%202024-09-30%20154602.png) 

To update the DRC insert the few commands in our file
![updatedrc](./images/Screenshot%202024-09-30%20160129.png)

Use followig commands  to run in tkcon window
```
 # Loading updated tech file
   tech load sky130A.tech

 # Must re-run drc check to see updated drc errors
  drc check

 # Selecting region displaying the new errors and getting the error messages 
  drc why
```
Here are the snapshots of DRC checks

![alt text](./images/poly9check.png)
![alt text](./images/drc_check.png)
![alt text](./images/drc_ckeck.png)
 

## Day 4
### Pre-layout Timing Analysis and Importance of Good Clock Tree
![trackfile](./images/opentrackfile.png)


Open the custom inverter layout
```
#Change the directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

#Open the inverter layout in magic tool
magic -T sky130A.tech sky130_inv.mag &

#Change the directory to tracks.info path
cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd
```

#### Use the following commands Commands for tkcon window to set grid as tracks of locali layer

```
help grid
grid 0.46um 0.34um 0.23um 0.17um
```
![alt text](./images/Screenshot%202024-10-01%20130045.png)
![alt text](./images/Screenshot%202024-10-01%20130633.png)


Save the lef file using this command
```
save sky130_vsdinv.mag
```
![leffile](./images/Screenshot%202024-10-01%20131806.png)

![leffile](./images/Screenshot%202024-10-01%20131919.png)

#### lef command
```
lef write
```
![alt text](<Screenshot 2024-10-01 131723-2.png>)

Snapshot of .lef file

![alt text](<Screenshot 2024-10-01 131919-1.png>)
![alt text](<Screenshot 2024-10-01 131806-1.png>)

Now copy the lef file to src

```
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
![image](./images/cplibfilestosrc.png)

#### Now we have to use the following commands to be added to config.tcl to include our custom cell in the openlane flow

```
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

```
Now run the synthesis using `run_synthesis`

![images](./images/runsynthesis.png)
![images](./images/Screenshot%202024-10-01%20145736.png)

#### We will use following commands to  improve timing and run synthesis


```
prep -design picorv32a -tag 30-09_04-44 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
```

`run_synthesis`

![images](./images/chiparea.png)
![images](./images/Screenshot%202024-10-01%20152614.png)
![images](./images/Screenshot%202024-10-01%20153620.png)

#### now run floorplan using `run_floorplan`
But it fails. Instead we can use these commands to run floorplan
```
init_floorplan
place_io
tap_decap_or

```
Now run placement using `run_placement`

![images](./images/Screenshot%202024-10-01%20190308.png)
![images](./images/placementzoom.png)

```
# Command to view internal connectivity layers in tkcon window
expand
```
![images](./images/Screenshot%202024-10-01%20191608.png)

### Perform post-synthesis timing analysis using the OpenSTA tool 

mybase.sdc
```
```
This is a presta snapshot
![images](./images/presta.png)

Now slack is reduced
![images](./images/reduceslack.png)

Again `run_synthesis`

![images](./images/Screenshot%202024-10-02%20011848.png)

After Slack MET run floorplan and placement

![images](./images/Screenshot%202024-10-02%20012043.png)



## Day 5
### Task- To generate Power Distribution Network (PDN) and load the layout and Perform detailed routing
1. Generate PDN (Power Delivery Network)
The Power Delivery Network (PDN) ensures that power (VDD) and ground (VSS) are properly distributed across the chip. OpenLane provides an automated way to generate the PDN.

#### Steps to Generate PDN in OpenLane
1. Invoke the OpenLane flow: If you haven't already, start by invoking OpenLane in interactive mode:
``cd Desktop/work/tools/openlane_working_dir/openlane
``
2. Set the design and configuration
```
prep -design picorv32a
```
3. Addiitional commands to include newly added lef to openlane flow merged.lef
```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
4. Generate PDN
``` 
gen_pdn
```
![alt text](<Screenshot 2024-10-02 011848-1.png>)

####  Perform Detailed Routing
Detailed routing connects the placed standard cells using metal layers according to the routing constraints, ensuring that signal and power nets are properly wired without design rule violations.
Steps 
1. Ensure you have the placement and CTS completed: Before starting routing, make sure the placement and clock tree synthesis (CTS) steps are successfully completed. If not, run:
```
run_placement
run_cts
```
c:\Users\ankit\OneDrive\Desktop\dhanvanti\nasscon_sep\Screenshot 2024-10-02 161229.png c:\Users\ankit\OneDrive\Desktop\dhanvanti\nasscon_sep\Screenshot 2024-10-02 160632.png c:\Users\ankit\OneDrive\Desktop\dhanvanti\nasscon_sep\Screenshot 2024-10-02 155618.png

2. run detailed routing
```
run_routing
```
![routing](./images/Screenshot%202024-10-03%20005835.png)
![routing](./images/Screenshot%202024-10-03%20005956.png)
![routing](./images/Screenshot%202024-10-03%20010633.png)

### post routing STA

![routing](./images/Screenshot%202024-10-03%20010409.png)
![routing](./images/Screenshot%202024-10-03%20010433.png)







