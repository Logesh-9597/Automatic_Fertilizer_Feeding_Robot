//including the libraries 
#include<SoftwareSerial.h>//TXRXsoftwarelibraryforbluetooth 
#include<LEDFader.h> 
//DefiningpinsforRGBled#
 defineGREEN13 
#defineBLUE5 
#defineRED4 
#definedelayTime3 
#defineLED_NUM3LEDFaderl
 eds[LED_NUM]={LEDFader() 
LEDFader(5), 
LEDFader(13) 
}; 
//InitializingpinsforbluetoothModule 
int bluetoothTx = 2; // bluetooth tx to 2 
pinintbluetoothRx=3;//bluetoothrxto3pin 
SoftwareSerialbluetooth(bluetoothTx, bluetoothRx); 
//FrontMotorPinsi
 ntEnable1=6; 
intMotor1_Pin1=7;i
 ntMotor1_Pin2=8; 
//BackMotorPins 
intMotor2_Pin1=9; 
intMotor2_Pin2=10; 
intEnable2=11; 
//FrontLightpins 
intfront_light1=A0; 
intfront_light2=A1; 
//Backlightpins 
int back_light1 = A2; 
int back_light2 = A3; 
int horn=12; 
char command;//variable to store the data 
int velocity=0;//Variable to control the speed of motor 
void setup() 
{ 
//Set the baud rate of serial communication and blue tooth module at same rate. 
12 
Serial.begin(9600); 
bluetooth.begin(9600); 
//SettingtheL298N,LEDandRGBLEDpinsasoutputpins. 
pinMode(Motor1_Pin1,OUTPUT); 
pinMode(Motor1_Pin2,OUTPUT);
 pinMode(Enable1,OUTPUT); 
pinMode(Motor2_Pin1,OUTPUT);
 pinMode(Motor2_Pin2,OUTPUT);
 pinMode(Enable2,OUTPUT); 
pinMode(front_light1, OUTPUT); 
pinMode(back_light1, OUTPUT); 
pinMode(front_light2, OUTPUT); 
pinMode(back_light2, OUTPUT); 
pinMode(horn, OUTPUT); 
pinMode(GREEN,OUTPUT); 
pinMode(BLUE, OUTPUT); 
pinMode(RED,OUTPUT); 
//SettingtheenableandRGBLEDpinsasHIGH. 
digitalWrite(Enable1, HIGH); 
digitalWrite(Enable2,HIGH); 
digitalWrite(GREEN, HIGH); 
digitalWrite(BLUE, HIGH); 
digitalWrite(RED,HIGH); 
} 
voidloop(){ 
if(bluetooth.available()>0){//Checking ifthere 
issomedataavailableornotcommand = bluetooth.read(); 
//Storing the data in the 'command' variable 
Serial.println(command);  
//Printingitonthe serialmonitor 
//Changepinmodeonlyifnewcommandisdifferentfromprevious.switc
 h(command){ 
case 'F': //Moving the Car Forward 
digitalWrite(Motor2_Pin2, LOW); 
digitalWrite(Motor2_Pin1, HIGH); 
digitalWrite(Motor1_Pin1, LOW); 
digitalWrite(Motor1_Pin2, LOW); 
break; 
case 'B'://MovingtheCarBackward 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor2_Pin2, HIGH); 
digitalWrite(Motor1_Pin1, LOW); 
digitalWrite(Motor1_Pin2, LOW); 
break; 
13 
case 'L': //Moving the Car Left 
digitalWrite(Motor1_Pin1, LOW); 
digitalWrite(Motor1_Pin2,HIGH); 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor2_Pin2, LOW); 
break; 
case 'R': //Moving the Car Right 
digitalWrite(Motor1_Pin2, LOW); 
digitalWrite(Motor1_Pin1,HIGH); 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor2_Pin2, LOW); 
break; 
case'S'://Stop 
digitalWrite(Motor2_Pin2, LOW); 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor1_Pin2, LOW); 
digitalWrite(Motor1_Pin1, LOW); 
break; 
case'I'://MovingtheCar Forwardright 
digitalWrite(Motor2_Pin2, LOW); 
digitalWrite(Motor2_Pin1, HIGH); 
digitalWrite(Motor1_Pin2, LOW); 
digitalWrite(Motor1_Pin1, HIGH); 
break; 
case 'J'://MovingtheCar backwardright 
digitalWrite(Motor1_Pin2, LOW); 
digitalWrite(Motor1_Pin1, HIGH); 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor2_Pin2, HIGH); 
break; 
case'G'://MovingtheCarForwardleft 
digitalWrite(Motor2_Pin2, LOW); 
digitalWrite(Motor2_Pin1, HIGH); 
digitalWrite(Motor1_Pin1, LOW); 
digitalWrite(Motor1_Pin2, HIGH); 
break; 
case'H'://MovingtheCarbackwardleft 
digitalWrite(Motor2_Pin1, LOW); 
digitalWrite(Motor2_Pin2, HIGH); 
digitalWrite(Motor1_Pin1, LOW); 
digitalWrite(Motor1_Pin2, HIGH); 
break; 
case'W'://FrontlightON 
digitalWrite(front_light1,HIGH); 
digitalWrite(front_light2,HIGH); 
break; 
14 
case'w'://FrontlightOFF 
digitalWrite(front_light1,LOW); 
digitalWrite(front_light2,LOW); 
break; 
case 'U': //Back light ON 
digitalWrite(back_light1,HIGH); 
digitalWrite(back_light2,HIGH); 
break; 
case 'u'://Back light OFF 
digitalWrite(back_light1, LOW); 
digitalWrite(back_light2, LOW); 
break; 
case 'V': //Horn On 
tone(horn,494); 
break; 
case'v'://HornOFFn 
Tone(horn); 
break; 
case'x'://TurnONEverything 
break; 
case'X'://TurnOFFEverything 
break; 
//ControllingtheSpeedofCar 
default: 
//Get velocity 
if(command=='q'){ 
velocity = 255;  
//Full velocity 
analogWrite(Enable2,velocity); 
} 
else{ 
//Chars '0'- '9'have an integer equivalence of 48 -57, 
accordingly. 
if((command>=48) &&(command<=57)){ 
//Subtracting48changes the rangefrom48-57to0-9. 
//Multiplying by 25 changes the range from 0-9 to 0-225. 
velocity=(command-48)*25; 
analogWrite(Enable2,velocity); 
} 
} 
} 
} 
RGB(); 
} 
Void RGB() 
{ 
//Update all LEDs and start 
newfadesifanyaredonefor(bytei=0;i<LED_NUM; i++) 
15 
{ 
LEDFader*led=&leds[i];l
 ed->update(); 
//ThisLEDis notfading, 
starta newfade 
if(led->is_fading()==false) 
{ 
intduration=random(1000,3000); 
//between1-3seconds 
//FadeUp 
if(led->get_value()==0) 
{ 
byteintensity=random(100,255); 
led->fade(intensity, duration); 
} 
//FadeDown 
else 
{ 
led->fade(0,duration); 
} 
} 
} 
}
