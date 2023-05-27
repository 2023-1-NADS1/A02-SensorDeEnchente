# A02-SensorDeEnchente


# Introdução
Dispositivo de medição de níivel de água para auxiliar na detecção do aumento do nível de água em rios, córregos e escoamentos em regiões afetadas por enchentes. Alertando a população local e interessados, sobre os riscos de possíveis enchentes a serem detectadas naquela região.
### 🚧 Projeto

![Sensor de nivel de agua](https://github.com/2023-1-NADS1/A02-SensorDeEnchente/assets/128257439/a42fe246-3f70-4496-8adc-6bebeb12cab6)


### 🔧 Instalação
- Para trabalhar com este projeto é necessario que tenha instalado na sua máquina a aplicação "MIT" para receber os dados vindos do ESP8266
 
## 📦 Tecnologias
  - ESP8266
  - Placa De Ensaio Protoboard
  - Jumpers de ligação – Cabos tipo macho
  - Cabo USB
  - Display LCD
  - Sensor de Obstáculo Infravermelho IR
  - Sensor de Distância Ultrassônico 

## ✒️ Autores
- AMANDA NUNES
- CHRISTYAN MAXIMO
- PAULO CARVALHO
- RICARDO CHINGOTTI
- LEON BOAVENTURA

## Código 

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <IRremote.h>
#include <ESP8266WiFi.h>
#include <NewPing.h>

#define TRIGGER_PIN 5
#define ECHO_PIN 4
#define IR_PIN 0

#define MAX_DISTANCE 200

NewPing sonar(TRIGGER_PIN, ECHO_PIN, MAX_DISTANCE);

IRrecv irrecv(IR_PIN);
decode_results results;

const char* ssid = "Nome da sua rede Wi-Fi";
const char* password = "Senha da sua rede Wi-Fi";

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  Serial.begin(9600);
  irrecv.enableIRIn();

  lcd.begin(16, 2);
  lcd.backlight();

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Conectando à rede Wi-Fi...");
  }
  Serial.println("Conectado à rede Wi-Fi.");
}

void loop() {
  long duration = sonar.ping_cm();
  
  if (irrecv.decode(&results)) {
    unsigned int value = results.value;
    Serial.print("Código IR recebido: ");
    Serial.println(value, HEX);
    irrecv.resume();
  }

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.setCursor(0, 1);
  lcd.print(duration);
  lcd.print(" cm");

  if (WiFi.status() == WL_CONNECTED) {
    // Faça algo quando estiver conectado à rede Wi-Fi
  }

  delay(500);
}

