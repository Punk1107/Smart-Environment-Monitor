# Smart-Environment-Monitor
const int tmpPin     = A0;
const int ldrPin     = A1;

const int coldLED    = 2;
const int normalLED  = 3;
const int hotLED     = 4;

const int buzzerPin  = 8;

const int redRGB     = 5;
const int greenRGB   = 6;
const int blueRGB    = 7;

const float coldThreshC = 20.0;
const float hotThreshC  = 28.0;

const int nightThresh   = 600;

unsigned long lastRGBStep = 0;
int wheelPos = 0;

void setup() {
  pinMode(coldLED  , OUTPUT);
  pinMode(normalLED, OUTPUT);
  pinMode(hotLED   , OUTPUT);
  pinMode(buzzerPin, OUTPUT);

  pinMode(redRGB  , OUTPUT);
  pinMode(greenRGB, OUTPUT);
  pinMode(blueRGB , OUTPUT);

  Serial.begin(9600);
}

void loop() {
  int tmpRaw = analogRead(tmpPin);
  float voltage = tmpRaw * 5.0 / 1023.0;
  float tempC   = (voltage - 0.5) * 100.0;

  digitalWrite(coldLED  , LOW);
  digitalWrite(normalLED, LOW);
  digitalWrite(hotLED   , LOW);
  digitalWrite(buzzerPin, LOW);

  if (tempC < coldThreshC) {
    digitalWrite(coldLED, HIGH);
  } else if (tempC < hotThreshC) {
    digitalWrite(normalLED, HIGH);
  } else {
    digitalWrite(hotLED, HIGH);
    digitalWrite(buzzerPin, HIGH);  // Active buzzer = digital HIGH
  }

  int ldrRaw = analogRead(ldrPin);
  bool isNight = (ldrRaw > nightThresh);

  if (millis() - lastRGBStep > 50) {
    lastRGBStep = millis();
    wheelPos = (wheelPos + 1) % 1536;

    int r, g, b;
    colourWheel(wheelPos, r, g, b);

    float brightness = isNight ? 0.2 : 1.0;
    analogWrite(redRGB  , (int)(r * brightness));
    analogWrite(greenRGB, (int)(g * brightness));
    analogWrite(blueRGB , (int)(b * brightness));
  }

  delay(100);
}

void colourWheel(int pos, int &r, int &g, int &b) {
  if      (pos < 256)  { r = 255;       g = pos;         b = 0;           }
  else if (pos < 512)  { r = 511 - pos; g = 255;         b = 0;           }
  else if (pos < 768)  { r = 0;         g = 255;         b = pos - 512;   }
  else if (pos < 1024) { r = 0;         g = 1023 - pos;  b = 255;         }
  else if (pos < 1280) { r = pos - 1024;g = 0;           b = 255;         }
  else                 { r = 255;       g = 0;           b = 1535 - pos;  }
}
