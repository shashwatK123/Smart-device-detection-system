#include <SoftwareSerial.h>
#define detector A0
#define detector_1 D2


int detector = 0;
int detector_1 = 0;


const String PHONE = "+917709276202";



void setup()
{
  Serial.begin(115200);
  sim800L.begin(9600);
  pinMode(detector,INPUT);
  pinMode(detector_1,INPUT);
  pinMode(D4, OUTPUT);
  Serial.println("Initializing...");
  sim800L.println("AT");
  delay(1000);
  sim800L.println("AT+CMGF=1");
  int rain_value = 0;
  int rain_value_1 = 0;
  delay(1000);
}
void loop()
{
  while(sim800L.available()){
  Serial.println(sim800L.readString());
  }
  
  int detector_value = analogRead(detector);
  Serial.println(detector_value);
  int detector_value_1 = digitalRead(detector_1);
  Serial.println(detector_value_1);
  
  if(detector_value >500) {
      Serial.println("Cell Phone is Detected_1.");
      send_sms();
      digitalWrite(D4,HIGH);
      delay(1000);
      detector_value = 0;  
  }
  else {
    digitalWrite(D4,LOW);
    Serial.println("Cell Phone NOT Detected_1.");
  }

  if(detector_value_1 ==1) {
      Serial.println("Cell Phone is Detected_2.");
     send_sms_1();
     digitalWrite(D4,HIGH);
     delay(1000);
     detector_value_1 = 0;
  }
  else {
    digitalWrite(D4,LOW);
  Serial.println("Cell Phone NOT Detected_2.");
}
delay(1500);
}
void send_sms()
{
    Serial.println("sending sms....");
    delay(50);
    sim800L.print("AT+CMGF=1\r");
    delay(1000);
    sim800L.print("AT+CMGS=\""+PHONE+"\"\r");
    delay(1000);
    sim800L.print("Cell Phone Detected_1");
    delay(100);
    sim800L.write(0x1A); //ascii code for ctrl-26 //Serial2.println((char)26); //ascii code for ctrl-26
    delay(5000);
    detector_value = 0;
}
void send_sms_1()
{
    Serial.println("sending sms....");
    delay(50);
    sim800L.print("AT+CMGF=1\r");
    delay(1000);
    sim800L.print("AT+CMGS=\""+PHONE+"\"\r");
    delay(1000);
    sim800L.print("Cell Phone Detected_2");
    delay(100);
    sim800L.write(0x1A); //ascii code for ctrl-26 //Serial2.println((char)26); //ascii code for ctrl-26
    delay(5000);
    detector_value_1 = 0;
}
//______________________________________________________________________________________
