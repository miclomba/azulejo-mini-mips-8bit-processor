# Mini-MIPS

An 8-bit MIPS-like single-cycle CPU.

Overall design:
![minimips.png](./screenshots/minimips.png)

ALU design:
![alu.png](./screenshots/alu.png)

## Instruction Set Architecture

This simple 8-bit processor instruction set has two 8-bit registers (`$r0` and `$r1`) and an 8-bit program counter (PC).

Mini-MIPS has separate instruction memory and data memory. Each has maximum capacity of 256 bytes.

The instruction set contains:

### R-Type Instructions

- and: $rd = $rd & $rx
- or: $rd = $rd | $rx
- not: $rd = ($rx)`
- xor: $rd = $rd ^ $rx
- add: $rd = $rd + $rx
- neg: $rd = -($rx)
- sll: $rd = $rx << 1 (shift left logical)
- sla: $rd = $rx \* 2 (shift left arithmetic)

### I-Type Instructions

- lui (load upper immediate):

* Loads an immediate value into the upper bits of a register.

- sw (store word):

* Stores the value from a register into memory at an address offset.

- lw (load word):

* Loads a value from memory into a register using an immediate offset.

- ori (bitwise OR immediate):

* Performs a bitwise OR between a register and an immediate value.

- disp (display):

* See the section on the 7-segment display, below.

See ![design.pdf](./docs/Design.pdf) for more details about the ISA.

### J-Type Instructions

- jump instruction

* jump instruction has 5-bit address. The address(5-bit) will be concatenated with the upper 3 bits of current value of PC.

- beg instruction

```
if $r0 == $r1
    PC = (PC + 1) + offset(sign extended)
else
    PC = PC + 1
```

## Prerequisites

Install `logisim 2.3.2`: https://www.cburch.com/logisim/

An executable `logisim/logisim-2.3.2.jar` is provided for your convenience.

Note: Your OS may refuse to run the jar file so you will need to provide an exception.

## Running

Open `logisim` UI, then open the `miniMips.circ` file.

## Testing

Tests can be loaded from Logisim as ROM images. Use the CPU clock button to tick the
program counter. Or run the program using logisim UI.

Several tests are provided in `/rom-images` directory. For example:

```
v2.0 raw
00 40 6f 20 50 7f 31 ff
```

Below is a sample Display-and-Halt program that can be converted to hex and loaded as a ROM image. The program loads
`15` (`F` in hex) into register `$r0` for display. We do the same for the `$r1` register. Then we branch
by `beq -1` (i.e. halt). The result is to display `FF` on the 7-segment display and stops the program.

```
lui     $r0, 0      # load upper 4 bits with all zeroes
ori     $r0, 15     # or with 15 (F in hex)
disp    $r0, 0      # display the content of $r0 on 0th display unit of the 7-segment display
lui     $r1, 0      # load upper 4 bits with all zeroes
ori     $r1, 15     # or with 15 (F in hex)
disp    $r1, 1      # display the content of $r1 on 1th display unit of the 7-segment display
beq     -1          # Halt
```
