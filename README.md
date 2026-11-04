# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
### server
```
import socket

s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
s.bind(('localhost', 8001))

s.listen(1)

print("ARP Server waiting for client...")

c, addr = s.accept()
print("Client connected")

ip = c.recv(1024).decode()

print("Received IP Address:", ip)

arp_table = {
    "10.104.8.61": "FC:6D:77:8A:DF:F4",
    "192.168.1.1": "AA:BB:CC:DD:EE:01",
    "192.168.1.2": "AA:BB:CC:DD:EE:02"
}

if ip in arp_table:
    mac = arp_table[ip]
else:
    mac = "IP Address not found"

c.send(mac.encode())

print("MAC Address:", mac)

c.close()
s.close()
```
### client
```
import socket

s = socket.socket()

s.connect(('localhost', 8001))

ip = input("Enter IP Address: ")

s.send(ip.encode())

mac = s.recv(1024).decode()

print("MAC Address:", mac)

s.close()
```
## OUPUT - ARP
### server & client:

<img width="1317" height="368" alt="image" src="https://github.com/user-attachments/assets/3a37449c-6afc-4af4-8da5-8249a3d2f5dc" />


## PROGRAM - RARP
### server
```
import socket

s = socket.socket()
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 8002))
s.listen(1)

print("RARP Server waiting for client...")

c, addr = s.accept()
print("Client connected")

mac = c.recv(1024).decode().strip().upper()

print("Received MAC Address:", mac)

rarp_table = {
    "FC:6D:77:8A:DF:F4": "10.104.8.61",
    "AA:BB:CC:DD:EE:01": "192.168.1.1",
    "AA:BB:CC:DD:EE:02": "192.168.1.2"
}

if mac in rarp_table:
    ip = rarp_table[mac]
else:
    ip = "MAC Address not found"

c.send(ip.encode())

print("IP Address:", ip)

c.close()
s.close()
```
### client
```
import socket

s = socket.socket()

s.connect(('localhost', 8002))

mac = input("Enter MAC Address: ").strip().upper()

s.send(mac.encode())

ip = s.recv(1024).decode()

print("IP Address:", ip)

s.close()

```


## OUPUT -RARP
### server & client :

<img width="1326" height="335" alt="image" src="https://github.com/user-attachments/assets/c68ecd9b-be71-4e86-9deb-7a91d773c35c" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
