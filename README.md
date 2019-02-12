# **Led Wi-Fi (Arduino ESP32**

# **Vidéo**
[![video](https://img.youtube.com/vi/32Ffw0l85_8.jpg)](https://www.youtube.com/watch?v=32Ffw0l85_8)

# **Branchements** 

![Branchements](https://image.noelshack.com/fichiers/2019/07/2/1549970805-branchements-esp32.png)

# **Matériels utilisés**   

* [Arduino ESP32](https://www.amazon.fr/SeeKool-ESP-32S-d%C3%A9veloppement-Bluetooth-Ultra-Low/dp/B07DPP3BGZ/ref=sr_1_1_sspa?ie=UTF8&qid=1549971003&sr=8-1-spons&keywords=esp32&psc=1)

![Arduino ESP32](https://images-na.ssl-images-amazon.com/images/I/61H9-mWSrAL._SL1001_.jpg)


* [Led](https://www.amazon.fr/Gazechimp-Electroluminescente-Brillante-Couleurs-Stockage/dp/B071S57B47/ref=sr_1_6?ie=UTF8&qid=1549971152&sr=8-6&keywords=led+arduino)

![Led](https://images-na.ssl-images-amazon.com/images/I/51W0VUwFbAL._SL1024_.jpg)




# **Bibliothèques utilisées**

* WiFi
* Adafruit-MQTT
* Adafruit-MQTT et 
* UTimerLib 

 # Code

  

``` c++
#include <Adafruit_MQTT.h>
#include <Adafruit_MQTT_Client.h>
//inclure les bibliotheque adafruit efface la ligne fonah
#include <WiFi.h>

WiFiClient wiFiClient;
Adafruit_MQTT_Client mqttClient(&wiFiClient, "192.168.1.103", 1883);
Adafruit_MQTT_Subscribe lumSubscriber(&mqttClient, "/lum");

void lumCallback(double x) {
  Serial.println(x);
  if(x == 1) {
    digitalWrite(19, HIGH);
  }
  else if ( x == 0) {
    digitalWrite(19, LOW);
  }
}
  
void setup() {
  
// put your setup code here, to run once:
  Serial.begin(115200);
  pinMode(19, OUTPUT);
  WiFi.begin("createch", "createch");
  delay(5000);
  
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
  lumSubscriber.setCallback(lumCallback);
  mqttClient.subscribe(&lumSubscriber);
  
}
 
void loop() {
 
  
  if (mqttClient.connected()) {
    mqttClient.processPackets(10000);
    mqttClient.ping();
  } else {
    mqttClient.disconnect();
    mqttClient.connect();
  }
}
```
