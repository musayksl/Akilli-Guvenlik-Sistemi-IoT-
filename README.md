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

