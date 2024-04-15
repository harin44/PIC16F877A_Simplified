
# PIC16F877A

A brief description of PIC16F877A

Has 40/44 pins and 5 I/O ports, 15 interrputs, 8 A/D input channels & has a parallel Slave Port 

* Operating freq. - 20MHZ(DC)

* Resets and Delays - POR, BOR(PWRT, OST)

* Flash Program Memory(14-bit words) - 8K

* Data Memory - 368 bytes

* EEPROM Data Memory - 256 bytes

* Interrputs - 15

* I/O Ports - Port(A,B,C,D,E)

* Timers 3

* Capture/Compare/PWM Modules - 2
 
* Serial Communication - MSSP, USART

* Parallel Communication - PSP

* 10-bit A-to-D Module - 8 input channels

* Analog Comparators - 2

* Pacakages - 40-pin PDIP/ 44-pin PLCC/ 44-pin TQFP/ 44-pin QFN


![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/c2a48127-bbd9-42b4-bd0d-1ec30d7a968d)

16f877A has three memory blocks
    
* program memory
* data memory

the above memories have seperate buses(to have concurrent access)

#### Program memory organization

has a 13-bit program counter that can address 8k word * 14 bit program memory space. This means that the PC can address up to 8,192 (2^13) memory locations, each containing 14 bits of data.

have 8K words * 14bits of flash program memory. This means that the program memory can store up to 8,192 or 4,096 instructions, each instruction being 14 bits wide.

what is address wraparound?

Accessing a location above the physically implemented address range of the program memory will cause a wraparound. For example, if you try to access address 8,200 in a PIC16F877A (which has 8K words), the address will wraparound to 8,200 - 8,192 = 8, which is within the valid address range

Reset vector is 0000h
interrupt vector is 0004h

 this information provides an understanding of how the program memory is organized and how the microcontroller accesses instructions during program execution.


program memory map and stack

![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/939cb061-647f-4f25-9978-5e3420429447)


Data memory organization

has several partitions 

* general purpose registers
* special function registers


![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/31cbccc9-9f4a-43c0-bd48-b83e8371114e)


