# Motorola MEK6800D2 JBUG ROM

This file documents my process in creating the jbug.asm source code file, using the listing from the
MEK6800D2 manual as the source.
I tried to keep this source code file as consistent as possible with the original source (including misspellings, etc).

I was not able to find an assembler which would assemble the exact original source syntax.

Most 6800 assemblers (a0.exe, pasm00.exe, etc.) will not accept the old "LDA A", "LDA B", etc. format, instead requiring the instructions to be written as "LDAA", "LDAB", etc.  (However, MC6800, available from https://sites.google.com/site/gogleoops/mc6800-assembler, does accept this syntax.)

I did change the syntax of all of these types of statements so that this source file could be assembled by most current 6800 assemblers.
The applicable statements were changed to:

ADDA				DECA and DECB		PSHA and PSHB
ANDA and ANDB		INCA and INCB		PULA
ASLA and ASLB		LDAA and LDAB		SBCA and SBCB
CLRA and CLRB		LSRA				STAA and STAB
CMPA and CMPB		ORAA and ORAB		SUBA and SUBB

However, MC6800 does not accept the "JMP X", "STA A X", "LDA A X" syntax (found in lines 84, 101, 657 and 691); these statements had to be rewritten as "JMP 0,X", "STA A 0,X", and "LDA A 0,X".

I did change the syntax in lines 84, 101, 657, 691 from "X" to "0,X".

The original source did not define the bytes at addresses E3F4, 5, 6 and 7.
I modified the source with an FCB statement forcing these 4 bytes to be "00", so that the resulting .s19 file would be consistent with the widely available jbug.rom and jbug.hex files (since some assemblers do not fill uninitialized source with "00").

This jbug.asm file was successfully assembled using pasm00, mc6800, and as11.

> as11 requires commenting out the "OPT" statement in line 11.

To assemble the source using Motorola's relocating macro assembler PASM00, I used the following syntax:

```shell
pasm00 -dxs -l jbug.lst jbug.asm
pld -s -m a -o jbug.s19 jbug.o
# jbug.s19 is not an .s19 file.  However, its name is encoded into the actual .s19 file by ubuilds (which gives it an "mx" extension!)
ubuildsjbug.s19
```

These statements create an object file jbug.mx, which will normally be renamed jbug.s19 by most users.
The s0 record at the top of the file will have the name encoded as "jbug.s19".

This object file will have two lines corresponding to uninitialized RAM at A000 - A0F0.
Probably most users will want to delete these lines.

I verified electronically that the .s19 file produced by this JBUG.ASM source file does assemble to the exact image found in jbug.rom and jbug.hex.

greg simmons/ab8im

Herb Johnson Aug 27 2019

I created JBUG_A68.ASM which has a leading semicolon before comments on opcoded lines.
My A68 assembler requires that.
But my A68 assembler accepts a * in column 1 as a comment line.
The result assembles correctly under A68.

Greg Simmons Aug 28 2019

I created JBUG2A68.ASM from your JBUG_A68.ASM, where I restored all "ADDA"-style sources to "ADD A" style.
