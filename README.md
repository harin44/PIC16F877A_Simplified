
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


each bank extenses upto 7Fh(128bytes), lowest location of each bank is reserved for the special function registers(SFR). registers are hardware registers used to control various aspects of the microcontroller, such as I/O pins, timers, and interrupts.Above the SFRs in each bank are the General Purpose Registers (GPRs), which are implemented as static RAM. These GPRs are used for general data storage and manipulation by the program running on the microcontroller.

All implemented banks contain SFRs, but some frequently used SFRs from one bank may be mirrored in another bank. This means that the same SFR can be accessed using different addresses in different banks. This mirroring is done for code reduction and quicker access to commonly used SFRs, as it allows the programmer to access the SFR using the closest bank, reducing the number of bank switches needed.

In the PIC microcontroller architecture, the General Purpose Register (GPR) file is a set of registers used for general data storage and manipulation by the program running on the microcontroller. The GPR file can be accessed in two ways:

*  *Direct Access:* Registers in the GPR file can be accessed directly by using their specific register names or addresses. For example, to access register '0x20' directly, you would use the instruction MOVWF 0x20 to move the contents of the W register to register '0x20'.

* *Indirect Access:* Registers in the GPR file can also be accessed indirectly through the File Select Register (FSR). The FSR is a special register used to point to a specific register in the GPR file. By loading an address into the FSR, you can then read from or write to the register pointed to by the FSR. This allows for more flexible and dynamic access to the GPR file, as the FSR can be changed during program execution to point to different registers.

Indirect access is often used in PIC assembly programming when working with arrays or when the specific register to be accessed is determined at runtime. Here's a simple example of indirect access:

assembly

    MOVLW 0x20      ; Load address of the register to be accessed
    MOVWF FSR       ; Move the address to the FSR
    MOVF INDF, W    ; Read the contents of the register pointed to by FSR into W

In this example, INDF is a special register that indirectly points to the register specified by the contents of the FSR. The MOVF INDF, W instruction reads the contents of the register pointed to by the FSR into the W register.


![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/20f2d8b5-0382-4bb6-aa79-5ac23a6994d9)

