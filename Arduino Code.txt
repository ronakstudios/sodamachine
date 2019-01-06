 #include <Servo.h>
 #include <math.h>
 #define PI 3.14159265
 
 Servo f,b,heightFinder;
int distanceHorizontal = 0;
int angle=45;
int distanceVerticle = 0,aboveHeight=0;
int oldDistance,newDistance;
boolean first=true;
void setup() {
    f.attach(9);
    //b.attach(A2);
    f.write(155);
    pinMode(A1,INPUT);
    pinMode(A0,INPUT);
    heightFinder.attach(11);
     heightFinder.write(angle);
     distanceHorizontal=analogRead(A0);
    pinMode(10, INPUT);
    oldDistance=analogRead(A0);
    newDistance=analogRead(A0);
    delay(17000);
}
void loop() {
  Serial.println(analogRead(A1));
  //Serial.println(analogRead(A1));
    int d = digitalRead(10);
    
if(d==1){
  // delay(10);
  //   delay(10);
  // digitalWrite(13,HIGH);
  // f.write(i);
  // b.write(i);
  // i+=10;
  // delay(100);
  // if(i>=100){
  //   i=0;
  //   f.write(i);
  //   delay(250);
  // }
  
  if(abs(oldDistance-newDistance)<=120){
    oldDistance=analogRead(A0);
  angle++;
  heightFinder.write(angle);
  
  delay(50);
  newDistance=analogRead(A0);
  if(angle>=140){
    angle=45;
    heightFinder.write(angle);
    delay(20000);
    
  }
  
  
  }else{
    
    if(first){
    int hypotonuse=oldDistance;
    distanceVerticle = (int) round(tan((angle-45)*PI/180.0)*distanceHorizontal);
    distanceVerticle=distanceVerticle/10-20;
    aboveHeight=analogRead(A1);
    first=false;
    }
    if(analogRead(A1)<aboveHeight){
      aboveHeight=analogRead(A1);
    }
    if(analogRead(A1)<(aboveHeight+distanceVerticle)){
    //pour water
    f.write(95);
    delay(100);
    Serial.println("Pouring Water");
    }else{
      f.write(155);
      delay(100);
    }
    
    
    
    Serial.print("DistanceVerticle: ");
    Serial.println(distanceVerticle);
    Serial.print("OldHeight: ");
    Serial.println(aboveHeight);
    Serial.print("AboveHeight: ");
    Serial.println(analogRead(A1));
    Serial.println();
  }
}else{

f.write(155);
delay(100);
distanceHorizontal = 0;
angle=45;
aboveHeight=0;
 distanceVerticle = 0;
 first=true;
     heightFinder.write(angle);
     distanceHorizontal=analogRead(A0);
    oldDistance=analogRead(A0);
    newDistance=analogRead(A0);
  
}
}
