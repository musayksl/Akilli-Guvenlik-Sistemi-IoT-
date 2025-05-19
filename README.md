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

// WiFi bilgileri
const char* ssid = "WIFI_ADINIZ";
const char* password = "WIFI_SIFRENIZ";

// Telegram Bot Bilgileri
#define BOT_TOKEN "TELEGRAM_BOT_TOKEN"
#define CHAT_ID "TELEGRAM_CHAT_ID"

// HC-SR04 Bağlantıları
#define TRIG_PIN D1
#define ECHO_PIN D2
#define MAX_DISTANCE 200

// Buzzer Bağlantısı (BD139 ile)
#define BUZZER_PIN D5

// Sensör Ayarları
NewPing sonar(TRIG_PIN, ECHO_PIN, MAX_DISTANCE);
unsigned long previousMillis = 0;
const long interval = 50; // Hızlı ölçüm (50ms)
bool buzzerActive = false;

void setup() {
  Serial.begin(115200);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  // WiFi Bağlantısı
  Serial.print("WiFi'ye bağlanılıyor...");
  WiFi.begin(ssid, password);
  int retryCount = 0;
  while (WiFi.status() != WL_CONNECTED && retryCount < 5) {
    delay(1000);
    Serial.print(".");
    retryCount++;
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nWiFi Bağlı!");
  } else {
    Serial.println("\nWiFi Bağlanamadı. Sadece Buzzer Çalışır.");
  }
}

void loop() {
  unsigned long currentMillis = millis();
  
  // Sensör Hızlı Ölçüm
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;
    int distance = sonar.ping_cm();
    Serial.print("Mesafe: ");
    Serial.print(distance);
    Serial.println(" cm");

    // Hareket Algılama (Mesafe 0-50 cm arası)
    if (distance > 0 && distance < 50) {
      Serial.println("Hareket Algılandı!");
      digitalWrite(BUZZER_PIN, HIGH); // Buzzer çalışır
      sendTelegramNotification();     // Telegram Bildirimi
      delay(2000);                    // 2 saniye bekle (buzzer ve bildirim)
      digitalWrite(BUZZER_PIN, LOW);  // Buzzer kapanır
    }
  }
}

// Telegram Bildirimi Gönderme Fonksiyonu
void sendTelegramNotification() {
  if (WiFi.status() == WL_CONNECTED) {
    WiFiClientSecure client;
    client.setInsecure();
    UniversalTelegramBot bot(BOT_TOKEN, client);
    bot.sendMessage(CHAT_ID, "Hareket Algılandı!", "");
    Serial.println("Telegram Bildirimi Gönderildi.");
  } else {
    Serial.println("WiFi Bağlı Değil. Bildirim Gönderilemedi.");
  }
}


 
 
```

