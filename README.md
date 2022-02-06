## vsd-sta-workshop-2022
VSD STA WORKSHOP 2022

# Topics covered
- STA definition
- Startpoint, endpoint, timing points, combinational logic
- Slack calculation
- DRC's
- Asynchronous pin timing checks
- Data to data checks (mainly applicable to DDR interface)
- Clock gating checks
- Clock skew
- Latch timing
- Multiple clocks
- Timing arc's
  - Unateness
- Timing sense
- Cell delay computation
- Clock network
- GBA (Graph based analysis) and PBA (Path based analysis) 
- Liberty file
- Crosstalk and noise
- Operating modes
- Variations
  - On chip variation
  - Intra die variation
  - Inter wafer variation
- Clock groups
  - synchronous clocks
  - Asynchronous clocks
   - physically exclusive
   - logically exclusive 
- Timing exceptions
 


Slack :
- The difference b/w required time (RT) and arrival time (AT) is called Slack
- If the difference (slack) is negative, it is called negative slack else if it is positive it is called positive slack
-- The worst negative slack number of all the endpoints is called worst negative slack (WNS)
-- The total negative slack value of all the slack is total negative slack (TNS)

Latch timing :
- Time borrowing (cycle stealing)

[Liberty file](https://people.eecs.berkeley.edu/~alanmi/publications/other/liberty07_03.pdf):
[](https://www.csee.umbc.edu/courses/graduate/CMPE641/Fall08/cpatel2/slides/lect05_LIB.pdf)

[Spef file](https://www.vlsisystemdesign.com/spef-format-part-1/):

Hold Check:
- Data launched by setup check must not be captured by previous capture edge.
- Data launched by next launch edge must not be captured by current capture edge

Clock gating check:
- When a signal can control path of a clock at a cell
- The signal must be used as a clock in downstream
  - Feed a clk pin of register
  - Feed an output port
  - Feed a generated clock
- Intention of this check is that transition on gating pin does not create unnecessary active edge of the clock in the fanout
Async checks
- De-assertion needs to meet recovery(setup) and removal(hold) check requirements to have circuit properly work
- assetion can happen any time
Skew:
- Positive skew
- Negative skew
- Useful skew






Questions:
- Does .lib characterization assumes only one input switching or multiple inputs switching for multi-input cells like NAND/NOR cells?
- Follow up question, how to model multi-input switching if the .lib char assumes single input switching?
- What happens if we have crosstalk on common path for setup slack and hold slack?
-  

Lab exercises:
- Difference b/w max and min lib
- Cells present in lib
- Difference b/w NAND2 and NAND3 in lib
- Pins for cell NAND2
- Understand how report timing gets all the values for each timing point?
- Interpolation of .lib values (understanding)
- report timing and understand the path
- report_timing
- report_rat
- report_at
- clock gating checks/async path checks are not reported in the OT

To run a script in openTimer using batch mode
```
ot-shell -i <run.tcl> -o <out.log>
```



To start with ot-shell :
```
unix>ot-shell
```

![image](https://user-images.githubusercontent.com/16179505/152670504-e9e58b6a-3cf8-461c-a63c-44a95384c70e.png)

```
## reading liberty model
read_celllib osu018_stdcells.lib
## reading netlist model
read_verilog simple.v
## Parsing constraints
read_sdc simple.sdc
## Removing common path pessimism if there
cppr -enable
dump_taskflow
## report timing reports for 5 paths
report_timing -num_paths 5
dump_graph
```
![image](https://user-images.githubusercontent.com/16179505/152670585-b5e829fb-90d8-4d73-9281-5c95657ffac3.png)

report timing shows the worst path, you can increase max paths to be dumped using -num_paths option
![image](https://user-images.githubusercontent.com/16179505/152670635-27bf81c1-0203-41ef-bb07-327f8a12f9cd.png)

timing report with all timing points marked. Critical timing path is from primary input port (inp1) to endpoint f1/D
![image](https://user-images.githubusercontent.com/16179505/152670746-7b99f0ac-02cd-463e-860b-a7bef4571726.png)


| Command       | type     | Arguments          | Description                                     | Example                     |
|---------------|----------|--------------------|-------------------------------------------------|-----------------------------|
| read_celllib  | builder  | [-min | -max] file | read the cell library for early and late splits | read_celllib mylib.lib      |
| read_verilog  | builder  | file               | read the verilog netlist                        | read_verilog mynetlist.v    |
| read_spef     | builder  | file               | read parasitics in SPEF                         | read_spef myparasitics.spef |
| read_sdc      | builder  | file               | read a Synopsys Design Constraint file          | read_sdc myrule.sdc         |
| update_timing | action   | none               | update the timing                               | update_timing               |
| report_timing | action   | [-num_paths k]     | report the critical paths                       | report_timing -num_paths 10 |
| report_tns    | action   | none               | report the total negative slack                 | report_tns                  |
| report_wns    | action   | none               | report the worst negative slack                 | report_wns                  |
| dump_graph    | accessor | [-o file]          | dump the timing graph to a DOT format           | dump_graph                  |
| dump_timer    | accessor | [-o file]          | dump the design statistics                      | dump_timer                  |

One of the biggest advantage with open timer is to dump the graph and it can be visulaized using graphviz software [](https://dreampuf.github.io/GraphvizOnline/#digraph%20TimingGraph%20%7B%0A%20%20%22u4%3AA%22%3B%0A%20%20%22u4%3AY%22%3B%0A%20%20%22u3%3AA%22%3B%0A%20%20%22inp2%22%3B%0A%20%20%22tau2015_clk%22%3B%0A%20%20%22u1%3AY%22%3B%0A%20%20%22out%22%3B%0A%20%20%22u1%3AB%22%3B%0A%20%20%22u1%3AA%22%3B%0A%20%20%22u2%3AY%22%3B%0A%20%20%22u4%3AB%22%3B%0A%20%20%22inp1%22%3B%0A%20%20%22u3%3AY%22%3B%0A%20%20%22f1%3AQ%22%3B%0A%20%20%22f1%3AD%22%3B%0A%20%20%22f1%3ACLK%22%3B%0A%20%20%22u2%3AA%22%3B%0A%20%20%22u1%3AY%22%20-%3E%20%22u4%3AA%22%3B%0A%20%20%22f1%3AQ%22%20-%3E%20%22u4%3AB%22%3B%0A%20%20%22u4%3AY%22%20-%3E%20%22f1%3AD%22%3B%0A%20%20%22u4%3AB%22%20-%3E%20%22u4%3AY%22%3B%0A%20%20%22u4%3AA%22%20-%3E%20%22u4%3AY%22%3B%0A%20%20%22u4%3AB%22%20-%3E%20%22u4%3AY%22%3B%0A%20%20%22u4%3AA%22%20-%3E%20%22u4%3AY%22%3B%0A%20%20%22u2%3AY%22%20-%3E%20%22u3%3AA%22%3B%0A%20%20%22u3%3AY%22%20-%3E%20%22out%22%3B%0A%20%20%22u3%3AA%22%20-%3E%20%22u3%3AY%22%3B%0A%20%20%22u3%3AA%22%20-%3E%20%22u3%3AY%22%3B%0A%20%20%22f1%3AQ%22%20-%3E%20%22u2%3AA%22%3B%0A%20%20%22u2%3AA%22%20-%3E%20%22u2%3AY%22%3B%0A%20%20%22u2%3AA%22%20-%3E%20%22u2%3AY%22%3B%0A%20%20%22tau2015_clk%22%20-%3E%20%22f1%3ACLK%22%3B%0A%20%20%22f1%3ACLK%22%20-%3E%20%22f1%3AD%22%3B%0A%20%20%22f1%3ACLK%22%20-%3E%20%22f1%3AQ%22%3B%0A%20%20%22f1%3ACLK%22%20-%3E%20%22f1%3AD%22%3B%0A%20%20%22f1%3ACLK%22%20-%3E%20%22f1%3AQ%22%3B%0A%20%20%22inp1%22%20-%3E%20%22u1%3AA%22%3B%0A%20%20%22inp2%22%20-%3E%20%22u1%3AB%22%3B%0A%20%20%22u1%3AB%22%20-%3E%20%22u1%3AY%22%3B%0A%20%20%22u1%3AA%22%20-%3E%20%22u1%3AY%22%3B%0A%20%20%22u1%3AB%22%20-%3E%20%22u1%3AY%22%3B%0A%20%20%22u1%3AA%22%20-%3E%20%22u1%3AY%22%3B%0A%7D%0A)
![image](https://user-images.githubusercontent.com/16179505/152671892-6e9b3bce-f20d-4836-97fc-acc6122290bf.png)


# Lab2
To find the arrival time
```
report_at -pin u1:a
```
![image](https://user-images.githubusercontent.com/16179505/152671995-4b132722-4d4d-42d4-a0d2-b488d2566595.png)



Here are the full set of commands :

| Command | Form | Description |
| :------ | :--- | :---------- |
| [read_celllib](#read_celllib) | builder | reads a liberty format library file |
| [read_verilog](#read_verilog) | builder | reads a gate-level verilog netlist  |
| [read_spef](#read_spef)       | builder | reads a file of net parasitics in SPEF |
| [read_sdc](#read_sdc)         | builder | reads a Synopsys Design Constraint (sdc) file of version 2.1 |
| [read_timing](#read_timing)   | builder | reads a TAU15 Contest assertion file of timing constraints |
| [set_units](#set_units)       | builder | specifies the units used to compute timing values and report results |
| [set_at](#set_at)             | builder | specifies the arrival time of an input port |
| [set_rat](#set_rat)           | builder | specifies the required arrival time of an output port |
| [set_slew](#set_slew)         | builder | specifies the transition time of an input port |
| [set_load](#set_load)         | builder | specifies the load capacitance of an output port |
| [insert_gate](#insert_gate)   | builder | inserts a gate (instance) to the design |
| [remove_gate](#remove_gate)   | builder | removes a gate (instance) from the design |
| [repower_gate](#repower_gate) | builder | repowers a gate (instance) with a new cell |
| [insert_net](#insert_net)     | builder | inserts an empty net to the design |
| [remove_net](#remove_net)     | builder | removes a net from the design |
| [connect_pin](#connect_pin)   | builder | connects a pin to a net |
| [disconnect_pin](#disconnect_pin) | builder | disconnects a pin from a net |
| [enable_cppr](#enable_cppr)   | builder | enables common path pessimism removal analysis |
| [disable_cppr](#disable_cppr) | builder | disables common path pessimism removal analysis |
| [update_timing](#update_timing) | action | updates the timer to keep all timing values up-to-date |
| [report_timing](#report_timing) | action | reports the critical paths in the design |
| [report_at](#report_at)         | action | reports the arrival time at a pin |
| [report_slew](#report_slew)     | action | reports the transition time at a pin |
| [report_rat](#report_rat)       | action | reports the required arrival time at a pin |
| [report_slack](#report_slack)   | action | reports the slack at a pin |
| [report_tns](#report_tns)       | action | reports the total negative slack of the design |
| [report_wns](#report_wns)       | action | reports the worst negative slack of the design |
| [report_fep](#report_fep)       | action | reports the total failing endpoints in the design |
| [report_area](#report_area)     | action | reports the aggregate cell areas of the design |
| [report_leakage_power](#report_leakage_power) | action | reports the aggregate cell leakage power of the design |
| [license](#license)       | accessor | shows the license information |
| [version](#version)       | accessor | shows the version of the OpenTimer |
| [dump_timer](#dump_timer) | accessor | dumps the design statistics  |
| [dump_taskflow](#dump_taskflow)| accessor | dumps the current lineage graph to a DOT format file |
| [dump_graph](#dump_graph) | accessor | dumps the current timing graph to a DOT format file |
