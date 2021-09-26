# 超音波感測器 + LED (紅色LED:亮<70cm, 緑色LED: 亮>270cm, 藍色LED:亮, 介於70cm ~ 270cm之間) + RS232 Output

## 電路圖
![chrome-capture (3)](https://user-images.githubusercontent.com/89329117/134792900-afd24909-c75d-4245-b952-c68abe95ecc0.gif)

## CODE
````c
const int Red = 13;
const int Green = 10;
const int Blue = 11;

int inches = 0;

int cm = 0;

long readUltrasonicDistance(int triggerPin, int echoPin)
{
  pinMode(triggerPin, OUTPUT);  // Clear the trigger
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  // Sets the trigger pin to HIGH state for 10 microseconds
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  // Reads the echo pin, and returns the sound wave travel time in microseconds
  return pulseIn(echoPin, HIGH);
}

void changColor(int type){
  analogWrite(Red,0);
  analogWrite(Green,0);
  analogWrite(Blue,0);
  switch (type){
    case 1://red
    analogWrite(Red,255);
    analogWrite(Green,0);
    analogWrite(Blue,0);
    break;
    case 2://green
    analogWrite(Red,0);
    analogWrite(Green,255);
    analogWrite(Blue,0);
    break;
    case 3://blue
    analogWrite(Red,0);
    analogWrite(Green,0);
    analogWrite(Blue,255);
    break;
  }
}

void setup()
{
  Serial.begin(9600);

}

void loop()
{
  // measure the ping time in cm
  cm = 0.01723 * readUltrasonicDistance(7, 7);

  if(cm <70)
  {
    changColor(1);
  }
  else if(cm>= 270)
  {
    changColor(2);
  }
  else if(cm>= 70&& cm<270)
  {
    changColor(3);
  }
  
  // convert to inches by dividing by 2.54
  inches = (cm / 2.54);
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.println("cm");
  delay(100); // Wait for 100 millisecond(s)
}
````