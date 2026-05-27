# THM – Tcpdump: The Basics

## 1. Room esmė
Šitas roomas aiškina `tcpdump` pagrindus.
Pagrindinė mintis:
> Tcpdump yra command-line packet capture įrankis, leidžiantis matyti network traffic tiesiai terminale arba įrašyti jį į `.pcap` failą analizei.
Šitas roomas dengia:
- kas yra tcpdump
- kas yra libpcap
- network interfaces
- basic packet capture
- capture į failą
- PCAP skaitymą
- packet count limit
- DNS/port name resolution išjungimą
- verbose output
- filtering expressions
- advanced filtering
- TCP flags
- packet display control
- ASCII/hex output
Cybersecurity kontekste tcpdump svarbus, nes:
- veikia terminale/serveriuose be GUI
- greitas troubleshooting įrankis
- leidžia capture’inti traffic labuose/servers
- gali išsaugoti PCAP Wireshark analizei
- padeda suprasti kas realiai vyksta networke
- labai naudingas THM/VPN/debug situacijose
---

## 2. Kas yra tcpdump?
`tcpdump` yra command-line packet capture ir packet analysis įrankis.
Paprastai:
> tcpdump = Wireshark be GUI, skirtas greitai pamatyti arba įrašyti network traffic terminale.
Jis gali:
- capture’inti live packets
- filtruoti traffic pagal host/port/protocol
- išsaugoti capture į `.pcap`
- skaityti `.pcap`
- rodyti packetus skirtingais formatais
- padėti troubleshootinti network issues
- būti naudojamas serveriuose per SSH
Svarbu:
> Tcpdump pats “nehackina”. Jis rodo traffic, kuris eina per pasirinktą interface.
---

## 3. libpcap
`tcpdump` remiasi biblioteka:
> libpcap
`libpcap` yra packet capture library Unix-like sistemoms.
Paprastai:
> libpcap = biblioteka, kuri leidžia įrankiams capture’inti packets iš network interfaces.
Ant Windows panaši istorija:
- WinPcap
- Npcap
Įrankiai, kurie remiasi panašiu packet capture pagrindu:
- tcpdump
- Wireshark
- tshark
Svarbu:
> Tcpdump yra tool. libpcap yra library po kapotu.
---

## 4. Tcpdump vs Wireshark
Wireshark:
- GUI
- patogu vizualiai analizei
- gerai dideliems PCAP tyrimams
- rodo layers gražiai
Tcpdump:
- terminalas
- greitas
- veikia per SSH
- geras live troubleshooting
- patogus capture’ui į failą
- tinka serveriams be desktop environment
Paprastai:
> tcpdump capture’ina greitai, Wireshark analizuoja patogiai.
Dažnas workflow:
```bash
sudo tcpdump -i eth0 -w capture.pcap
```
Tada:
```bash
wireshark capture.pcap
```
Svarbu:
> Tcpdump ir Wireshark nėra priešai. Jie dirba kartu kaip plaktukas ir didinamasis stiklas.
---

## 5. Kodėl reikia sudo?
Packet capture dažnai reikalauja elevated privileges.
Todėl dažnai:
```bash
sudo tcpdump
```
Kodėl?
> Network interface traffic skaitymas yra privilegijuotas veiksmas.
Jeigu gauni permission denied:
```bash
sudo tcpdump -i any
```
Svarbu:
> Jeigu capture’ini ne savo/lab traffic, čia jau legalumo problema. THM/own lab — ok. Svetimas networkas — ne.
---

## 6. Network interfaces
Prieš capture reikia žinoti interface.
Rodyti interfaces su tcpdump:
```bash
sudo tcpdump -D
```
Linux dažni interface’ai:
```txt
eth0
ens5
wlan0
lo
tun0
any
```
`eth0` / `ens5`:
> wired/cloud VM network interface.
`wlan0`:
> Wi-Fi.
`lo`:
> loopback.
`tun0`:
> VPN interface, dažnai THM OpenVPN.
`any`:
> capture’inti iš visų interfaces, kai palaikoma.
Patikrinti Linux interfaces:
```bash
ip a
```
Svarbu:
> Jei capture’ini blogą interface, nematysi reikalingo traffic. THM VPN traffic dažnai ieškok per `tun0`.
---

## 7. Basic capture
Basic capture ant interface:
```bash
sudo tcpdump -i eth0
```
Capture ant visų:
```bash
sudo tcpdump -i any
```
Sustabdyti:
```txt
Ctrl + C
```
Output pavyzdys:
```txt
12:34:56.789 IP 192.168.1.10.51514 > 93.184.216.34.80: Flags [S], seq 12345, win 64240
```
Paprastai matai:
- timestamp
- protocol
- source IP/port
- destination IP/port
- flags
- packet info
Svarbu:
> Tcpdump output pradžioje atrodo kaip laidų krūva, bet jame yra tie patys OSI/TCP-IP gabalai.
---

## 8. Capture packet count
Riboti packetų kiekį:
```bash
sudo tcpdump -i any -c 5
```
`-c 5` reiškia:
> capture’inti 5 packets ir sustoti.
Naudinga:
- greitam testui
- kad terminalas neužsipiltų traffic
- THM klausimams
- troubleshootingui
Pvz:
```bash
sudo tcpdump -i tun0 -c 10
```
Svarbu:
> Jei neapriboji, tcpdump veiks tol, kol sustabdysi su `Ctrl+C`.
---

## 9. Išjungti DNS ir portų vardų vertimą
Pagal default tcpdump gali bandyti versti:
- IP -> hostname
- port -> service name
Tai kartais klaidina arba lėtina.
Išjungti name resolution:
```bash
sudo tcpdump -n
```
Išjungti IP ir port name resolution:
```bash
sudo tcpdump -nn
```
Pavyzdys:
be `-nn` gali matyti:
```txt
example.com.http
```
su `-nn`:
```txt
93.184.216.34.80
```
Svarbu:
> `-nn` yra beveik visada geras įprotis mokantis ir analizuojant, nes rodo realius IP ir portus.
---

## 10. Verbose output
Verbose:
```bash
sudo tcpdump -v
```
Daugiau verbose:
```bash
sudo tcpdump -vv
```
Dar daugiau:
```bash
sudo tcpdump -vvv
```
Kuo daugiau `v`, tuo daugiau detalių.
Gali rodyti:
- TTL
- ID
- flags
- checksum info
- protocol details
- packet length
Svarbu:
> Verbose output naudinga analizei, bet gali greitai užversti akimis. Naudok pagal poreikį.
---

## 11. Saugoti capture į failą
Įrašyti į PCAP:
```bash
sudo tcpdump -i any -w capture.pcap
```
`-w` reiškia:
> write raw packets to file.
Su packet limit:
```bash
sudo tcpdump -i any -c 100 -w capture.pcap
```
Su interface:
```bash
sudo tcpdump -i tun0 -w thm.pcap
```
Svarbu:
> Kai naudoji `-w`, tcpdump dažniausiai nerodo packetų teksto terminale, nes rašo raw capture į failą.
---

## 12. Skaityti PCAP failą
Skaityti iš PCAP:
```bash
tcpdump -r capture.pcap
```
Su `-nn`:
```bash
tcpdump -nn -r capture.pcap
```
Su verbose:
```bash
tcpdump -nn -vv -r capture.pcap
```
Filtruoti skaitant:
```bash
tcpdump -nn -r capture.pcap host 10.10.10.10
```
Svarbu:
> `-r` skaito PCAP. `-w` rašo PCAP.
Minimalus skirtumas:
```txt
-w = write
-r = read
```
---

## 13. Basic filter syntax
Tcpdump filter rašomas komandos gale.
Pvz:
```bash
sudo tcpdump -i any host 8.8.8.8
```
Bendra struktūra:
```bash
tcpdump [options] [filter expression]
```
Options:
```txt
-i, -c, -nn, -v, -w, -r
```
Filter expression:
```txt
host, src, dst, port, tcp, udp, icmp
```
Svarbu:
> Tcpdump filters naudoja BPF/libpcap syntax. Tai nėra tas pats kaip Wireshark display filter syntax.
---

## 14. Filter pagal host
Traffic į arba iš host:
```bash
sudo tcpdump -i any host 8.8.8.8
```
Tik source host:
```bash
sudo tcpdump -i any src host 8.8.8.8
```
Tik destination host:
```bash
sudo tcpdump -i any dst host 8.8.8.8
```
Trumpiau:
```bash
sudo tcpdump -i any src 8.8.8.8
```
```bash
sudo tcpdump -i any dst 8.8.8.8
```
Svarbu:
> `host` reiškia abi kryptys. `src` ir `dst` susiaurina kryptį.
---

## 15. Filter pagal port
Bet kuris traffic per portą:
```bash
sudo tcpdump -i any port 80
```
Tik source port:
```bash
sudo tcpdump -i any src port 80
```
Tik destination port:
```bash
sudo tcpdump -i any dst port 80
```
Keli portai:
```bash
sudo tcpdump -i any 'port 80 or port 443'
```
Range:
```bash
sudo tcpdump -i any portrange 20-25
```
Svarbu:
> Jei filtrą sudaro `or/and`, geriau dėti į kabutes, kad shell nesugadintų interpretacijos.
---

## 16. Filter pagal protocol
ICMP:
```bash
sudo tcpdump -i any icmp
```
TCP:
```bash
sudo tcpdump -i any tcp
```
UDP:
```bash
sudo tcpdump -i any udp
```
ARP:
```bash
sudo tcpdump -i any arp
```
DNS:
```bash
sudo tcpdump -i any udp port 53
```
HTTP:
```bash
sudo tcpdump -i any tcp port 80
```
HTTPS:
```bash
sudo tcpdump -i any tcp port 443
```
Svarbu:
> `tcp port 80` yra ne tas pats kas “HTTP visada”, bet dažniausiai tai HTTP pagal default portą.
---

## 17. Loginiai operatoriai
AND:
```bash
sudo tcpdump -i any 'host 10.10.10.10 and port 80'
```
OR:
```bash
sudo tcpdump -i any 'port 80 or port 443'
```
NOT:
```bash
sudo tcpdump -i any 'not port 22'
```
Kombinuota:
```bash
sudo tcpdump -i any 'host 10.10.10.10 and (port 80 or port 443)'
```
Svarbu:
> Parentheses dažnai reikia escape’inti arba dėti visą filterį į quotes.
Geras įprotis:
```bash
'filter expression here'
```
---

## 18. Filter pagal network
Pagal subnet/network:
```bash
sudo tcpdump -i any net 192.168.1.0/24
```
Source network:
```bash
sudo tcpdump -i any src net 192.168.1.0/24
```
Destination network:
```bash
sudo tcpdump -i any dst net 192.168.1.0/24
```
Naudinga:
- LAN traffic
- THM subnet
- VPN ranges
Svarbu:
> `net` filtruoja pagal IP network range.
---

## 19. Packet length filters
Filtruoti pagal packet size:
```bash
sudo tcpdump -i any greater 1000
```
Mažesni nei:
```bash
sudo tcpdump -i any less 100
```
Kombinuota:
```bash
sudo tcpdump -i any 'greater 1000 and less 1500'
```
Naudinga:
- rasti didelius packets
- pastebėti file transfer
- THM klausimams
- data exfil clues
Svarbu:
> Packet size nėra exploit įrodymas, bet gali būti geras signalas.
---

## 20. TCP flags
TCP flags svarbūs analizei.
Dažni:
- SYN
- ACK
- FIN
- RST
- PSH
- URG
SYN:
> connection start.
SYN-ACK:
> serveris sutinka connection.
ACK:
> acknowledgement.
FIN:
> graceful close.
RST:
> reset / connection rejected or killed.
Tcpdump gali filtruoti TCP flags per byte offset/bits.
SYN packets:
```bash
sudo tcpdump -i any 'tcp[tcpflags] & tcp-syn != 0'
```
RST packets:
```bash
sudo tcpdump -i any 'tcp[tcpflags] & tcp-rst != 0'
```
FIN packets:
```bash
sudo tcpdump -i any 'tcp[tcpflags] & tcp-fin != 0'
```
SYN be ACK:
```bash
sudo tcpdump -i any 'tcp[tcpflags] & (tcp-syn|tcp-ack) == tcp-syn'
```
Svarbu:
> TCP flag filters atrodo baisiai, bet mintis paprasta: žiūrim į TCP flags bits.
---

## 21. Three-way handshake tcpdump’e
Kai client jungiasi prie serverio per TCP:
```txt
Client -> SYN
Server -> SYN-ACK
Client -> ACK
```
Tcpdump output gali rodyti:
```txt
Flags [S]
Flags [S.]
Flags [.]
```
Reikšmė:
- `[S]` = SYN
- `[S.]` = SYN + ACK
- `[.]` = ACK
Kiti:
- `[F]` = FIN
- `[R]` = RST
- `[P]` = PSH
Svarbu:
> Tcpdump TCP flags trumpiniai leidžia greitai matyti connection būseną.
---

## 22. Display packet contents ASCII
Rodyti packet content ASCII:
```bash
sudo tcpdump -i any -A
```
Naudinga plaintext protokolams:
- HTTP
- FTP
- SMTP
- POP3
- IMAP
Pvz HTTP:
```bash
sudo tcpdump -i any -A 'tcp port 80'
```
Galima matyti:
- GET/POST
- Host header
- User-Agent
- cookies
- form data
Svarbu:
> `-A` naudinga plaintext traffic. HTTPS/SSH turinio normaliai neperskaitysi.
---

## 23. Display packet contents hex + ASCII
Hex + ASCII:
```bash
sudo tcpdump -i any -X
```
Daugiau link-layer + hex:
```bash
sudo tcpdump -i any -XX
```
`-X`:
> rodo packet data hex ir ASCII.
`-XX`:
> rodo ir link-layer header.
Naudinga:
- raw packet inspection
- protocol analysis
- mokymuisi
- kai ASCII nepakanka
Svarbu:
> `-X` yra daugiau data, bet ir daugiau triukšmo. Nenaudok visur automatiškai.
---

## 24. Rodyti MAC/link-layer info
Rodyti link-layer header:
```bash
sudo tcpdump -i any -e
```
Gali matyti:
- source MAC
- destination MAC
- EtherType
Naudinga:
- ARP
- LAN traffic
- MAC/IP ryšiams
- layer 2 analizei
Svarbu:
> Jei nori matyti MAC adresus tcpdump’e, naudok `-e`.
---

## 25. Timestamp control
Nerodyti timestamp:
```bash
sudo tcpdump -t
```
Žmogiškesnis timestamp:
```bash
sudo tcpdump -tttt
```
Rodyti delta tarp packetų:
```bash
sudo tcpdump -ttt
```
Naudinga:
- timeline analizei
- delay troubleshooting
- packet timing
Svarbu:
> Laikas capture analizėje svarbus. Kartais “kas po ko” svarbiau nei pats packet content.
---

## 26. Snap length
Snap length nusako kiek bytes iš kiekvieno packet saugoti.
Default dažnai pakanka, bet kartais gali būti ribota.
Nustatyti pilną packet capture:
```bash
sudo tcpdump -i any -s 0 -w capture.pcap
```
`-s 0`:
> capture’inti full packet, ne apkarpytą.
Svarbu:
> Jei nori vėliau pilnai analizuoti Wireshark’e, naudok `-s 0`.
---

## 27. Buffering / live output
Tcpdump output kartais bufferinamas.
Line-buffered output:
```bash
sudo tcpdump -l
```
Naudinga kai pipe’ini į kitą komandą:
```bash
sudo tcpdump -l -i any tcp | grep "Flags"
```
Svarbu:
> `-l` naudinga, kai tcpdump output nori apdoroti realiu laiku.
---

## 28. Praktiniai capture patterns
Capture THM target traffic:
```bash
sudo tcpdump -i tun0 -nn host MACHINE_IP
```
Capture ping:
```bash
sudo tcpdump -i tun0 -nn icmp
```
Capture HTTP:
```bash
sudo tcpdump -i tun0 -nn -A 'host MACHINE_IP and tcp port 80'
```
Capture DNS:
```bash
sudo tcpdump -i any -nn udp port 53
```
Capture SSH excluding own SSH session:
```bash
sudo tcpdump -i any -nn 'not port 22'
```
Capture į failą:
```bash
sudo tcpdump -i tun0 -nn -s 0 -w thm_capture.pcap host MACHINE_IP
```
Skaityti vėliau:
```bash
tcpdump -nn -r thm_capture.pcap
```
Atidaryti Wireshark:
```bash
wireshark thm_capture.pcap
```
---

## 29. Tcpdump output skaitymas
Pavyzdys:
```txt
IP 192.168.1.10.51514 > 93.184.216.34.80: Flags [S], seq 12345, win 64240
```
Reiškia:
- `IP` = IPv4 packet
- `192.168.1.10.51514` = source IP + source port
- `>` = kryptis
- `93.184.216.34.80` = destination IP + destination port
- `Flags [S]` = SYN
- `seq` = sequence number
- `win` = TCP window size
Svarbu:
> Paskutinis skaičius po IP dažnai yra portas, jei tai TCP/UDP.
Pvz:
```txt
192.168.1.10.51514
```
reiškia:
```txt
IP: 192.168.1.10
Port: 51514
```
---

## 30. DNS capture pavyzdys
Komanda:
```bash
sudo tcpdump -i any -nn udp port 53
```
Tada kitam terminale:
```bash
dig example.com
```
Tcpdump gali rodyti:
```txt
IP 192.168.1.10.53000 > 8.8.8.8.53: 12345+ A? example.com.
IP 8.8.8.8.53 > 192.168.1.10.53000: 12345 1/0/0 A 93.184.216.34
```
Reiškia:
- client klausia DNS A record
- DNS server atsako IP
Svarbu:
> DNS capture labai gerai parodo request/response logiką.
---

## 31. ICMP capture pavyzdys
Terminal 1:
```bash
sudo tcpdump -i any -nn icmp
```
Terminal 2:
```bash
ping -c 4 8.8.8.8
```
Matysi:
```txt
ICMP echo request
ICMP echo reply
```
Svarbu:
> Jei matai request, bet nematai reply, problema gali būti routing/firewall/target/down.
---

## 32. HTTP capture pavyzdys
Terminal 1:
```bash
sudo tcpdump -i any -nn -A 'tcp port 80'
```
Terminal 2:
```bash
curl http://example.com/
```
Gali matyti:
```http
GET / HTTP/1.1
Host: example.com
User-Agent: curl/...
```
Svarbu:
> HTTP yra plaintext, todėl su `-A` gali matyti request headers/body.
HTTPS:
```bash
sudo tcpdump -i any -nn -A 'tcp port 443'
```
matysi šifruotą chaosą, ne HTTP content.
---

## 33. Save now, analyze later
Dažnas geras workflow:
```bash
sudo tcpdump -i tun0 -nn -s 0 -w capture.pcap host MACHINE_IP
```
Tada:
```bash
tcpdump -nn -r capture.pcap
```
Arba:
```bash
wireshark capture.pcap
```
Kodėl gerai:
- terminale neužverčia output
- turi įrodymą analizei
- gali grįžti vėliau
- gali naudoti Wireshark filters
Svarbu:
> Jei neaišku ką žiūri, capture į failą ir atidaryk Wireshark. Nėra gėda naudoti geresnį mikroskopą.
---

## 34. Tcpdump ir legalumas
Tcpdump yra packet capture įrankis.
Svarbu:
> Capture’inti galima tik savo traffic, lab traffic arba traffic, kuriam turi leidimą.
Nenaudoti:
- svetimam Wi-Fi sniffinti
- darbo networke be leidimo
- kitų žmonių credentials gaudyti
- production traffic be authorization
THM/CTF/own lab:
> ok.
Svetimas network:
> ne.
---

## 35. Troubleshooting su tcpdump
Tcpdump padeda atsakyti:
```txt
Ar packet išėjo?
Ar atsakymas grįžo?
Ar DNS query įvyko?
Ar TCP handshake įvyko?
Ar serveris siunčia RST?
Ar ICMP grįžta?
Ar traffic eina per teisingą interface?
Ar port traffic išvis matomas?
```
Pavyzdys: “web neveikia”
1. Tikrinu DNS:
```bash
sudo tcpdump -i any -nn udp port 53
```
2. Tikrinu TCP 80/443:
```bash
sudo tcpdump -i any -nn 'tcp port 80 or tcp port 443'
```
3. Žiūriu ar yra SYN/SYN-ACK:
```bash
sudo tcpdump -i any -nn 'tcp[tcpflags] & (tcp-syn|tcp-ack) != 0'
```
4. Tikrinu ar traffic eina per VPN:
```bash
sudo tcpdump -i tun0 -nn host MACHINE_IP
```
Svarbu:
> Tcpdump dažnai pasako, ar problema tavo mašinoje, kelyje, ar serveryje.
---

## 36. Tcpdump vs Wireshark filters
Tcpdump capture filter syntax:
```bash
host 10.10.10.10 and port 80
```
Wireshark display filter syntax:
```wireshark
ip.addr == 10.10.10.10 && tcp.port == 80
```
Tcpdump:
```bash
tcpdump -i any 'tcp port 80'
```
Wireshark:
```wireshark
tcp.port == 80
```
Svarbu:
> Nemaišyti tcpdump/BPF filter syntax su Wireshark display filters. Čia du skirtingi dialektai.
---

## 37. Dažniausios tcpdump klaidos
Klaida:
```txt
Nėra sudo
```
Sprendimas:
```bash
sudo tcpdump ...
```
Klaida:
```txt
Blogas interface
```
Sprendimas:
```bash
sudo tcpdump -D
ip a
```
Klaida:
```txt
Matai domain/service vardus vietoj IP/portų
```
Sprendimas:
```bash
-nn
```
Klaida:
```txt
Pametei traffic dėl per griežto filtro
```
Sprendimas:
> pradėk plačiau, tada siaurink.
Klaida:
```txt
Naudoji Wireshark filterį tcpdump’e
```
Sprendimas:
> naudok BPF syntax.
Klaida:
```txt
Pamiršai quotes su and/or/parentheses
```
Sprendimas:
```bash
'host 10.10.10.10 and (port 80 or port 443)'
```
---

## 38. Komandos
Interfaces:
```bash
sudo tcpdump -D
ip a
```
Basic capture:
```bash
sudo tcpdump -i any
```
Packet limit:
```bash
sudo tcpdump -i any -c 10
```
No name resolution:
```bash
sudo tcpdump -i any -nn
```
Verbose:
```bash
sudo tcpdump -i any -nn -v
sudo tcpdump -i any -nn -vv
sudo tcpdump -i any -nn -vvv
```
Write PCAP:
```bash
sudo tcpdump -i any -nn -s 0 -w capture.pcap
```
Read PCAP:
```bash
tcpdump -nn -r capture.pcap
```
Host filter:
```bash
sudo tcpdump -i any -nn host 10.10.10.10
```
Source/destination:
```bash
sudo tcpdump -i any -nn src host 10.10.10.10
sudo tcpdump -i any -nn dst host 10.10.10.10
```
Port:
```bash
sudo tcpdump -i any -nn port 80
sudo tcpdump -i any -nn tcp port 80
```
Protocols:
```bash
sudo tcpdump -i any -nn icmp
sudo tcpdump -i any -nn tcp
sudo tcpdump -i any -nn udp
sudo tcpdump -i any -nn arp
```
DNS:
```bash
sudo tcpdump -i any -nn udp port 53
```
HTTP ASCII:
```bash
sudo tcpdump -i any -nn -A 'tcp port 80'
```
Hex + ASCII:
```bash
sudo tcpdump -i any -nn -X 'host 10.10.10.10'
```
MAC/link-layer:
```bash
sudo tcpdump -i any -nn -e
```
Network:
```bash
sudo tcpdump -i any -nn net 192.168.1.0/24
```
Greater/less:
```bash
sudo tcpdump -i any -nn greater 1000
sudo tcpdump -i any -nn less 100
```
TCP SYN:
```bash
sudo tcpdump -i any -nn 'tcp[tcpflags] & tcp-syn != 0'
```
TCP RST:
```bash
sudo tcpdump -i any -nn 'tcp[tcpflags] & tcp-rst != 0'
```
THM VPN target:
```bash
sudo tcpdump -i tun0 -nn host MACHINE_IP
```
---

## 39. Mano paaiškinimas žmonių kalba
Wireshark yra gražus network mikroskopas su ekranu.
Tcpdump yra tas pats principas, bet kaip mechaniko stetoskopas:
> greitai prisidedi prie vamzdžio ir klausai, ar kažkas teka.
`-i`:
> kur klausausi?
`-c`:
> kiek packetų paimu?
`-nn`:
> rodyk tikrus IP/portus, ne gražius vardus.
`-w`:
> įrašyk į failą.
`-r`:
> perskaityk iš failo.
`host`:
> rodyk traffic su šitu hostu.
`port`:
> rodyk traffic su šitu portu.
`-A`:
> parodyk tekstą, jei jis plaintext.
`-X`:
> parodyk raw hex + ASCII.
Svarbiausia:
> Tcpdump nėra skirtas grožiui. Jis skirtas greitai pamatyti faktą: packet išėjo ar neišėjo, grįžo ar negrįžo.
---

## 40. Ką būtina atsiminti
- tcpdump yra command-line packet capture tool.
- tcpdump naudoja libpcap.
- `-D` parodo interfaces.
- `-i` pasirenka interface.
- `-c` riboja packetų kiekį.
- `-n` išjungia hostname resolution.
- `-nn` išjungia hostname ir port name resolution.
- `-v`, `-vv`, `-vvv` didina verbosity.
- `-w` rašo PCAP.
- `-r` skaito PCAP.
- `-A` rodo ASCII.
- `-X` rodo hex + ASCII.
- `-e` rodo link-layer/MAC info.
- `-s 0` capture’ina full packet.
- `host`, `src`, `dst`, `port`, `net` yra pagrindiniai filters.
- `and`, `or`, `not` kombinuoja filters.
- Tcpdump filters nėra Wireshark display filters.
- `Ctrl+C` stabdo capture.
- HTTP/FTP/SMTP/POP3 plaintext gali matytis su `-A`.
- HTTPS/SSH content šifruotas, bet metadata matoma.
- Capture’inti galima tik legalų traffic.
---

## 41. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- sudėtingus BPF byte offsets beyond TCP flags
- custom packet carving
- performance tuning high-volume capture
- ring buffers
- remote packet capture
- tcpdump su IDS pipeline
- advanced tcp analysis
- packet fragmentation gylį
- pcap-ng internals
- libpcap programming
Dabar svarbiausia:
> Mokėti pasirinkti interface, capture’inti traffic, naudoti basic filters, įrašyti/skaityti PCAP ir parodyti plaintext/hex output.
---

## 42. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kas yra tcpdump?
2. Kokia biblioteka susijusi su tcpdump?
3. Kuo tcpdump skiriasi nuo Wireshark?
4. Kodėl tcpdump dažnai reikia `sudo`?
5. Kaip parodyti interfaces?
6. Ką daro `-i`?
7. Ką daro `-c`?
8. Ką daro `-nn`?
9. Kuo skiriasi `-w` ir `-r`?
10. Ką daro `-v`, `-vv`, `-vvv`?
11. Kaip capture’inti tik ICMP?
12. Kaip capture’inti tik DNS?
13. Kaip capture’inti tik HTTP?
14. Kaip filtruoti pagal hostą?
15. Kaip filtruoti pagal destination hostą?
16. Kaip filtruoti pagal portą?
17. Kaip filtruoti pagal subnetą?
18. Ką daro `and`, `or`, `not`?
19. Kodėl verta naudoti quotes aplink sudėtingą filterį?
20. Kaip įrašyti capture į `.pcap`?
21. Kaip skaityti `.pcap`?
22. Ką daro `-A`?
23. Ką daro `-X`?
24. Ką daro `-e`?
25. Ką daro `-s 0`?
26. Kaip atrodo TCP SYN flag tcpdump outpute?
27. Ką reiškia `Flags [S.]`?
28. Kodėl HTTPS turinio nepamatysi kaip normal text?
29. Kuo tcpdump filter syntax skiriasi nuo Wireshark display filter syntax?
30. Kada naudotum tcpdump vietoj Wireshark?
---

## 43. Mano verdiktas
Šitas roomas yra labai praktiškas, nes duoda realų terminalinį packet analysis įrankį.
Statusas:
```txt
Status: Foundation / CLI Packet Capture
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš SOC traffic analysis, THM troubleshooting, Wireshark gilesnę analizę, VPN debug ir server-side network troubleshooting
```
Svarbiausia mintis:
> Wireshark padeda patogiai analizuoti, bet tcpdump leidžia greitai capture’inti ir tikrinti traffic ten, kur GUI nėra.
Core mental model:
```txt
-i = interface
-c = count
-nn = no names, real IP/ports
-w = write PCAP
-r = read PCAP
host/port/protocol = ką filtruoti
-A/-X = kaip rodyti packet content
```
Jeigu supranti tcpdump, tu jau gali terminale atsakyti į vieną svarbiausių troubleshooting klausimų:
> “Ar traffic išvis eina?”
