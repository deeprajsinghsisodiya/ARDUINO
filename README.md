# ARDUINO�� 115200


```
at


OK
AT +CIOBAUD9600


ERROR
AT+CIOBAUD9600


ERROR
AT+CIOBAUD=9600


ERROR
AT+CWMODE=1


OK
AT+CWLAP

+CWLAP:(4,"FTTH-3CD7",-44,"4c:ae:1c:40:3c:d7",9,16,0)

OK
AT+CWGAP="FTTH-3CD7","lovekush"


ERROR
AT+CWJAP="FTTH-3CD7","lovekush"

WIFI CONNECTED
WIFI GOT IP

OK
AT+CISFR


ERROR
AT+CIFSR

+CIFSR:STAIP,"192.168.1.10"
+CIFSR:STAMAC,"48:3f:da:6a:56:67"
```

#### While connecting esp8266 with arduino 

```
follow this.
while uploading reset pin of Arduino should be connected to ground. and gpoi 0 should be connected to ground.
esp gnd - ar gnd
esp gpio0- ar gnd
esp rx- ar rx
esp tx- ar tx
esp ch-en - ar 3.3v
esp vcc - ar 3.3v

restart ide select the board and port. 9600 bits per second in device manager com settings, 115200 in tools(Arduino) upload speed.

once upload is done disconnect reset and gpio0 from ground. disconnect vcc and reconnect to 3.3v

while uploading when show erroe connect reset pin to groud -tx will blink and try again uploading.





#### Serial communication esp8266 and mega2560

If you want to establish serial communication between an ESP8266 and an Arduino Mega, you can use the hardware serial ports of both devices. Here's an example where the ESP8266 sends a "hello" string to the Arduino Mega over Serial communication:

ESP8266 Code (Sender):
cpp
Copy code
void setup() {
  Serial.begin(115200);  // Serial port for debugging

  // Your setup code here
}

void loop() {
  Serial.println("hello");  // Send "hello" to Arduino Mega
  delay(2000);  // 2-second delay
}
Arduino Mega Code (Receiver):
cpp
Copy code
void setup() {
  Serial.begin(115200);  // Serial port for debugging
  Serial1.begin(115200); // Serial1 for communication with ESP8266

  // Your setup code here
}

void loop() {
  if (Serial1.available()) {
    // Read the incoming data
    char receivedChar = Serial1.read();

    // Print the received character to the Serial Monitor
    Serial.print(receivedChar);

    // Check if the received data forms the complete "hello" string
    if (receivedChar == '\n') {
      // Perform an action when the complete string is received
      Serial.println("Received: hello");
    }
  }

  // Your loop code here
}
In this example:

The ESP8266 sends the "hello" string through the Serial port.
The Arduino Mega reads the incoming data from Serial1 (TX1, RX1 pins) and prints it to the Serial Monitor.
It checks if the received character is a newline character ('\n'), indicating the end of the "hello" string. If so, it prints a message to the Serial Monitor.
Connect the TX pin of the ESP8266 to the RX1 pin (pin 19) of the Arduino Mega, and connect the ground (GND) of both devices. Adjust the baud rate (115200 in this case) and pin numbers as needed for your specific hardware configuration.

Note: Ensure that the voltage levels are compatible between the ESP8266 (operating at 3.3V) and the Arduino Mega (which typically operates at 5V). If necessary, use level shifters or voltage dividers to ensure proper communication.


```


#### Final code for steeper control esp8266 and mega just to receive value and print


```
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

const char *ssid = "FTTH-3CD7";
const char *password = "lovekush";

IPAddress staticIP(192, 168, 1, 100);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

void handleRoot() {
  String html = "<html><body>";
  html += "<h1>ESP8266 Web Server</h1>";
  html += "<form action='/setvalue' method='get'>";
  html += "Enter value (0-100): <input type='text' name='value'>";
  html += "<input type='submit' value='Submit'>";
  html += "</form>";
  html += "</body></html>";
  
  server.send(200, "text/html", html);
}

void handleValue() {
  String valueStr = server.arg("value");
  int value = valueStr.toInt();

  Serial.print("Received value: ");
  Serial.println(value);

  if (value >= 0 && value <= 100) {
    Serial.println("Sending value to Arduino Mega");
    Serial.println(value);  // Sending value as a debug message
    server.send(200, "text/plain", "Value received successfully");
  } else {
    server.send(400, "text/plain", "Invalid value. Please enter a value between 0 and 100.");
  }
}

void setup() {
  Serial.begin(115200);

  WiFi.config(staticIP, gateway, subnet);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
 // Print the IP address after successful connection
  Serial.print("Connected to WiFi. IP Address: ");
  Serial.println(WiFi.localIP());

  server.on("/", HTTP_GET, handleRoot);
  server.on("/setvalue", HTTP_GET, handleValue);

  server.begin();
}

void loop() {
  server.handleClient();
  // Add your other code logic here
}

```

```

void setup() {
  Serial.begin(115200);    // Serial port for debugging
  Serial1.begin(115200);   // Serial1 for communication with ESP8266
}

void loop() {
  if (Serial1.available() > 0) {
    int receivedValue = Serial1.parseInt();
    Serial.print("Received value from ESP8266: ");
    Serial.println(receivedValue);
  }
  
  // Your other code logic here
}

```
