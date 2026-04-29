# IOT-pathways-
smart IOT pathway system 
components used :
  LDR
  IR
  LED
  ESP8266
  BUZZER 
PROGRAM :
#define IR1 17
#define IR2 18
#define IR3 19

#define LDR_PIN 34
#define BUZZER 26

// Green LEDs
#define G1 4
#define G2 32
#define G3 15

// Red LEDs
#define R1 16
#define R2 22
#define R3 23

// EXTRA LED
#define EXTRA_LED 25

void setup() {
  pinMode(IR1, INPUT);
  pinMode(IR2, INPUT);
  pinMode(IR3, INPUT);

  pinMode(BUZZER, OUTPUT);

  pinMode(G1, OUTPUT);
  pinMode(G2, OUTPUT);
  pinMode(G3, OUTPUT);

  pinMode(R1, OUTPUT);
  pinMode(R2, OUTPUT);
  pinMode(R3, OUTPUT);

  pinMode(EXTRA_LED, OUTPUT);

  Serial.begin(115200);
}

void loop() {
  int count = 0;

  // IR detect = LOW
  if (digitalRead(IR1) == LOW) count++;
  if (digitalRead(IR2) == LOW) count++;
  if (digitalRead(IR3) == LOW) count++;

  int ldrValue = analogRead(LDR_PIN);

  Serial.print("IR Count: ");
  Serial.print(count);
  Serial.print(" | LDR: ");
  Serial.println(ldrValue);

  // Default OFF
  digitalWrite(BUZZER, LOW);

  digitalWrite(G1, LOW);
  digitalWrite(G2, LOW);
  digitalWrite(G3, LOW);

  digitalWrite(R1, LOW);
  digitalWrite(R2, LOW);
  digitalWrite(R3, LOW);

  // HUMAN detected
  if (count == 1) {
    if (ldrValue > 2000) {  // DARK
      digitalWrite(G1, HIGH);
      digitalWrite(G2, HIGH);
      digitalWrite(G3, HIGH);
    }
  }

  // VEHICLE detected
  else if (count >= 2) {
    digitalWrite(BUZZER, HIGH);

    if (ldrValue > 2000) {  // DARK
      digitalWrite(R1, HIGH);
      digitalWrite(R2, HIGH);
      digitalWrite(R3, HIGH);
    }
  }

  // =========================
  // EXTRA LED (FINAL FIX)
  // =========================

  if (ldrValue < 2000) {
    // Bright → DIM
    analogWrite(EXTRA_LED, 80);   // dim light
  } 
  else {
    // Dark → FULL BRIGHT
    analogWrite(EXTRA_LED, 255);
  }

  delay(100);
}
