# Mini-MIPS

An 8-bit MIPS-like single-cycle CPU.

Overall design:
![minimips.png](./screenshots/minimips.png)

ALU design:
![alu.png](./screenshots/alu.png)

## Instruction Set Architecture

This is the instruction set for the simple 8-bit processor. There is only two 8-bit registers
(`$r0` and `$r1`). There is an 8-bit PC.

Mini-MIPS has separate instruction memory and data memory. Each has maximum capacity of 256 bytes.

See `docs/Design.pdf` for more details about the ISA.
![design.pdf](./docs/Design.pdf)

## Testing

Tests can be loaded from Logisim as ROM images and then the CPU clock button can be used tick the
program counter.

Several tests are provided in `/rom-images` directory. For example:

```
v2.0 raw
00 40 6f 20 50 7f 31 ff
```

Here is a sample program that you can convert to hex and load as a ROM image. The program loads `15` (in
decimal = `F` in hex) into register `$r0` and display. We do the same for `$r1`. Then we branch
by `beq -1` (it goes back to itself! In other words, “halt”). The result is to display `FF` on the
7-segment display and stops the program. It is a Display-and-Halt program.

```
lui     $r0, 0      # load upper 4 bits with all zeroes
ori     $r0, 15     # or with 15(F in hex)
disp    $r0, 0      # display the content of $r0 on 0th display unit
lui     $r1, 0      # load upper 4 bits with all zeroes
ori     $r1, 15     # or with 15(F in hex)
disp    $r1, 1      # display the content of $r1 on 1th display unit
beq     -1          # Halt
```
