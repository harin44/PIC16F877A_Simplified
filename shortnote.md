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

EXAMPLE:

    BSF 03h, 5 ;goto bank1
    MOVLW 06h ; put 00110 into W
    MOVWF 85h ; move 00110 into TRISA
    BCF 03h, 5 ; come back to bank0

EXAMPLE:

    BSF 03h, 5 ; go to bank1
    MOVLW 00h ; put 0000 into W
    MOVWF 85h ; move 0000h into TRISA - all pins set to output
    BCF 03h,5 ; come back to bank0
    

EXAMPLE:

           bsf 03h, 5
           movlw 00h
           movwf 85h
           bcf 03h, 5
    Start  movlw 02h
           movwf 05h
           movlw 00h
           movwf 05h
           goto start


writing to ports

EXAMPLE:

    STATUS equ 03h
    TRISA equ 85h
    PORTA equ 05h

        bsf  STATUS,5
        movlw 00h
        movwf TRISA
        bcf STATUS,5

    START  movlw 02h
           movwf PORTA
           movlw 00h
           movwf PORTA
           goto Start

    
delay loops

EXAMPLE:

    ;*******setup the constants******

    STATUS equ 03h
    TRISA equ 85h
    PORTA equ 05h
    COUNT1 equ 08h
    COUNT2 equ 09h

   ;*******setup the port****

       BSF STATUS,5 
       MOVLW 00h
       MOVWF TRISA
       BCF STATUS,5

    ;******turn on the LED*******

       START MOVLW 02h
             MOVWF PORTA

    ;*****start of delay loop1*****

      LOOP1    DECFSZ  COUNT1,1 ; substract 1 from 255
               GOTO LOOP1
               DECFSZ COUNT2,1 ; substract 1 from 255
               GOTO LOOP1

    ;******delay finshed now turn off the LED*****

            MOVLW 00h
            MOVWF PORTA
            

    ;******add another delay*********

    LOOP2    DECFSZ COUNT1,1
             GOTO LOOP2
             DECFSZ COUNT2,1
             GOTO LOOP2

    ;******now go back to start of the program******
            GOTO  Start

    ;*****end of program******

    end
    
