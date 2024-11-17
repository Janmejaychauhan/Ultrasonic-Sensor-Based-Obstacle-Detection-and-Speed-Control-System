#include <LiquidCrystal.h>
LiquidCrystal lcd(12, 11, 6, 5, 4, 3);

const int trigPin = 10;
const int echoPin = 9;
const int buzzPin = 2;
int speed = 15; 

void setup() {
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.print("ObstacleDetector");
  pinMode(trigPin, OUTPUT); // trig pin will have pulses output
  pinMode(echoPin, INPUT);  // echo pin should be input to get pulse width
  pinMode(buzzPin, OUTPUT); // buzz pin is output to control buzzing
}

void loop() {
  int duration, distance;

  // Output pulse with 1ms width on trigPin
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure the pulse input in echo pin
  duration = pulseIn(echoPin, HIGH);

  // Distance is half the duration divided by 29.1 (from datasheet)
  distance = (duration / 2) / 29.1;

  lcd.clear();
  lcd.print("Obstacle Dist:");
  lcd.setCursor(0, 1);
  lcd.print(distance);
  lcd.print(" m");
  Serial.print("Obstacle Distance: ");
  Serial.print(distance);
  Serial.println("m");

  if ((distance <= 20 && distance > 0) || speed<=0) {
    // Immediate brake application
    lcd.clear();
    lcd.print("Brakes Applied!");
    Serial.println("Brakes Applied!");
    speed = 0; // Stop the vehicle

    // Continuous buzz for warning
    while (true) {
      digitalWrite(buzzPin, HIGH);
      delay(100);
      digitalWrite(buzzPin, LOW);
      delay(100);
    }
  } else if (distance > 10 && distance <= 100) {
    // Warning zone: Reduce speed gradually and buzz
    digitalWrite(buzzPin, HIGH);
    speed = max(speed - 1, 0); // Decrease speed but keep it >= 0
    Serial.print("Speed Reduced: ");
    Serial.print(speed);
    Serial.println(" km/h");

    lcd.clear();
    lcd.print("Speed: ");
    lcd.print(speed);
    lcd.setCursor(0, 1);
    lcd.print("Reduce Speed!");
  } else {
    // Safe zone: Increase speed gradually
    digitalWrite(buzzPin, LOW);
    speed = min(speed + 1, 90);
    if(speed>=90){
      Serial.println("speed is too high");
    }else{
      Serial.print("Speed: ");
      Serial.print(speed);
      Serial.println(" km/h");
    }
    

    lcd.clear();
    lcd.print("Speed: ");
    lcd.print(speed);
    lcd.setCursor(0, 1);
    lcd.print("Safe to Drive");
  }

  delay(100); // Delay for sensor stability
}
