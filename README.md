# printf function
x86-64 assembly language implementation of printf function supporting the formats listed below.
# Supports
| Specificator | Function | Requires |
|--------------|----------|----------|
| %c | Char output | ASCII-code |
| %d | Decimal output | 4-byte integer |
| %b | Binary output | 4-byte integer |
| %o | Oct output | 4-byte integer |
| %h | Hex output | 4-byte integer |
| %s | String output | Offset to string |
| %% | Percent output | none |
# Remarks
 - In C language %h is used for the output of short integers whereas %x is used for the hex output of ints.
 - printf.asm is a compact version of copypast.asm
