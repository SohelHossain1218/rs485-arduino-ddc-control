# rs485-arduino-ddc-control
Arduino Nano RS485 slave controlled by Siemens DDC (LED &amp; Buzzer control)
#include <SoftwareSerial.h>

#define DERE 2                 // MAX485 DE & RE
SoftwareSerial rs485(4, 3);    // RO, DI

#define LED1 LED_BUILTIN       // Pin 13
#define LED2 7
#define LED3 9
#define LED4 10
#define BUZZER 11

#define SLAVE_ID 69

bool led1State = false;
bool led2State = false;
bool led3State = false;
bool led4State = false;
bool buzzerState = false;

void setup() {
  Serial.begin(9600);
  rs485.begin(9600);

  pinMode(DERE, OUTPUT);
  digitalWrite(DERE, LOW); // RX mode

  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);
  pinMode(LED4, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  Serial.println("RS485 Slave Ready - ID 69");
}

void loop() {
  if (rs485.available()) {
    String data = rs485.readStringUntil('\n');
    data.trim();
    char c = rs485.read();
    Serial.print(c);


    Serial.println("Received: " + data);

    int commaIndex = data.indexOf(',');
    if (commaIndex == -1) return;

    int rxID = data.substring(0, commaIndex).toInt();
    int cmd  = data.substring(commaIndex + 1).toInt();

    if (rxID != SLAVE_ID) return;

    switch (cmd) {
      case 101:
        led1State = !led1State;
        digitalWrite(LED1, led1State);
        break;

      case 102:
        led2State = !led2State;
        digitalWrite(LED2, led2State);
        break;

      case 103:
        led3State = !led3State;
        digitalWrite(LED3, led3State);
        break;

      case 104:
        led4State = !led4State;
        digitalWrite(LED4, led4State);
        break;

      case 105:
        buzzerState = !buzzerState;
        digitalWrite(BUZZER, buzzerState);
        break;
    }
  }
}
