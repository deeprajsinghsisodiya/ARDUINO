# ARDUINO�� 115200

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

```




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
