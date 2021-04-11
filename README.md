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

## LAB2:


![sk1_l5_3](https://user-images.githubusercontent.com/80053265/114039078-1043cc00-98a0-11eb-8d96-8524e85dd2b9.PNG)
![sk1_l5_again2](https://user-images.githubusercontent.com/80053265/114039146-1e91e800-98a0-11eb-8567-a364419ab66b.PNG)
![sk1_l6_1a](https://user-images.githubusercontent.com/80053265/114039210-2f425e00-98a0-11eb-8ea8-d76e7ec6a5f3.PNG)
![sk1_l6_1b](https://user-images.githubusercontent.com/80053265/114039247-379a9900-98a0-11eb-96fb-179bf4b77865.PNG)
![sk1_l6_4 (2)](https://user-images.githubusercontent.com/80053265/114039433-5a2cb200-98a0-11eb-9963-7955a7c4004b.PNG)
![sk1_l7_1 (2)](https://user-images.githubusercontent.com/80053265/114039513-6f094580-98a0-11eb-9bb8-1a547d250462.PNG)
![sk1_l7_2](https://user-images.githubusercontent.com/80053265/114039644-8d6f4100-98a0-11eb-8ccb-6e9ff20b6cc4.PNG)
![sk1_l8_1](https://user-images.githubusercontent.com/80053265/114039696-952ee580-98a0-11eb-867d-9a3684222ea3.PNG)
![sk1_l8_2](https://user-images.githubusercontent.com/80053265/114039723-9bbd5d00-98a0-11eb-8cee-6db7940414f6.PNG)
![sk1_l8_3](https://user-images.githubusercontent.com/80053265/114039740-9d872080-98a0-11eb-8330-d533fb400141.PNG)
![sk2_l5_1](https://user-images.githubusercontent.com/80053265/114039747-9f50e400-98a0-11eb-8c61-61d44915826b.PNG)
![sk2_l5_2](https://user-images.githubusercontent.com/80053265/114039753-a24bd480-98a0-11eb-93c9-4064f0aab380.PNG)

 
## DAY3: Standard cell characterization and introduction to sky130 tech files.
## LAB3


![sk1_l1_1](https://user-images.githubusercontent.com/80053265/114301071-aebe7000-9ae0-11eb-9e34-1321c94af5c3.PNG)
![sk1_L1_2](https://user-images.githubusercontent.com/80053265/114301131-fe04a080-9ae0-11eb-883b-abaa18fedf1f.PNG)
![sk1_l1_3](https://user-images.githubusercontent.com/80053265/114301142-052bae80-9ae1-11eb-8e4f-6d6c4e79015f.PNG)
![sk1_l5_1](https://user-images.githubusercontent.com/80053265/114301146-09f06280-9ae1-11eb-813f-bdeaacfdf09d.PNG)
![sk2_l8_3](https://user-images.githubusercontent.com/80053265/114301188-5fc50a80-9ae1-11eb-9449-da840303e50c.PNG)
![sk2_l9_6](https://user-images.githubusercontent.com/80053265/114301203-75d2cb00-9ae1-11eb-93f6-5323c041cd9a.PNG)
![sk2_l9_3](https://user-images.githubusercontent.com/80053265/114301218-8420e700-9ae1-11eb-86b7-044db5a403a6.PNG)
![sk2_l96](https://user-images.githubusercontent.com/80053265/114301237-9ef35b80-9ae1-11eb-89cb-c89903a84708.PNG)
![sk3_l1_1(modified_spicefile1)](https://user-images.githubusercontent.com/80053265/114301241-a7e42d00-9ae1-11eb-8910-9e75afbb82ad.PNG)
![sk3_l2_1](https://user-images.githubusercontent.com/80053265/114301249-b29ec200-9ae1-11eb-971b-efae0bbf0f54.PNG)
![sk3_l2_4](https://user-images.githubusercontent.com/80053265/114301257-bc282a00-9ae1-11eb-80cd-1b0705b0384a.PNG)



## DAY4: Introduction to clock tree synthesis and tritonCTS.
## LAB4


![sk1_l1_2](https://user-images.githubusercontent.com/80053265/114301575-08279e80-9ae3-11eb-993d-0b7c79c22e5e.PNG)
![sk1_l1_3](https://user-images.githubusercontent.com/80053265/114301576-0cec5280-9ae3-11eb-83dc-2062aac1b3f0.PNG)
![sk1_l1_5](https://user-images.githubusercontent.com/80053265/114301588-1b3a6e80-9ae3-11eb-896c-f8662472657e.PNG)
![sk1_l2_1](https://user-images.githubusercontent.com/80053265/114301598-2f7e6b80-9ae3-11eb-81ad-6bbb860ebcb0.PNG)
![sk1_l2_2](https://user-images.githubusercontent.com/80053265/114301603-386f3d00-9ae3-11eb-9daa-8e90df41a19e.PNG)
![sk1_l2_3](https://user-images.githubusercontent.com/80053265/114301624-4e7cfd80-9ae3-11eb-854b-5db83fc8de35.PNG)
![sk1_l2_5](https://user-images.githubusercontent.com/80053265/114301630-5b015600-9ae3-11eb-8b40-598e92f32ba8.PNG)
![sk1_l2_6](https://user-images.githubusercontent.com/80053265/114301640-66ed1800-9ae3-11eb-93d1-bd4806fefb31.PNG)
![sk1_l3_2](https://user-images.githubusercontent.com/80053265/114301699-99971080-9ae3-11eb-814f-184da31f8caf.PNG)
![sk1_l7_1 (2)](https://user-images.githubusercontent.com/80053265/114301709-a582d280-9ae3-11eb-8a58-08ea7dd65d8d.PNG)
![sk1_l7_2](https://user-images.githubusercontent.com/80053265/114301727-af0c3a80-9ae3-11eb-8e05-4e576046ab37.PNG)
![sk1_l7_3](https://user-images.githubusercontent.com/80053265/114301747-bdf2ed00-9ae3-11eb-8544-9bb42608b9af.PNG)
![sk1_l7_4](https://user-images.githubusercontent.com/80053265/114301766-cc410900-9ae3-11eb-886e-876f5c97ca92.PNG)
![sk1_l7_5](https://user-images.githubusercontent.com/80053265/114301781-d5ca7100-9ae3-11eb-93fc-0d32f8fd1623.PNG)
![sk1_l7_6](https://user-images.githubusercontent.com/80053265/114301796-e11d9c80-9ae3-11eb-9f93-937b584cf1e8.PNG)
![sk1_l7_7](https://user-images.githubusercontent.com/80053265/114301806-eaa70480-9ae3-11eb-8769-21126220440d.PNG)
![sk1_l7_8](https://user-images.githubusercontent.com/80053265/114301871-3659ae00-9ae4-11eb-88fa-5c6738dbf4e7.PNG)
![sk1_l7_11](https://user-images.githubusercontent.com/80053265/114301887-44a7ca00-9ae4-11eb-96bf-fba28f83a24f.PNG)
![sk1_l7_12](https://user-images.githubusercontent.com/80053265/114301898-54bfa980-9ae4-11eb-9e91-8d454914ee89.PNG)
![sk2_l3_1](https://user-images.githubusercontent.com/80053265/114301908-5e491180-9ae4-11eb-9a83-0edf5f0db5c0.PNG)
![sk2_l3_2](https://user-images.githubusercontent.com/80053265/114301912-61dc9880-9ae4-11eb-9ed6-fd7f793283c0.PNG)
![sk2_l3_3](https://user-images.githubusercontent.com/80053265/114301925-702ab480-9ae4-11eb-93fa-03b45b0c968d.PNG)
![sk3_l4_1](https://user-images.githubusercontent.com/80053265/114301962-90f30a00-9ae4-11eb-9729-7cc74f25985d.PNG)
![sk3_l4_1a (2)](https://user-images.githubusercontent.com/80053265/114302009-c566c600-9ae4-11eb-872a-5b8f642daf08.PNG)
![sk3_l4_4](https://user-images.githubusercontent.com/80053265/114302046-f0511a00-9ae4-11eb-8c32-d39607f63b87.PNG)
![sk3_l4_7](https://user-images.githubusercontent.com/80053265/114302112-44f49500-9ae5-11eb-9967-2fc56b1967c8.PNG)
![sk4_l3_7](https://user-images.githubusercontent.com/80053265/114302201-9c930080-9ae5-11eb-8d58-abd20b8ac072.PNG)
![sk4_l4_2](https://user-images.githubusercontent.com/80053265/114302225-bcc2bf80-9ae5-11eb-8072-688c5c19b9b8.PNG)



## DAY5: Introduction to routing using tritonRoute
## LAB5


![sk2_l1_1](https://user-images.githubusercontent.com/80053265/114302328-39559e00-9ae6-11eb-92f3-d0aed1bf1f3c.PNG)
![sk2_l1_3](https://user-images.githubusercontent.com/80053265/114302359-5b4f2080-9ae6-11eb-881d-4d8b7bd1bfc8.PNG)
![sk2_l3_1](https://user-images.githubusercontent.com/80053265/114302408-85084780-9ae6-11eb-91ec-244d1ce555d0.PNG)
![sk3_l4_1](https://user-images.githubusercontent.com/80053265/114302419-8df91900-9ae6-11eb-80ea-aa4889e48382.PNG)
![sk3_l4_2](https://user-images.githubusercontent.com/80053265/114302436-a1a47f80-9ae6-11eb-8bea-e2518c92aef7.PNG)
![sk3_l4_3](https://user-images.githubusercontent.com/80053265/114302441-a701ca00-9ae6-11eb-8e0a-3b5bfabe0e7b.PNG)
