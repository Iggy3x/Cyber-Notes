# Extending Your Network

## Room esmė
Šis TryHackMe room aiškina, kaip tinklai išplečiami už vietinio LAN ribų ir kaip servisai tampa pasiekiami iš interneto.

Pagrindinės temos:
- port forwarding
- firewalls
- VPN
- LAN networking devices
- network simulator

Trumpai:
> Local network paprastai yra už routerio, NAT ir firewall. Kad išorinis traffic pasiektų vidinį service, reikia port forwarding arba VPN.

---

## Introduction to Port Forwarding

Port forwarding leidžia išoriniam traffic pasiekti konkretų service vidiniame tinkle.

Be port forwarding, servisai dažniausiai pasiekiami tik tame pačiame local network.

Pavyzdys:
- vidiniame tinkle yra web serveris
- serverio IP: 192.168.1.10
- serveris klausosi port 80
- LAN viduje jį galima pasiekti
- iš interneto tiesiogiai jo pasiekti negalima

Kad web serveris būtų pasiekiamas iš interneto, routeryje reikia sukonfigūruoti port forwarding.

Logika:
- public IP gauna request į konkretų portą
- routeris patikrina port forwarding rule
- routeris perduoda traffic į konkretų internal IP ir portą

Trumpai:
> Port forwarding pasako routeriui, kur siųsti traffic, kuris ateina į konkretų portą.

---

## Kodėl reikia Port Forwarding?

Namų ar mažo biuro tinkle įrenginiai dažniausiai naudoja private IP adresus.

Pavyzdžiai:
- 192.168.1.10
- 192.168.1.25
- 10.0.0.5
- 172.16.0.20

Private IP adresai nėra tiesiogiai pasiekiami iš interneto.

Internete dažniausiai matomas tik routerio public IP.

Todėl routeris turi žinoti:
> Jei traffic ateina į mano public IP ant konkretaus porto, kuriam vidiniam įrenginiui ir portui jį perduoti?

---

## Port Forwarding pavyzdys

Tarkim:
- public IP: 82.62.51.70
- internal web server: 192.168.1.10
- web server port: 80

Port forwarding rule:
> 82.62.51.70:80 → 192.168.1.10:80

Kas vyksta:
1. žmogus iš interneto jungiasi prie 82.62.51.70:80
2. routeris gauna request
3. routeris patikrina port forwarding rule
4. routeris perduoda traffic į 192.168.1.10:80
5. vidinis web serveris atsako
6. routeris grąžina atsakymą klientui

Trumpai:
> Port forwarding sujungia public IP + public port su internal IP + internal port.

---

## Kur konfigūruojamas Port Forwarding?

Port forwarding dažniausiai konfigūruojamas routeryje.

Svarbus sakinys:
> Port forwarding is configured at the router of a network.

Trumpai:
> Router yra įrenginys, kuris dažniausiai sprendžia, kur nukreipti incoming traffic.

---

## Port Forwarding vs Firewall

Port forwarding ir firewall nėra tas pats.

### Port Forwarding
Port forwarding nurodo, kur siųsti traffic, kuris ateina į konkretų portą.

Jis atsako į klausimą:
> Kur nukreipti šitą traffic?

### Firewall
Firewall nusprendžia, ar traffic leidžiamas, ar blokuojamas.

Jis atsako į klausimą:
> Ar šitam traffic leidžiama praeiti?

Trumpai:
> Port forwarding nurodo kryptį.
> Firewall leidžia arba blokuoja traffic.

---

## Firewall

Firewall yra network security įrenginys arba software, kuris kontroliuoja network traffic.

Firewall gali kontroliuoti:
- incoming traffic
- outgoing traffic
- traffic tarp network segmentų
- traffic pagal IP adresus
- traffic pagal portus
- traffic pagal protocol
- traffic pagal connection state

Trumpai:
> Firewall yra traffic kontrolės vartai.

---

## Firewalls 101

Firewall tikrina traffic pagal taisykles.

Firewall rule gali remtis:
- source IP
- destination IP
- source port
- destination port
- protocol
- direction
- action

Pavyzdys:
> Allow TCP traffic to destination port 80

Tai reiškia:
- leisti TCP traffic
- kuris eina į portą 80
- dažniausiai HTTP web service

---

## Firewall OSI Layers

Firewalls dažniausiai veikia OSI Layer 3 ir Layer 4 lygyje.

### Layer 3 / Network Layer
Čia firewall gali žiūrėti:
- source IP
- destination IP

### Layer 4 / Transport Layer
Čia firewall gali žiūrėti:
- TCP
- UDP
- source port
- destination port

Trumpai:
> Firewall dažnai dirba su IP adresais ir portais.

---

## Stateful Firewall

Stateful firewall tikrina ne tik pavienį packet, bet ir visą connection context.

Jis seka ryšio būseną.

Pavyzdys:
- ar connection jau buvo pradėtas
- ar packet priklauso egzistuojančiam ryšiui
- ar traffic atrodo kaip normalus atsakymas į anksčiau pradėtą connection

Trumpai:
> Stateful firewall žiūri į visą ryšį, ne tik į vieną packet.

---

## Stateless Firewall

Stateless firewall tikrina individualius packet’us pagal statines taisykles.

Jis neseka pilno connection state.

Pavyzdys:
> Jei packet atitinka rule, leidžiam. Jei neatitinka, blokuojam.

Trumpai:
> Stateless firewall žiūri į atskirus packet’us.

---

## Stateful vs Stateless Firewall

| Dalykas | Stateful Firewall | Stateless Firewall |
|---|---|---|
| Ką tikrina? | Visą connection context | Individualų packet |
| Ar seka ryšio būseną? | Taip | Ne |
| Sprendimo pagrindas | Connection state + rules | Static rules |
| Lankstumas | Didesnis | Mažesnis |
| Paprastumas | Sudėtingesnis | Paprastesnis |

Trumpai:
> Stateful = supranta ryšio eigą.
> Stateless = tikrina pavienius packet’us pagal rules.

---

## Firewall Actions

Firewall rule dažniausiai turi action.

Dažni action tipai:
- allow
- deny
- reject
- drop

---

## Allow

Allow reiškia, kad traffic leidžiamas.

Pavyzdys:
> Allow TCP port 443

Tai leidžia HTTPS traffic.

---

## Deny / Reject

Deny arba reject reiškia, kad traffic blokuojamas.

Dažnai siuntėjui grąžinamas atsakymas, kad connection atmestas.

Trumpai:
> Reject pasako „ne“.

---

## Drop

Drop reiškia, kad traffic tyliai numetamas.

Siuntėjas gali negauti jokio atsakymo.

Trumpai:
> Drop ignoruoja traffic.

Cyber kontekste:
> Drop dažnai atrodo kaip tylus neatsakymas.

---

## Inbound ir Outbound Traffic

### Inbound Traffic
Traffic, kuris ateina į network arba host.

Pavyzdys:
> Kažkas iš interneto jungiasi prie tavo web serverio.

### Outbound Traffic
Traffic, kuris išeina iš network arba host.

Pavyzdys:
> Tavo kompas jungiasi prie website.

Trumpai:
> Inbound = traffic į vidų.
> Outbound = traffic į išorę.

---

## Practical Firewall

Practical firewall užduotyje reikia suprasti, kaip firewall rule gali blokuoti blogą traffic ir leisti gerą traffic.

Svarbu žiūrėti:
- source IP
- destination IP
- destination port
- protocol
- action

Pavyzdinė logika:
> Jei blogas traffic ateina iš konkretaus source IP į konkretų destination IP ir portą, reikia sukurti rule, kuri tą traffic blokuoja.

Svarbiausia:
> Firewall rule turi būti pakankamai konkreti, kad blokuotų blogą traffic, bet nesugadintų gero traffic.

---

## VPN Basics

VPN reiškia Virtual Private Network.

VPN sukuria saugų tunelį tarp tavo įrenginio ir kito network.

VPN leidžia:
- saugiai prisijungti prie remote network
- pasiekti vidinius servisus per internetą
- šifruoti traffic tarp VPN client ir VPN server
- atrodyti taip, lyg būtum remote network viduje

Trumpai:
> VPN leidžia prisijungti prie kito tinklo per saugų tunelį.

---

## Kaip VPN veikia

Be VPN:
> Tavo kompas → internetas → website

Su VPN:
> Tavo kompas → encrypted tunnel → VPN server → remote network / internet

VPN sukuria tunelį, per kurį tavo traffic keliauja saugiau.

---

## VPN naudojimo pavyzdys

Įmonė turi du ofisus:
- Office A
- Office B

Jie yra skirtingose vietose, bet nori dalintis resursais:
- files
- internal servers
- printers
- internal apps

VPN leidžia jiems bendrauti taip, lyg jie būtų tame pačiame private network.

Trumpai:
> VPN sujungia atskirus tinklus per internetą.

---

## VPN ir TryHackMe

TryHackMe naudoja VPN, kad tavo kompas galėtų pasiekti THM lab network.

Be VPN:
> tavo kompas nemato THM target IP.

Su VPN:
> tavo kompas turi route į THM private lab network.

Dažnai naudojamas .ovpn failas.

Svarbu:
> OpenVPN turi likti aktyvus, kol dirbi su THM target.

---

## VPN Interface

Kai prisijungi prie VPN, sistemoje atsiranda virtualus network interface.

Dažnai jis vadinasi:
- tun0

Naudingos patikros:
- pažiūrėti network interfaces
- pažiūrėti routing table
- patikrinti, ar target pasiekiamas

Trumpai:
> Jei VPN veikia, turėtum matyti papildomą interface ir route į THM network.

---

## VPN Technologies

Room mini kelias VPN technologijas:
- PPP
- PPTP
- IPSec

---

## PPP

PPP reiškia Point-to-Point Protocol.

PPP naudojamas tiesioginiam ryšiui tarp dviejų point’ų.

Room kontekste:
> PPP gali būti naudojamas autentifikacijai ir duomenų šifravimui.

Trumpai:
> PPP = point-to-point ryšio protokolas.

---

## PPTP

PPTP reiškia Point-to-Point Tunnelling Protocol.

PPTP naudoja PPP tuneliavimui.

Trumpai:
> PPTP = senesnė VPN tunnelling technologija, paremta PPP.

---

## IPSec

IPSec reiškia Internet Protocol Security.

IPSec veikia su IP framework ir naudojamas saugiam IP traffic perdavimui.

IPSec gali užtikrinti:
- encryption
- authentication
- data integrity

Trumpai:
> IPSec = saugumo technologija IP lygyje.

---

## VPN vs Port Forwarding

### Port Forwarding
Port forwarding leidžia iš interneto pasiekti konkretų vidinį service.

Pavyzdys:
> Public IP:80 → internal web server:80

### VPN
VPN leidžia klientui prisijungti prie remote network per saugų tunelį.

Pavyzdys:
> Laptopas → VPN tunnel → company network

Trumpai:
> Port forwarding atidaro konkretų service į išorę.
> VPN įleidžia vartotoją į remote network saugiu tuneliu.

---

## LAN Networking Devices

LAN tinkluose dažnai naudojami keli pagrindiniai įrenginiai:
- router
- switch
- hub
- access point
- firewall

---

## Router

Router jungia skirtingus tinklus.

Pavyzdys:
> LAN ↔ Internet

Router dažnai daro:
- routing
- NAT
- port forwarding
- basic firewall funkcijas
- traffic forwarding tarp network’ų

Router naudoja IP adresus ir dirba Layer 3 lygyje.

Trumpai:
> Router jungia skirtingus tinklus ir perduoda traffic tarp jų.

---

## Routing

Routing yra procesas, kai router nusprendžia, kur siųsti packet toliau.

Router žiūri į destination IP ir routing table.

Trumpai:
> Routing = sprendimas, kur packet turi keliauti toliau.

---

## Switch

Switch jungia įrenginius tame pačiame LAN.

Switch dažniausiai dirba Layer 2 lygyje ir naudoja MAC adresus.

Switch žino:
- kuris MAC address yra kuriame port’e
- kur siųsti frame
- kaip efektyviau perduoti traffic tame pačiame LAN

Trumpai:
> Switch jungia LAN įrenginius ir naudoja MAC adresus.

---

## Layer 2 Switch

Layer 2 switch dirba su MAC adresais.

Jis persiunčia Ethernet frame’us tame pačiame network.

Trumpai:
> Layer 2 switch = MAC address based switching.

---

## Layer 3 Switch

Layer 3 switch gali atlikti ir routing funkcijas.

Jis gali dirbti su IP adresais ir jungti skirtingus VLAN arba network segmentus.

Trumpai:
> Layer 3 switch = switch su routing galimybėmis.

---

## Hub

Hub yra senesnis tinklo įrenginys.

Hub gavęs traffic iš vieno įrenginio išsiunčia jį visiems prijungtiems įrenginiams.

Tai reiškia:
- mažiau efektyvu
- daugiau collision
- mažiau saugu
- visi gauna traffic, net jei jis skirtas ne jiems

Trumpai:
> Hub rėkia visiems.
> Switch siunčia ten, kur reikia.

---

## Access Point

Access Point leidžia wireless įrenginiams prisijungti prie wired network.

Pavyzdys:
- laptopas jungiasi per Wi-Fi
- access point perduoda traffic į LAN

Trumpai:
> Access Point yra Wi-Fi įėjimas į tinklą.

---

## Router vs Switch

| Dalykas | Router | Switch |
|---|---|---|
| Ką jungia? | Skirtingus tinklus | Įrenginius tame pačiame tinkle |
| OSI layer | Layer 3 | Dažniausiai Layer 2 |
| Adresai | IP | MAC |
| Pagrindinė funkcija | Routing | Switching |
| Pavyzdys | LAN ↔ Internet | PC ↔ Printer ↔ Server |

Trumpai:
> Router jungia tinklus.
> Switch jungia įrenginius tame pačiame tinkle.

---

## Switch vs Hub

| Dalykas | Switch | Hub |
|---|---|---|
| Traffic siuntimas | Tik kur reikia | Visiems |
| Efektyvumas | Didesnis | Mažesnis |
| Saugumas | Geresnis | Blogesnis |
| Adresų naudojimas | MAC addresses | Praktiškai netikrina |
| Modernus naudojimas | Taip | Beveik ne |

Trumpai:
> Hub yra bukas splitteris.
> Switch yra protingesnis LAN traffic valdytojas.

---

## Network Simulator

Network simulator leidžia pamatyti, kaip packet keliauja per tinklą.

Jis padeda suprasti:
- iš kur packet ateina
- kur packet keliauja
- kokie įrenginiai dalyvauja
- kaip routeriai ir switch’ai perduoda traffic
- kaip firewall taisyklės gali blokuoti traffic
- kaip source ir destination keičiasi priklausomai nuo network path

Trumpai:
> Network simulator leidžia vizualiai pamatyti packet kelionę.

---

## Kaip viskas susijungia

Paprastas namų tinklas:

Internet
→ Router
→ Switch / Wi-Fi
→ Devices

Kas ką daro:
- router jungia LAN su internetu
- NAT leidžia private IP naudoti vieną public IP
- firewall kontroliuoja traffic
- port forwarding leidžia iš interneto pasiekti vidinį service
- switch jungia LAN įrenginius
- access point leidžia jungtis per Wi-Fi
- VPN leidžia saugiai prisijungti prie remote network

---

## Cybersecurity kontekstas

Šitos temos svarbios, nes prieš analizę reikia suprasti network path.

Reikia klausti:
- ar target yra local network?
- ar target yra remote network?
- ar reikia VPN?
- ar service pasiekiamas tiesiogiai?
- ar portas atidarytas?
- ar firewall blokuoja traffic?
- ar portas forwarded?
- ar target yra už NAT?
- ar traffic eina per router?
- ar tai Layer 2 ar Layer 3 problema?

Trumpai:
> Prieš bet kokią analizę reikia suprasti kelią iki target.

---

## Kaip tai siejasi su Nmap

Nmap rezultatai priklauso nuo network path ir firewall rules.

Galimi port status’ai:
- open
- closed
- filtered

### Open
Service veikia ir portas pasiekiamas.

### Closed
Host atsako, bet portas nenaudojamas.

### Filtered
Nmap negali tiksliai nustatyti, nes traffic galimai blokuojamas firewall.

Trumpai:
> Filtered dažnai reiškia, kad kažkas tarp tavęs ir target blokuoja arba dropina traffic.

---

## Kaip tai siejasi su THM VPN

Kai dirbi su THM target:
- turi būti aktyvus VPN
- VPN ryšys turi likti paleistas
- target IP dažniausiai yra THM lab network
- tavo normalus LAN IP nėra tas pats kaip THM VPN IP
- dažnai svarbus tun0 interface

Trumpai:
> Jei VPN neveikia, target gali būti nepasiekiamas, net jei tavo komandos teisingos.

---

## Praktinis mental model

Kai jungiesi prie target, mąstyk taip:

Mano kompas
→ mano network interface
→ router / VPN tunnel
→ remote network
→ target host
→ target port / service

Jei kažkas neveikia, tikrinu:
1. ar turiu network connection
2. ar VPN veikia
3. ar route egzistuoja
4. ar target pasiekiamas
5. ar portas atviras
6. ar firewall neblokuoja
7. ar service realiai veikia

---

## Svarbiausi sakiniai įsiminti

Port forwarding nukreipia traffic iš public IP porto į vidinį IP ir portą.

Port forwarding konfigūruojamas routeryje.

Firewall sprendžia, ar traffic leidžiamas, ar blokuojamas.

Firewall dažnai dirba Layer 3 ir Layer 4 lygyje.

Stateful firewall seka visą connection.

Stateless firewall tikrina individualius packet’us.

VPN sukuria saugų tunelį į remote network.

Router jungia skirtingus tinklus.

Routing yra procesas, kai router nusprendžia, kur siųsti packet toliau.

Switch jungia įrenginius tame pačiame LAN.

Layer 2 switch naudoja MAC adresus.

Layer 3 switch gali atlikti routing.

Hub siunčia traffic visiems.

Access Point leidžia Wi-Fi įrenginiams prisijungti prie LAN.

NAT leidžia private IP naudoti vieną public IP.

Public IP matomas internete.

Private IP naudojamas local network viduje.

---

## Mini žodynas

### Port Forwarding
Traffic nukreipimas iš public IP porto į konkretų vidinį IP ir portą.

### Firewall
Sistema, kuri leidžia arba blokuoja network traffic pagal taisykles.

### Stateful Firewall
Firewall, kuris vertina visą connection state.

### Stateless Firewall
Firewall, kuris tikrina individualius packet’us pagal statines taisykles.

### VPN
Virtual Private Network. Saugus tunelis į remote network.

### PPP
Point-to-Point Protocol.

### PPTP
Point-to-Point Tunnelling Protocol.

### IPSec
Internet Protocol Security. IP framework pagrindu veikianti saugumo technologija.

### Router
Įrenginys, jungiantis skirtingus tinklus.

### Routing
Packet keliavimo krypties parinkimas pagal destination IP ir routing table.

### Switch
Įrenginys, jungiantis LAN įrenginius.

### Layer 2 Switch
Switch, kuris dirba su MAC adresais.

### Layer 3 Switch
Switch, kuris gali atlikti routing pagal IP.

### Hub
Senas tinklo įrenginys, kuris traffic siunčia visiems prijungtiems įrenginiams.

### Access Point
Wi-Fi įėjimo taškas į LAN.

### NAT
Network Address Translation. Private IP vertimas į public IP.

### Public IP
Internete matomas IP adresas.

### Private IP
Vidiniame tinkle naudojamas IP adresas.

---

## Mano verdiktas
Šitas room paaiškina, kaip LAN tampa sujungtas su platesniu pasauliu.

Pagrindinis mental model:
> Local devices dažniausiai slepiasi už routerio, NAT ir firewall. Kad išorinis traffic pasiektų vidinį service, reikia port forwarding. Kad vartotojas saugiai patektų į remote network, naudojamas VPN.

Labai trumpai:
> Router jungia tinklus.
> Switch jungia LAN įrenginius.
> Firewall kontroliuoja traffic.
> Port forwarding nukreipia traffic į vidinį service.
> VPN sukuria saugų tunelį į remote network.
