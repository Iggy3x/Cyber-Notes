# THM – Wireshark: The Basics

## 1. Room esmė
Šitas roomas aiškina Wireshark pagrindus.
Pagrindinė mintis:
> Wireshark leidžia matyti network traffic packet lygyje: kas su kuo kalba, kokiu protokolu, per kokius portus ir kokia data keliauja.
Šitas roomas dengia:
- Wireshark interface
- PCAP files
- packet capture
- packet filtering
- packet dissection
- ARP traffic
- ICMP traffic
- TCP traffic
- DNS traffic
- HTTP traffic
- HTTPS traffic
- basic exploit PCAP analysis
Cybersecurity kontekste Wireshark svarbus, nes:
- leidžia analizuoti realų traffic
- padeda troubleshootinti network problemas
- padeda matyti plaintext credentials/protocols
- padeda suprasti OSI/TCP-IP sluoksnius praktiškai
- padeda incident response ir SOC analizėje
- parodo, kas realiai vyko networke, ne tik ką “turėjo” vykti
---

## 2. Kas yra Wireshark?
Wireshark yra packet capture ir packet analysis įrankis.
Paprastai:
> Wireshark = mikroskopas network traffic’ui.
Jis gali:
- capture’inti live traffic
- atidaryti PCAP failus
- rodyti packets pagal laiką
- dissect’inti protokolus
- filtruoti traffic
- sekti TCP streams
- matyti DNS/HTTP/ARP/ICMP/TCP detales
- exportinti objektus/failus iš capture
PCAP reiškia:
> Packet Capture
PCAP/PCAPNG failas:
> išsaugotas network traffic įrašas.
Svarbu:
> Wireshark pats “nehackina”. Jis leidžia matyti ir analizuoti traffic.
---

## 3. Kodėl Wireshark svarbus?
Wireshark padeda atsakyti į klausimus:
- Kas kalbėjo su kuo?
- Koks buvo source IP?
- Koks buvo destination IP?
- Kokie portai naudoti?
- Koks protokolas?
- Ar buvo DNS request?
- Ar buvo HTTP request?
- Ar traffic plaintext ar encrypted?
- Ar yra failed TCP connections?
- Ar matosi credentials?
- Ar yra suspicious IP/domain?
- Ar yra keisti packet patterns?
Security praktikoje Wireshark naudojamas:
- troubleshooting
- malware traffic analysis
- phishing investigation
- credential leak detection
- protocol analysis
- incident response
- CTF/THM PCAP užduotys
- learning networking deeply
Svarbi mintis:
> Jei OSI yra teorija, Wireshark yra vieta, kur OSI pamatai savo akimis.
---

## 4. Wireshark interface
Pagrindinės Wireshark dalys:
- Packet List pane
- Packet Details pane
- Packet Bytes pane
- Display Filter bar
- Menu / toolbar
Packet List pane:
> rodo packetus eilutėmis.
Dažnos kolonos:
- No.
- Time
- Source
- Destination
- Protocol
- Length
- Info
Packet Details pane:
> rodo išskleistus packet sluoksnius/protokolus.
Pvz:
```txt
Frame
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
Hypertext Transfer Protocol
```
Packet Bytes pane:
> rodo raw bytes/hex + ASCII.
Svarbu:
> Packet list parodo “kas vyksta”, packet details parodo “kaip tai supakuota”, packet bytes parodo raw data.
---

## 5. Packet dissection
Packet dissection reiškia packet išskaidymą į sluoksnius/protokolus.
Paprastai:
> Wireshark paima raw packet ir parodo jį žmogui suprantamomis dalimis.
Pvz HTTP packet gali turėti:
```txt
Frame
Ethernet II
IPv4
TCP
HTTP
```
Tai atitinka:
```txt
L2 -> L3 -> L4 -> L7
```
Frame:
> bendras capture frame info.
Ethernet II:
> Layer 2 MAC info.
IPv4:
> Layer 3 IP info.
TCP:
> Layer 4 ports/flags/sequence.
HTTP:
> Layer 7 request/response data.
Svarbu:
> Wireshark labai gerai sujungia networking theory su praktika.
---

## 6. Capture vs open PCAP
Wireshark gali:
1. Capture’inti live traffic
2. Atidaryti jau išsaugotą PCAP
Live capture:
> realiu laiku įrašai traffic iš interface.
Open PCAP:
> analizuoji jau paruoštą traffic failą.
THM dažnai naudoja PCAP failus, nes:
- saugiau
- repeatable
- visi gauna tą patį traffic
- galima fokusuotis į analizę, ne capture setup
Svarbu:
> Live capture gali matyti jautrų traffic. Analizuok tik savo/lab/leidžiamą networką.
---

## 7. Network interfaces
Prieš capture reikia pasirinkti interface.
Dažni interface’ai:
- Ethernet
- Wi-Fi
- loopback
- tun0 / VPN
- any
Linux THM/VPN kontekste:
```txt
tun0
```
dažnai yra VPN interface.
Wireshark rodys traffic tik iš pasirinkto interface.
Jei pasirinksi blogą interface:
> gali nematyti reikalingo traffic.
Praktika:
- web traffic per Wi-Fi -> rinkis Wi-Fi interface
- THM VPN traffic -> tikrink tun0
- viskas vienu metu Linux -> galima naudoti any, jei palaikoma
---

## 8. Capture filters vs display filters
Yra du filtrų tipai:
- capture filters
- display filters
Capture filter:
> nusprendžia, kas bus įrašyta capture metu.
Display filter:
> paslepia/parodo jau capture’intus packets ekrane.
Svarbiausias skirtumas:
> Capture filter mažina ką surenki. Display filter tik filtruoja ką matai.
Jeigu capture filter per griežtas:
> prarasi traffic ir vėliau jo nebepamatysi.
Display filter saugesnis mokymuisi:
> traffic lieka faile, tu tik keiti vaizdą.
Pavyzdys display filter:
```wireshark
http
```
Pavyzdys capture filter:
```bash
tcp port 80
```
Svarbu:
> Pradžioje daugiau naudok display filters. Capture filters naudok atsargiai.
---

## 9. Display filters
Display filter rašomas viršuje filter bar’e.
Dažni filtrai:
```wireshark
arp
icmp
tcp
udp
dns
http
tls
```
Pagal IP:
```wireshark
ip.addr == 192.168.1.10
```
Tik source IP:
```wireshark
ip.src == 192.168.1.10
```
Tik destination IP:
```wireshark
ip.dst == 192.168.1.10
```
Pagal TCP portą:
```wireshark
tcp.port == 80
```
Source TCP port:
```wireshark
tcp.srcport == 443
```
Destination TCP port:
```wireshark
tcp.dstport == 443
```
Pagal UDP portą:
```wireshark
udp.port == 53
```
Kombinuoti:
```wireshark
ip.addr == 10.10.10.10 && tcp.port == 80
```
Arba:
```wireshark
dns || http
```
Svarbu:
> Wireshark display filters nėra tas pats kas tcpdump/capture filter syntax.
---

## 10. Naudingi filter operatoriai
Lygu:
```wireshark
==
```
Nelygu:
```wireshark
!=
```
AND:
```wireshark
&&
```
OR:
```wireshark
||
```
NOT:
```wireshark
!
```
Contains:
```wireshark
contains
```
Pvz:
```wireshark
http contains "password"
```
Matches regex:
```wireshark
matches
```
Pvz:
```wireshark
http.request.uri matches "login|admin"
```
Svarbu:
> Pradžioje užtenka `==`, `&&`, `||`, `contains`.
---

## 11. ARP traffic
ARP reiškia:
> Address Resolution Protocol
ARP atsako į klausimą:
> Kokį MAC adresą turi šitas IP lokaliame tinkle?
Wireshark filter:
```wireshark
arp
```
ARP request atrodo kaip:
```txt
Who has 192.168.1.1? Tell 192.168.1.10
```
ARP reply:
```txt
192.168.1.1 is at aa:bb:cc:dd:ee:ff
```
ARP veikia Layer 2/Layer 3 riboje:
- IP žinomas
- reikia MAC
Svarbu:
> ARP request yra broadcast. ARP reply dažniausiai unicast.
Security mintis:
> ARP spoofing/poisoning gali būti matomas kaip keisti IP -> MAC mapping pokyčiai.
---

## 12. ICMP traffic
ICMP reiškia:
> Internet Control Message Protocol
Dažnai naudojamas ping.
Wireshark filter:
```wireshark
icmp
```
Ping naudoja:
- ICMP Echo Request
- ICMP Echo Reply
Echo Request:
> “ar tu gyvas?”
Echo Reply:
> “taip, gyvas.”
Wireshark’e matysi:
- source IP
- destination IP
- type/code
- sequence number
- data
Svarbu:
> Jei matai request be reply, gali būti firewall, routing problema arba host down.
---

## 13. TCP traffic
TCP reiškia:
> Transmission Control Protocol
Wireshark filter:
```wireshark
tcp
```
TCP yra:
- connection-oriented
- reliable
- ordered
- uses ports
- uses flags
Svarbūs TCP flags:
- SYN
- SYN-ACK
- ACK
- FIN
- RST
Three-way handshake:
```txt
Client -> SYN -> Server
Client <- SYN-ACK <- Server
Client -> ACK -> Server
```
Wireshark filtrai:
```wireshark
tcp.flags.syn == 1
```
Tik SYN be ACK:
```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```
Reset packets:
```wireshark
tcp.flags.reset == 1
```
Svarbu:
> TCP handshake parodo, ar connection užsimezgė.
Jei matai daug SYN be SYN-ACK:
> host/port gali būti nepasiekiamas arba firewall filtruoja.
Jei matai RST:
> connection atmesta/resetinta.
---

## 14. TCP streams
TCP stream leidžia matyti visą pokalbį tarp client ir server.
Wireshark:
```txt
Right click packet -> Follow -> TCP Stream
```
Naudinga:
- matyti HTTP request/response
- matyti plaintext credentials
- matyti failo turinį plaintext protokoluose
- suprasti sesijos eigą
Svarbu:
> Follow TCP Stream yra vienas svarbiausių Wireshark veiksmų beginner analizėje.
Bet:
> Jei traffic encrypted, stream turinys bus šifruotas ir žmogui neperskaitomas.
---

## 15. DNS traffic
DNS reiškia:
> Domain Name System
Wireshark filter:
```wireshark
dns
```
DNS dažniausiai naudoja:
```txt
UDP 53
```
DNS query:
> client klausia domain IP.
DNS response:
> serveris atsako records.
Dažni records:
- A
- AAAA
- CNAME
- MX
- TXT
- NS
Wireshark’e žiūrėti:
- queried domain
- response IP
- query type
- DNS server IP
- response code
Svarbu:
> DNS traffic gali parodyti, kokius domainus hostas bandė pasiekti.
Security mintis:
> Keisti/random domainai gali būti malware C2, phishing, tunneling ar beaconing clue.
---

## 16. HTTP traffic
HTTP yra plaintext web protocol.
Wireshark filter:
```wireshark
http
```
HTTP default:
```txt
TCP 80
```
Wireshark’e gali matyti:
- GET request
- POST request
- Host header
- User-Agent
- URI/path
- status code
- cookies
- form data
- response content
HTTP request pavyzdys:
```http
GET /index.html HTTP/1.1
Host: example.com
```
HTTP POST gali turėti:
```txt
username=admin&password=test
```
Svarbu:
> HTTP plaintext traffic gali atskleisti credentials ir sensitive data.
Naudingi filtrai:
```wireshark
http.request
```
```wireshark
http.request.method == "GET"
```
```wireshark
http.request.method == "POST"
```
```wireshark
http.host contains "example"
```
```wireshark
http contains "password"
```
---

## 17. HTTPS / TLS traffic
HTTPS = HTTP over TLS.
Default:
```txt
TCP 443
```
Wireshark filter:
```wireshark
tls
```
arba:
```wireshark
tcp.port == 443
```
HTTPS traffic turinys šifruotas.
Matysi:
- IPs
- ports
- TLS handshake
- SNI kartais
- certificate info kartais
- packet sizes/timing
Nematysi:
- HTTP path
- cookies
- POST body
- HTML content
- credentials
nebent turi keys/decryption setup.
Svarbu:
> HTTPS slepia content, bet metadata vis tiek matoma.
Security mintis:
> Encrypted traffic analizėje svarbu metadata: domain/SNI, certificate, timing, size, destination IP, JA3/advanced fingerprints vėliau.
---

## 18. SNI Wireshark’e
SNI reiškia:
> Server Name Indication
TLS handshake metu client gali nurodyti hostname.
Wireshark filter:
```wireshark
tls.handshake.extensions_server_name
```
Arba ieškoti:
```wireshark
tls contains "example.com"
```
SNI naudinga, nes:
> Net jei HTTPS content encrypted, kartais gali matyti kokį hostname client bandė pasiekti.
Svarbu:
> Ne visada SNI bus matomas ateityje dėl encrypted client hello/modernių privatumo mechanizmų, bet beginner lygyje dažnai matomas.
---

## 19. Packet colours
Wireshark naudoja colouring rules.
Spalvos padeda greitai matyti traffic tipus.
Pvz dažnai:
- TCP traffic viena spalva
- HTTP kita
- errors/warnings kita
- retransmissions/RESET gali išsiskirti
Svarbu:
> Spalvos yra pagalbos priemonė, ne galutinis verdiktas. Visada tikrink packet details.
---

## 20. Statistics
Wireshark turi Statistics meniu.
Naudinga:
```txt
Statistics -> Protocol Hierarchy
Statistics -> Conversations
Statistics -> Endpoints
Statistics -> I/O Graphs
```
Protocol Hierarchy:
> kokie protokolai yra capture’e ir kiek jų.
Conversations:
> kas su kuo kalbėjo.
Endpoints:
> kokie IP/MAC/ports dalyvavo.
I/O Graphs:
> traffic kiekis per laiką.
Praktinė mintis:
> Jei PCAP didelis, pirma žiūrėk Statistics. Neik iškart aklai per 50k packetų kaip archeologas su šaukštu.
---

## 21. Expert Information
Wireshark turi Expert Information.
Jis parodo galimas problemas:
- warnings
- errors
- retransmissions
- malformed packets
- connection resets
Svarbu:
> Expert Info nėra automatinė tiesa. Tai hint’ai, kur verta pažiūrėti.
Naudinga:
```txt
Analyze -> Expert Information
```
Security/troubleshooting mintis:
> Expert Info gali greitai parodyti keistus TCP resetus, retransmissions, protocol errors.
---

## 22. Export objects
Wireshark gali exportinti objektus iš kai kurių protokolų.
Pvz HTTP:
```txt
File -> Export Objects -> HTTP
```
Naudinga:
- ištraukti images
- parsisiųstus failus
- documents
- payloads
- suspicious downloads
Svarbu:
> Exportuotus failus iš suspicious PCAP analizuok atsargiai. Neatidarinėk random executable ant pagrindinės sistemos.
---

## 23. Find Packet
Ieškoti packetuose:
```txt
Ctrl + F
```
Galima ieškoti pagal:
- display filter
- hex value
- string
- packet bytes
Naudinga ieškoti:
```txt
password
admin
login
flag
token
GET
POST
User-Agent
```
Svarbu:
> PCAP analizėje `Ctrl+F` + geras filteris dažnai išgelbsti nuo beprasmio scrollinimo.
---

## 24. Name resolution
Wireshark gali bandyti versti IP į vardus.
Tai vadinama name resolution.
Kartais naudinga, bet gali klaidinti arba lėtinti.
Tipai:
- MAC name resolution
- network name resolution
- transport name resolution
Svarbu:
> Analizėje dažnai geriau matyti raw IP/ports, kad nebūtum apgautas gražių vardų.
Jei reikia:
```txt
View -> Name Resolution
```
---

## 25. Capture legalumas
Wireshark yra galingas, bet jautrus įrankis.
Svarbu:
> Capture’inti galima tik savo traffic, lab traffic arba traffic, kuriam turi leidimą.
Nenaudoti:
- svetimam Wi-Fi sniffinti
- darbo networke be leidimo
- kitų žmonių credentials gaudyti
- production traffic be authorization
THM/CTF/own lab:
> saugi vieta mokytis.
---

## 26. Basic analysis workflow
Kai atidarai PCAP:
1. Pažiūrėk bendrą vaizdą:
```txt
Statistics -> Protocol Hierarchy
```
2. Pažiūrėk endpoints:
```txt
Statistics -> Endpoints
```
3. Pažiūrėk conversations:
```txt
Statistics -> Conversations
```
4. Filtruok pagal protokolus:
```wireshark
dns
http
tcp
icmp
arp
```
5. Ieškok įdomių requestų:
```wireshark
http.request
dns
```
6. Sek TCP stream:
```txt
Right click -> Follow -> TCP Stream
```
7. Ieškok sensitive strings:
```txt
Ctrl + F -> password/admin/token/flag
```
8. Užsirašyk:
```txt
source IP
destination IP
protocol
ports
domain
URI
credentials
files
errors
timeline
```
Svarbu:
> Pirma bendras vaizdas, tada filtrai, tada detalės.
---

## 27. ARP analysis checklist
Filtras:
```wireshark
arp
```
Žiūrėti:
- Who has?
- Is at?
- ar daug ARP requestų?
- ar tas pats IP turi kelis MAC?
- ar gateway MAC keičiasi?
Galimos mintys:
- normalus ARP discovery
- host ieško gateway
- ARP spoofing požymiai
- network misconfig
Svarbu:
> ARP yra lokalus. Jei matai ARP, kalba vyksta tame pačiame local segment.
---

## 28. ICMP analysis checklist
Filtras:
```wireshark
icmp
```
Žiūrėti:
- Echo Request
- Echo Reply
- Time Exceeded
- Destination Unreachable
- source/destination IP
- sequence numbers
Galimos mintys:
- ping test
- traceroute/tracert
- host unreachable
- routing/firewall problemos
Svarbu:
> ICMP nėra tik ping. Jis naudojamas ir klaidų pranešimams.
---

## 29. TCP analysis checklist
Filtras:
```wireshark
tcp
```
Žiūrėti:
- ar yra SYN -> SYN-ACK -> ACK
- ar yra RST
- ar daug retransmissions
- kokie source/destination ports
- ar connection normaliai užsidaro FIN/ACK
Naudingi filtrai:
```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```
```wireshark
tcp.flags.reset == 1
```
```wireshark
tcp.analysis.retransmission
```
Svarbu:
> TCP handshake yra connection health indikatorius.
---

## 30. DNS analysis checklist
Filtras:
```wireshark
dns
```
Žiūrėti:
- queried domains
- query type A/AAAA/MX/TXT
- response IP
- NXDOMAIN
- DNS server
- suspicious/random domains
Naudingi filtrai:
```wireshark
dns.qry.name contains "example"
```
```wireshark
dns.flags.rcode != 0
```
Svarbu:
> DNS dažnai parodo, kur hostas bandė jungtis dar prieš pačią connection.
---

## 31. HTTP analysis checklist
Filtras:
```wireshark
http
```
Žiūrėti:
- Host
- GET/POST
- URI/path
- User-Agent
- Referer
- Cookies
- Status code
- form data
- file downloads
Naudingi filtrai:
```wireshark
http.request
```
```wireshark
http.request.method == "POST"
```
```wireshark
http.response.code == 200
```
```wireshark
http contains "password"
```
Follow stream:
```txt
Right click -> Follow -> TCP Stream
```
Svarbu:
> HTTP plaintext yra labai informatyvus ir labai nesaugus, jei siunčia sensitive data.
---

## 32. HTTPS/TLS analysis checklist
Filtras:
```wireshark
tls
```
Žiūrėti:
- Client Hello
- Server Hello
- SNI
- certificate info
- TLS version
- cipher suite
- source/destination IP
- port 443
Naudingi filtrai:
```wireshark
tcp.port == 443
```
```wireshark
tls.handshake.type == 1
```
```wireshark
tls.handshake.extensions_server_name
```
Svarbu:
> HTTPS turinio nematysi be decryption keys, bet vis tiek gali analizuoti metadata.
---

## 33. Wireshark ir OSI mental model
Wireshark packet details dažnai atrodo taip:
```txt
Frame
Ethernet II
IPv4
TCP
HTTP
```
Tai reiškia:
```txt
L2 Data Link = Ethernet/MAC/frame
L3 Network = IP/packet
L4 Transport = TCP/UDP/ports
L7 Application = HTTP/DNS/etc
```
Core:
```txt
MAC = local delivery
IP = final host/network routing
Port = service/process
Protocol = application language
```
Svarbu:
> Kai Wireshark’e matai frame -> IP -> TCP -> HTTP, tu realiai matai encapsulation išpakuotą akimis.
---

## 34. Komandos šalia Wireshark
Tcpdump capture į failą:
```bash
sudo tcpdump -i any -w capture.pcap
```
Tcpdump skaityti failą:
```bash
tcpdump -r capture.pcap
```
Tcpdump HTTP ASCII:
```bash
sudo tcpdump -i any port 80 -A
```
Tcpdump DNS:
```bash
sudo tcpdump -i any port 53
```
Tshark basic:
```bash
tshark -r capture.pcap
```
Tshark filtrai:
```bash
tshark -r capture.pcap -Y "http"
```
Atidaryti PCAP su Wireshark:
```bash
wireshark capture.pcap
```
Svarbu:
> Wireshark yra GUI analizė. tcpdump/tshark yra CLI capture/analysis. Visi jie tame pačiame pasaulyje.
---

## 35. Praktinė vertė white hat mokymuisi
Wireshark reikalingas:
- networking supratimui
- traffic analysis
- SOC/blue team
- malware traffic basics
- incident response
- web testing
- protocol analysis
- troubleshooting THM VPN/network issues
- suprasti kas vyksta po tools output
Praktinis mindset:
```txt
Koks source?
Koks destination?
Koks protocol?
Koks portas?
Ar connection įvyko?
Ar yra DNS prieš connection?
Ar traffic plaintext?
Ar matosi credentials?
Ar yra keistas domain/IP?
Ar yra errors/retransmissions?
```
Svarbu:
> Wireshark moko ne spėlioti, o žiūrėti į faktus packet lygyje.
---

## 36. Mano paaiškinimas žmonių kalba
Wireshark yra kaip CCTV kamerų sistema networke.
Ne “kažkas neveikia”, o:
> pažiūrim, kas realiai vyko.
ARP:
> kas turi šitą MAC?
ICMP:
> ar tu gyvas?
TCP:
> susitariam dėl connection.
DNS:
> koks IP šitam vardui?
HTTP:
> duok puslapį / štai puslapis.
HTTPS:
> matau, kad kalbat, bet turinys užrakintas.
Follow TCP Stream:
> parodyk visą pokalbį vienoje vietoje.
Display filter:
> parodyk tik tą traffic, kuris man dabar rūpi.
Svarbiausia:
> Wireshark nėra magija. Jis tiesiog parodo, ką networkas jau pasakė.
---

## 37. Ką būtina atsiminti
- Wireshark analizuoja packets ir PCAP failus.
- PCAP = packet capture.
- Packet List rodo packetų sąrašą.
- Packet Details rodo išskleistus protocol layers.
- Packet Bytes rodo raw bytes.
- Capture filter nusprendžia ką įrašysi.
- Display filter nusprendžia ką matai.
- Pradžioje saugiau naudoti display filters.
- ARP filter: `arp`.
- ICMP filter: `icmp`.
- TCP filter: `tcp`.
- DNS filter: `dns`.
- HTTP filter: `http`.
- TLS/HTTPS filter: `tls` arba `tcp.port == 443`.
- Follow TCP Stream parodo visą TCP conversation.
- HTTP plaintext gali rodyti credentials.
- HTTPS slepia content, bet metadata matoma.
- DNS dažnai parodo domains prieš connection.
- TCP handshake rodo connection užmezgimą.
- Statistics padeda gauti bendrą PCAP vaizdą.
- Expert Info yra hints, ne absoliuti tiesa.
- Capture’inti galima tik legalų/leidžiamą traffic.
---

## 38. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- TCP window scaling gylį
- TLS decryption setup
- JA3/JA4 fingerprinting
- packet reassembly internals
- display filter regex gylį
- custom dissectors
- Lua dissectors
- advanced malware PCAP analysis
- Zeek/Suricata integration
- pcap carving advanced
- Wireshark profile tuning
Dabar svarbiausia:
> Mokėti atsidaryti PCAP, suprasti interface, naudoti basic filters, sekti TCP stream ir atpažinti ARP/ICMP/TCP/DNS/HTTP/HTTPS traffic.
---

## 39. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kas yra Wireshark?
2. Kas yra PCAP?
3. Kuo skiriasi Packet List, Packet Details ir Packet Bytes?
4. Kuo capture filter skiriasi nuo display filter?
5. Kodėl display filter saugesnis mokymuisi?
6. Koks filter rodo ARP?
7. Koks filter rodo ICMP?
8. Koks filter rodo DNS?
9. Koks filter rodo HTTP?
10. Koks filter rodo TLS/HTTPS?
11. Kaip filtruoti pagal IP adresą?
12. Kaip filtruoti pagal TCP portą?
13. Ką daro `Follow TCP Stream`?
14. Ką ARP request reiškia Wireshark’e?
15. Kuo ICMP Echo Request skiriasi nuo Echo Reply?
16. Kokie trys packetai sudaro TCP handshake?
17. Ką gali reikšti TCP RST?
18. Kodėl DNS traffic svarbus analizėje?
19. Ką gali matyti HTTP traffic’e?
20. Kodėl HTTPS turinio paprastai nematai?
21. Kas yra SNI?
22. Kam naudinga Statistics -> Protocol Hierarchy?
23. Kam naudinga Statistics -> Conversations?
24. Kodėl Expert Info nėra galutinė tiesa?
25. Kodėl negalima capture’inti svetimo traffic be leidimo?
26. Kaip Wireshark padeda suprasti OSI?
27. Ką pirmiausia darytum atsidaręs didelį PCAP?
28. Kaip ieškotum žodžio `password` PCAP’e?
29. Ką reiškia, jei matai HTTP POST su credentials?
30. Kodėl Wireshark yra svarbus SOC/IR mokymuisi?
---

## 40. Mano verdiktas
Šitas roomas yra vienas svarbiausių momentų, kur networking teorija pradeda tapti realiu vaizdu.
Statusas:
```txt
Status: Foundation / Packet Analysis
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš Packet Operations, Traffic Analysis, SOC rooms, tcpdump/tshark ir malware traffic basics
```
Svarbiausia mintis:
> Wireshark leidžia matyti ne “website neveikia” ar “ping neveikia”, o konkrečius packetus: kas išėjo, kas grįžo, koks protokolas, kokie portai, kokie headers, kokios klaidos.
Core mental model:
```txt
Frame -> Ethernet/MAC -> IP -> TCP/UDP -> Application protocol
```
Jeigu supranti:
- kaip filtruoti traffic
- kaip sekti TCP stream
- kaip atpažinti ARP/ICMP/DNS/HTTP/TLS
- kaip skaityti packet details
tada networking nebėra abstrakti teorija. Tai tampa matoma mechanika.
