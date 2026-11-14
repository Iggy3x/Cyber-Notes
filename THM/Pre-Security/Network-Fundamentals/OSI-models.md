# Netowkring Concepts #

# OSI model (Open system Interconnection # 

*OSI model* Tai konceptuaus frameworkas apibreziantis *kaip data keliauja per networka* nuo vieno device iki kito.
Tai *ne protokolu rinkinys* o *mastymo modelis* kuris leidzia suprasti, troubleshootint ir analizuot network problemas.

Didziausas OSI privalumas:
* Skirtingi devices , OS ir gamintojai gali komunkuoti tarpusavyje
* Kiekvienas sluoksnis turi *aiskia atsakomybe* 
* Lengva nustatyti *kurioje vietoje luzta komunikacija* 

-------------------------------------------------------------------------


# Osi layeriai #

1. `Physical Layer`
Tai fizinis duomenu perdavimas

- Kabeliai
- WIfi signalai
- Elektriniai/optiniai signalai
- Bitai (1 ir 0)

Jokios logikos , jokios adresacijos - tik signalas.

2. `Data link layer`
Tai data perdavimas *tame paciame network segmente* 
Data Link layer veikia tame pačiame local network segmente / broadcast domain.

- Naudoja *MAC adresa*
- Kuria *frames*
- Uztikrina kad data butu tinkamo formato perdavimui

*SVARBU*:
- Kiekvienas NIC turi *unikalu MAC adresa* 
- MAC yra fizinis adresas (gamyklinis, bet spoofinamas) 
- Dirba tik *local networke* 

*PVZ*:
- Ethernet (802.3)
- WiFi (802.11)


3. `Network Layer`
Tai routingas tarp skirtingu networku

- Naudoja *IP adresus* 
- Nusprendzia *kuriuo keliu keliaus packetai* 
- Skaido ir surenak data (packets)
- Perduoda packets tarp skirtingų networks
- Naudoja logical addressing per IP
- Priima routing sprendimus

Routing spredimai remiasi:
- Shortest path 
- Reliability
- Connection speed

*PVZ*:
- IP
- ICMP
- Routeriai (layeriai  3 devices) 


4. `Transport layer`
Tai end-to-end komunikacija tarp procesu
Portai identifikuoja aplikaciją/procesą hoste.
IP nurodo hostą, portas nurodo paslaugą/procesą.

- Segmentuoja data
- Valdo flow control
- Naudoja *port numbers (1-65535)* 
- Port 0 yra reserver ir relaiai normalioms connections nenaudojamas

*TCP - Transmission Control PRotocol*
- Connection oriented
- Reliable
- Error Checking
- Sequence (kiekvienas byte turi numeri) 
- Sequence numbers padeda sudeti data teisinga tvarka.
*Naudoja three-way handhsak* 
1. SYN
2. Syn-Ack
3. Ack

Naudojamas:
- Web browsing
- Email
- File trnsfer

*Minusas-letesnis* 
* UDP - User Datagram Protocol

- Connectionless
- Minimal error checking su checksum
- Nėra retransmission / delivery guarantee
- Greitas
- Port-Based


Naudojamas: 
- Streaming
- VoIP
- DHCP
- DNS

---> TCP= accuracy
---> UDP= speed
______________________________________________________________________

5. `Session Layer`
**Kas tai:** session management

- Establishina connection
- Palaiko ją
- Synchronizuoja
- Nutraukia

Pavyzdžiai:
- Login session
- Remote connections
- RPC
- NFS

👉 Jei nutrūksta ryšys – session layer sprendžia, kas toliau.
Session layer konceptualiai atsakingas už session sukūrimą, palaikymą, sinchronizavimą ir nutraukimą. Realiame TCP/IP modelyje šios funkcijos dažnai įkrenta į Application layer.

__________________________________________

6. `Presentatio Layer`
Tai data formatavimas

- Encoding/Decoding
-Compression
-Encryption

Pvz:
- ASCII
- Unicode
- MIME (email attachments)
- JPEG, PNG, MPEG
- SSL/TLS

* Uztrikrina kad layer supras data.
- SSL/TLS dažnai aiškinamas prie Presentation layer dėl encryption, bet realiame TCP/IP stack’e jis sėdi tarp Application ir Transport.

____________________________________________________

7. `Application layer`
Networko services end user aplikacijoms

Pvz:
- HTTP/HTTPS
- FTP
- DNS
- SMTP
- POP3
- IMAP
- SSH
- Telnet

Tai yra protokolai ne pacios programos!
Application layer = protokolai, kuriuos naudoja aplikacijos, ne pačios aplikacijos.

## OSI LAYER TABLE (GREITA ORIENTACIJA)

| Layer | Name | Function | Examples |
|-----|------|---------|----------|
| 7 | Application | User-facing network services | HTTP, FTP, DNS |
| 6 | Presentation | Encoding, encryption | MIME, Unicode |
| 5 | Session | Session control | RPC, NFS |
| 4 | Transport | End-to-end delivery | TCP, UDP |
| 3 | Network | Routing & IP | IP, ICMP |
| 2 | Data Link | Frames & MAC | Ethernet, WiFi |
| 1 | Physical | Signals | Cable, WiFi signal |



---

# TCP/IP Modeliai #

TCP/IP model- *praktinis interneto modelis* naudojamas realiame pasaulyje

# TP/IP layeriai #

- *Aplication* --> 5,6,7
- *Tranpsort* --> 4
- *Internet* --> 3
- *Link*--> 1,2


# Osi vs TCP/IP Mapping # 


| OSI | TCP/IP |
|----|-------|
| 7 Application | Application |
| 6 Presentation | Application |
| 5 Session | Application |
| 4 Transport | Transport |
| 3 Network | Internet |
| 2 Data Link | Link |
| 1 Physical | Link |


---

# 3. IP Adresai

- *Pulic IP* - matomas internete
- *Private IP* - local networke

Ip indentifikuoja *hosta*, bet ne procesa

---

# 4. Port Numbers

Portai indetifikuoja *procesa* ant hosto
- Range: *1-65535*
- Port 0 - reseved


Pvz:
- 80 - HTTP
- 443 - HTTPS
- 22 - SSH
- 23 -Telnet


---

#  5. Encapsulation (Core conceptas)

*Encapsulation* - procesas kai kiekvienas layer prideda savo header ( ir kartais trailer) 

Eiga:
1. Application data
2. Transport --> TCP segment / UDP Datagram
3. Network --> IP packet
4. Data link --> Frame
5. Physical --> Bits

Gavejo puseje vyksta *decapsulation*


### PRAKTINIS PAVYZDYS (TryHackMe paieška)

1. Įvedi paiešką THM puslapyje
2. Browser sukuria HTTP request (Application)
3. TCP establishina connection (3-way handshake)
4. IP prideda source & destination IP
5. Data link prideda MAC adresus
6. Routeriai nuiminėja ir uždeda L2 headers.Routeris nekeičia galutinio destination IP, bet kiekviename hop’e keičiasi L2 frame/MAC informacija.
7. Serveris gauna ir atlieka decapsulation


---

#  6. Telnet  #

*telnet* - remote terminal protocol

- Veikia per text commands
- Naudoja TCP 
- *Unencrypted* 
- Port 23

Naudojamas:
- Testavimui
- Legacy sistemoms

SSH yra secure replacement for telnet.




---


### MNEMONIC (ATSIMINTI)
**PLEASE DO NOT THROW SPINACH PIZZA AWAY**

Physical  
Data Link  
Network  
Transport  
Session  
Presentation  
Application 
