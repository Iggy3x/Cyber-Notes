#   NMAP BASICS   #

1. Host discovery - kas yra gyvas tinkle
Jaigu butu paprasyta patikrinti, kokie irenginiai (devices) yra online tinkle 192.168.0.0/24, galima naudoti paprastus irankius: 
* <ping>
* <arp-scan> 
* <nmap -sn> 
* Kitus host disvoery toolus

Kodel /24 tinkel yra 254 ip adresai, o ne 256?
IPv4 /24 tinkle:
* Viso adresu:256
* 2 adresai rezervuoti:
	* 192.168.0.0 --> network adresas
	  Indentifikuoja pati tinkla, ne konkretu irengini
	* 192.168.0.255 --> broadcast adresas
	  Naudojamas siusti paketams visimes hostams tinkle

* Realus hostai:
	* 192.168.0.1 - 192.168.0.254 --> visi 254 irenginiai

___________________________________________________________________________________________

<ping> apribojimai:
* <ping> naudoja ICMP Echo request
* Jei targeto firewall blokuoja icmp traffic:
	* <ping> negaus atsakymo
	* BET HOSTAS VIS TIEK GALI BUTI ONLINE
___________________________________________________________________________________________

<arp-scan> apribojimai:
* Veikia tik local network`e
* T.y. ai kurie yrenginiai yra:
	* tame paciame ethernet segmente
	* arba tam paciam wifi tinkle
* Neveikia per routerius ( remote network) 

___________________________________________________________________________________________

2. Targetu nurodymas nmap`e

nmap leidzia nurodyti targetus keliais budais.

IP RANGE NAUDONAT < - > 
Jei nori skanuoti IP adresus nuo 192.168.0.1 iki 192.168.0.10:
*nmap 192.168.0.1-10* 
Naudinga:
* maziems intervalams
* rankiniams testams
192.168.0.1-10 = IP range
192.168.0.0/24 = subnet
__________________________________________________________________________________________

IP subnet naudojant < / > 
Jei nori skanuoti visa subneta:
*nmap 192.168.0.0/24*
Tai ekvivalentu:
*192.168.0.0 - 192.168.0.255*
Nmap automatiskai ignoruos:
* network adresa
* broadcast adresa
___________________________________________________________________________________________

Hostname

Targeta galima nurodyti hostname:
*example.thm* 
Tokiu atveju:
* nmap pirmiausias atlieka *DNS resolution*
* tada skanuoda gauta *IP* adresa

___________________________________________________________________________________________

3. Ping scan- tik host discovery

Jeigu tikslas: 
*Suzinoti kas online  be portu scan*
Naudojama:
<nmap -sn *target*>

<-sn> reikiska:
* atliekamas host discovery
* PORTAI NESKANUOJAMI

___________________________________________________________________________________________

Root/sudo svarba

Kad nmap galetu:
* siusti ICMP
* naudoti ARP
* atlikti SYN scan 

reikia:
<sudo>
Be <sudo>: 
* nmap bus apribotas
* bus naudojamas TCP connect scan <-st>
* nebus packetu crafting galimybiu

___________________________________________________________________________________________

4. Scanning a "Local" Network

Local network:
* tinklas, prie kurio esame TIESIOGIAI PRISIJUNGE 
* ethener arba wi-fi
 
PVZ:
< nmap -sn 192.168.66.0/24 > 
Toku atveju nmap:
* naudoja *ARP* requestus
* greitai ir patikimai aptinka gyvus hostus
* nepriklauso nuo *ICMP*

___________________________________________________________________________________________

5. Scanning a "remote" netowkr

Remote network:
* tinklas, nuo kurio mus skiria BENT VIENAS ROUTERIS
Tokiu atveju:
* ARP neveikia
* naudojami ICMP,TCP ar kiti metodai

___________________________________________________________________________________________

List Scan <-sL> 
< nmap -sL *target* >
<sL>:
* neskanuoja
* nesiuncia paketu
* tik parodo:
	* kaip nmap interpertuoja targetus
	* kokie IP/hostname bus naudojami
Naudojamas kaip:
* "dry run" 
* patikrinimas pries rimta scana

___________________________________________________________________________________________

6. Port scanning - kas klausosi?

TCP portu skanavimas
Ranknis testas su <telnet> 
< telnet *ip* *port* >

Jei:
* prisijungia -> portas greiciausiai OPEN
* neprisijungia -> CLOSED arba FILTERED

Tai:
* letas
* rankinis
* netinka dideliam skanavimui

___________________________________________________________________________________________

7. TCP connect scan <-sT>

<nmap -sT *target* >

Kas vyksta:
* Nmap PILNAI UZBAIGIA TCP 3-WAY HANDSHAKE
* jei portas open -> prisijungimas sekmingas
* tada connection NUTRAUKIAMAS

Savybes:
* Daugiau logu targete
* naudojamas kai NER ROOT TEISIU

_____________________________________________________________________________

8. TCP SYN Scan < -sS > - stealth scan

<sudo nmap -sS *target*>
Kas vyksta:
* siunciamas tik *SYN*
* jei targetas atsako *SYN/ACK* -> portas OPEN
* jei *RST* -> portas CLOSED
* 3-way handshake NEUZBAIGEMAS

Privalumai:
* maziau logu
* greitesnis
* laikomas " stealth" 

_____________________________________________________________________________

9. UDP Scan < -sU *target* >

<sudo nmap -sU *target*>
UDP naudoja:
* DNS
* DHCP
* NTP
* SNMP
* VoIP

Savybes:
* nera connection
* scan letas
* daznas rezultatas *open|filtered*

____________________________________________________________________________

10. Portu ribojimas
-F -> fast mode / THM kontekste: 100 dažniausių portų
--top-ports 10 -> 10 dažniausių portų
-p- -> visi 65535 TCP portai

* <-F> -> SKanuoja 100 daznaiusiu portu
* <nmap -p 10-1024 > -> konkretus portu intervalas
* <-p-> -> visi portai nuo 1 iki 65535 
* <nmap -p 22,80,443 target>
* <nmap -p 1-1023 target>
* <nmap -p- target>
Default Nmap scan dažniausiai tikrina 1000 common TCP ports.
-F yra greitesnis “fast” scan, THM room kontekste nurodomas kaip 100 common ports.

*Well-know ports:1-1023*


_____________________________________________________________________________

11. Version Detection - daugiau info

OS detecntion < -O > 
<sudo nmap -O *IP* >

Nmap:
* analizuoja *TCP/IP* stack elgsena
* bando nustatyti operacine sistema

_____________________________________________________________________________

Service ir Version detection < -sV > 
<sudo nmap -sV *ip*>

Nmap:
* nustato service tipa
* bando nustatyi versija
_____________________________________________________________________________

Aggressive Scan < -A > 
<sudo nmap -A *IP*>

Apjungia:
* OS detection
* service detection
* script scanning
* traceroute

Minusai:
* labai triuksmingas
* lengvai trigerina IDS/IPS
* -A patogus labuose, bet realiame penteste naudoti atsargiai, nes triukšmingas.
_____________________________________________________________________________

12. FOrcing the scan < -Pn > 

Pagal nutylejima:
* jei hostas neatsako į host discovery probes -> Nmap gali laikyti jį down ir neskanuoti portų. 
<nmap -Pn *target*>

Reiskia:
* laikyti hosta online
* skanuoti net jei nera ICMP atsako

## Host discovery niuansas
Nmap host discovery nėra vien tik ping.
Priklausomai nuo situacijos, Nmap gali naudoti:
- ARP, jei target yra local network
- ICMP echo / timestamp / netmask probes
- TCP SYN/ACK probes
Local networke ARP discovery dažnai patikimesnis už ICMP, nes ARP reikalingas normaliai LAN komunikacijai.
Remote networke ARP neveikia, todėl naudojami ICMP/TCP metodai.
`-Pn` išjungia host discovery ir liepia Nmap laikyti targetą online.
_____________________________________________________________________________

13. Timing - kaip gretai skanuojam

Timing templates:
* <-T0> paranoid – labai lėtas
* <-T1> sneaky
* <-T2> polite
* <-T3> normal (default)
* <-T4> aggressive
* <-T5> insane

Timing keičia:
* packet intervalus
* retry skaičių
* timeout’us
THM/lab praktikoje dažnai naudojamas:
-T4
Realiuose/leidžiamuose testuose timing reikia derinti pagal scope, stability ir taisykles.
____________________________________________________________________________

Parallelism
< --min-parallelism <num>>
< --max-parallelism <num>>

Valdo:
* kiek probe`u vyksta vienu metu

_____________________________________________________________________________

Rate control
< --min-rate <pps> >
< --max-rate <pps> >
Valdo:
* paketu skaiciu per sekunde
* visam scanui


_____________________________________________________________________________
Host timeout

< --host-timeout 30s > 
Jei hostas per letas:
* nmap nutraukia bandymas
* pereina prie kito


_____________________________________________________________________________

14. Output - ka matai ir ka isssaugai

VERBOSITY
* < -v > -> bazine info
* < -vv >, < -vvvv > -> daugiau info
* galima spausti v scano metu

_____________________________________________________________________________

DEBUG
* <-d> -> debug info
* <-d9> -> max debug indo (daxuje triuksmo) 

_____________________________________________________________________________

OUTPUT FORMATAI
* < -oN file > -> normal
* < -oX file > -> XML
* < -oG file > -> grepable
* < -oA base > -> visi formatai
-oA scanname sukuria:
scanname.nmap
scanname.xml
scanname.gnmap
Verta visada saugoti rimtesnius scanus:
nmap -sV -oA nmap_initial target
_____________________________________________________________________________

15. Svarbi pastaba apie sudo
Root/sudo leidžia Nmap kurti raw packets.
Dėl to galima:
- SYN scan `-sS`
- OS detection `-O`
- kai kurie host discovery metodai
- low-level packet crafting

Be root Nmap dažnai naudoja TCP Connect Scan `-sT`, nes OS pati užmezga connection.
* su <sudo> -> Pilnas nmap funkcionalumas
* be <sudo> -> ribotas
* SYN scan, OS detection ir packet crating REIKALAUJA ROOT
_____________________________________________________________________________ 
## Nmap port states
open:
- portas priima connection
- service greičiausiai klausosi

closed:
- host pasiekiamas
- portas uždarytas
- target atsako RST arba kitu reject signalu

filtered:
- Nmap negali aiškiai nustatyti
- dažnai firewall/drop/filter
- nėra normalaus atsakymo

open|filtered:
- dažna UDP scan būsena
- Nmap negali atskirti ar portas open, ar packetai filtruojami
