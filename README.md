
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


PORTA 5pins -bi-directional i/o port
PORTB 8pins -bi-directional i/o port, can be S/w programmed for internal weak pull-up 
PORTC 8pins -bi-directional i/o port
PORTD 8pins -bi-directional i/o port, parallel slave port when interfacing to a microcontroller bus.
PORTE 3pins -bi-directional i/o port

OSC1 - external clock input (ST buffer when in RC mode)
OSC2 - clock output(in RC mode OSC2 pin output has 1/4 the freq. of OSC1)

MCLR - master clear(Reset) input - this is set to high when programming and is set to low to reset the microcontroller


![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/c2a48127-bbd9-42b4-bd0d-1ec30d7a968d)

16f877A has three memory blocks
    
* program memory
* data memory
* flash program memory 

the above memories have seperate buses(to have concurrent access)

#### Program memory organization

has a 13-bit program counter that can address 8k word * 14 bit program memory space. This means that the PC can address up to 8,192 (2^13) memory locations, each containing 14 bits of data.

have 8K words * 14bits of flash program memory. This means that the program memory can store up to 8,192 or 4,096 instructions, each instruction being 14 bits wide.

what is address wraparound?

Accessing a location above the physically implemented address range of the program memory will cause a wraparound. For example, if you try to access address 8,200 in a PIC16F877A (which has 8K words), the address will wraparound to 8,200 - 8,192 = 8, which is within the valid address range

Reset vector is 0000h
interrupt vector is 0004h
page 0 - 0005h-07FFh & page 1 - 0800h-0FFFh ; On-Chip program memory


 this information provides an understanding of how the program memory is organized and how the microcontroller accesses instructions during program execution.


program memory map and stack

![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/939cb061-647f-4f25-9978-5e3420429447)



Data memory organization

has several partitions 

* general purpose registers
* special function registers


![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/31cbccc9-9f4a-43c0-bd48-b83e8371114e)


* each bank extenses upto 7Fh(128bytes), lowest location of each bank is reserved for the special function registers(SFR).

* registers are hardware registers used to control various aspects of the microcontroller, such as I/O pins, timers, and interrupts.Above the SFRs in each bank are the General Purpose Registers (GPRs), which are implemented as static RAM. These GPRs are used for general data storage and manipulation by the program running on the microcontroller.

All implemented banks contain SFRs, but some frequently used SFRs from one bank may be mirrored in another bank. This means that the same SFR can be accessed using different addresses in different banks. This mirroring is done for code reduction and quicker access to commonly used SFRs, as it allows the programmer to access the SFR using the closest bank, reducing the number of bank switches needed.

#### General Purpose Registers 

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

#### Bank 0

    'INDF' - 0x00 - indirect data memory registers
    'TMR0 - 0x01 - timer0 reg
    'PCL' - 0x02 - program low counter
    'STATUS' 0x03 - status register 
    'FSR' - 0x04 - file select register
    'PORTA' - 0x05 - portA data register
    'PortB' - 0x06 - portB data register
    'EEDATA' - 0x08' - EEPROM data register
    'EEADR' - 0x09 - EEPROM address data register
    'PCLATH' - 0x0A - program counter low-order bits
    'INTCON' - 0x0B - interrupt control register

#### Bank 1

    'OPTION_REG' - 0x81 - option register
    'PCL' - 0x82 -  program counter high
    'TRISA' - 0x85 - PortA Data direction reg
    'TRISB' - 0x86 - portB data direction reg
    'EECON1' - 0x88 - EEPROM control reg1
    'EECON2' - 0x89 - EEPROM control reg2


#### Bank 2

    'PIR1' - 0x0C - pheripharal interrupt reg 1
    'PIR2' - 0x0D - pheripharal interrupt reg 2
    'TMR1L' - 0x0E - timer low byte reg
    'TMR1H' - 0x0f - timer high byte reg
    'T1CON' - 0x10 - timer1 control reg
    'EECON1 - 0x18 - EEPROM control reg1
    'EECON2 - 0x19 - EEPROM control reg2

#### Bank 3

this is a virtual register that allows access to additional memory locations. to access bank 3 we need to set the 'BSR'(Bank Select Reg.) to 0b11.

    'INDF3' - 0x100 - indirect data memory register for bank 3
    'TMR1L' - 0x101 - timer1 low byte register for bank 3
    'TMR1H' - 0x102 - timer1 high byte reg for bank3
    'T1CON' - 0x103 - timer1 control reg for bank3
    'TMR2' - 0x105 - timer2 reg for bank3
    'T2CON' - 0x106 - timer2 control reg for bank3
    'SSPCON' - 0x107 - SSP buffer reg for bank3
    'CCPR1L' - 0x108 - CCP1 low byte reg for bank3
    'CCPR1H' - 0x10A - CCP1 high byte reg for bank3
    'CCP1CON' - 0x10B - CCP1 control reg for bank3
    'RCSTA' - 0x18D - USART receive control reg  for bank3
    'TXREG' - 0x19A - USART transmit reg  for bank3
    'TXSTA' - 0x19B - USART transmit control reg for bank3
    'SPBRG' - 0x19E - USART baud rate generator reg for bank3



#### Special Purpose Registers 


these are used by the cpu. Classified into two sets core and pheripheral.these are implemented as static RAM. 

#### SFR summary

Status Reg(address 03h, 83h, 103h, 183h)

**recommended that only BCF, BSF, SWAPF, & MOVWF to be used with the status register since they donot affect the Z, C, DC bits*

*~~TO~~ & ~~PD~~ bits are not writeable also if status reg is a destination(dst) for a specific instruction then Z, DC & C bits write, to these are disabled. these bits are set or cleared according to the device logic.*

*C & DC bits operates as borrow and digit borrow bit in substraction(SUBLW, SUBWF)*


    |R/W-0| R/W-0 | R/W-0 | R-1 | R-1 |R/W-x | R/W-x | R/W-x |

    | IRP | RP1   | RP0   | ~TO~|~PD~ |  Z   |   DC  |   C   |

    bit7                                               bit0


bit7  IRP: Register Bank Select, for *indirect addressing*

    1 = Bank2, 3(100h - 1FFh)
    0 = Bank0, 1(00h - FFh)

bit6-5 RP1:RP0: Register bank slect, for *direct addressing*(ech bank is 128 bytes)

    11 = Bank3(180h - 1FFh)
    10 = Bank2(100h - 17Fh)
    01 = Bank1(80h - FFh)
    00 = Bank0(00h - 7Fh)

bit4  ~~TO~~: Timeout bit

    1 = after power up/ by the CLRWDT instruction / SLEEP instruction
    0 = WDT time-out occured

bit3 ~~PD~~: power-down bit

    1 = after power down / by the CLRWDT instruction
    0 = by the execution of SLEEP instruction

bit2 Z: zero bit

    1 = result of arithmetic / logic operation is zero
    0 = result of an arithmetic / logic operation is not zero

bit1 DC: digital carry/borrow bit (ADDWF, ADDLW, SUBLW, SUBWF) *note for borrow the polarity is reversed*

    1 = A carry-out from the 4th low order bit of result occured
    0 = no carry out from the 4th low order bit of the result

bit0 C: carry/ borrow bit(ADDWF, ADDLW, SUBLW, SUBWF, SUBWF)

    1 = a carry-out from the MSB of the result occured
    0 = no carry-out from the MSB of the result bit occured


OPTION_REG reg(address 81h, 181h)

R/W reg, contains various control bits to configure the TMR0 prescale/WDT postscaler, ext INT interrput, TMR0 and the weak pullup on PORT-B

    | R/W-1 | R/W-1 | R/W-1 | R/W-1 | R/W-1 | R/W-1 | R/W-1 | R/W-1|
    | ~RBPU~| INTEDG| T0CS  | T0SE  | PSA   | PS2   | PS1   | PS0  | 
    bit7                                                            bit0

bit7 ~RBPU~: PORTB pullup enable bit

    1 = PORTB pull up is disabled
    0 = PORTB pull up is enabled by individual port latches

bit6 ~INTEDG~ interrupt edge select bit

    1 = interrupt on rising edge of RB0/INT pin
    0 = interrupt on falling edge of RB0/INT pin

bit5 ~T0CS~ TMR0 clock source select bit

    1 = transition on RA4/T0CKI pin
    0 = internal instruction cycle clock(CLKO) 

bit4 T0SE: TMR0 source edge select bit

    1 = after power up, CLRWDT / SLEEP
    0 = WDT time-out occurs

bit3 PSA: prescalar assignement bit

    1 = prescalar assigned assigned to WDT
    0 = prescalar assigned to the Timer0 module

bit2-0: prescalar rate select bits

    bit value  | TMR0  | WDT Rate
    000        | 1:2   | 1:1
    001        | 1:4   | 1:2
    010        | 1:8   | 1:4
    011        | 1:16   | 1:8
    100        | 1:32   | 1:16
    101        | 1:64   | 1:32
    110        | 1:128   | 1:64
    111        | 1:256   | 1:128

Special Notes for Easy Referance.

What is is a micro controller?

a small computer on a single IC containing processor core/memory/programmable I/O and other pheripharals.

capabilities of a microcontroller?

programmable
configurable  H/W functionality
can perform single or multiple tasks
inbuilt pheripaharals

building blocks & functions

CPU -fetch and execute instructions
program memeory - store progrma that are to be executed
data/user memeory - store data(volatile-temporary memory, non-volatile memory-retain info even after power of and restart)
I/O ports
timers
pheripharals

PIC mircrocontroller i/O pin can source(give from pin) or sink(from out to pin) upto maximum of 25mA.

PORTA 5pins, bi-directional port
PORTB 8pins, bi-directional port
PORTC 8pins, bi-directional port
PORTD 8pins
PORTE 3pins

15 interrupts
8 A/D input channels
has a parallel slave port
35 instructions
VDD range 2V-5V(supply voltage)
VSS ground 0V
 


note for normal operation MCLR (Master Clear (input)/ programming input voltage(output))(a reset pin) is set to high for normal operation if MCLR is set to low the pic will not execute logic and pic will start to execute when it is set to high.

OSC1/CLK1 - oscillator crystal/ external clock input
OSC2/CLK2 - oscillator crystal/ clock output












