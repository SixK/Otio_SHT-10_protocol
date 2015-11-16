Otio_SHT-10_Protocol
====================

Here is source code to decode Otio SHT-10 Temperature and Humidity sensor using an arduino and RF433Mhz Receiver  
This may work with some other various device clones, but has only been tested on Otio SHT-10 model.  

This code is based on following page work :  
http://arduino.cc/forum/index.php/topic,142871.msg1106336.html#msg1106336


**Byte type is defined by time between 2 rising edges :**  
Synchro byte : 9320us  
1 : 4500us  
0 : 2530us  

**Here are Typical messages sent by sensor (3x36 bytes - 3 times the same message):**  
11001110 0011 111000001000 01100010 1110 - 11001110 0011 111000001000 01100010 1110 - 110011100011110....  
11001110 0011 100000001000 11001010 1111 11001110 0011 100000001000 11001010 1111 11001110001100  
11001110 0100 010001110000 01100010 1001 110011100100010001110000011000101001   


**Here is how 36 Bytes datagram is composed :**  

| | 36-29    | 28-25    | 24-13 | 12-9            | 8-5              | 4-1              |
|----------|----------|-------|-----------------|------------------|--------------------|------|
| | Header   | Infos | T 0.1 °C  (LSB) | Humidity (unit) | Humidity (Decade) | CRC  |
| Trame    | 11001110 | 0101  | 111001110000    | 1010             | 0010               | 1011 |
| Result |          |       | 23.1            | 5                | 4                  |      |


**How to get Channel from Header :**  

| Header   | Meaning        |
|----------|----------------------|
| 11001010 | xxxx10xx --> Channel 2 |
| 11000110 | xxxx01xx --> Channel 1 |
| 11001110 | xxxx11xx --> Channel 3 |


**How to get Extra infos :**  

| Infos (ABCD) | Meaning | Value                                   |
|--------------|---------------|------------------------------------------|
| A            | Battery state | 0 --> OK  1 --> KO                        |
| BC           | Unknown      |                                          |
| D            | Sending mode  | 1 --> Tx button pushed  0 --> normal mode |


**Reverse engineering done with :**  
- Arduino leonardo  
- RF433Mhz Receiver  
- PC with sound card and Audacity (Low cost Oscilloscope)  
- Male 2 Male Jack 3.5 cable  
- 1 wire tight between RF433Mhz Receiver data pin and left/right Jack 3.5 channel  
- 1 wire tight between Arduino ground and Jack 3.5 ground


SixK
