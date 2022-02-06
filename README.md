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

[Spef file](https://www.vlsisystemdesign.com/spef-format-part-1/):

Questions:
- Does .lib characterization assumes only one input switching or multiple inputs switching for multi-input cells like NAND/NOR cells?
- Follow up question, how to model multi-input switching if the .lib char assumes single input switching?
- What happens if we have crosstalk on common path for setup slack and hold slack?
-  

