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
