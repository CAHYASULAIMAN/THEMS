#include <OneWire.h>
#include <DallasTemperature.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <DHT.h>

#define MQ2_PIN A0
#define ONE_WIRE_BUS 12
#define DHT_PIN 2
#define DHT_TYPE DHT11
DHT dht(DHT_PIN, DHT_TYPE);
Adafruit_SSD1306 display(128, 64, &Wire, 14);

#define BUTTON_PIN 16
#define BUZZER_PIN 13

// Setup a oneWire instance to communicate with any OneWire device
OneWire oneWire(ONE_WIRE_BUS);

// Pass oneWire reference to DallasTemperature library
DallasTemperature sensors(&oneWire);

enum DisplayState {
  MQ2,
  HUMIDITY,
  TEMPERATURE
};

DisplayState currentDisplay = MQ2;
bool buttonState = false;
bool lastButtonState = false;


void setup() {
  Serial.begin(9600);
  sensors.begin();  // Start up the library
  pinMode (MQ2_PIN, INPUT);

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("modul oled gagal termuat");
  }
  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(10,3);
  display.setTextSize(2);
  display.print("  S S ");
  display.display();
  pinMode(BUTTON_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  dht.begin();
}

void loop() {
  // Membaca tombol
  buttonState = digitalRead(BUTTON_PIN);
  int mq2Value = analogRead(MQ2_PIN);
  if (mq2Value > 550) {
  digitalWrite(BUZZER_PIN, HIGH);
  } else {
  digitalWrite(BUZZER_PIN, LOW);
  }
  
  // Deteksi perubahan tombol
  if (buttonState && !lastButtonState) {
    changeDisplayState();
  }
  lastButtonState = buttonState;
  
  // Menampilkan data berdasarkan kondisi
  display.clearDisplay();
  
  switch (currentDisplay) {
    case MQ2:
      displayMQ2();
      break;
    case HUMIDITY:
      displayHumidity();
      break;
    case TEMPERATURE:
      displayTemperature();
      break;
  }
  
  display.display();
  delay(100); // debounce
}

void changeDisplayState() {
  switch (currentDisplay) {
    case MQ2:
      currentDisplay = HUMIDITY;
      break;
    case HUMIDITY:
      currentDisplay = TEMPERATURE;
      break;
    case TEMPERATURE:
      currentDisplay = MQ2;
      break;
  }
}

void displayMQ2() {
  int mq2Value = analogRead(MQ2_PIN);
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("MQ-2 Gas Sensor");
  display.setCursor(0, 10);
  display.print("Value: ");
  display.print(mq2Value);

  // if (mq2Value > 550) {
  //   digitalWrite(BUZZER_PIN, HIGH);
  // } else {
  //   digitalWrite(BUZZER_PIN, LOW);
  // }
}

void displayHumidity() {
  float humidity = dht.readHumidity();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("Humidity Sensor");
  display.setCursor(0, 10);
  display.print("Humidity: ");
  display.print(humidity);
  display.print(" %");
}

void displayTemperature() {
  sensors.requestTemperatures(); 
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("Temperature Sensor");
  display.setCursor(0, 10);
  display.print("Temp: ");
  display.print(sensors.getTempCByIndex(0));
  display.print(" C");
}
