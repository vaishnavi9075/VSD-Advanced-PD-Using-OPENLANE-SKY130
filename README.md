# VSD-Advanced-PD-Using-OPENLANE-SKY130
This repository is to give brief idea about 'VSD-IAT Cloud based workshop on Advanced PHYSICAL DESIGN performing full RTL2GDS Flow using OPENLANE/SKY130'. 
## Flow of Course
* DAY1: Familiarity to open-lane framework.
* LAB1
* DAY2: Floorplanning and introduction to library cells.
* LAB2
* DAY3:  Standard cell characterization and introduction to sky130 tech files
* LAB3
* DAY4: Introduction to clock tree synthesis and tritonCTS.
* LAB4 
* DAY5:Introduction to routing using tritonRoute.
* LAB5

## DAY1:  Familiarity to open-lane framework.
* About OpenLANE: 
             OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for design exploration and optimization.The goal of OpenLANE is to produce clean GDSII without any human intervention. OpenLANE is tuned for Skywater 130nm open-source PDK and can be used to produce hard macros and chips.
  ![SK2_L4_1](https://user-images.githubusercontent.com/80053265/114035661-ef2dac00-989c-11eb-80b2-d057858593d2.PNG)


* OpenLANE flow consists of several stages. By default, all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLANE can also be run interactively as shown here.
  ![SK2_L2_1](https://user-images.githubusercontent.com/80053265/114035644-eb9a2500-989c-11eb-980d-5bff1f34a0ad.PNG)

1. Synthesis
      * Yosys - Performs RTL synthesis using GTech mapping
      * abc - Performs technology mappin to standard cells described in the PDK. We can adjust synthesis techniques using different integrated abc scripts to get desired results
      * OpenSTA - Performs static timing analysis on the resulting netlist to generate timing reports
      * Fault – Scan-chain insertion used for testing post fabrication. Supports ATPG and test patterns compaction
  
2. Floorplan and PDN  
      * Init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
      * Ioplacer - Places the macro input and output ports
      * PDN - Generates the power distribution network
      * Tapcell - Inserts welltap and decap cells in the floorplan
      
3. Placement.  
          Placement is done in two steps, one with global placement in which we place the designs across the chip, but they will not be legal placement with some standard 
          cells overlapping each other, to fix this we perform a detailed placement which legalizes the design and ensures they fit in the standard cell rows
      * RePLace - Performs global placement
      * Resizer - Performs optional optimizations on the design
      * OpenPhySyn - Performs timing optimizations on the design
      * OpenDP - Perfroms detailed placement to legalize the globally placed components
  
 4. CTS
      * TritonCTS - Synthesizes the clock distribution network
  
 5. Routing
      * FastRoute - Performs global routing to generate a guide file for the detailed router    
      * TritonRoute - Performs detailed routing from global routing guides
      * SPEF-Extractor - Performs SPEF extraction that include parasitic information
  
 6. GDSII Generation
      * Magic - Streams out the final GDSII layout file from the routed def
  
 7. Checks
      * Magic - Performs DRC Checks & Antenna Checks
      * Netgen - Performs LVS Checks


## LAB1:


SKY130A lib files that has been used are:

![2](https://user-images.githubusercontent.com/80053265/114036660-e4274b80-989d-11eb-82c7-11efe6672c80.PNG)


The following snapshots are of Invoking OpenLane, Package Importing & Prepare Design.
  * ./flow.tcl is the script which runs the OpenLANE flow.
  * OpenLANE can be run interactively or in autonomous mode.
  * To run interactively, use the -interactive option with the ./flow.tcl script.
  * package require openlane 0.9 imports the package in openlane.
  * Prep is used to make file structure for our design.
  
![6c](https://user-images.githubusercontent.com/80053265/114036928-1a64cb00-989e-11eb-98f0-8a1d32ea705f.PNG)
![6d](https://user-images.githubusercontent.com/80053265/114036983-26508d00-989e-11eb-8659-18ade4ad118d.PNG)


Config.tcl files in Design folder i.e., picorv32a gives the Design specific configuration switches used by OpenLANE.
An example of a configuration file is given:

![8](https://user-images.githubusercontent.com/80053265/114037003-2d779b00-989e-11eb-8017-a47487b49955.PNG)


Now, to run synthesis the command used is
   * run_systhesis
   
![11](https://user-images.githubusercontent.com/80053265/114037079-41bb9800-989e-11eb-8af9-7c5bf8e00f63.PNG)

Statistics of our Design 'Picorv32a' looks like:

![12a](https://user-images.githubusercontent.com/80053265/114037146-5730c200-989e-11eb-82ec-b7761e652d33.PNG)
![12b](https://user-images.githubusercontent.com/80053265/114037161-59931c00-989e-11eb-9ca0-0c8d2c9c667a.PNG)




## DAY2: Floorplanning and introduction to library cells.
   * In Floorplanning we typically set the:
      1. Die Area
      2. Core Area
      3. Core Utilization
      4. Aspect Ratio
      5. Place Macros
      6. Power distribution network (Normally done here but done later in OpenLANE)
      7. Place input and output pins
      
      
## LAB2:


  We can change the  clock period of the design in config.tcl file with the command:
  * set env(CLOCK_PERIOD)
  
![sk1_l5_3](https://user-images.githubusercontent.com/80053265/114039078-1043cc00-98a0-11eb-8d96-8524e85dd2b9.PNG)

  Further changing the clock period to 12ns can be seen in the snapshot.
  
![sk1_l5_again2](https://user-images.githubusercontent.com/80053265/114039146-1e91e800-98a0-11eb-8567-a364419ab66b.PNG)

  * Variable Description of floorplanning & Placement given in README.md is:
  
![sk1_l6_1a](https://user-images.githubusercontent.com/80053265/114039210-2f425e00-98a0-11eb-8ea8-d76e7ec6a5f3.PNG)
![sk1_l6_1b](https://user-images.githubusercontent.com/80053265/114039247-379a9900-98a0-11eb-96fb-179bf4b77865.PNG)


Now, we can run floorplan in openlane. The command used is,
   * run_floorplan
   
![sk1_l6_4 (2)](https://user-images.githubusercontent.com/80053265/114039433-5a2cb200-98a0-11eb-9963-7955a7c4004b.PNG)

The .def file is generated which describes core area and placement of standard cell SITES:

![sk1_l7_1 (2)](https://user-images.githubusercontent.com/80053265/114039513-6f094580-98a0-11eb-9bb8-1a547d250462.PNG)


To view our floorplan in Magic we need to provide three files as input:
   * Magic technology file (sky130A.tech)
   * Def file of floorplan
   * Merged LEF file
   * command used: magic -T (directory of tech file) lef read (directory of lef file) def read (directory of .floorplan.def file) &
   
![sk1_l7_2](https://user-images.githubusercontent.com/80053265/114039644-8d6f4100-98a0-11eb-8ccb-6e9ff20b6cc4.PNG)

Here, pin ports, preplaced cells can be seen by zooming in the magic view as:

![sk1_l8_1](https://user-images.githubusercontent.com/80053265/114039696-952ee580-98a0-11eb-867d-9a3684222ea3.PNG)
![sk1_l8_2](https://user-images.githubusercontent.com/80053265/114039723-9bbd5d00-98a0-11eb-8cee-6db7940414f6.PNG)

Selecting the perticular cell in magic view and typing 'What' in tkcon window we can see which cell is that:

![sk1_l8_3](https://user-images.githubusercontent.com/80053265/114039740-9d872080-98a0-11eb-8330-d533fb400141.PNG)


Now, we will move to Placement. Command used is:
    * run_placement.
      and we will see the following output in openlane terminal.
      
![sk2_l5_1](https://user-images.githubusercontent.com/80053265/114039747-9f50e400-98a0-11eb-8c61-61d44915826b.PNG)

To view placement in Magic the command mirrors viewing floorplanning:
    * magic -T (directory of tech file) lef read (directory of lef file) def read (directory of .placement.def file) &
    
![sk2_l5_2](https://user-images.githubusercontent.com/80053265/114039753-a24bd480-98a0-11eb-93c9-4064f0aab380.PNG)



 
## DAY3: Standard cell characterization and introduction to sky130 tech files.
   Here, we have seen 16 MASK CMOS Process and the steps involved in it are:
   * Substrate Selection : Selection of base layer on which other regions will be formed.
   * Create an active region for transistors : SiO2 and Si3N2 deposited. Pockets created using photoresist and lithography.
   * Nwell & Pwell formation : Pwell uses boron and nwell uses phosphorus. Drive in diffusion by placing it in a high temperature furnace.
   * Creating Gate terminal : For desired threshold value NA (doping Concentration) and Cox to be set.
   * Lightly Doped Drain (LDD) formation : LDD done to avoid hot electron effect and short channel effect.
   * Source and Drain formation : Forming the source and drain.
   * Contacts & local interconnect Creation : SiO2 removed using HF etch. Titanium deposited using sputtering.
   * Higher Level metal layer formation : Upper layers of metals deposited.
   
   
## LAB3

Following 3 are the zoomed in view of magic view of design after placement:

![sk1_l1_1](https://user-images.githubusercontent.com/80053265/114301071-aebe7000-9ae0-11eb-9e34-1321c94af5c3.PNG)
![sk1_L1_2](https://user-images.githubusercontent.com/80053265/114301131-fe04a080-9ae0-11eb-883b-abaa18fedf1f.PNG)
![sk1_l1_3](https://user-images.githubusercontent.com/80053265/114301142-052bae80-9ae1-11eb-8e4f-6d6c4e79015f.PNG)


Magic Layout View of Inverter Standard Cell
![sk1_l5_1](https://user-images.githubusercontent.com/80053265/114301146-09f06280-9ae1-11eb-813f-bdeaacfdf09d.PNG)


To extract the parasitic spice file for the associated layout one needs to create an extraction file:
![sk2_l9_6](https://user-images.githubusercontent.com/80053265/114301203-75d2cb00-9ae1-11eb-93f6-5323c041cd9a.PNG)

After generating the extracted file we need to output the .ext file to a spice file:
![sk2_l9_3](https://user-images.githubusercontent.com/80053265/114301218-8420e700-9ae1-11eb-86b7-044db5a403a6.PNG)

Run the what command in the tkcon window:
![sk2_l96](https://user-images.githubusercontent.com/80053265/114301237-9ef35b80-9ae1-11eb-89cb-c89903a84708.PNG)


Spice file that extracted is:
![sk3_l1_1(modified_spicefile1)](https://user-images.githubusercontent.com/80053265/114301241-a7e42d00-9ae1-11eb-8910-9e75afbb82ad.PNG)

To run the simulation with ngspice, invoke the ngspice tool with the spice file as input:
The plot can be viewed by plotting the output vs time while sweeping the input:
![sk3_l2_1](https://user-images.githubusercontent.com/80053265/114301249-b29ec200-9ae1-11eb-971b-efae0bbf0f54.PNG)
![sk3_l2_4](https://user-images.githubusercontent.com/80053265/114301257-bc282a00-9ae1-11eb-80cd-1b0705b0384a.PNG)




## DAY4: Introduction to clock tree synthesis and tritonCTS.
      Place & Routing (pnr) is performed using an abstract view of GDS files generated by Magic. The abstract information will include metal & pin information. The pnr tool
      will use the abstract view information, formally defined as a LEF information, to perform interconnect routing in conjunction to routing guides generated from the pnr flow. 
   
## LAB4
tracks.info file :
   Tracks are used during the routing stage, routes can go over the tracks, or metal traces can go over the tracks. What the file is saying is that for the li1 layer the x or
horizontal track is at an offset of 0.23 and a pitch of 0.46. The offset is the distance from the origin to the routing track in either the x or y direction. It is half
the pitch so that means the tracks are centered around the origin.
![sk1_l1_3](https://user-images.githubusercontent.com/80053265/114301576-0cec5280-9ae3-11eb-83dc-2062aac1b3f0.PNG)

To ensure a cell is aligned with routing grids in Magic we can display a grid on top of the gds file. for that the command used in tkcon window are
   * grid [xspacing yspacing xorigin yorigin]
   
![sk1_l1_5](https://user-images.githubusercontent.com/80053265/114301588-1b3a6e80-9ae3-11eb-896c-f8662472657e.PNG)


saving the mag file using command in tkcon window:
   * save filename.mag
   
![sk1_l2_1](https://user-images.githubusercontent.com/80053265/114301598-2f7e6b80-9ae3-11eb-81ad-6bbb860ebcb0.PNG)


Generating lef output for cell:
    * lef write
    
![sk1_l2_2](https://user-images.githubusercontent.com/80053265/114301603-386f3d00-9ae3-11eb-9daa-8e90df41a19e.PNG)

The generated lef file is as shown in snapshot:

![sk1_l2_3](https://user-images.githubusercontent.com/80053265/114301624-4e7cfd80-9ae3-11eb-854b-5db83fc8de35.PNG)


Reconfigure synthesis switches in the config.tcl file:

![sk1_l2_6](https://user-images.githubusercontent.com/80053265/114301640-66ed1800-9ae3-11eb-93d1-bd4806fefb31.PNG)


Now, the synthesis has been completed.
![sk1_l3_2](https://user-images.githubusercontent.com/80053265/114301699-99971080-9ae3-11eb-814f-184da31f8caf.PNG)


Now, we try to reduce the slack by changing SYNTH_STRATEGY and SYNTH_SIZING. Commands used are:
  * set ::env(SYNTH_STRATEGY)
  * set ::env(SYNTH_SIZING)
  
![sk1_l7_1 (2)](https://user-images.githubusercontent.com/80053265/114301709-a582d280-9ae3-11eb-8a58-08ea7dd65d8d.PNG)

Here, we can see reduced slack:

![sk1_l7_2](https://user-images.githubusercontent.com/80053265/114301727-af0c3a80-9ae3-11eb-8e05-4e576046ab37.PNG)
![sk1_l7_3](https://user-images.githubusercontent.com/80053265/114301747-bdf2ed00-9ae3-11eb-8544-9bb42608b9af.PNG)

Again, we run floorplan in openlane:

![sk1_l7_4](https://user-images.githubusercontent.com/80053265/114301766-cc410900-9ae3-11eb-886e-876f5c97ca92.PNG)

Running placement:

![sk1_l7_5](https://user-images.githubusercontent.com/80053265/114301781-d5ca7100-9ae3-11eb-93fc-0d32f8fd1623.PNG)

The placement.def file has been generated in respective directory.

![sk1_l7_6](https://user-images.githubusercontent.com/80053265/114301796-e11d9c80-9ae3-11eb-9f93-937b584cf1e8.PNG)


Magic view of postplacement is:

![sk1_l7_7](https://user-images.githubusercontent.com/80053265/114301806-eaa70480-9ae3-11eb-8769-21126220440d.PNG)

zoomed in view is as:

![sk1_l7_8](https://user-images.githubusercontent.com/80053265/114301871-3659ae00-9ae4-11eb-88fa-5c6738dbf4e7.PNG)

Here, is the clear view of 'vsdinv' cell placed in the picorv32a design:

![sk1_l7_11](https://user-images.githubusercontent.com/80053265/114301887-44a7ca00-9ae4-11eb-96bf-fba28f83a24f.PNG)


Using 'expand' in tkcon window and seeing the output in layout window:

![sk1_l7_12](https://user-images.githubusercontent.com/80053265/114301898-54bfa980-9ae4-11eb-9e91-8d454914ee89.PNG)


The sdc file in src folder is as shown in snapshot:

![sk2_l3_2](https://user-images.githubusercontent.com/80053265/114301912-61dc9880-9ae4-11eb-9ed6-fd7f793283c0.PNG)


Now, we will run cts. Command used is:
   * run_cts
   
![sk3_l4_1](https://user-images.githubusercontent.com/80053265/114301962-90f30a00-9ae4-11eb-9729-7cc74f25985d.PNG)


We will have a look for the .tcl files in openroad:

![sk3_l4_1a (2)](https://user-images.githubusercontent.com/80053265/114302009-c566c600-9ae4-11eb-872a-5b8f642daf08.PNG)

or_cts.tcl file is shown here. The highlighted lines are being executed when we run the command 'run_cts' in openlane flow.

![sk3_l4_4](https://user-images.githubusercontent.com/80053265/114302046-f0511a00-9ae4-11eb-8c32-d39607f63b87.PNG)


![sk3_l4_7](https://user-images.githubusercontent.com/80053265/114302112-44f49500-9ae5-11eb-9967-2fc56b1967c8.PNG)
![sk4_l3_7](https://user-images.githubusercontent.com/80053265/114302201-9c930080-9ae5-11eb-8d58-abd20b8ac072.PNG)
![sk4_l4_2](https://user-images.githubusercontent.com/80053265/114302225-bcc2bf80-9ae5-11eb-8072-688c5c19b9b8.PNG)




## DAY5: Introduction to routing using tritonRoute
   * Global and Detailed Routing.
          OpenLANE uses TritonRoute as the routing engine for physical implementations of designs. Routing consists of two stages:
        1. Global Routing - Routing guides are generated for interconnects on our netlist defining what layers, and where on the chip each of the nets will be reputed
        2. Detailed Routing - Metal traces are iteratively laid across the routing guides to physically implement the routing guides
        
        
## LAB5

Again, the complete openlane flow has been run as earlier:

![sk2_l1_1](https://user-images.githubusercontent.com/80053265/114302328-39559e00-9ae6-11eb-92f3-d0aed1bf1f3c.PNG)

Generating pdn (power delivery network) using command:
    * gen_pdn
    
![sk2_l1_3](https://user-images.githubusercontent.com/80053265/114302359-5b4f2080-9ae6-11eb-881d-4d8b7bd1bfc8.PNG)


Routing is completed and we can see it as:

![sk2_l3_1](https://user-images.githubusercontent.com/80053265/114302408-85084780-9ae6-11eb-91ec-244d1ce555d0.PNG)

The no. of violations can be seen at highlighted lines:

![sk3_l4_1](https://user-images.githubusercontent.com/80053265/114302419-8df91900-9ae6-11eb-80ea-aa4889e48382.PNG)


After routing has been completed interconnect parasitics can be extracted to perform sign-off post-route STA analysis. The parasitics are extracted into a SPEF file. The 
SPEF extractor is not included within OpenLANE as of now.

![sk3_l4_2](https://user-images.githubusercontent.com/80053265/114302436-a1a47f80-9ae6-11eb-8bea-e2518c92aef7.PNG)

And finally these are the 4 verilog files we have in synthesis directory. 

![sk3_l4_3](https://user-images.githubusercontent.com/80053265/114302441-a701ca00-9ae6-11eb-8e0a-3b5bfabe0e7b.PNG)



## Acknowledgements
     * Kunal Ghosh - Co-founder (VSD Corp. Pvt. Ltd)
     * Nickson Jose - Teaching Assistant.
     * Praharsha Mahurkar - Teaching Assistant.
     * Akurathi Radhika - Teaching Assistant.
