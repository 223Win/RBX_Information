
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
Using the algorithm you will be able to encode all opcodes
```lua
Encoded = ( B - (OpcodeIndex * I) ) + 1
if (Encoded <= 0) then
	B += M+1
```
This algorithm probably will not change but if it does then this entire document will not longer be valid in some way
### How to use
To use this you should go through each and every luau opcode, and use the encoding algorithm to encode every opcode, keeping a list of the decoded and encoded, or you can map it.
This could look like:
```lua
EncodedMap[Encoded] = LuauOpcode
```
`Encoded` being the encoded opcode and the `LuauOpcode` being the Normal Unencoded Opcode from Luau
Now you can do something like:
```lua
local DecodedOpcode = EncodedMap[EncodedOpcode] -- Gives you LuauOpcode
```

## Information

* `B` is not needed since its the same as `M` just not as a constant value, as it can change via the encoding process
* In the case `I` ever changes so will `M` vice versa. However in rare cases this can change and only `I` or `M` will change. This normally occurs when Roblox or Luau Updates their bytecode.
* How to get `I`... Take the first 2 opcodes that are next to each other and subtract the first with the second. This gives you `I`
* How to get `M`... Take a Opcode that when subtracted by `I` will be negative, get the next opcode that is encoded and add the negative value + 1 to the encoded Opcode. This provides `M`
* However a better method is to look at luau. You may notice ```cpp #define LUAU_INSN_OP(insn) ((insn) & 0xff) ``` The value that its doing a bit and on is `M`
* Once you have all `I`, `B`, and `M` you can fully decrypt all of Roblox's Opcodes using the Algorithm shown above.
* Please note the algorithm above is a encoder, using the encoding you can map each opcode with the encoded one, this allows you to decode very easily
