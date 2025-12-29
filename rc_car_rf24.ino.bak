#include <SPI.h>
#include <RF24.h>

RF24 radio(7, 8);
const byte ADDRESS[6] = "RC001";

struct Command {
  int8_t throttle; // -100 .. 100
  int8_t steer;    // -100 .. 100
};

Command cmd;
unsigned long lastPacketTime = 0;

void setup() {
  Serial.begin(9600);
  delay(2000);
  Serial.println("=== RC CAR RX START ===");

  radio.begin();
  radio.setPALevel(RF24_PA_LOW);
  radio.setDataRate(RF24_250KBPS);
  radio.setChannel(108);
  radio.openReadingPipe(0, ADDRESS);
  radio.startListening();

  Serial.println("NRF READY");
}

void loop() {
  if (radio.available()) {
    radio.read(&cmd, sizeof(cmd));
    lastPacketTime = millis();

    // 👉 PLUJEMY TYLKO GDY COŚ SIĘ DZIEJE
    if (cmd.throttle != 0 || cmd.steer != 0) {
      Serial.print("RX | throttle=");
      Serial.print(cmd.throttle);
      Serial.print(" | steer=");
      Serial.println(cmd.steer);
    }
  }

  // FAILSAFE (bez spamu)
  if (millis() - lastPacketTime > 1000) {
    // tu później będzie STOP silnika
    lastPacketTime = millis(); // żeby nie spamowało
  }
}
