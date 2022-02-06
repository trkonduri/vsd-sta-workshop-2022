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
