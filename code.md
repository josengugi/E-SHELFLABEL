
// LCD 16x2 Multiple screens auto scroll example

//#include <LiquidCrystal.h>

//LiquidCrystal lcd(0x27,20,4); //Pines arduino a lcd
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,20,4);

//Counter to change positions of pages

int page_counter=1 ;       //To move beetwen pages

//Variables for auto scroll
unsigned long previousMillis = 0;
unsigned long interval = 5000; //Desired wait time 10 seconds


//-------Pins-----//
int up = 8;               //Up button
int down = 10;           //Down button              
//---------Storage debounce function-----//
boolean current_up = LOW;          
boolean last_up=LOW;            
boolean last_down = LOW;
boolean current_down = LOW;
        

void setup() {
 // lcd.begin(16 ,4); 
  lcd.init();                      // initialize the lcd 
  lcd.init();
  // Print a message to the LCD.
  lcd.backlight();

}


   //---- De-bouncing function for all buttons----//
boolean debounce(boolean last, int pin)
{
boolean current = digitalRead(pin);
if (last != current)
{
delay(4);
current = digitalRead(pin);
}
return current;
}


void loop() {
  


current_up = debounce(last_up, up);         //Debounce for Up button
current_down = debounce(last_down, down);   //Debounce for Down button

//----Page counter function to move pages----//

//Page Up
    if (last_up== LOW && current_up == HIGH){ 
      lcd.clear();                     //When page is changed, lcd clear to print new page   
      if(page_counter <3){              //Page counter never higher than 3(total of pages)
      page_counter= page_counter +1;   //Page up
      
      }
      else{
      page_counter= 1;                //return to page 1
      }
  }
  
    last_up = current_up;

//Page Down
    if (last_down== LOW && current_down == HIGH){
      lcd.clear();                     //When page is changed, lcd clear to print new page    
      if(page_counter >1){              //Page counter never lower than 1 (total of pages)
      page_counter= page_counter -1;   //Page down
      
      }
      else{
      page_counter= 3;              //return to page 3
      }
  }
    
    last_down = current_down;

//------- Switch function to write and show what you want---// 
  switch (page_counter) {
   
    case 1:{     //Design of home page 1
      lcd.setCursor(0,0);
      lcd.print("EXCEL ORANGE 1L");
      lcd.setCursor(10,2);
      lcd.print(" RRP 250/-");
      lcd.setCursor(0,3);
      lcd.print("2438837499");
      
    }
    break;

    case 2: { //Design of page 2 
     lcd.setCursor(3,1);
     lcd.print("OFFER ENDS ON");
     lcd.setCursor(5,2);
    lcd.print("12/12/21");
    
       
    }
   // break;

    //case 3: {   //Design of page 3 
    // lcd.setCursor(1,1);
    // lcd.print("OFFER ENDS ON");
    // lcd.setCursor(4,2);
    // lcd.print("29/5/21");
   // }
    break;
    
  }//switch end
  
//-----------Auto scroll function---------------//

     unsigned long currentMillis = millis();            //call current millis
    // lcd.setCursor(0,1);                               // Show millis counter status
   //  lcd.print((currentMillis-previousMillis)/500);
     
     if (currentMillis - previousMillis > interval) {  //If interval is reached, scroll page
     previousMillis = currentMillis;                   //replace previous millis with current millis as new start point
     lcd.clear();                                      //lcd clear if page is changed.
     if (page_counter <2){                             //Page counter never higher than 3 (total of pages)
     page_counter = page_counter +1;                   //Go to next page
     }
     else{
      page_counter=1;                                  //if counter higher than 3 (last page) return to page 1
     }
     } 
      
if (digitalRead(8) == HIGH || digitalRead(10)==HIGH){ // Reset millis counter If any button is pressed
  previousMillis = currentMillis;
}

     

}//loop end
