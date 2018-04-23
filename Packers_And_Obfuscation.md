# The Unpacking Stub

The unpacking stub is loaded by the operating system apon program execution.
Its porpouse is to unpack the program, thus it is typicaly small.

Jobs of the Unpacking Stub:
- unpack the original executable into memory
- resolve imports of original executable
- transfer execution to the original entry point

# Detecting Packers and Obfuscation

### Entropy
entropy is typically measured in quantity of bits-per-byte.
Since there are 8 bits per byte, entropy ranges from 0.0 to 8.0.


- 4.5-7.0 is normal range for code
- 7.0+ is indicative of packed or obfuscated code.
- Encryption tends to have higher entropy than packed code
- images and other file formats may contain high entropy and produce false positives

### Other Indicators
- lack of strings - Strings may be obfuscated or locaded in packed section.
- lack of imports, particularly if only imports are LoadLibrary and GetProcAddress
- Opening in a disassimbaler results in small amount of recognized code and rest of analysis fails.
- Odd section names, may be indicative of known packer
- Odd section size, particularly a Raw Data size of 0 or Raw Data != Virtual Size.
- small number of fuctions, If there is only one or two functions, it is likley the unpacker code.

### Tools to use
- PeStudio - signature detection + PE viewer
- detect it easy(DIE) - plot entropy levels.
- disassembler: ida,binja,r2,etc.

### Finding the Entry Point

- look near function exit (code block that doesnt loop back or point where disassembley fails).
The entry point address will likley jumped to there.
- look for a jmp to memory block, alocaded memory or code that failed disassembley.
