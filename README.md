# ECE-218-MO
#include "mbed.h"
#include "arm_book_lib.h"
#define DUTY_MIN 0.025
#define DUTY_MAX 0.125
#define PERIOD 20
#include "display.h"
#include "user_interface.h"

DigitalIn driverSeatButton(D12);       
DigitalIn ignitionButton(USER_BUTTON);  
DigitalOut led2(LED2);                   
AnalogIn potentiometer(A0);              
                  

const float POT_DIVISION_1 = 0.33;
const float POT_DIVISION_2 = 0.66;
const float POT_DIVISION_3 = 0.84;
void displayInit();
int main() {

    PwmOut servo(PF_9);
    led2 = 0;     
    
    

    while (1) {
        displayInit( DISPLAY_CONNECTION_GPIO_4BITS );
        if (driverSeatButton && ignitionButton) {
            delay(2000);
            led2 = 1;
            
        }
        float potValue = potentiometer.read();
        if (potValue < POT_DIVISION_1) {
            
            displayCharPositionWrite ( 0,0 );
            displayStringWrite( "MODE: HIGH" );
            servo.period_ms(PERIOD);
            servo.write(DUTY_MIN);
            delay(700);
            servo.write(DUTY_MAX);
            delay(700);
        } else if (potValue < POT_DIVISION_2) {
            
            displayCharPositionWrite ( 0,0 );
            displayStringWrite( "MODE: INT" );
            servo.period_ms(PERIOD);
            servo.write(DUTY_MIN);
            delay(2500);
            servo.write(DUTY_MAX);
            delay(2500);
        
        } else if (potValue < POT_DIVISION_3){
            
            displayCharPositionWrite ( 0,0 );
            displayStringWrite( "MODE: LOW" );
            servo.period_ms(PERIOD);
            servo.write(DUTY_MIN);
            delay(1500);
            servo.write(DUTY_MAX);
            delay(1500);
        } else {
        
            displayCharPositionWrite ( 0,0 );
            displayStringWrite( "MODE: OFF" );
        }
    
    
    }
}
