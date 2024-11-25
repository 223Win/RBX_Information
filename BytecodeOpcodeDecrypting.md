
# Roblox Opcode Decoding
## Basic Variable Information
* B - Opcode Base (1st Opcode Encoded Usually LOP_BREAK)
* M - Max Opcode (Largest Encoded Opcode)
* I - Opcode Iterator (Iterator for opcodes: `0x1D` | `29`

## Current Variables
| B | M | I |
|---|---|---|
| `0xE3` or `227` | `0xFF` or `255` | `0x1D` or `29`

## Algorithm
Using the algorithm you will be able to decode all opcodes
```lua
Decoded = ( B - (OpcodeIndex * I) ) + 1
if (Decoded <= 0) then
	B += M+1
```
This algorithm probably will not change but if it does then this entire document will not longer be valid in some way

## Information

* B is normally not able to be obtained so the use of the next Opcode is a better option to get B from (LOP_LOADNIL)
* In the case `I` ever changes so will `M` vice versa. However in rare cases this can change and only `I` or `M` will change. This normally occurs when Roblox or Luau Updates their bytecode.
* How to get `I`... Take the first 2 opcodes that are next to each other and subtract the first with the second. This gives you `I`
* How to get `M`... Take a Opcode that when subtracted by `I` will be negative, get the next opcode that is encoded and add the negative value + 1 to the encoded Opcode. This provides `M`
* Once you have all `I`, `B`, and `M` you can fully decrypt all of Roblox's Opcodes using the Algorithm shown above. 
