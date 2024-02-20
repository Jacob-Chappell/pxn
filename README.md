# RoboticOwl4000's PXN
Prefix notation pre-processor for C, SystemVerilog, and other text based programming languages.

## PXN Block

A pxn block is signified by a "#{" and "}#" pair (by default, see Operators section). When the pre-processor is run on a text file, the contents of any pxn block will be converted to parenthasized infix notation.

## Operation Ordering Rules

All rules will be evaluated based on the closest operand to the operator (i.e. left to right for prefix, and right to left for postfix)

When reading the rules, use the folowing information:
- "op" stands for any operator
- "x", "y", and "z" are operands
- "f()" is any function/task
- "vec[]" is any list/vector/array
- All examples are given for prefix mode

| PXN Notation | Infix notation | Notes |
|---|---|---|
|```#{op x y}#```|```x op y```|
|```#{op (x y z)}#```|```(x op y) op z```|Order is evaluated by closest to operator first|
|```#{op1 f(op2 x y) z}#```|```f(x op2 y) op1 z```|A space between f and () will cause undefined behavior|
|```#{op1 op2x y}#```|```op2x op1 y```|op2 will be considered an unary operator|
|```#{op1 vec[op2 x y] z}#```|```vec[x op2 y] op1 z```|Follows the same rules as functions|

In the above example, the whitespace is incredibly important, as it is how the pre-processor distinguishes between unary and other operators, along with figuring out whether something is a function. Multi-dimentional arrays are completely supported, and work exactly as one would expect.

All parentheses are considered to be the same thing, so a function that uses {} is completely fine (as long as neither has a # next to it).

## Operators

All operators will be defined in a config file. Each can be multiple characters, and will be mached exactly by the pre-processor. The options for each operator will be:
- Unary only (i.e. ~ or !)
- Binary only (i.e. % or /)
- Unconstrained (i.e. + or &)
- Infix (used for , in function calls or : in vector slicing)
- Paren (used to indicate that an op pair should be considered parentheses)
- Block (used to set the start and end charsets for a pxn block, i.e. #{ and #})

All config files will completely overwrite the internal default config when added to remove ambiguity. This can cause issues if the config files are created haphazardly. As such, it is highly recommended to build from the standard config built in to the tool, which can be found in the repo. 

## Command Syntax

ro-pxn [OPTIONS] INPUT_FILE

**Options**

\-o [OUTPUT FILE]

\-c [CONFIG_FILE]

\--version

\--help
