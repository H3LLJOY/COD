# Chip Logic (By William Stallings)

As with other integrated circuit products, semiconductor memory comes in packaged chips. Each chip contains an array of memory cells. In the memory hierarchy as a whole, we saw that there are trade-offs among speed, capacity, and cost. These trade-offs also exist when we consider the organization of memory cells and functional logic on a chip. For semiconductor memories, one of the key design issues is the number of bits of data that may be read/written at a time. At one extreme is an organization in which the physical arrangement of cells in the array is the same as the logical arrangement (as perceived by the processor) of words in memory. The array is organized into W words of B bits each. For example, a 16-Mbit chip could be organized as 1M 16-bit words. At the other extreme is the so-called 1-bit-per-chip organization, in which data are read/written 1 bit at a time. We will illustrate memory chip organization with a DRAM; ROM organization is similar, though simpler.

Figure 5.3 shows a typical organization of a 16-Mbit DRAM. In this case, 4 bits are read or written at a time. Logically, the memory array is organized as four square arrays of 2048 by 2048 elements. Various physical arrangements are possible. In any case, the elements of the array are connected by both horizontal (row) and vertical (column) lines. Each horizontal line connects to the Select terminal of each cell in its row; each vertical line connects to the Data-In/Sense terminal of each cell in its column.

Address lines supply the address of the word to be selected. A total of log2 W lines are needed. In our example, 11 address lines are needed to select one of 2048 rows. These 11 lines are fed into a row decoder, which has 11 lines of input and 2048 lines for output. The logic of the decoder activates a single one of the 2048 outputs depending on the bit pattern on the 11 input lines (2^11 = 2048). An additional 11 address lines select one of 2048 columns of 4 bits per column. Four data lines are used for the input and output of 4 bits to and from a data buffer.

On input (write), the bit driver of each bit line is activated for a 1 or 0 according to the value of the corresponding data line. On output (read), the value of each bit line is passed through a sense amplifier and presented to the data lines. The row line selects which row of cells is used for reading or writing. Because only 4 bits are read/written to this DRAM, there must be multiple DRAMs connected to the memory controller to read/write a word of data to the bus.

Note that there are only 11 address lines (A0–A10), half the number you would expect for a 2048 x 2048 array. This is done to save on the number of pins. The 22 required address lines are passed through select logic external to the chip and multiplexed onto the 11 address lines. First, 11 address signals are passed to the chip to define the row address of the array, and then the other 11 address signals are presented for the column address. These signals are accompanied by row address select (RAS) and column address select (CAS) signals to provide timing to the chip. The write enable (WE) and output enable (OE) pins determine whether a write or read operation is performed. Two other pins, not shown in Figure 5.3, are ground (Vss) and a voltage source (Vcc).

As an aside, multiplexed addressing plus the use of square arrays result in a quadrupling of memory size with each new generation of memory chips. One more pin devoted to addressing doubles the number of rows and columns, and so the size of the chip memory grows by a factor of 4.

![Screenshot 2024-05-18 070721](Images\Screenshot 2024-05-18 070721.png)

------




# Internal Organization of Memory Chips (By me)

- A memory cell stores 1 bit of information, and multiple cells form a matrix to create a memory chip.

## Memory Cell Organization inside the chip

- Each row of cells makes up a memory word, with all cells in a row connected to a common line called the word line.(Physical implementation maybe different. Maybe one row could have Multiple words. Let's talk about that later. )
- An address decoder is used to drive the word line, enabling one word line based on the address in the address bus.
- Cells in each column are connected by two lines known as bit lines, connected to data input and output lines through a Sense/Write circuit. *During a Read operation, the Sense/Write circuit sense (read) the information stored in the cells selected by a word line and transmit this information to the output data line. During a write operation, the sense/write circuit receive information and store it in the cells of the selected word.*

## 16 x 8 Memory Chip Organization

![](Images\fig_c.jpg)



- A memory chip with 16 words of 8 bits each is referred to as a 16 x 8 organization.
- Data input and output lines of each Sense/Write circuit are connected to a single bidirectional data line to reduce pins.
- For 16 words, an address bus size of 4 is needed.
- Two control lines, Read/Write (R/W) and Chip Select (CS), specify the operation and select a chip in a multi-chip memory system.

# Larger Memory Units (Ex - A unit with 1024 memory cells)

1. ## 128 x 8 Memory Chips

   ​                                         ![fig_d](Images\fig_d.jpg) 

   

   - Organized as 128 words of 8 bits each.
   - Data bus size: 8 bits.
   - Address bus size: 7 bits (2^7 = 128).
     

2. ## 1024 x 1 Memory Chips

![fig_e](Images\fig_e.jpg)



- Organized as 1024 words of 1 bit each.
- Data bus size: 1 bit.
- Address bus size: 10 bits (2^10 = 1024).



------

## Memory Address Decoding

- A memory location is identified by the contents of the memory address bus, decoded by a decoder.

- Two decoding methods based on memory organization:
  - Each memory word organized in a row, using the entire address bus to decode the address.(Ex-All Above ones)
  
  - Several memory words organized in one row, dividing the address bus into row and column address groups.(Now in a single row we have multiple Words)
  
    - Ex-Lets say we have 1024 memory cells(1024 bites) and a size of a word is 1 bite. Typically we need to have 10 bits address lines for this if we use one word in a row. 
    - But let's consider an incident  where we use 32 words in a single row. That implies that we only need 32 rows to have all words.(32 x 32 = 1024).In this case also we still need 10 lines for address selecting. But 5 for row selection and 5 for column selection. A row address selects a row of 32 cells, all of which are accessed in parallel. However, according to the column address, only one of these cells is connected to the external data line via the input output multiplexers. 
    - The arrangement for row address and column address decoders is shown in the figure below
  
    ![fig_f](Images\fig_f.jpg)







































# Chip Logic Summary

Semiconductor memory chips contain arrays of memory cells with trade-offs in speed, capacity, and cost. These chips can be organized in various ways, impacting the number of bits read or written at a time.

**Key Points:**

- **Organization Extremes:**
  - Physical arrangement matches logical arrangement (e.g., 1M 16-bit words in a 16-Mbit chip).
  - 1-bit-per-chip organization reads/writes one bit at a time.
- **16-Mbit DRAM Example:**
  - Organized into four 2048x2048 element arrays.
  - Elements are connected by horizontal (row) and vertical (column) lines.
  - Address lines select the word to be read/written.
  - 11 address lines select rows; another 11 select columns.
  - 4 data lines handle input/output of 4 bits to/from a data buffer.
- **Addressing and Operations:**
  - Row and column addresses are multiplexed to save pins.
  - Row Address Select (RAS) and Column Address Select (CAS) signals provide timing.
  - Write Enable (WE) and Output Enable (OE) pins determine read/write operations.
  - Ground (Vss) and voltage source (Vcc) pins are also present.
- **Memory Growth:**
  - Multiplexed addressing and square arrays quadruple memory size with each new chip generation.

------

This summary captures the essential points about the organization and functioning of memory chips, focusing on a typical 16-Mbit DRAM example.
