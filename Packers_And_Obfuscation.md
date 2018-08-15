# The Unpacking Stub

The unpacking stub is loaded by the operating system upon program execution.
Its purpose is to unpack the program, thus it is typically small.

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
- lack of strings - Strings may be obfuscated or located in packed section.
- lack of imports, particularly if only imports are LoadLibrary and GetProcAddress
- Opening in a disassembler results in small amount of recognized code and rest of analysis fails.
- Odd section names, may be indicative of known packer
- Odd section size, particularly a Raw Data size of 0 or Raw Data != Virtual Size.
- small number of functions, If there is only one or two functions, it is likely the unpacker code.

### Tools to use
- PeStudio - signature detection + PE viewer
- detect it easy(DIE) - plot entropy levels.
- disassembler: ida,binja,r2,etc.
- binwalk - see if known file signatures are found in sample.

### Finding the Original Entry Point (OEP)

- If there is only one function, look near function exit (code block that doesn't loop back or point where disassembly fails). Could use a debugger and check value stored on stack before ret. This value may be entry point.
- look for a jmp to memory block, allocated memory or code that failed disassembly.
- check for strings indicative of memory allocation such as VirtualAllocEx. Then find cross reference. likely called by loadlibrary function or similar.
- look for use of imports such as malloc or similar.

If you found the memory offset that code will be deobfuscated/unpacked and loaded into, find cross-reference's. First few will likely be unpacking into the location and last one will likely be a jmp instruction.


To confirm the correct OEP is found:
- Look for 0x55 (push ebp) in deobfuscated code - indicative of function prolog.
- check for execute read write permission in code section. (!vprot [addr] in windbg).

### Unpacking After we find original entry point (OEP)
- load malware in windbg
- adjust base address so addresses line up with disassembler
- set break point and run to instruction where we jump to OEP
- use !vprot [addr of OEP] to find base address.
- use process hacker to dump memory of unpacked code. (base_addr - OEP will be the entry point in dumped binary).
### Analyzing the unpacked shellcode and final unpacking
- open in binja
- make function at base_addr - OEP
- The resulting code will likely be resolving api's, adjusting import table (if it is not shellcode), and finally transferring code execution.
- Use the same techniques to unpack the final binary.
### Rebuild the import table
- Scylla
- other tools
- manual rebuild
- TODO
