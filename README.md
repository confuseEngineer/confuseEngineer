#define TRIG_PIN 14  // GPIO14 (D14)
#define ECHO_PIN 27     // GPIO27 (D27)
#define VIBRATION_PIN 4     // GPIO4 (D4)
#define BUZZER_PIN 12   // GPIO12 (D12)
#define DISTANCE_THRESHOLD   50 // centimeters

// variables will change:
long duration_us, distance_cm;

void setup() {
  Serial.begin(9600);           // initialize serial port
  pinMode(TRIG_PIN, OUTPUT);    // set Arduino Uno pin to output mode
  pinMode(ECHO_PIN, INPUT);     // set Arduino Uno pin to input mode
  pinMode(BUZZER_PIN, OUTPUT);  // set Arduino Uno pin to output mode for Buzzer
  pinMode(VIBRATION_PIN, OUTPUT); // set Arduino Uno pin to output mode for Vibration Motor
}

void loop() {
  // generate 10-microsecond pulse to TRIG pin
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // measure duration of pulse from ECHO pin
  duration_us = pulseIn(ECHO_PIN, HIGH);
  // calculate the distance
  distance_cm = 0.005 * duration_us;

  // Adjust buzzer and vibration motor based on distance
  if (distance_cm < DISTANCE_THRESHOLD) {
    if (distance_cm < 20) {
      // Short beep and continuous vibration for close distance
      tone(BUZZER_PIN, 1000, 200); // Tone at 1000 Hz for 200 milliseconds
      digitalWrite(VIBRATION_PIN, HIGH);
    } else {
      // Long beep and short vibration for medium distance
      tone(BUZZER_PIN, 500, 500); // Tone at 500 Hz for 500 milliseconds
      digitalWrite(VIBRATION_PIN, HIGH);
      delay(50); // Vibrate for 50 milliseconds
    }
  } else {
    // No feedback if distance exceeds threshold
    noTone(BUZZER_PIN);
    digitalWrite(VIBRATION_PIN, LOW);
  }

  // Print the distance to Serial Monitor (optional)
  Serial.print("Distance: ");
  Serial.print(distance_cm);
  Serial.println(" cm");

  delay(500); // Adjust the delay based on your preference
}
