float d;
float distance;
void setup() {
 Serial.begin(9600);
 pinMode(7,INPUT);//echo pin of ultraSonic
 pinMode(8,OUTPUT);//trig pin of ultraSonic
 pinMode(10,OUTPUT);// relay
 pinMode(9,OUTPUT);// buzzer pin
 Serial.println("LABEL,Date,Time,Distance,Tank Status");
}
int low=23;
int high=5;

void vol() //distance calculation...
{
 digitalWrite(8,HIGH);
 delayMicroseconds(8);
 digitalWrite(8,LOW);
 delayMicroseconds(2);
 d=pulseIn(7,HIGH);
 distance=0.034*d/2;
 Serial.print(distance);
 Serial.print(",");
 if(distance>9)
 {
  Serial.println("Tank is empty");
 }else if(distance<9 and distance>high)
 {
  Serial.println("Tank is half filled");
 }
 else{
  Serial.println("Tank is Full");
 }
 //Serial.print(",");
}
 
void loop() {
  vol();
 while(1)
  {
    Serial.print("DATA,DATE,TIME,");
   b:
   digitalWrite(10,HIGH);// Pump On...
   delay(2000);
   vol();
   if(distance<high) //check high...
    {
     digitalWrite(9,HIGH);// buzzer on.....
     delay(1000);
     
     digitalWrite(9,LOW);
     goto a;
    }
  }
 while(1)
  {
    Serial.print("DATA,DATE,TIME,");
   a:
   digitalWrite(10,LOW);// pump off...
   delay(100);
   //Serial.print("Tank empty Pump turned on");
   vol();
   if(distance>low) //check low
    {
     digitalWrite(9,HIGH);//Buzzer beeping......
     delay(1000);
     //Serial.print("Tank is filling");
     digitalWrite(9,LOW);
     delay(1000);
     digitalWrite(9,HIGH);
     delay(1000);
     digitalWrite(9,LOW);
     delay(1000);
     
     goto b;
    }
  }

}
