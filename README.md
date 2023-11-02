# pes_traffic

## Traffic Light Controller : RTL to GDS and Physical Design using OpenLane 

  
### Introduction

Designing a Traffic Light Controller using Verilog is a fascinating and practical application of digital design and hardware description languages. 

Traffic light controllers are vital components of modern urban infrastructure, ensuring the smooth and safe flow of traffic at intersections.

Traffic Light Controller typically involves the design and simulation of a digital circuit that mimics the behavior of traffic lights at an intersection. The controller manages the timing and sequencing of red, yellow, and green lights for 
multiple directions of traffic

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/0966d48d-e74a-4d8a-9eec-5d876df75b25)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/6d5a2633-e26a-449b-bb1a-0992e6ad0c84)


### State Diagram of the design using Intel Quartus Prime Lite

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/84c48ffb-a1f3-4c6f-8714-1e3b2eaa1538)

### RTL of the design using Intel Quartus Prime Lite

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/e1b3c00f-7442-486d-8af2-6218ccfcd69e)



<details>
  <summary>RTL to GDS using Yosys and iverilog</summary>
    <br>


<details>
<summary>Tools Used and Installation  </summary>
<br>
   

##### Iverilog:

  ``` 
   sudo apt-get update
   sudo apt-get install iverilog

```

##### Yosys:

   ```
   git clone https://github.com/YosysHQ/yosys.git
   sudo apt install make
   sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
   sudo make install
```

</details>
<details>
<summary>Pre Simulation of design </summary>
<br>

### Simulation using iverilog 

After successfull installation lets create folder for our files
```
mkdir pes_traffic_controller
```

Copy the design file and testbench file provided and paste the created directory

Use this commands for simulation

```
cd pes_traffic_controller
iverilog pes_traffic.v  pes_tb_traffic.v
./a.out
gtkwave dump.vcd
```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/7381f2c8-843f-4079-b563-5f9e56908c5e)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/c8bb293c-59c1-429e-8a21-86867125ac15)

Output Waveform:

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/6a30bb0a-ce5d-4c35-a4a6-8bba21e8bd5a)


</details>
<details>
<summary>Synthesis Using GLS  </summary>
<br>
   
### Introduction

Synthesis is a critical step in the design of integrated circuits. It transforms the high-level, abstract representation of a design, known as Register-Transfer Level (RTL), into a gate-level netlist. This netlist is composed of actual logic gates that are available in the specific technology libraries for the target chip.

The synthesis process unfolds in several stages:

   Conversion of RTL to Basic Logic Gates: Initially, the RTL description is translated into a network of fundamental logic gates like AND, OR, and flip-flops.

  Mapping to Technology-Dependent Gates: The next step involves matching the basic logic gates from the RTL description to the corresponding technology-specific gates available in the chosen library.

   Optimizing the Netlist: After mapping, optimization comes into play. The goal is to enhance the netlist by making it more efficient, but without violating any constraints set by the designer. This optimization can involve minimizing gate count, reducing power consumption, and improving performance.

#### Synthesis using GLS of our design 

Invoking Yosys , reading the skywater130 pdk library and reading the design file 

Use command

```
yosys
read_liberty -lib /home/azeem/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog pes_traffic.v
``` 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/8b1359df-05ae-49e9-9d84-ce8651a7b0aa)

Synthesis: 

for synthesis use command ``` synth -top pes_traffic ``` 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/76ecd4e9-cc0c-4185-a993-8d95e581ec09)

Printing Stat:

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/cf4c391e-2dba-4bda-ad90-f8f36ab14523)

ABC: 

To run ABC use command ``` abc -liberty /home/azeem/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib ```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/d2bd53e3-1937-458f-a949-9d0d9eb2d58b)

ABC results:

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/d6f3fdf5-5c7c-41c2-81bd-956912432c71)


Layout :

Use command ``` show ``` to get Layout 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/5cdeb55d-8f14-4315-8565-1d2990a1a3da)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/fc56ff2b-7529-4717-849f-1661d5f0443c)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/851c1612-1326-4ec7-812c-95d74eac16ec)

Netlist :

Use Command ``` write_verilog -noattr pes_traffic_netlist.v ```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/a7fd43a1-6d75-43ac-a6e6-44f7cb695658)

</details>
<details>
<summary>Post Simulation </summary>
<br>
   
### Simulation created netlist using iverilog 

Use command 
```
iverilog /home/azeem/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v /home/azeem/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v pes_traffic_netlist.v pes_tb_traffic.v
./a.out
gtkwave dump.vcd
```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/e1db5423-4200-4d0d-a16f-2df010838e80)

Final Post Simulation Output Waveform:

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/b77690d9-77fc-4d30-a851-1f1db854bd24)

</details>
<details>
<summary>Conclusions </summary> </summary>
<br>

As we can see Pre and Post Simulation Results match , so we can confirm there is no GLS mismatch. We can Go ahead with PD development 

Results:

- Total Number of Cells : 292 
- Total Number of Input Signals : 62
- Total Number of Output Signals : 71 
- Total Number of Internal Signals : 160 



</details>
<details>
<summary>Referrence Repos </summary>
<br>

https://github.com/AzeemRG/asic_special_topic

Also Checkout for Physical Design Using OpenLane

https://github.com/AzeemRG/Pes_Openlane_pd




</details>

</details>

<details>
  <summary>Physical Design using OpenLane</summary>
    <br>

# Introduction to OpenLane 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/74df30d0-f050-4ca7-ab17-d4f7da9df9c7)

OpenLane is an open-source, automated digital ASIC (Application-Specific Integrated Circuit) design flow framework. It plays a pivotal role in simplifying and streamlining the process of designing custom integrated circuits, making it more accessible to a wider range of engineers and designers.

Here is the basic OpenLane Design Flow

RTL Design

Synthesis

Floor Planning

Placement and Routing

Clock Tree Synthesis

Power Planning

Physical Verification

GDSII Generation

Manufacturing

Testing

Packaging

Deployment


<details>
<summary>Complete Installation </summary>
<br>

## Docker Installation

## OpenLane Installation

## Magic Installation 


</details>
<details>
<summary> Getting Started </summary>
<br>

Launching OpenLane

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/98cbd04d-a047-456d-b006-b293424b7092)

Before getting started make sure to to create your design folder 

```
    cd OpenLane\designs
    mkdir pes_traffic
    cd pes_traffic
    mkdir src
```
Add ur verilog file inside and outside of src folder 

Create config.json file using ``` gedit config.json ```  and add inside and outside of the src folder 

Note : Also make sure to add pdk file..  in my case its skywater130 pdk 

Config file should look like this

```

{
    "DESIGN_NAME": "pes_traffic",
    "VERILOG_FILES": "dir::src/pes_traffic.v",
    "CLOCK_PORT": "clk",
    "CLOCK_NET": "clk",
    "GLB_RESIZER_TIMING_OPTIMIZATIONS": true,
    "CLOCK_PERIOD": 5,
    "PL_RANDOM_GLB_PLACEMENT": 1,
    "PL_TARGET_DENSITY": 0.5,
    "FP_SIZING" : "relative",

"LIB_SYNTH": "dir::src/sky130_fd_sc_hd__typical.lib",
"LIB_FASTEST": "dir::src/sky130_fd_sc_hd__fast.lib",
"LIB_SLOWEST": "dir::src/sky130_fd_sc_hd__slow.lib",
"LIB_TYPICAL": "dir::src/sky130_fd_sc_hd__typical.lib",
"TEST_EXTERNAL_GLOB": "dir::../pes_ic/src/*",
"SYNTH_DRIVING_CELL":"sky130_vsdinv",

    "pdk::sky130*": {
        "FP_CORE_UTIL": 5,
        "scl::sky130_fd_sc_hd": {
            "FP_CORE_UTIL": 2
        }
    }

    

}

```

To undergo interactive flow use command 

```
 .\flow.tcl -interactive
package require openlane 0.9
prep -design <UR_DESIGN_NAME>
```



</details>
<details>
<summary> Synthesis </summary>
<br>

use command ```run_synthesis``` to initiate it 

 ![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/3a57352a-eaaf-49dd-b7c4-18e6c29dd39c)

to see the report or printing stats to go the following path 

``` /home/azeem/OpenLane/designs/pes_traffic/runs/RUN_Direct/reports ``` in that go to synthesis and check the report 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/d23ba5d5-9b13-407b-bb8e-a888a8aab10f)


![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/f9be828d-2a62-454c-90c3-88a11f831cd7)

We can see info about cells and area of chip module through this step and get intresting factors like flop ratio or cell ratio etc 

</details>
<details>
<summary> Floorplan </summary>
<br>

use command ```run_floorplan``` to initiate it 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/04c69744-9469-48af-9b32-c705a44e8ec0)

to see the floorplan layout go the path ``` cd  /home/azeem/OpenLane/designs/pes_traffic/runs/RUN_2023.11.02_19.46.32/results/floorplan ```

Use magic tool to see the layout using the .def file created during the process

``` magic -T /home/azeem/OpenLane/pdk/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_traffic.def & ```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/e59be8d9-602c-43ac-9ade-ea473cce7230)


</details>
<details>
<summary> Placement </summary>
<br>

use command ``` run_placement``` 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/220a0105-586f-4391-8700-79eda3824db9)

use magic tool to see the layout by going to path same path but this time its placement `` ......results/placements

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/4b69d7b4-f3b8-4db5-8640-99fd96e0f3da)


![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/c49ee4de-0725-49f5-af33-a4fe8ed2f557)


![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/77b395cb-9ba8-4e1e-8aa4-f23e66f1a672)


</details>
<details>
<summary> CTS </summary>
<br>

use command ```run_cts ```

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/3f7e199a-f9e8-4e6b-b6d1-d5aef6ee0235)

to see the slack reports go the log folder using path 
```
/home/azeem/OpenLane/designs/pes_traffic/runs/RUN_2023.11.02_19.46.32/logs/cts

in that open 12-cts_sta.log

```
we can see slack is meet and there skew in under control 

 ![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/37ced2f4-bf90-4bc2-bd78-efdaf42e596a)


![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/da9e94f2-9a70-4ef8-89de-f0ac2da53c04)

Note : log file is added for more information 


</details>
<details>
<summary> Routing </summary>
<br>

use command ``` run_routing ``` 

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/a845c542-d89c-483d-9ab6-870729acaa12)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/4a8c96d3-e3b1-460e-9b4d-1462de34aa63)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/63687bbb-a2de-4086-a407-14052cf293a2)



</details>
<details>
<summary> Non-interactive flow </summary>
<br>






