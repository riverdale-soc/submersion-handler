| Supported Targets | ESP32 | ESP32-S2 |
| ----------------- | ----- | -------- |

# Submersion Handler
Implements ESP32 module of submersion detection and location system to receive MOB packets from submersion module and propagate GPS and MOB state to drone controller attached via SPI. 


<img width="529" alt="image" src="https://github.com/riverdale-soc/submersion-handler/assets/68623356/fc765620-b0e3-437a-a3c1-4e567394f1f0">

## At Boot
* Initialize SPI master
* Receive handshake from SPI Slave
* Start WiFi
* Initialize ESPNOW
* Register ESPNOW sending and receiving callback function
* Entry into idle task waiting for ESP-NOW events

## At ESP-NOW Event
* Register MAC Address of Source
* Decompose payload into state and coordinates 
* Push payload to SPI Master Queue

## Design Decisions
To minimize latency, the SPI bus is used to push raw bytes of MOB status payload at 25Mhz. This removes need to parse ESP-NOW payload into strings and can be simply propagated as it came. This means payload can be propagated in 5.2 microseconds using the standard payload.
### ESP-NOW Payload
| MOB State | Latitude | Longitude | Altitude |
| --------  | -------- | --------- | -------- |
| 1 byte    | 4 bytes  | 4 bytes   | 4 bytes  |  


