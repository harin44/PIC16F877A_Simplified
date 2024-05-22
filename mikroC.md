#  mikroC basics
purpose of writing C language is to break the bigger problem into several smaller pieces.

data types in c

    char(0-255)
    int(-32768 - 32767)
    float
    double

mikroC Keywords

![image](https://github.com/harin44/PIC16F877A_Simplified/assets/94885392/2211f37a-dfc5-4eab-9449-6bee93157052)


example-1:

turn on few LEDs on PORTB(turn on RB0,RB2, RB4, RB6)

    int k;
    
    void main() {
        ANSEL = 0; // configure all I/O ports as digital
        ANSELH = 0;
        PORTB =  0x55
        TRISB = 0; //set portB as output

        Delay_ms(1000); //set delay to 1s

        PORTB = 0
    

    for(k=1; k<20; k++){
        switch(PORTB){
            case 0x00: 
                PORTB = 0xFF;
                Delay_ms(100);
                break;

            case 0xFF: 
                PORTB = 0x00;
                Delay_ms(500);
        }
    }

    PORTB = 0x55;

    while(1){
        PORTB = ~PORTB;
        Delay_ms(200);
    }
    }
