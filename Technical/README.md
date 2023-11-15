# Technical Design Document

This write-up will serve as an attempt of explaining how this cartridge functions. I'm not going to explain the majority of the cart, as you can find most of the explanation in <a href="https://github.com/MouseBiteLabs/Game-Boy-MBC3-Cartridge/tree/main/Technical">my MBC3 repo.</a> Therefore, this write-up will mostly focus on how the multicart portion works and the necessary components to safely implement it.

Please correct me on any information I may have gotten wrong.

## Schematic

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC3-Multicart/assets/97127539/d0b08991-53ee-4bca-b7ae-edaa103427b0)

## Multicart Basics

A byte is eight bits - corresponding to the eight digital pins D0 to D7. The address pins A0 to A21 determine how many bytes the EEPROM can hold in memory. There are 22 address pins on the 29F033, and they can be either 0 or 1 - therefore the EEPROM can hold 2^22, or 4,194,304, bytes total. Each byte is located at a different "address". Note that the ROM chip 29F033 is a 4 MB EEPROM - see where the 4 comes from? So, this should mean we can hold one 4 MB ROM file on the chip. But, we could also store two 2 MB ROMs, or four 1 MB ROMs, because those add up to 4 MB.

If we want to hold 2 MB of data we only need 21 address pins, A0 through A20, because 2^21 is 2,097,152 - half of the total bytes with 22 address pins. **So omitting one address pin halves the amount of bytes the EEPROM can hold.** But we still have that extra address pin A21; the total space of the chip is *still* 4 MB. If we set A21 at 0 while programming the first game using address pins A0 through A20, then we can set it to 1 to use the rest of the available space on the chip for a second game. Then we simply toggle the A21 pin to 0 or 1 to switch games. These are typically referred to as "memory banks".

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC3-Multicart/assets/97127539/a069322a-3be1-4b8f-8a22-4e24da3ded10)

If we wanted to do four games, we just take the next highest address pin, A20, and toggle that between 0 and 1 as well. Then you have two address pins that can be either 0 or 1, giving you four combinations, and thus four banks of memory.

## D Flip-Flop

A flip-flop in the most basic sense is a device that can change an output between an ‘on’ state and an ‘off’ state based on various inputs, and is generally used as a “memory cell device,” or a device that holds a bit of memory (either 0 or 1) without reacting to the input until called upon. There are various types of flip-flops with different features, but for the sake of this article, we’ll be looking at the “D flip-flop”. The ‘D’ stands for ‘data’. Check out a single D flip-flop cell below.

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC3-Multicart/assets/97127539/2fedd60e-c378-4073-88bf-37eefe695a89)

Here we see six different pins: D, >, S, R, Q, and ‘Q bar’ (henceforth denoted as /Q). The functions of these pins are pretty straightforward.

D : Data input
> : Clock input (CLK)
S : Set
R : Reset
Q : Data out
/Q : Inverse data out

First, let’s go over how the data, clock, and data out pins relate to each other. We can see how they affect each other based on this simplified truth table.

D	CLK	Q	/Q
0	Rising Edge	0	1
1	Rising Edge	1	0
X	Not Rising Edge	Q	/Q
You can see from this table that the output Q becomes whatever is on the input D when the clock changes from low to high (or, the rising edge). When the clock signal is not moving, or if it falls from high to low, the Q and /Q outputs stay exactly what it was, no matter what the D pin is doing. The output therefore only updates to the input whenever the clock signal rises from low to high. And, if you hadn’t guessed already, /Q is always the inverse of the Q pin. Here’s a quick timing diagram to help explain the truth table. Note that only when the clock has a rising edge, does the state of the Q pins update to whatever we input to D at that time.


Now, the S and R pins. These pins basically override whatever is on the Data pin and forces the Q and /Q pins to a certain value. If they are both disabled, then the output follows the data pin based on the clock pin as described above. We don’t really use the S and R pins in this explainer, but here’s the truth table anyway.

S	R	Q	/Q
1	0	1	0
0	1	0	1
1	1	1	1
You can think of this as “when the chip is set, the output is 1” and “when the chip is reset, the output is 0”. Normally, you shouldn’t put S and R to both be true, but the D flip-flop can reconcile this by setting both Q and /Q high.

Note that for many flip-flops, like the 74HC74, the S and R pins are inverse logic – that is, they are actually /S and /R. This means that the truth table is inverted for both the S and R columns. Practically speaking, this just means we need to set these pins high in order to enable the flip-flop to operate normally.
