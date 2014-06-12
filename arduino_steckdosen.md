# Funksteckdosen

RF Link Sender:
http://www.watterott.com/de/RF-Link-Sender-434MHz

Elro 3x CH-Steckdosen-Schalter mit Fernbedienung:
http://www.topd.ch/de/000409206

RC-Switch Library
https://code.google.com/p/rc-switch/wiki/HowTo_Send

Einstellungen für Steckdose matchen auf "10101"

Beispiel-Programm, welches die Steckdose jede Sekunde ein- und wieder ausschaltet.
#include <RCSwitch.h>

RCSwitch mySwitch = RCSwitch();

void setup() {

  // Transmitter is connected to Arduino Pin #10  
  mySwitch.enableTransmit(10);
  
}

void loop() {

  mySwitch.switchOn("11001", "01000");

  // Wait a second
  delay(1000);
  
  // Switch off
  mySwitch.switchOff("11001", "01000");
  
  // Wait another second
  delay(1000);
  
}

# Schaltung über Relays

8-Kanal Relay Modul
http://www.play-zone.ch/de/8-kanal-relais-modul-karte-5v-30-vdc-kit.html

Mit dem Strominput vom Arduino können nicht alle 8 Relays gespiesen werden.
5 können hingegen gespiesen werden.

Hier eine kurze Beschreibung:
http://arduino-info.wikispaces.com/ArduinoPower#4-8

Wichtig: Low bedeuted Relay an, HIGH bedeuted Relay aus.

Ein kurzes Programm um die Relays an zu sprechen:
void setup() {                
  for(int i = 6; i <= 13; i++) {
    digitalWrite(i, HIGH);
    pinMode(i, OUTPUT);
  }
  delay(5000);
}

void loop() {

  for(int i = 6; i <= 13; i++) {
    digitalWrite(i, HIGH);
  }
  delay(2000);
  
  for(int i = 6; i <= 13; i++) {
    digitalWrite(i, LOW);
  }
  delay(2000);
}

# Input mit den Input-Buttons
https://www.sparkfun.com/products/9336

Diese können direkt, ohne Resistor verkabelt werden:
COM -> zum Input-Kanal
NO -> 5V
NC -> GND

