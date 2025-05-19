# 🚀 Akıllı IoT Güvenlik Sistemi - NodeMCU + HC-SR04 + Buzzer + Telegram 🚀

Bu proje, NodeMCU (ESP8266), HC-SR04 ultrasonik sensör ve bir buzzer kullanarak gerçek zamanlı bir güvenlik sistemi sağlar.  
Hareket algılandığında alarm çalar ve Telegram üzerinden anında bildirim gönderir.  

---

## 💡 Özellikler:
- 🚀 **Hızlı ve Güvenilir:** Sensör, hareketi anında algılar.  
- 🔔 **Güvenli Alarm:** Buzzer, BD139 transistör ile güvenli bir şekilde kontrol edilir.  
- 📲 **Anında Bildirim:** Telegram botu ile anında telefonunuza uyarı gelir.  
- 🌐 **Güvenli WiFi Bağlantısı:** WiFi bağlantısı otomatik olarak kontrol edilir.  
- 🔋 **Enerji Verimli:** Kod, NodeMCU'yu gereksiz yere yormaz.  

---

## 📌 Kullanılan Bileşenler:
- NodeMCU (ESP8266 - ESP-12E)  
- HC-SR04 Ultrasonik Sensör  
- Buzzer (BD139 Transistör ile Güvenli Kontrol)  
- Telegram Bot (API ile Anında Bildirim)  
- Arduino IDE  

---

## 📌 Bağlantı Şeması:
- HC-SR04 VCC → NodeMCU Vin (5V)  
- HC-SR04 GND → NodeMCU GND  
- HC-SR04 Trig → D1 (GPIO5)  
- HC-SR04 Echo → D2 (GPIO4) (Gerilim bölücü ile)  
- Buzzer (+) → 5V  
- Buzzer (-) → BD139 Collector  
- BD139 Base → D5 (GPIO14) (1kΩ direnç ile)  
- BD139 Emitter → GND  

---
📌 Arduino IDE'yi açın.

📌 Gerekli kütüphaneleri yükleyin:

- NewPing (HC-SR04 için)

- UniversalTelegramBot (Telegram API)

- ESP8266WiFi (NodeMCU WiFi)

📌 Aşağıdaki bilgileri kodda düzenleyin ve kodu yükleyin:

const char* ssid = "WIFI_ADINIZ";
const char* password = "WIFI_SIFRENIZ";
#define BOT_TOKEN "TELEGRAM_BOT_TOKEN"
#define CHAT_ID "TELEGRAM_CHAT_ID"

📌 Kod:

```cpp
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>
#include <NewPing.h>

#define TRIG_PIN D1
#define ECHO_PIN D2
#define MAX_DISTANCE 200
#define BUZZER_PIN D5

NewPing sonar(TRIG_PIN, ECHO_PIN, MAX_DISTANCE);

void setup() {
  Serial.begin(115200);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);
  WiFi.begin("WIFI_ADINIZ", "WIFI_SIFRENIZ");
  while (WiFi.status() != WL_CONNECTED) delay(500);
  Serial.println("WiFi Bağlı!");
}

void loop() {
  int distance = sonar.ping_cm();
  if (distance > 0 && distance < 100) {
    Serial.println("Hareket Algılandı!");
    digitalWrite(BUZZER_PIN, HIGH);
    sendTelegramNotification();
    delay(2000);
    digitalWrite(BUZZER_PIN, LOW);
  }
}

void sendTelegramNotification() {
  WiFiClientSecure client;
  client.setInsecure();
  UniversalTelegramBot bot("TELEGRAM_BOT_TOKEN", client);
  bot.sendMessage("TELEGRAM_CHAT_ID", "Hareket Algılandı!", "");
}
```

