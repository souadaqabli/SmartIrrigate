# SmartIrrigate
The SmartIrrigate project is a smart irrigation system designed to optimize water usage in agriculture. It uses sensors to monitor soil moisture levels, weather data, and crop requirements, adjusting irrigation schedules in real time to conserve water and improve crop yield. The system aims to reduce water wastage, lower costs, and promote sustainable farming practices through automation and data-driven insights.
# Prototype
<img width="461" alt="image" src="https://github.com/user-attachments/assets/ca184da1-b4c2-4216-b763-be716c37278a" />

# Arduino Code

<img width="329" alt="image" src="https://github.com/user-attachments/assets/f8562fd2-e335-4626-a037-5404332411ad" />
#include <SoftwareSerial.h>
#define PUMP_PIN 4 // Définir la broche utilisée pour contrôler la pompe
SoftwareSerial bluetooth(2, 3); // TX, RX
int sensor_pin = A0;
float hum;
unsigned long previousMillis = 0;
const long interval = 15*1000; // Intervalle de temps entre 2 mésures (15 secondes)
const unsigned long pumpDuration = 6000; // Durée d'activation de la pompe (6 secondes)
int SEUIL_HUMIDITE = 50; // Valeur seuil d'humidité par défaut (ici 50%)
//Constantes de conversion
float a= - 0.12;
int b =125;
float HUMIDITY;
void setup() {
Serial.begin(9600);
bluetooth.begin(9600);
pinMode(PUMP_PIN, OUTPUT); // Set pin as OUTPUT
pinMode(sensor_pin, INPUT); // set pin as INPUT
Serial.print(" Sueil_humidité par defaut est : ");
Serial.print(SEUIL_HUMIDITE);
Serial.println("%");
}
void loop() {
unsigned long currentMillis = millis();
if (currentMillis - previousMillis >= interval) {
previousMillis = currentMillis;
hum = analogRead(sensor_pin);
HUMIDITY = a*hum +b;
Serial.print("Humidité: ");
Serial.print(HUMIDITY);
Serial.println(" %");
if (bluetooth.available()) {
Serial.println("Blutooth connecté");
 int receivedData = bluetooth.parseInt(); // Mettre à jour le seuil d'humidité
 // Affiche les données reçues sur le moniteur série
Serial.print("Sueil_humidité reçu: ");
Serial.print(receivedData);
Serial.println("%");
SEUIL_HUMIDITE = receivedData;
}
Serial.print("Sueil_humidité pour cette plante :");
Serial.print(SEUIL_HUMIDITE);
Serial.println("%");
// Vérifie si l'humidité est inférieure au seuil défini par l'utilisateur
if (HUMIDITY <= SEUIL_HUMIDITE ) {
Serial.println("La plante doit être arrosée !!");
Serial.println("Pump ON");
digitalWrite(PUMP_PIN, HIGH);
delay(pumpDuration);
digitalWrite(PUMP_PIN, LOW);
Serial.println("Pump OFF");
} else{
Serial.println("La plante est suffisamment arrosée");
}
}
}


# Detailed Report
[SmartIrrigate livrable.pdf](https://github.com/user-attachments/files/19250614/SmartIrrigate.livrable.pdf)
