#   Networking Essentials #

*Objectives* 

* DHCP - Dynamic Host configuration protocol
* ARP - Address resolution protocol
* NAT - Network address translation
* ICMP - Internet control , message protocol
	* Ping
	* traceroute/tracert
* Routing basics

-------------------------------------------

# DHCP - Duok man network settings  #

Kai irenginys jungiasi prie tinklo jam reikia:
* IP adress
* Subnet Mask
* Default gateway (router)
* DNS server
Visa tai automatiskai suteikia DHCP.

# Pagrindai # 

* Application layer protokolas
* Naudoja UDP
	* DHCP Serveris klausosi ant  UDP 67 (server listens)
	* DHCP Serveris klausosiant  UDP 68 (client sends)
* Dauguma irenginiu naudoja DHCP (telefonai,laptopai) pagal nutylejima

*SVARBU*:
* Klientas siuncia is UDP 68 i serverio UDP 67
* Serveris atdako is UDP 67 i kliento UDP 68

# DORA procesas #

1. Discover
Klientas broadcastina <DHCPDISCOVER> - ar cia yra DHCP serveris?
2. Offer
Serveris atsako <DHCPOFFER> - siulo IP adresa + nustatymus
3. Request
Klientas siuncia <DHCPREQUEST> - Priimu sita ip
4. Acknowledge
Serveris patvirtina su <DHCPACK> - IP oficialiai priskirtas

Jeigu DHCP neveikia - nera IP, nera interneto, nors Wi-Fi gali atrodyti "prisijunges"

DORA:
1. DHCPDISCOVER – client broadcast: ar yra DHCP serveris?
2. DHCPOFFER – serveris siūlo IP + settings
3. DHCPREQUEST – client pasirenka/prašo siūlomo IP
4. DHCPACK – serveris patvirtina lease
DHCP duoda ne tik IP. Dažniausiai duoda:
- IP address
- subnet mask
- default gateway
- DNS server
- lease time

------------------------------------------------------------------

# ARP - Layer 3 --> Layer 2 tiltas #
*# ARP – Layer 3 IP į Layer 2 MAC vertimas

ARP atsako į klausimą:
> Kokį MAC adresą turi šitas IP lokaliame tinkle?

ARP reikalingas, nes IP packet turi būti įdėtas į Ethernet/Wi-Fi frame, o frame reikia destination MAC.

*Kam reikalingas ARP?* 
Ip adresai ( layer 3) neveikia tiesiogiai ethernet tinkle
Kad paketas butu isiustas fiziskai, reikia MAC adreso (layer 2) 

ARP atsako i klausima:
Kuris MAC adresas turi sita IP?

Kaip veikia:
1. Irenginys siuncia *ARP request* (broadcast)
	* Kas turi IP 192.168.1.1.?
2. Tikras savininkas atsako *ARP Reply*:
	* "CIa mano mac: AA:BB:CC:DD:EE:FF
3. Rezultatas irasomas i *ARP CACHE*

!SVARBU ZINOTI! 
* ARP veikia tik lokaliam tinkle
* Neperzengia routeriu
* Dazna ataku vieta *ARP spoofing/poisoning*

ARP request yra broadcast.
ARP reply dažniausiai yra unicast.
ARP veikia tik lokaliame network segmente. Jei destination IP yra kitame networke, tavo įrenginys per ARP ieško ne galutinio serverio MAC, o default gateway/routerio MAC.

---------------------------------------------------------------------

# ICMP - tinklo diagnostika   #

Internet control message protocol naudojamas:
* Klaidu pranesimams
* tinklo diagnostikai (network troubleshooting)
* conectivity tikrinimui


*PING*

* Naudojama *ICMP Echo Request/ Echo Reply*
* Tikrina:
	* Ar target gyvas (atsako) 
	* RTT (round-trip time)
Jeigu ping neatsako tai nebutinai reiskia kad hostas down, gali buti blokuojamas ICMP firewall/policy

*traceroute/tracert* 
* Linux: <traceroute>
* Windows: <tracert>

Naudoja *TTL* (time to live) mechanizma:
* Kiekvienas routeris sumazina *TTL* -1
* kai *TTL* = 0 --> pakentas numetamas
* Routeris siuncia *ICMP Time Exceeded (type 11)*
Taip suzinomas kelias iki *target* -hop po hopo

Traceroute veikia manipuliuodamas TTL:
- siunčia packetus su TTL 1, 2, 3...
- kiekvienas routeris sumažina TTL per 1
- kai TTL tampa 0, routeris numeta packetą
- routeris grąžina ICMP Time Exceeded
- taip matomas kelias hop-by-hop
----------------------------------------------------------------------------

# Routing kaip paketai randa kelia # 

Routing = sprendimas kur siusti paketa toliau pagal destination IP
Routeris ziuri i:
* destination IP
* routing table
* longest prefix match
* next hop
* inteface
Jaigu destination yra local networke:
*siunciama teisiai tam gotstui per ARP/MAC

Jeigu destination yra kitame networke:
* siunciama i default gateway/routeri

Default gateway:
* routeris, kuriam hostas siuncia traffic, kai destination nera jo kokaliame subnete
Routing table = taisyklių lentelė, pagal kurią įrenginys nusprendžia kur siųsti packetą.

# Pagrindiniai Routing protokolai #

*OSPF*-Open shortest path first
* Link - state protokolas
* Routeriai dalinasi visa topologija
* Greitas, efektyvus daznas enterprise

*EIGRP*-Enhanced Interior Gateway Routing protocol
* Cisco proprietary
* Kombinuoja distance-vector ir link-state idejas
* Greitas konvertavimas

*RIP*-Routing information protocol
* Paprastas, senas
* Matuoja hop countus
* Max 15 hopu --> labai ribotas

*BGP*-Border Gateway Protocol
* Interneto stuburas
* Naudojamas tarp ISP
* Ne "greiciausias", o politiskai teisingsa routing


-----------------------------------------------------------------------------

# NAT - Network address translation  # 

Ideja
Vienas viesas IP --> Daug privaciu IP

Kodel reikalingas:
* IPv4 adresu trukumas
* Vidaus tinklo izoliacija
* Papildomas saugumo sluoksnis (ne firewall, bet padeda)

Kaip veikia
Routeris palaiko *NAT lentele,kur*:
* Vidinis IP + portas <-> viesas ip + portas
PVZ:
192.168.1.10:52344 --> 81.xxx.xxx.xxx:40001

NAT seka sesijas kitaip nei paprastas routing


# NAT – Network Address Translation

NAT leidžia private IP adresams komunikuoti su internetu per public IP.

Dažniausias namų/ofiso variantas:
> Daug private IP -> vienas public IP naudojant portus.

Tai dažnai vadinama:
- PAT – Port Address Translation
- NAT overload

Pvz:
192.168.1.10:52344 -> 81.x.x.x:40001
192.168.1.11:52345 -> 81.x.x.x:40002

Routeris palaiko NAT lentelę, kad žinotų, kuriam vidiniam įrenginiui grąžinti atsakymą.
NAT nėra firewall.
NAT gali paslėpti vidinius IP ir apsunkinti tiesioginį inbound pasiekiamumą, bet saugumo politika turi būti daroma firewall taisyklėmis.

_____________________________________________________________________________________

---

# Mano pataisymai / papildymai

## DHCP
- DHCP server listens on UDP 67.
- DHCP client listens on UDP 68.
- Clientas siunčia iš UDP 68 į serverio UDP 67.
- Serveris atsako iš UDP 67 į kliento UDP 68.
- DORA:
  1. DHCPDISCOVER
  2. DHCPOFFER
  3. DHCPREQUEST
  4. DHCPACK
- DHCP duoda:
  - IP address
  - subnet mask
  - default gateway
  - DNS server
  - lease time

## ARP
- ARP verčia IP į MAC lokaliame network segmente.
- ARP request yra broadcast.
- ARP reply dažniausiai yra unicast.
- ARP neperžengia routerių.
- Jei destination IP yra kitame networke, ARP ieško default gateway/routerio MAC, ne galutinio serverio MAC.
- ARP cache saugo IP -> MAC mapping.
- ARP spoofing/poisoning yra dažna local network attack tema.

## ICMP
- ICMP naudojamas diagnostic ir error messages.
- Ping naudoja ICMP Echo Request / Echo Reply.
- Ping neatsakymas nebūtinai reiškia, kad hostas down.
- Traceroute/tracert remiasi TTL:
  - kiekvienas routeris sumažina TTL per 1
  - TTL = 0 -> packet numetamas
  - routeris grąžina ICMP Time Exceeded
- Windows `tracert` dažniausiai naudoja ICMP Echo.
- Linux `traceroute` dažnai naudoja UDP probes pagal default.

## Routing
- Routing = sprendimas kur siųsti packetą pagal destination IP.
- Routeris žiūri į routing table.
- Jei destination yra local subnet’e, siunčiama tiesiai hostui.
- Jei destination yra kitame networke, siunčiama į default gateway.
- Default gateway = routeris, kuriam siunčiamas traffic į nelokalius networkus.
- Routing table = taisyklių lentelė, pagal kurią pasirenkamas next hop/interface.
- Routing protokolai:
  - OSPF – link-state, dažnas enterprise
  - EIGRP – Cisco proprietary, advanced distance-vector
  - RIP – senas, hop count, max 15 hops
  - BGP – interneto routing tarp autonomous systems, paremtas policies

## NAT
- NAT = Network Address Translation.
- Leidžia private IP naudoti public IP komunikacijai su internetu.
- Dažniausias variantas namuose/ofise yra PAT/NAT overload:
  - daug private IP dalinasi vienu public IP per skirtingus portus.
- NAT lentelė seka mapping:
  - internal IP:port -> public IP:port
- NAT nėra firewall.
- NAT paslepia vidinius IP ir apsunkina tiesioginį inbound pasiekiamumą, bet tikras traffic control yra firewall darbas.

## Core mental model
- DHCP = duoda network settings.
- ARP = randa MAC pagal IP lokaliame tinkle.
- ICMP = diagnostika ir klaidų pranešimai.
- Routing = nusprendžia, kur siųsti packetą toliau.
- NAT = verčia private IP/port į public IP/port.
