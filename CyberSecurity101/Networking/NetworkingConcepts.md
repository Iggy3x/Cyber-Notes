# THM – Networking Concepts

## 1. Room esmė
Šitas roomas yra networking pagrindų sujungimas į vieną aiškesnį modelį.
Pagrindinė mintis:
> Network komunikacija nėra magija. Data keliauja per sluoksnius, kiekvienas sluoksnis prideda savo informaciją, o gavėjo pusėje viskas išpakuojama atgal.
Šitas roomas dengia:
- OSI model
- TCP/IP model
- Physical layer
- Data Link layer
- Network layer
- Transport layer
- Session layer
- Presentation layer
- Application layer
- IP addresses
- subnets
- UDP
- TCP
- encapsulation
- life of a packet
Cybersecurity kontekste svarbiausia:
> Jeigu supranti, kurioje vietoje keliauja signalas, frame, packet, segment, portas, IP ir MAC, tada nmap/Wireshark/tcpdump/VPN/web requestai pradeda daryti daug daugiau sense.
---

## 2. Network models
Network model yra būdas suskaidyti komunikaciją į sluoksnius.
Paprastai:
> Network model = schema, kuri paaiškina, kas už ką atsakingas, kai data keliauja per networką.
Kodėl to reikia:
- lengviau mokytis
- lengviau troubleshootinti
- lengviau suprasti protokolus
- lengviau atskirti problemas
- skirtingi gamintojai gali kurti suderinamus įrenginius/protokolus
Svarbiausi modeliai:
- OSI model
- TCP/IP model
Svarbu:
> OSI yra konceptualus mokymosi/troubleshooting modelis. TCP/IP yra praktiškesnis realaus interneto modelis.
---

## 3. OSI Model
OSI reiškia:
> Open Systems Interconnection
OSI turi 7 layers:
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application
Mnemonic:
> Please Do Not Throw Spinach Pizza Away
Nuo apačios į viršų:
```txt
1 Physical
2 Data Link
3 Network
4 Transport
5 Session
6 Presentation
7 Application
```
Nuo viršaus į apačią:
```txt
7 Application
6 Presentation
5 Session
4 Transport
3 Network
2 Data Link
1 Physical
```
Svarbi mintis:
> Siunčiant data eina iš viršaus žemyn. Gaunant data eina iš apačios į viršų.
---

## 4. Layer 1 – Physical
Physical layer yra fizinis signalų perdavimas.
Paprastai:
> Physical = bitai kaip signalas per fizinę terpę.
Čia yra:
- kabeliai
- Ethernet cable
- fiber optic cable
- Wi-Fi radio signal
- electrical signals
- optical signals
- wireless signals
- antenos
- 0 ir 1 perdavimas
Physical layer nekalba apie:
- IP
- MAC logic
- ports
- applications
- encryption
- routing
Čia tiesiog:
> Kaip 0 ir 1 fiziškai keliauja iš vieno įrenginio į kitą.
Pavyzdžiai:
- copper Ethernet cable
- optical fibre
- Wi-Fi 2.4 GHz
- Wi-Fi 5 GHz
- Wi-Fi 6 GHz
Troubleshooting pavyzdžiai:
- blogas kabelis
- nėra Wi-Fi signalo
- network card unplugged
- sugedęs port
- blogas signal strength
- pažeistas fiber
---

## 5. Layer 2 – Data Link
Data Link layer leidžia įrenginiams komunikuoti tame pačiame network segment.
Paprastai:
> Data Link = vietinis pristatymas tame pačiame LAN/network segmente.
Čia naudojami:
- MAC addresses
- frames
- Ethernet
- Wi-Fi
- switches
- local network communication
MAC reiškia:
> Media Access Control
MAC address dažniausiai atrodo taip:
```txt
00:1A:2B:3C:4D:5E
```
MAC adresas yra 6 bytes / 48 bits.
Dažnai rašomas hexadecimal formatu.
Pirmi 3 bytes dažnai rodo gamintoją/vendor.
Svarbiausia:
> MAC adresai veikia lokaliame segmente. Jie nėra naudojami end-to-end per visą internetą.
---

## 6. Frame
Layer 2 data unit vadinama:
> Frame
Frame turi:
- source MAC
- destination MAC
- payload
- trailer/check info
Paprastai:
> Frame = L2 konteineris, kuris pristato data iki kito įrenginio tame pačiame local segment.
Svarbu:
> Kiekviename hop’e L2 frame gali būti nuimamas ir sukuriamas naujas.
Pvz:
```txt
Laptop -> Router
source MAC = laptop MAC
destination MAC = router MAC
```
Toliau routeris sukuria naują frame kitam hop’ui.
Core mintis:
> MAC = kam perduodu dabar lokaliai?
---

## 7. Layer 3 – Network
Network layer atsakingas už packetų keliavimą tarp skirtingų networkų.
Paprastai:
> Network layer = IP addressing ir routing.
Čia naudojami:
- IP addresses
- packets
- routers
- routing
- ICMP
Layer 3 atsako į klausimą:
> Kur yra galutinis hostas?
IP address nurodo logical adresą.
Pvz:
```txt
192.168.1.10
10.10.10.10
8.8.8.8
```
Routeriai veikia Layer 3 lygyje.
Jų darbas:
- gauti packet
- pažiūrėti destination IP
- nuspręsti next hop
- perduoti packet toliau
Svarbiausia:
> IP leidžia data keliauti tarp skirtingų networks.
---

## 8. Packet
Layer 3 data unit vadinama:
> Packet
Packet turi:
- source IP
- destination IP
- protocol info
- payload
Paprastai:
> Packet = L3 konteineris, kuris leidžia data keliauti per networks.
Svarbu:
> Kelionėje per routerius destination IP dažniausiai lieka tas pats, bet MAC/L2 frame keičiasi kiekviename hop’e.
Trumpai:
```txt
MAC keičiasi per hop’us.
IP lieka end-to-end.
```
Mental model:
```txt
MAC = next local step
IP = final host
Port = process/app on that host
```
---

## 9. Layer 4 – Transport
Transport layer leidžia komunikuoti aplikacijoms/procesams skirtinguose hostuose.
Paprastai:
> Transport = end-to-end komunikacija tarp procesų.
Čia naudojami:
- TCP
- UDP
- ports
- segments
- datagrams
- flow control
- error handling
- ordering
Port numbers:
```txt
0–65535
```
Portas identifikuoja paslaugą/procesą hoste.
Pvz:
```txt
80  = HTTP
443 = HTTPS
22  = SSH
53  = DNS
```
Svarbiausia:
> IP randa hostą. Portas randa paslaugą/procesą tame hoste.
---

## 10. TCP
TCP reiškia:
> Transmission Control Protocol
TCP yra:
- connection-oriented
- reliable
- ordered
- error checked
- slower than UDP
- uses retransmission
TCP naudoja three-way handshake:
1. SYN
2. SYN-ACK
3. ACK
Paprastai:
> TCP pirmiausia susitaria dėl connection, tada siunčia data.
TCP tinka:
- web browsing
- HTTPS
- SSH
- file transfer
- email
- situations where accuracy matters
TCP garantuoja:
- data pasiekimą arba klaidos aptikimą
- ordering
- retransmission
- flow control
Svarbi mintis:
> TCP = patikimumas ir tvarka.
---

## 11. UDP
UDP reiškia:
> User Datagram Protocol
UDP yra:
- connectionless
- fast
- lightweight
- no delivery guarantee
- no ordering guarantee
- no retransmission
- minimal error checking su checksum
UDP nenaudoja three-way handshake.
Paprastai:
> UDP tiesiog siunčia datagrams be ilgo susitarimo.
UDP tinka:
- streaming
- VoIP
- online gaming
- DNS
- DHCP
- situations where speed matters more than perfect delivery
Svarbi mintis:
> UDP = greitis ir mažesnis overhead, bet mažiau garantijų.
Ne sakyti:
```txt
UDP neturi jokio error checking.
```
Geriau:
```txt
UDP turi checksum, bet neturi TCP tipo delivery/retransmission guarantee.
```
---

## 12. TCP vs UDP
Trumpai:
```txt
TCP = reliable, ordered, connection-oriented
UDP = faster, connectionless, no delivery guarantee
```
Lentelė:
| Savybė | TCP | UDP |
|---|---|---|
| Connection | Taip | Ne |
| Reliability | Aukšta | Žema |
| Ordering | Taip | Ne |
| Retransmission | Taip | Ne |
| Speed | Lėčiau | Greičiau |
| Overhead | Didesnis | Mažesnis |
| Pavyzdžiai | HTTPS, SSH, FTP | DNS, DHCP, VoIP |
Svarbu:
> Nėra “TCP geras, UDP blogas”. Jie skirti skirtingoms situacijoms.
---

## 13. Layer 5 – Session
Session layer atsakingas už session valdymą tarp aplikacijų.
Paprastai:
> Session = komunikacijos sesijos sukūrimas, palaikymas, sinchronizavimas ir nutraukimas.
Funkcijos:
- establish session
- maintain session
- synchronize communication
- recover from interruptions konceptualiai
- terminate session
Pavyzdžiai:
- RPC
- NFS
Svarbu:
> Realiame TCP/IP pasaulyje session funkcijos dažnai būna įgyvendintos aplikacijose/protokoluose, o ne kaip atskiras aiškiai matomas sluoksnis.
---

## 14. Layer 6 – Presentation
Presentation layer atsakingas už data formatą.
Paprastai:
> Presentation = data formatavimas, encoding, compression, encryption.
Čia gali būti:
- encoding
- decoding
- compression
- decompression
- encryption
- decryption
Pavyzdžiai:
- ASCII
- Unicode
- JPEG
- PNG
- MPEG
- MIME
- TLS/SSL dažnai aiškinamas čia dėl encryption
Svarbu:
> Presentation layer rūpinasi, kad data būtų suprantama aplikacijai tinkamu formatu.
Praktinis niuansas:
> TLS realiame TCP/IP stack’e ne visada gražiai telpa į vieną OSI layer, bet mokymuisi dažnai dedamas prie Presentation dėl encryption.
---

## 15. Layer 7 – Application
Application layer yra arčiausiai user-facing aplikacijų.
Paprastai:
> Application layer = protokolai, kuriuos naudoja aplikacijos.
Pavyzdžiai:
- HTTP
- HTTPS
- DNS
- FTP
- SMTP
- POP3
- IMAP
- SSH
- Telnet
Svarbu:
> Application layer nėra pati programa. Tai protokolai, kuriuos programa naudoja.
Pvz:
- Firefox/Chrome yra browseriai
- HTTP/HTTPS yra application layer protokolai
- Thunderbird/Outlook yra mail clients
- SMTP/IMAP/POP3 yra email protokolai
---

## 16. TCP/IP Model
TCP/IP model yra praktiškesnis realaus interneto modelis.
Dažnai naudojami sluoksniai:
- Application
- Transport
- Internet
- Link
Kartais išskiriamas Physical layer.
Mapping:
| OSI | TCP/IP |
|---|---|
| 7 Application | Application |
| 6 Presentation | Application |
| 5 Session | Application |
| 4 Transport | Transport |
| 3 Network | Internet |
| 2 Data Link | Link |
| 1 Physical | Link/Physical |
Svarbiausia:
> OSI yra geras mokymuisi, TCP/IP yra artimesnis realiai interneto architektūrai.
---

## 17. IP Addresses
IP address yra logical host identifier.
Paprastai:
> IP adresas leidžia rasti hostą networke.
IPv4 pavyzdžiai:
```txt
192.168.0.1
172.16.159.243
10.10.10.10
8.8.8.8
```
Kodėl reikia IP?
> Kad hostas būtų unikaliai identifikuojamas tinkle ir kiti hostai žinotų, kur siųsti data.
Analogija:
> IP = pašto adresas networke.
Svarbu:
> IP identifikuoja hostą arba network interface, bet ne konkrečią programą. Programą/paslaugą identifikuoja portas.
---

## 18. IPv4
IPv4 naudoja 32 bits.
Rašomas kaip 4 octets:
```txt
192.168.1.10
```
Kiekvienas octet yra 0–255.
Pvz:
```txt
192 . 168 . 1 . 10
```
Private IPv4 ranges:
```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```
Private IP naudojami LAN/private networks.
Public IP naudojami internete.
Svarbu:
> Private IP nėra tiesiogiai routinamas viešame internete.
---

## 19. IPv6
IPv6 naudoja 128 bits.
Pavyzdys:
```txt
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```
Kodėl atsirado IPv6?
> IPv4 adresų kiekis ribotas, o interneto įrenginių kiekis išaugo.
IPv6 turi labai daug adresų.
Šitam lygy:
> Reikia žinoti, kad IPv6 egzistuoja ir sprendžia IPv4 adresų trūkumo problemą, bet THM/labs dažnai vis dar remiasi IPv4.
---

## 20. Subnets
Subnet yra mažesnė network dalis.
Paprastai:
> Subnet = IP adresų range, kuris priklauso tam pačiam network segmentui.
Pvz:
```txt
192.168.1.0/24
```
Dažnai `/24` reiškia:
```txt
192.168.1.0 – 192.168.1.255
```
Naudojami host adresai dažnai:
```txt
192.168.1.1 – 192.168.1.254
```
Nes:
```txt
192.168.1.0   = network address
192.168.1.255 = broadcast address
```
Svarbu:
> Subnetting padeda suskirstyti tinklus į mažesnes logines dalis.
Kodėl naudinga:
- tvarka
- mažiau broadcast chaos
- geresnė segmentacija
- security boundaries
- lengvesnis valdymas
---

## 21. CIDR trumpai
CIDR notation atrodo taip:
```txt
192.168.1.0/24
```
`/24` reiškia:
> pirmi 24 bits yra network dalis.
Kuo didesnis prefix:
```txt
/24
/25
/26
```
tuo mažesnis subnet.
Kuo mažesnis prefix:
```txt
/16
/8
```
tuo didesnis subnet.
Labai basic orientacija:
```txt
/24 ≈ 256 adresai
/16 ≈ 65k adresų
/8  ≈ 16M adresų
```
Svarbu:
> Šitam roomui nereikia tapti subnetting matematiku. Reikia suprasti, kad slash notation apibrėžia network dydį.
---

## 22. Encapsulation
Encapsulation reiškia, kad kiekvienas sluoksnis prideda savo header, o kartais ir trailer.
Paprastai:
> Encapsulation = data supakavimas sluoksniais.
Siunčiant:
1. Application data
2. Transport prideda TCP/UDP header
3. Network prideda IP header
4. Data Link prideda L2 header/trailer
5. Physical perduoda bits kaip signalą
Schema:
```txt
Application Data
-> TCP Segment / UDP Datagram
-> IP Packet
-> Ethernet/Wi-Fi Frame
-> Bits
```
Svarbu:
> Kiekvienas layer prideda informaciją, kuri reikalinga jo funkcijai.
---

## 23. Decapsulation
Decapsulation vyksta gavėjo pusėje.
Paprastai:
> Decapsulation = sluoksnių išpakavimas.
Gavėjo pusėje:
1. Physical gauna bits
2. Data Link nuima frame header/trailer
3. Network nuima IP header
4. Transport nuima TCP/UDP header
5. Application gauna data
Schema:
```txt
Bits
-> Frame
-> Packet
-> Segment/Datagram
-> Application Data
```
Svarbu:
> Encapsulation siunčiant eina žemyn per layers. Decapsulation gaunant eina aukštyn.
---

## 24. Data units pagal layer
Greita lentelė:
| Layer | Data unit |
|---|---|
| Application | Data |
| Transport TCP | Segment |
| Transport UDP | Datagram |
| Network | Packet |
| Data Link | Frame |
| Physical | Bits |
Svarbu:
> Kai matai žodžius segment, packet, frame, jie nėra random. Jie nurodo, kuriame sluoksnyje kalbam.
---

## 25. Life of a Packet – THM paieškos pavyzdys
Pavyzdys: įvedi paiešką THM puslapyje.
Eiga:
1. Įvedi search query THM puslapyje.
2. Browseris paruošia HTTP request.
3. Kadangi HTTPS naudoja TCP, TCP turi užmegzti connection per three-way handshake.
4. TCP sukuria segmentus.
5. IP layer prideda source IP ir destination IP.
6. Data Link layer prideda source MAC ir destination MAC, kad packet pasiektų routerį/local next hop.
7. Physical layer paverčia tai signalais per kabelį/Wi-Fi.
8. Routeris gauna frame, nuima L2 info, pažiūri į destination IP.
9. Routeris parenka next hop ir sukuria naują L2 frame.
10. Packet keliauja per kelis routerius.
11. Destination serveris gauna data ir vykdo decapsulation.
12. Web serveris/aplikacija apdoroja requestą.
13. Response keliauja atgal panašiu principu.
Svarbiausia:
> Destination IP rodo galutinį serverį. Destination MAC kiekviename hop’e rodo tik sekantį lokalų gavėją.
---

## 26. Routerio vaidmuo
Routeris jungia skirtingus networks.
Paprastai:
> Routeris peržiūri destination IP ir nusprendžia, kur siųsti packet toliau.
Routeris:
- gauna L2 frame
- išima IP packet
- tikrina destination IP
- žiūri routing table
- pasirenka next hop
- sukuria naują L2 frame
- siunčia toliau
Svarbi mintis:
> Routeriai keičia L2 frame informaciją, bet ne galutinį destination IP normalioje routing kelionėje.
Notes frazė:
```txt
MAC keičiasi kiekviename hop’e, nes jis skirtas vietiniam pristatymui. IP išlieka, nes jis nurodo galutinį source/destination per visą kelionę.
```
---

## 27. Switch vaidmuo
Switch veikia daugiausia Layer 2.
Paprastai:
> Switch naudoja MAC addresses ir persiunčia frames tame pačiame LAN.
Switch:
- mokosi MAC adresus
- susieja MAC su portais
- siunčia frame į tinkamą portą
- mažina nereikalingą traffic
Svarbu:
> Switch neperkelia packetų tarp skirtingų IP networks kaip routeris. Jis dirba local LAN segmentuose.
---

## 28. OSI troubleshooting
OSI padeda suprasti, kur lūžta komunikacija.
Pavyzdžiai:
Layer 1:
```txt
Kabelis, Wi-Fi signalas, fizinis ryšys
```
Layer 2:
```txt
MAC, switch, local LAN, ARP
```
Layer 3:
```txt
IP, routing, gateway, ICMP
```
Layer 4:
```txt
TCP/UDP, ports, firewall, connection
```
Layer 7:
```txt
HTTP, DNS, SSH, application protocol
```
Troubleshooting eiga:
```txt
Ar turiu fizinį ryšį?
Ar turiu IP?
Ar matau gateway?
Ar DNS veikia?
Ar portas atidarytas?
Ar aplikacija atsako?
```
Svarbu:
> Neskubėk iškart prie aplikacijos. Kartais problema yra DNS, gateway, firewall arba tiesiog VPN nepakeltas.
---

## 29. Praktinė vertė Wireshark/tcpdump
Šitas roomas labai padeda suprasti packet capture.
Wireshark/tcpdump matysi:
- Ethernet frame
- source MAC
- destination MAC
- IP packet
- source IP
- destination IP
- TCP/UDP
- source port
- destination port
- application protocol
Pvz:
```txt
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
Hypertext Transfer Protocol
```
Tai nėra random:
```txt
L2 -> L3 -> L4 -> L7
```
Svarbu:
> Packet capture realiai rodo encapsulation išskleistą akimis.
---

## 30. Praktinė vertė nmap
Nmap dirba su Layer 3/4 ir service discovery.
Kai darai:
```bash
nmap 10.10.10.10
```
Tu tikrini:
- ar host pasiekiamas
- kokie TCP/UDP ports atviri
- kokios services veikia
Nmap output:
```txt
22/tcp open ssh
80/tcp open http
443/tcp open https
```
Reiškia:
- IP rado hostą
- portai rodo paslaugas
- application layer protokolai veikia ant transport layer portų
Svarbu:
> Nmap nėra “hack”. Tai network/service discovery įrankis.
---

## 31. Praktinė vertė THM/VPN
THM labuose dažnai gauni target IP:
```txt
10.10.x.x
```
Kai jungiesi per OpenVPN, tavo machine gauna kelią į THM private network.
Tikrinimai:
```bash
ip a
ip route
ping -c 4 10.10.x.x
nmap 10.10.x.x
```
Jeigu neveikia:
```txt
Ar openvpn veikia?
Ar yra tun0?
Ar yra route į THM network?
Ar target up?
Ar ICMP blokuojamas?
Ar reikia nmap -Pn?
```
Svarbu:
> Jei ping neatsako, host nebūtinai miręs. Gali būti firewall/ICMP block.
---

## 32. Komandos
Pažiūrėti IP Linux:
```bash
ip a
```
Pažiūrėti route:
```bash
ip route
```
Ping:
```bash
ping -c 4 8.8.8.8
```
Ping target:
```bash
ping -c 4 10.10.10.10
```
Traceroute:
```bash
traceroute example.com
```
Jei nėra:
```bash
sudo apt install traceroute
```
DNS lookup:
```bash
dig example.com
```
Arba:
```bash
nslookup example.com
```
Nmap basic:
```bash
nmap 10.10.10.10
```
Nmap be host discovery:
```bash
nmap -Pn 10.10.10.10
```
TCP port scan:
```bash
nmap -sT -p 80,443 10.10.10.10
```
Service detection:
```bash
nmap -sV 10.10.10.10
```
Tcpdump ICMP:
```bash
sudo tcpdump -i any icmp
```
Tcpdump host:
```bash
sudo tcpdump -i any host 10.10.10.10
```
Tcpdump port:
```bash
sudo tcpdump -i any port 80
```
---

## 33. Core mental model
Svarbiausias modelis:
```txt
L1 Physical  = signalas / bits
L2 Data Link = MAC / frame / local delivery
L3 Network   = IP / packet / routing
L4 Transport = TCP/UDP / ports / process-to-process
L7 App       = HTTP/DNS/SSH/etc
```
Dar trumpiau:
```txt
MAC = kam perduodu dabar?
IP = kur galutinis hostas?
Port = kuriai programai/paslaugai tame hoste?
Protocol = kokia kalba jie kalba?
```
Labai svarbu:
> MAC nėra “interneto adresas”. MAC yra local delivery adresas.
---

## 34. Mano paaiškinimas žmonių kalba
Įsivaizduok siuntinį.
Application:
> parašai žinutę arba requestą.
Transport:
> nusprendžia ar siųsti patikimai su parašu/TCP, ar greitai/UDP.
Network:
> užrašo galutinį miestą/adresą/IP.
Data Link:
> užrašo, kam perduoti siuntinį šiame sandėlyje/MAC.
Physical:
> fiziškai pajuda mašina/kabelis/Wi-Fi signalas.
Kiekviename sandėlyje:
- galutinis adresas ant siuntinio lieka tas pats
- vietinis “perduoti sekančiam kurjeriui” keičiasi
Tai ir yra:
```txt
IP lieka, MAC keičiasi.
```
---

## 35. Ką būtina atsiminti
- OSI model turi 7 layers.
- TCP/IP model yra praktiškesnis realiam internetui.
- Physical layer perduoda bits per fizinę terpę.
- Data Link layer naudoja MAC ir frames.
- MAC veikia lokaliame network segmente.
- Network layer naudoja IP ir packets.
- Routeriai dirba Layer 3 ir route’ina tarp networks.
- Transport layer naudoja TCP/UDP ir ports.
- TCP yra reliable/connection-oriented.
- UDP yra fast/connectionless/no delivery guarantee.
- Session layer konceptualiai valdo sessions.
- Presentation layer tvarko formatą/encryption/encoding.
- Application layer turi protokolus kaip HTTP/DNS/SSH.
- IP identifikuoja hostą.
- Portas identifikuoja procesą/paslaugą.
- Subnet yra IP range/network dalis.
- Encapsulation prideda headers/trailers keliaujant žemyn.
- Decapsulation išima headers/trailers keliaujant aukštyn.
- Frame, packet, segment/datagram nėra tas pats.
- MAC keičiasi kiekviename hop’e.
- IP išlieka kaip galutinio source/destination logika.
---

## 36. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- subnetting binary matematiką iki dugno
- IPv6 advanced
- TCP flags visus edge cases
- MTU/MSS gylį
- VLAN trunking
- routing protocols
- BGP/OSPF
- ARP spoofing praktines atakas
- TCP congestion control
- packet fragmentation gylį
- Wireshark expert analizę
Dabar svarbiausia:
> Suprasti sluoksnius, IP/MAC/port skirtumus, TCP vs UDP, subnet idėją ir encapsulation.
---

## 37. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kam reikalingas network model?
2. Kiek layers turi OSI model?
3. Koks mnemonic OSI layeriams?
4. Ką daro Physical layer?
5. Ką daro Data Link layer?
6. Kas yra MAC address?
7. Kodėl MAC veikia tik lokaliai?
8. Kas yra frame?
9. Ką daro Network layer?
10. Kas yra IP address?
11. Kas yra packet?
12. Ką daro routeris?
13. Kodėl routeris keičia L2 frame, bet ne galutinį destination IP?
14. Ką daro Transport layer?
15. Kas yra port?
16. Kuo TCP skiriasi nuo UDP?
17. Kas yra TCP three-way handshake?
18. Kodėl UDP naudojamas streaming/VoIP/gaming?
19. Ką daro Session layer?
20. Ką daro Presentation layer?
21. Kas yra Application layer protocol?
22. Kuo OSI skiriasi nuo TCP/IP modelio?
23. Kas yra subnet?
24. Ką reiškia `/24` labai basic lygyje?
25. Kas yra encapsulation?
26. Kas yra decapsulation?
27. Kuo skiriasi segment, datagram, packet ir frame?
28. Kodėl Wireshark’e matai L2/L3/L4/L7 info?
29. Kodėl IP randa hostą, bet portas randa paslaugą?
30. Kodėl ping neatsakymas nebūtinai reiškia, kad hostas miręs?
---

## 38. Mano verdiktas
Šitas roomas yra vienas svarbiausių tavo networking pagrindui.
Statusas:
```txt
Status: Foundation / Core Networking
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš Nmap, Wireshark, tcpdump, VPN troubleshooting, web/security rooms
```
Svarbiausia mintis:
> Networking nėra atskiri random terminai. Tai sluoksniuota sistema: signalas -> frame -> packet -> segment/datagram -> application data.
Jeigu supranti:
- MAC veikia lokaliai
- IP keliauja tarp networks
- portas rodo paslaugą
- TCP/UDP yra transport layer
- encapsulation supakuoja data sluoksniais
tada labai daug cyber tools pradeda atrodyti logiškai.
Core rule:
> Kai stringi, paklausk savęs: kuriame layer problema?
