# MIPS processor
 cycle-accurate simulator for a 5-stage pipelined MIPS processor in C++

The MIPS program is provided to the simulator as a text file “imem.txt” file which is used to initialize the Instruction Memory. Each line of the file corresponds to a Byte stored in the Instruction Memory in binary format, with the first line at address 0, the next line at address 1 and so on. Four contiguous lines correspond to a whole instruction. Note that the words stored in memory are in “Big-Endian” format, meaning that the most significant byte is stored first.
The Data Memory is initialized using the “dmem.txt” file. The format of the stored words is the same as the Instruction Memory. As with the instruction memory, the data memory addresses also begin at 0 and increment by one in each line.
The instructions that the simulator supports and their encodings are shown in Table 1. Note that all instructions, except for “halt”, exist in the MIPS ISA. The MIPS Green Sheet defines the semantics of each instruction. 
