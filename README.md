# pes_traffic

## Traffic Light Controller : RTL to GDS

### Introduction
Designing a Traffic Light Controller using Verilog is a fascinating and practical application of digital design and hardware description languages. 

Traffic light controllers are vital components of modern urban infrastructure, ensuring the smooth and safe flow of traffic at intersections.

Traffic Light Controller typically involves the design and simulation of a digital circuit that mimics the behavior of traffic lights at an intersection. The controller manages the timing and sequencing of red, yellow, and green lights for 
multiple directions of traffic

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/0966d48d-e74a-4d8a-9eec-5d876df75b25)

![image](https://github.com/AzeemRG/pes_traffic/assets/128957056/6d5a2633-e26a-449b-bb1a-0992e6ad0c84)


### State Diagram of the design

### Tools Used and Installation

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


### Synthesis using GLS






