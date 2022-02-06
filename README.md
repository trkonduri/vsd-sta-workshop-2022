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
- Latch timing
- Multiple clocks
- Timing arc's
  - Unateness
- Timing sense
- Cell delay computation
- Clock network
- GBA (Graph based analysis) and PBA (Path based analysis) 
- Liberty file


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
- 

