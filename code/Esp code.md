# **Esp code**



## **house 1 code**

\#include <WiFi.h>

\#include <esp\_now.h>



\#define H1\_PIN 32

\#define CALIBRATION 7.5



volatile uint32\_t h1Pulse = 0;



typedef struct { uint32\_t pulses; } FlowPacket;

FlowPacket packet;



uint8\_t mainMAC\[] = {0xD4,0xE9,0xF4,0x65,0x03,0x44};



void IRAM\_ATTR h1ISR() { h1Pulse++; }



void setup() {

  Serial.begin(115200);

  WiFi.mode(WIFI\_STA);



  pinMode(H1\_PIN, INPUT\_PULLUP);

  attachInterrupt(digitalPinToInterrupt(H1\_PIN), h1ISR, RISING);



if (esp\_now\_init() != ESP\_OK) {

  Serial.println("ESP-NOW init failed");

  return;

}





  esp\_now\_peer\_info\_t peer{};

  memcpy(peer.peer\_addr, mainMAC, 6);

  esp\_now\_add\_peer(\&peer);



  Serial.println("✅ H1 ESP32 READY");

}



void loop() {

  static unsigned long last = 0;



  if (millis() - last >= 1000) {

    last = millis();



    noInterrupts();

    packet.pulses = h1Pulse;

    h1Pulse = 0;

    interrupts();



    esp\_now\_send(mainMAC, (uint8\_t\*)\&packet, sizeof(packet));

    Serial.printf("H1 Pulses: %d\\n", packet.pulses);

  }

}







## house 2 code

 	 #include <WiFi.h>

\#include <esp\_now.h>



\#define H2\_PIN 27



volatile uint32\_t h2Pulse = 0;



typedef struct { uint32\_t pulses; } FlowPacket;

FlowPacket packet;



uint8\_t mainMAC\[] = {0xD4,0xE9,0xF4,0x65,0x03,0x44};



void IRAM\_ATTR h2ISR() { h2Pulse++; }



void setup() {

  Serial.begin(115200);

  WiFi.mode(WIFI\_STA);



  pinMode(H2\_PIN, INPUT\_PULLUP);

  attachInterrupt(digitalPinToInterrupt(H2\_PIN), h2ISR, RISING);



 if (esp\_now\_init() != ESP\_OK) {

  Serial.println("ESP-NOW init failed");

  return;

}



  esp\_now\_peer\_info\_t peer{};

  memcpy(peer.peer\_addr, mainMAC, 6);

  esp\_now\_add\_peer(\&peer);



  Serial.println("✅ H2 ESP32 READY");

}



void loop() {

  static unsigned long last = 0;



  if (millis() - last >= 1000) {

    last = millis();



    noInterrupts();

    packet.pulses = h2Pulse;

    h2Pulse = 0;

    interrupts();



    esp\_now\_send(mainMAC, (uint8\_t\*)\&packet, sizeof(packet));

    Serial.printf("H2 Pulses: %d\\n", packet.pulses);

  }

}









## 

## main esp code



\#include <WiFi.h>

\#include <esp\_now.h>

\#include <WebServer.h>

\#include <ArduinoJson.h>



WebServer server(80);





uint8\_t H1\_MAC\[] = {0xAA, 0xBB, 0xCC, 0xDD, 0xEE, 0x54};

uint8\_t H2\_MAC\[] = {0x11, 0x22, 0x33, 0x44, 0x55, 0xD4};



\#define MAIN\_PIN 33

\#define SOLENOID 14



\#define CALIBRATION 7.5

\#define FLOW\_PRESENT 0.3

\#define SENSOR\_TIMEOUT 5000

\#define TOLERANCE 1.0



volatile uint32\_t mainPulse = 0;



// Received pulses

uint32\_t h1Pulse\_rx = 0;

uint32\_t h2Pulse\_rx = 0;



// Last update times

unsigned long h1Last = 0;

unsigned long h2Last = 0;



// ISR

void IRAM\_ATTR mainISR() { mainPulse++; }



// Data packets

typedef struct { uint32\_t pulses; } FlowPacket;

FlowPacket h1Data, h2Data;



// ESP-NOW receive

void onDataRecv(const esp\_now\_recv\_info \*info, const uint8\_t \*data, int len) {

  uint8\_t mac\[6];

  memcpy(mac, info->src\_addr, 6);



  if (memcmp(mac, H1\_MAC, 6) == 0) {

    memcpy(\&h1Data, data, sizeof(FlowPacket));

    h1Pulse\_rx = h1Data.pulses;

    h1Last = millis();

  }

  else if (memcmp(mac, H2\_MAC, 6) == 0) {

    memcpy(\&h2Data, data, sizeof(FlowPacket));

    h2Pulse\_rx = h2Data.pulses;

    h2Last = millis();

  }

}



void setup() {

  Serial.begin(115200);

  WiFi.mode(WIFI\_STA);



  pinMode(MAIN\_PIN, INPUT\_PULLUP);

  pinMode(SOLENOID, OUTPUT);

  digitalWrite(SOLENOID, HIGH);



  attachInterrupt(digitalPinToInterrupt(MAIN\_PIN), mainISR, RISING);



  esp\_now\_init();

  esp\_now\_register\_recv\_cb(onDataRecv);



  Serial.println("✅ MAIN ESP32 READY");



WiFi.mode(WIFI\_AP);

WiFi.softAP("WaterSystem", "12345678");



Serial.print("AP IP: ");

Serial.println(WiFi.softAPIP());



server.on("/data", HTTP\_GET, \[]() {

  StaticJsonDocument<256> doc;



  doc\["main"]\["f"] = mainPulse / CALIBRATION;

  doc\["main"]\["p"] = mainPulse;

  doc\["main"]\["working"] = true;



  doc\["h1"]\["f"] = h1Pulse\_rx / CALIBRATION;

  doc\["h1"]\["p"] = h1Pulse\_rx;

  doc\["h1"]\["working"] = (millis() - h1Last < SENSOR\_TIMEOUT);



  doc\["h2"]\["f"] = h2Pulse\_rx / CALIBRATION;

  doc\["h2"]\["p"] = h2Pulse\_rx;

  doc\["h2"]\["working"] = (millis() - h2Last < SENSOR\_TIMEOUT);



  String json;

  serializeJson(doc, json);

  server.send(200, "application/json", json);

});



server.begin();

server.on("/data", HTTP\_GET, \[](AsyncWebServerRequest \*request) {

  StaticJsonDocument<256> doc;



  doc\["main"]\["f"] = mainFlow;

  doc\["main"]\["p"] = mainPulse;

  doc\["main"]\["working"] = mainWorking;



  doc\["h1"]\["f"] = h1Flow;

  doc\["h1"]\["p"] = h1Pulse;

  doc\["h1"]\["working"] = h1Working;



  doc\["h2"]\["f"] = h2Flow;

  doc\["h2"]\["p"] = h2Pulse;

  doc\["h2"]\["working"] = h2Working;



  String response;

  serializeJson(doc, response);

  request->send(200, "application/json", response);

});





}



void loop() {

server.handleClient();



  static unsigned long last = 0;



  if (millis() - last >= 1000) {

    last = millis();



    noInterrupts();

    uint32\_t mp = mainPulse;

    mainPulse = 0;

    interrupts();



    float mainFlow = mp / CALIBRATION;

    float h1Flow   = h1Pulse\_rx / CALIBRATION;

    float h2Flow   = h2Pulse\_rx / CALIBRATION;



    bool h1Fault = false;

    bool h2Fault = false;



   if (mainFlow > FLOW\_PRESENT \&\& !h1Fault \&\& !h2Fault) {



  float diffA = mainFlow - h1Flow;

  float diffB = h1Flow - h2Flow;



  if (diffA > TOLERANCE) {

    Serial.println("🚨 LEAK/THEFT in ZONE A (MAIN → H1)");

    digitalWrite(SOLENOID, LOW);

  }

  else if (diffB > TOLERANCE) {

    Serial.println("🚨 LEAK/THEFT in ZONE B (H1 → H2)");

    digitalWrite(SOLENOID, LOW);

  }

  else {

    Serial.println("✅ FLOW NORMAL");

  }

}

    }

  }

}

