# THM – Intro to LAN

## 1. Room esmė
Šitas roomas aiškina LAN pagrindus.
LAN reiškia:
> Local Area Network
Paprastai:
> LAN = lokalus tinklas ribotoje vietoje.
LAN pavyzdžiai:
- namų tinklas
- tattoo shop tinklas
- ofiso tinklas
- mokyklos tinklas
- mažos įmonės tinklas
- THM lab environment vidinis tinklas
LAN nėra visas internetas.
LAN yra vidinis tinklas, kuriame įrenginiai komunikuoja tarpusavyje dažniausiai per:
- switch
- router
- ethernet
- Wi-Fi
- private IP adresus
Šitas roomas dengia:
- LAN topologies
- star topology
- bus topology
- ring topology
- switch
- router
- routing
- subnetting primer
- ARP
- DHCP
- ryšį su OSI Model
---

## 2. Kas yra LAN?
LAN yra lokalus tinklas.
Paprastai:
> LAN = keli įrenginiai vienoje ribotoje aplinkoje, sujungti į bendrą tinklą.
Pvz. namuose:
```txt
Laptopas
Telefonas
TV
Printeris
Routeris
```
Visi jie gali būti tame pačiame LAN.
Pvz.:
```txt
192.168.1.10  laptopas
192.168.1.11  telefonas
192.168.1.20  printeris
192.168.1.1   routeris
```
Čia visi įrenginiai yra tame pačiame private network.
Svarbu:
> LAN dažniausiai naudoja private IP adresus.
Dažni private IP ranges:
```txt
192.168.x.x
10.x.x.x
172.16.x.x – 172.31.x.x
```
THM targetai dažnai irgi būna private network range:
```txt
10.10.x.x
```
---

## 3. Kas yra topology?
Topology reiškia tinklo dizainą arba išdėstymą.
Paprastai:
> Topology = kaip įrenginiai fiziškai arba logiškai sujungti tinkle.
Tai gali reikšti:
- kaip sujungti laidai
- kaip įrenginiai pasiekia vienas kitą
- kur yra centrinis tinklo įrenginys
- kur yra single point of failure
- kaip traffic keliauja per tinklą
Roomas mini pagrindines topologijas:
- star topology
- bus topology
- ring topology
---

## 4. Star topology
Star topology yra dažniausia moderniuose tinkluose.
Joje visi įrenginiai jungiasi prie centrinio įrenginio.
Dažniausiai tas centrinis įrenginys yra:
- switch
- hub
Schema:
```txt
       PC1
        |
PC2 -- Switch -- PC3
        |
       PC4
```
Paprastai:
> Star topology = visi įrenginiai prijungti prie centrinio tinklo įrenginio.

## Privalumai
- lengva pridėti naujus įrenginius
- gerai scale’inasi
- patikimesnė nei bus topology
- jeigu vienas įrenginys sugenda, visas tinklas nebūtinai miršta
- lengviau valdyti nei senas topologijas

## Minusai
- reikia daugiau laidų
- reikia centrinio įrenginio
- brangiau įrengti
- daugiau maintenance
- jeigu centrinis switch/hub miršta, tinklas griūna

Svarbiausia rizika:
> Star topology turi centrinį single point of failure.
Jeigu miršta switchas, visi prie jo prijungti įrenginiai praranda ryšį.

## Praktinė cybersecurity reikšmė
Star topology yra svarbi, nes daug modernių LAN taip ir veikia.
Jeigu testuoji įmonės LAN, labai dažnai žiūri į:
- switchus
- VLAN
- ports
- connected hosts
- network segmentation
- misconfigurations
---

## 5. Bus topology
Bus topology naudoja vieną pagrindinį kabelį.
Tas kabelis vadinamas:
> backbone cable
Schema:

```txt
PC1 --- PC2 --- PC3 --- PC4
```
Visi įrenginiai jungiasi prie to paties bendro kabelio.
Paprastai:
> Bus topology = visi įrenginiai dalinasi vienu pagrindiniu kabeliu.

## Privalumai
- pigu įrengti
- mažiau kabelių
- paprasta mažuose tinkluose
- nereikia tiek daug dedikuotos įrangos

## Minusai
- lengvai atsiranda bottleneck
- visi duomenys keliauja ta pačia linija
- sunkiau troubleshootinti
- mažai redundancy
- nutrūkus backbone kabeliui, griūna visas tinklas
Svarbiausia rizika:
> Vienas backbone kabelis yra single point of failure.

## Praktinė cybersecurity reikšmė
Bus topology moderniuose tinkluose reta, bet principas svarbus:
> Kai daug įrenginių dalinasi viena komunikacijos linija, atsiranda bottleneck ir matomumo/saugumo problemos.
Tai padeda suprasti, kodėl modernūs tinklai naudoja switchus, segmentation ir geresnį traffic valdymą.
---

## 6. Ring topology
Ring topology reiškia, kad įrenginiai sujungti į žiedą.
Schema:

```txt
PC1 --- PC2
 |       |
PC4 --- PC3
```
Duomenys keliauja per žiedą, kol pasiekia tikslą.
Paprastai:
> Ring topology = įrenginiai sujungti ratu, o traffic keliauja per kitus įrenginius.

## Privalumai
- mažiau kabelių nei star topology
- mažiau priklausomybės nuo centrinės įrangos
- gali būti lengviau suprasti traffic kryptį
- mažiau bottleneck nei bus topology tam tikrose situacijose

## Minusai
- duomenys gali turėti keliauti per kelis įrenginius
- neefektyvu, jei tikslas yra toli žiede
- jeigu nutrūksta kabelis arba vienas įrenginys sugenda, gali griūti visas tinklas
- fault gali sustabdyti visą ring

Svarbiausia rizika:
> Vienas sugedęs įrenginys arba nutrauktas kabelis gali paveikti visą ring topology.

## Praktinė cybersecurity reikšmė
Ring topology moko vieno principo:
> Network design lemia resilience.
Kitaip tariant, ne tik firewall’ai svarbu. Pats tinklo dizainas gali būti silpnas arba tvirtas.
---

## 7. Switch
Switch yra tinklo įrenginys, kuris sujungia daug įrenginių lokaliame tinkle.
Paprastai:
> Switch = įrenginys, kuris jungia LAN įrenginius ir siunčia data ten, kur reikia.
Prie switch gali jungtis:
- kompiuteriai
- printeriai
- serveriai
- access pointai
- kameros
- kiti switchai
Switch turi ports.
Portų kiekis gali būti skirtingas:
```txt
4
8
16
24
32
48
64
```

## Ką daro switch?
Switch seka, kuris įrenginys prijungtas prie kurio porto.
Paprastai:
> Switch prisimena MAC adresus ir pagal juos siunčia frame’us į teisingą portą.
Svarbu:
- switch yra efektyvesnis už hub
- switch nesiunčia visko visiems, jei žino kur siųsti
- switch sumažina nereikalingą traffic
- switch padeda tinklui veikti greičiau ir tvarkingiau
---

## 8. Switch vs Hub
Hub yra paprastesnis ir kvailesnis įrenginys.
Hub gavęs traffic siunčia jį visiems portams.
Switch gavęs traffic bando siųsti tik ten, kur reikia.
| Įrenginys | Kaip veikia | Efektyvumas |
|---|---|---|
| Hub | siunčia visiems | prastas |
| Switch | siunčia konkrečiam targetui | geras |
Paprastai:
> Hub rėkia visam kambariui. Switch kalba su konkrečiu žmogumi.
Security prasme hub yra blogiau, nes daugiau įrenginių gali matyti traffic.
Moderniuose tinkluose dažniausiai naudojami switchai.
---

## 9. Router
Router jungia skirtingus tinklus.
Paprastai:
> Router = įrenginys, kuris sujungia networks ir nukreipia data tarp jų.
Pvz. namuose router jungia:
```txt
Tavo LAN  <->  Internetas
```

Tavo LAN:

```txt
192.168.1.0/24
```

Internetas/public pusė:

```txt
ISP network
```

Routerio darbas yra routing.
Routing reiškia:
> Routing = proceso metu parenkamas kelias, kuriuo data keliauja tarp tinklų.
Paprastai:
> Switch jungia įrenginius tame pačiame LAN, router jungia skirtingus tinklus.

## Switch vs Router
| Įrenginys | Paskirtis |
|---|---|
| Switch | jungia įrenginius tame pačiame LAN |
| Router | jungia skirtingus tinklus |
Pavyzdys:

```txt
Laptopas -> Switch -> Router -> Internetas
```
---

## 10. Redundancy
Redundancy reiškia atsarginį kelią arba atsarginį komponentą.
Paprastai:
> Redundancy = jeigu vienas kelias miršta, yra kitas kelias.
Pavyzdys:
```txt
Switch A ---- Router A
   |             |
Switch B ---- Router B
```
Jeigu vienas path miršta, traffic gali keliauti kitu.
#Privalumas:
- mažiau downtime
- patikimesnis tinklas
- geriau įmonėms
- mažiau rizikos, kad vienas gedimas nužudys viską

#Minusas:
- brangiau
- sudėtingiau
- daugiau maintenance
- gali būti lėtesnis path
- reikia geresnio planavimo
Security ir infrastructure prasme:
> Redundancy yra ne prabanga, o normalus rimtos infrastruktūros principas.
---

## 11. Subnetting primer
Subnetting reiškia tinklo padalinimą į mažesnius tinklus.
Paprastai:
> Subnetting = didesnio network padalinimas į mažesnius subnetus.
Pvz.:
```txt
192.168.1.0/24
```
gali būti padalintas į mažesnes dalis.
Subnetting padeda:
- organizuoti tinklą
- sumažinti broadcast chaosą
- atskirti skyrius
- pagerinti saugumą
- valdyti IP adresus
- segmentuoti infrastruktūrą
Pavyzdys įmonėje:
```txt
192.168.10.0/24  Office PCs
192.168.20.0/24  Printers
192.168.30.0/24  Servers
192.168.40.0/24  Guest Wi-Fi
```
Svarbi idėja:
> Ne visi įrenginiai turi būti viename dideliame tinkle.
Security prasme subnetting labai svarbus, nes jis leidžia segmentuoti tinklą.
Pvz.:
```txt
Guest Wi-Fi neturėtų tiesiogiai matyti internal serverių.
```
---

## 12. Network segmentation
Network segmentation yra praktinė subnetting idėja.
Paprastai:
> Segmentation = tinklo suskaldymas į atskiras dalis pagal paskirtį ir riziką.
Pvz.:

```txt
Staff devices
Guest Wi-Fi
Servers
Printers
CCTV
Payment systems
Admin systems
```
Kodėl tai svarbu?
Jeigu viskas viename tinkle, vienas kompromituotas įrenginys gali matyti per daug.
Blogas modelis:
```txt
Guest phone
Office laptop
Printer
Server
Payment system
CCTV
```
visi viename tinkle.
Geresnis modelis:
```txt
Guest Wi-Fi        izoliuotas
Office devices     atskiras subnet
Servers            atskiras subnet
Payment systems    griežtai izoliuoti
```
Svarbiausia pamoka:
> Flat network yra patogus, bet saugumo prasme dažnai blogas.
Flat network = viskas viename dideliame tinkle be normalios segmentacijos.
---

## 13. ARP
ARP reiškia:
> Address Resolution Protocol
ARP naudojamas rasti MAC adresą pagal IP adresą lokaliame tinkle.
Paprastai:
> ARP = “kas turi šitą IP? Pasakyk savo MAC.”
Pvz. laptopas nori kalbėti su:
```txt
192.168.1.20
```
Bet LAN lygyje jam reikia MAC adreso.
Tada jis siunčia ARP request:
```txt
Who has 192.168.1.20?
```
Įrenginys su tuo IP atsako:
```txt
192.168.1.20 is at aa:bb:cc:dd:ee:ff
```
Tada laptopas gali siųsti traffic į tą MAC adresą.
---

## 14. ARP cache
Sistema saugo ARP informaciją cache’e.
Paprastai:
> ARP cache = laikina lentelė, kuri susieja IP adresus su MAC adresais.
Linux’e galima pažiūrėti:
```bash
ip neigh
```
arba:

```bash
arp -a
```
Pavyzdys:
```txt
192.168.1.1 dev wlan0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
```
Tai reiškia:
- IP: 192.168.1.1
- interface: wlan0
- MAC: aa:bb:cc:dd:ee:ff
- status: reachable
---

## 15. ARP security reikšmė
ARP neturi stiprios autentifikacijos.
Tai reiškia, kad ARP galima piktnaudžiauti.
Pvz.:
- ARP spoofing
- ARP poisoning
- man-in-the-middle atakos lokaliame tinkle
Principas:
> Jeigu gali apgauti įrenginius dėl IP -> MAC mapping, gali nukreipti traffic per save.
Čia dar nereikia gilintis į atakas iki dugno, bet reikia suprasti:
> ARP yra būtinas LAN veikimui, bet nėra labai saugus protokolas.
---

## 16. DHCP
DHCP reiškia:
> Dynamic Host Configuration Protocol
DHCP automatiškai duoda įrenginiui network configuration.
Paprastai:
> DHCP = automatinis IP ir tinklo nustatymų dalintojas.
Kai įrenginys prisijungia prie tinklo, DHCP gali jam duoti:
- IP adresą
- subnet mask
- default gateway
- DNS serverius
- lease time
Pvz. namuose router dažnai veikia kaip DHCP serveris.
Laptopas prisijungia prie Wi-Fi ir gauna:
```txt
IP:              192.168.1.77
Subnet mask:     255.255.255.0
Default gateway: 192.168.1.1
DNS server:      192.168.1.1 arba 8.8.8.8
```
Be DHCP reikėtų viską suvesti rankiniu būdu.
---

## 17. DHCP procesas
DHCP procesas dažnai aiškinamas kaip DORA:
```txt
D - Discover
O - Offer
R - Request
A - Acknowledge
```

## Discover
Klientas klausia:
```txt
Ar yra DHCP serveris?
```
## Offer
DHCP serveris pasiūlo konfiguraciją:
```txt
Gali naudoti šitą IP.
```
## Request
Klientas sako:
```txt
Noriu šito IP.
```
## Acknowledge
DHCP serveris patvirtina:
```txt
Gerai, šitas IP tavo tam tikram laikui.
```
Paprastai:
> DHCP nėra tik IP davimas. Jis duoda visą bazinę tinklo konfiguraciją.
---

## 18. Default gateway
Default gateway yra įrenginys, per kurį traffic keliauja į kitus tinklus.
Dažniausiai namuose tai routeris.
Pvz.:
```txt
Default gateway: 192.168.1.1
```
Paprastai:
> Gateway = durys iš tavo LAN į kitus tinklus.
Jeigu nori pasiekti internetą, tavo traffic dažniausiai eina per default gateway.
Pvz.:
```txt
Laptopas -> Routeris -> Internetas
```
Jeigu default gateway blogas arba neveikia, gali matyti LAN įrenginius, bet nepasiekti interneto.
---

## 19. DNS DHCP kontekste
DHCP dažnai duoda ir DNS serverį.
DNS serveris leidžia naudoti vardus vietoj IP.
Pvz.:

```txt
google.com -> IP adresas
```
Jeigu DNS blogas:
- internetas gali “atrodyti neveikiantis”
- ping į IP gali veikti
- bet website vardai neveiks
Pavyzdys:
```bash
ping 8.8.8.8
```
veikia, bet:
```bash
ping google.com
```
neveikia.
Tada labai tikėtina DNS problema.
---

## 20. Praktinis troubleshooting modelis
Kai neveikia network, netikrinti chaotiškai.
Eiti sluoksniais.
## 1. Ar turi IP?
```bash
ip a
```
Ieškoti, ar interface turi IP adresą.
## 2. Ar turi route?
```bash
ip route
```
Ieškoti default route:
```txt
default via 192.168.1.1
```
## 3. Ar pasieki gateway?
```bash
ping -c 4 192.168.1.1
```
## 4. Ar pasieki interneto IP?
```bash
ping -c 4 8.8.8.8
```
## 5. Ar veikia DNS?
```bash
ping -c 4 google.com
```
Jeigu IP veikia, bet domain neveikia, tikėtina DNS problema.
---

## 21. Komandos
## Pažiūrėti IP adresus
```bash
ip a
```
## Pažiūrėti interface’us
```bash
ip link
```
## Pažiūrėti routing lentelę
```bash
ip route
```
## Pažiūrėti ARP/neighbor lentelę
```bash
ip neigh
```
arba:
```bash
arp -a
```
## Ping gateway
```bash
ping -c 4 192.168.1.1
```
## Ping public IP
```bash
ping -c 4 8.8.8.8
```
## Ping domain vardą
```bash
ping -c 4 google.com
```
## Nmap scan lokaliam subnetui
```bash
nmap 192.168.1.0/24
```
## Nmap scan be ping discovery
```bash
nmap -Pn 192.168.1.10
```
## Pažiūrėti hostname
```bash
hostname
```
## Pažiūrėti DNS config su systemd-resolved
```bash
resolvectl status
```
## DHCP lease info gali būti skirtingose vietose pagal distro
Kartais:
```bash
journalctl | grep DHCP
```
arba NetworkManager sistemose:
```bash
nmcli device show
```
---

## 22. Praktinė vertė white hat mokymuisi

## Kur tai naudosi?
### 1. Nmap
Kai skenuoji subnetą:
```bash
nmap 192.168.1.0/24
```
Turi suprasti:
- kas yra subnet
- ką reiškia `/24`
- kodėl randami keli hostai
- kodėl kai kurie hostai neatsako
- kodėl `-Pn` kartais reikalingas
### 2. THM VPN
Kai jungiesi prie THM per OpenVPN, tu praktiškai prisijungi prie kito private network.
Tavo Warhorse gauna interface:
```txt
tun0
```
Tada gali pasiekti THM targetus:
```txt
10.10.x.x
```
### 3. Enumeration
LAN supratimas padeda suvokti:
- kas yra target
- kas yra gateway
- kas yra kitas hostas
- kas yra service
- kodėl hostai tame pačiame subnet gali matyti vienas kitą
### 4. ARP attacks
Vėliau ARP bus svarbus suprantant:
- ARP spoofing
- MITM
- local network attacks
- traffic interception
### 5. DHCP attacks/misconfig
DHCP bus svarbus suprantant:
- rogue DHCP
- wrong gateway
- DNS hijacking
- network misconfiguration
### 6. Realus troubleshooting
Kai kažkas neveikia labuose, nepanikuoti.
Tikrinimo eiga:
```txt
VPN -> interface -> IP -> route -> gateway -> target -> service
```
Ne “blet neveikia”, o “kurioje vietoje lūžta grandinė”.
Nors “blet neveikia” galima pasakyti kaip ritualinę diagnostikos maldą. Tiesiog ne ten sustoti.
---

## 23. Mano paaiškinimas žmonių kalba
LAN yra kaip mažas rajonas.
- Įrenginiai = namai
- IP = namo adresas
- MAC = žmogaus/mašinos identitetas vietiniame kieme
- Switch = vietinė sankryža arba paskirstymo dėžė
- Router = išvažiavimas iš rajono į greitkelį
- Gateway = pagrindiniai vartai iš LAN
- DHCP = rajono administratorius, kuris išdalina adresus
- ARP = klausia kaimynų “kas turi šitą adresą?”
- Subnet = rajono kvartalas
- DNS = telefonų knyga / vardų vertėjas
Svarbiausia:
> LAN yra tavo artima aplinka. Internetas yra už rajono ribų.

---
## 24. Ką būtina atsiminti
- LAN = Local Area Network.
- LAN yra lokalus/private tinklas.
- Topology = tinklo dizainas/išdėstymas.
- Star topology naudoja centrinį įrenginį, dažniausiai switch.
- Star topology yra dažniausia, bet centrinis switch gali būti single point of failure.
- Bus topology naudoja vieną backbone kabelį.
- Bus topology pigi, bet blogai scale’inasi ir turi bottleneck problemą.
- Ring topology jungia įrenginius žiedu.
- Ring topology gali griūti dėl vieno gedimo.
- Switch jungia įrenginius tame pačiame LAN.
- Router jungia skirtingus tinklus.
- Routing = duomenų kelio parinkimas tarp tinklų.
- Redundancy = atsarginiai keliai arba komponentai.
- Subnetting = tinklo dalinimas į mažesnius tinklus.
- Segmentation = saugumo ir valdymo tikslais atskirti tinklo dalis.
- ARP verčia IP į MAC lokaliame tinkle.
- DHCP automatiškai duoda IP ir kitus network nustatymus.
- Default gateway yra išėjimas iš LAN į kitus tinklus.
- DNS leidžia naudoti vardus vietoj IP adresų.

---
---

## 26. Mini testas sau

Atsakyk nežiūrėdamas:

1. Ką reiškia LAN?
2. Kas yra topology?
3. Kuo star topology skiriasi nuo bus topology?
4. Kodėl star topology dažnai naudojama šiandien?
5. Koks pagrindinis star topology single point of failure?
6. Kodėl bus topology gali tapti bottleneck?
7. Kaip veikia ring topology?
8. Kas yra switch?
9. Kuo switch skiriasi nuo hub?
10. Kas yra router?
11. Kuo switch skiriasi nuo router?
12. Kas yra routing?
13. Kas yra redundancy?
14. Kas yra subnetting?
15. Kodėl segmentation svarbi security kontekste?
16. Ką reiškia ARP?
17. Ką ARP išsprendžia?
18. Kas yra ARP cache?
19. Ką reiškia DHCP?
20. Kokius nustatymus DHCP gali duoti įrenginiui?
21. Ką reiškia DHCP DORA procesas?
22. Kas yra default gateway?
23. Kaip patikrinti savo IP Linux’e?
24. Kaip pažiūrėti routing lentelę?
25. Kaip pažiūrėti ARP/neighbor lentelę?
26. Kaip atskirti DNS problemą nuo interneto problemos?

---
