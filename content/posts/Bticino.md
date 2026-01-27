+++
title = 'Bticino Klingelanlage „smart“ machen'
date = 2022-07-23T21:47:39+01:00
draft = false
+++

In unserer letzten Wohnung habe ich die in die Jahre gekommene Klingelanlage mit der [ESP-Überallklingel](https://www.heise.de/select/ct/2018/17/1534215254552977) „smart“ gemacht. In unserer aktuellen Wohnung konnte ich das Ding nicht mehr nutzen, weil Bticino mit seinem 2-Draht-System irgendein komisches Protokoll spricht und ich mit der Schaltung aus der ESP-Überallklingel nicht mehr einfach nur auf Spannung warten konnte.

Nach ein paar Monaten in der neuen Wohnung habe ich mir das Thema noch einmal angeschaut, ich fand keine brauchbare Lösung und stürzte mich in ein Durcheinander aus [TensorFlow](https://www.tensorflow.org/), [Teachable Machine](https://teachablemachine.withgoogle.com/) und Android App Programmierung. Die Idee war recht naheliegend: Wenn ich die Klingel hören kann, sollte das doch auch ein Stück Technik für mich übernehmen können. Zu meiner Überraschung habe ich es geschafft, ein KI-Modell auf den Sound unserer Klingel zu trainieren und das ganze in geklaute Kotlin-Snippets zu gießen. Leider war der 24/7 Betrieb für das verwendete Galaxy A51 nicht gesund und es gab immer wieder False Positives. Ich nahm wieder einige Monate Abstand.

Nun fiel mir vor kurzem die Hardware der ESP-Überallklingel in die Hände. Ich suchte wieder nach Lösungen und bin endlich fündig geworden. Bticino bietet ein [Funk-Gong-Interface (A00082)](https://web.archive.org/web/20220723190331/https://www.esc-shop.de/WebRoot/Store30/Shops/89603324/60F5/5D63/F5BF/033A/3D4F/0A0C/6D0B/D0CC/A00082_Datenblatt_LE000A82-D-DE_V1.3.pdf) an, das an unsere Classe 100 V12E Hausstation angeschlossen werden kann und bei einem Klingeln einen potentialfreien Kontakt schaltet. Lässt man nun die Schutzschaltung an der ESP-Überallklingel weg und schaltet den Reset-Pin stattdessen mit dem Schaltkontakt des Funk-Gong-Interface, ist man fertig.

Ich habe den Code trotzdem noch etwas angepasst. Da ich nicht auf Batteriebetrieb angewiesen bin, kann ich auf den Deep Sleep verzichten und muss nicht nach einem Reset (= Klingeln) auf den Aufbau der WLAN-Verbindung und den Connect zum MQTT-Broker warten, da vergehen schon mal 5 Sekunden vom Klingeln bis zur Notification.

Zum Schluss habe ich alles in ein [Gehäuse aus dem 3D-Drucker](https://twitter.com/ostpol/status/1550860537298407424) geklebt und im Medienkasten im Flur verschwinden lassen. Dank NodeRED lässt sich auch hier jede erdenkliche Automation bauen. Bei uns schreibt ein Telegram-Bot eine Nachricht und die Echo Dots beginnen zu erzählen.

```
//Basiert auf https://www.heise.de/select/ct/2018/17/1534215254552977
//MQTT reconnect von https://randomnerdtutorials.com/esp32-mqtt-publish-subscribe-arduino-ide/
//ezButton Library https://github.com/ArduinoGetStarted/button

#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <PubSubClient.h>
#include <ezButton.h>

const char *wifi_ssid = "SSID";
const char *wifi_password = "PSK";

IPAddress mqttserver(192, 168, 1, 222);

WiFiClient espClient;
PubSubClient client(espClient);

const char *deviceName = "ESP-Klingel";

ezButton button(12);

void setup()
{
  Serial.begin(115200);

  button.setDebounceTime(200);
  
  WiFi.hostname(deviceName);
  WiFi.begin(wifi_ssid, wifi_password);
  WiFi.mode(WIFI_STA);

  while (WiFi.status() != WL_CONNECTED)
  {
    delay(50);
  }

  client.setServer(mqttserver, 1883);
  client.connect(deviceName);
  if (client.connected())
  {
    client.publish("esp-klingel/status", "Hello.");
  }
}

void reconnect()
{
  // Loop until we're reconnected
  while (!client.connected())
  {
    Serial.print("Attempting MQTT connection...");
    // Attempt to connect
    if (client.connect(deviceName))
    {
      Serial.println("connected");
    }
    else
    {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void loop()
{
  button.loop();

  //MQTT Reconnect
  if (!client.connected())
  {
    reconnect();
  }

  if (button.isPressed())
  {
    client.publish("esp-klingel/klingel", "1");
    Serial.println("Published message");
  }
}
```
