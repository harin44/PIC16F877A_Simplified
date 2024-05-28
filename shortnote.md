## Note on 28_05_24

#    PIC Shortnote

PORTS available

    PORTA(RA0, RA1, RA2, RA3, RA4, RA5),  6PINS
    PORTB(RB0, RB1, RB2, RB3, RB4, RB5, RB6, RB7),  8PINS
    PORTC(RC0, RC1, RC2, RC3, RC4, RC5, RC6, RC7),   8PINS
    PORTD(RD0, RD1, RD2, RD3, RD4, RD5, RD6, RD7), 8PINS
    PORTE(RE0, RE1, RE2) 3PINS

bank select bit of pic 16F877A 

RP0 --> bit5 
RP1 --> bit6

bank0 if 00 / bank1 if 01 / bank2 if 10 / bank3 if 11

TRISA(85h) / TRISB(86h)/ TRISC(87h) / TRISD(88h) / TRISE(89h) --> set to 0, output / set to 1, input

PORTA(05h) / PORTB(06h) / PORTC(07h) / PORTD(08h) / PORTE(09h) --> if output , set to 1 , input , set to 0





