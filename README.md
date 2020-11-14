# MIPS processor
 A cycle-accurate simulator for a 5-stage pipelined MIPS processor in C++

The MIPS program is provided to the simulator as a text file “imem.txt” file which is used to initialize the Instruction Memory. Each line of the file corresponds to a Byte stored in the Instruction Memory in binary format, with the first line at address 0, the next line at address 1 and so on. Four contiguous lines correspond to a whole instruction. Note that the words stored in memory are in “Big-Endian” format, meaning that the most significant byte is stored first.
The Data Memory is initialized using the “dmem.txt” file. The format of the stored words is the same as the Instruction Memory. As with the instruction memory, the data memory addresses also begin at 0 and increment by one in each line.
The instructions that the simulator supports and their encodings are shown in Table 1. Note that all instructions, except for “halt”, exist in the MIPS ISA. The MIPS Green Sheet defines the semantics of each instruction. 


![image](https://user-images.githubusercontent.com/63943580/99158562-1bece700-26a2-11eb-8e02-caed9956642b.png)


# Pipleline Structure

The MIPS pipeline has the following 5 stages:
1. Fetch (IF): fetches an instruction from instruction memory. Updates PC.
2. Decode (ID/RF): reads from the register RF and generates control signals required in
subsequent stages. In addition, branches are resolved in this stage by checking for the
branch condition and computing the effective address.
3. Execute (EX): performs an ALU operation.
4. Memory (MEM): loads or stores a 32-bit word from data memory.
5. Writeback (WB): writes back data to the RF.

Each pipeline stages takes inputs from flip-flops. The input flip-flops for each pipeline stage are
described in the tables below

![image](https://user-images.githubusercontent.com/63943580/99158592-62424600-26a2-11eb-81fa-1749032e5b95.png)

![image](https://user-images.githubusercontent.com/63943580/99158603-830a9b80-26a2-11eb-8e70-735b30d370bd.png)

![image](https://user-images.githubusercontent.com/63943580/99158609-8ef65d80-26a2-11eb-882f-788e2e5517b3.png)


Types of Hazards
1. RAW Hazards: RAW hazards are dealt with using either only forwarding (if possible) or,
if not, using stalling + forwarding. 
2. Control Flow Hazards:
    1. Branches are always assumed to be NOT TAKEN. That is, when a beq is fetched
in the IF stage, the PC is speculatively updated as PC+4.
    2. Branch conditions are resolved in the ID/RF stage. To make your life easier,
will ensure that every beq instruction has no RAW dependency with its
previous two instructions. In other words, you do NOT have to deal with
RAW hazards for branches!
    3. Two operations are performed in the ID/RF stage: (i) Read_data1 and
Read_data2 are compared to determine the branch outcome; (ii) the effective
branch address is computed.
    4.  If the branch is NOT TAKEN, execution proceeds normally. However, if the
branch is TAKEN, the speculatively fetched instruction from PC+4 is quashed in
its ID/RF stage using the nop bit and the next instruction is fetched from the
effective branch address. Execution now proceeds normally.
