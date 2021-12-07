# Remote-controlled-RADAR-system

#include <Servo.h>

  int trigpin=12;
  int echopin=11;
  float  traveltime;
  float distance;
  int servopin=9;
  
  Servo myservo;                                       // create servo object to control a servo
                                                           
  int pos = 0; 
  float box;
  long int t1;
  long int t2;
  int flag=0;
  int block=0;
  float timetaken;
  float degreesweep;
  int mynumber;
  int flag1=0;
  int potpin=A2;
  float potvalue;
  int newpotvalue;
  int redled=2;
           

void setup() {

  Serial.begin(9600);                             // begin serial monitor
  pinMode(trigpin,OUTPUT);
  pinMode(echopin,INPUT);
  pinMode(redled,OUTPUT);
  myservo.attach(servopin);                       // attaches the servo on pin 9 to the servo object
}

void loop() 
{

 if(flag1==0)
 {
  Serial.println("Enter 1 for REMOTE CONTROL and 2 for AUTOMATED");
  flag1=1;
  while(Serial.available()==0)
  {
  }
  mynumber=Serial.parseInt();
  Serial.print("You have selected ");
     if(mynumber==1)
     {
      Serial.println("REMOTE control mode");
      }

      if(mynumber==2)
     {
      Serial.println("AUTOMATED mode");
      }
  }


  if(mynumber==2)
  { 
  for (pos = 0; pos <= 180; pos += 5) {            // goes from 0 degrees to 180 degrees with angle changing with discrete value of 5n
    // in steps of 5 degree
    myservo.write(pos);
    digitalWrite(trigpin,LOW);
  delayMicroseconds(10);
  digitalWrite(trigpin,HIGH);
  delayMicroseconds(10);
  digitalWrite(trigpin,LOW);
 
  
    

  traveltime=pulseIn(echopin,HIGH);
  Serial.print(traveltime);
  Serial.print("   ");

  box =34300*traveltime/1000000;
  distance =box/2;
  Serial.print(distance);
  Serial.println(" cm"); 
   if(distance<=10)
   {
    digitalWrite(redled,HIGH);
    delay(200);
    digitalWrite(redled,LOW);
    }
  }
  }

  if(mynumber==1)
  {

   potvalue=analogRead(potpin);                   //read value from remote controller (potentiometer)
   
   newpotvalue=potvalue*180/1023;
   Serial.print(newpotvalue);
   Serial.print(" degree  ");
   myservo.write(newpotvalue);

   myservo.write(newpotvalue);
   digitalWrite(trigpin,LOW);
   delayMicroseconds(10);
   digitalWrite(trigpin,HIGH);
   delayMicroseconds(10);
   digitalWrite(trigpin,LOW);
 
  
    

  traveltime=pulseIn(echopin,HIGH);
  Serial.print(traveltime);
  Serial.print("   ");

  box =34300*traveltime/1000000;
  distance =box/2;
  Serial.print(distance);
  Serial.println(" cm");
  if(distance<=10)
   {
    digitalWrite(redled,HIGH);                   // WARNING light !
    delay(200);
    digitalWrite(redled,LOW);
  }

 
   delay(1000);                      
  }
}
  
