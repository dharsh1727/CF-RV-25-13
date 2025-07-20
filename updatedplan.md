#   CF-RV-25-13

##  TEAM MEMBERS: Priyadharshan L & Vignesh B K

### PROBLEM STATEMENT:
C-Class core/debug RTL implementation and simulation: Implement atleast one instruction
trigger ( hardware breakpoint mcontrol6 ) in C-Class. Evaluate with manual 3-window
simulation with simulator, openocd and gdb. Also, test trigger programs from risc-v debug tests
(setup provided by Shakti team). Extra credit: Test on FPGA.

## INITIAL PLAN:
## GENERAL FLOW:

- Getting familiarized with concepts( instruction triggers, working of 3 window simulation
setup,C-class core).
- Studying RISC V debug module version 1.0 to understand the mcontrol6 trigger config.
- Understanding the existing debug logic.
- Implementation of mcontrol6 by making changes with the respective modules from the
C-class core. (before proceeding, we will be confirming about these modification points
through the discord group)
- After implementation, the 3 window simulation will be set up to verify the working.
  - Simulator - Verilator - to simulate the modified core
  - Openocd - bridges verilator and GDB
  - GDB - it controls the mcontrol6 trigger config.
    
---
As of now, we have planned to modify the stage1.bsv file in the C-class core repo. We intend to
modify the file by adding the trigger check logic inside the check_trigger() function using
trigger configuration signals (tdata1, tdata2) that represent mcontrol6 format. Following that, we
will add a logic to decode the tdata1(mcontrol6 format) into its fields (such as select, match..)
and send it to the check_trigger() logic to raise a trap when pc or instruction matches tdata2
based on select, match values. (we have to figure out where to apply this decoding logic yet).

3-window simulation: The trigger configuration data will be written via GDB using OpenOCD
then we simulate the modified RTL using Verilator. We run the program, configure the trigger
with GDB, and verify if the core halts at the correct breakpoint address.

---

## PLAN as of 18th JULY:

We have gone through the .bsv files required for modification which includes
| File / Module          | Description |
|------------------------|-------------|
| stage1.bsv       | for check_trigger logic |
| ccore_types.bsv  | trigger type definitions |
| pipe_ifcs.bsv    | pipeline interfaces between different stages  |

- Inorder to have a unique exception code for triggers stage5.bsv has been identified as the ideal stage. We have to study how the entire pipeline works with the exception works.

- Currently, we are planning to go through the csrbox where the generated bsv files are available to implement trigger field decoding logic. Then we will look into implementing changes to the code.

- Also we have installed GDB and trying to install openocd to run a 3-window simulation to get a gist of how it works with regular core.

- Moreover we are planning to run the c-class core with higher verbosity level to understand the pipeline flow.



