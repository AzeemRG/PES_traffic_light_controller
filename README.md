# pes_traffic

## Traffic Light Controller : RTL to GDS

  
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





