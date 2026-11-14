# Packets & Frames

## Room esmė
Šis TryHackMe room aiškina, kaip duomenys tinkle suskaidomi į mažesnes dalis ir perduodami iš vieno įrenginio į kitą.

Pagrindinė mintis:
> Didelė informacija nėra siunčiama kaip vienas didelis gabalas. Ji suskaidoma į mažesnes dalis, perduodama per tinklą, o gavėjo pusėje vėl sudedama atgal.

---

## Packet
Packet yra duomenų gabalas OSI modelio Layer 3 / Network Layer lygyje.

Packet turi IP informaciją, pvz.:
- source IP address
- destination IP address
- IP header
- payload
- TTL
- checksum

Trumpai:
> Packet = Layer 3 duomenų gabalas su IP adresavimo informacija.

Packet atsako į klausimą:
> Kur šie duomenys turi keliauti per tinklą?

---

## Frame
Frame yra duomenų gabalas OSI modelio Layer 2 / Data Link lygyje.

Frame apgaubia packet ir prideda Layer 2 informaciją, pvz.:
- source MAC address
- destination MAC address
- frame header
- trailer / FCS
- payload, kuriame gali būti packet

Trumpai:
> Frame = Layer 2 vokas, kuris perneša packet per vietinį tinklą.

Frame atsako į klausimą:
> Kaip šie duomenys pasieks kitą įrenginį tame pačiame local network / link?

---

## Packet vs Frame

| Dalykas | Packet | Frame |
|---|---|---|
| OSI layer | Layer 3 / Network Layer | Layer 2 / Data Link |
| Adresavimo tipas | IP address | MAC address |
| Naudojamas kam? | Keliauti tarp tinklų | Judėti vietiniame tinkle |
| Pavyzdys | IP packet | Ethernet frame |
| Pagrindinė funkcija | Pasakyti, kur data turi keliauti | Pernešti packet iki kito local hop |

Trumpai:
> IP adresai = packet.
> MAC adresai = frame.

---

## Encapsulation
Encapsulation yra procesas, kai duomenys keliaudami žemyn per OSI sluoksnius yra apgaubiami papildoma informacija.

Supaprastintas vaizdas:

Application data
→ TCP segment
→ IP packet
→ Ethernet frame

Kiekvienas sluoksnis prideda savo header informaciją.

Pavyzdys:
- Application layer sukuria data.
- Transport layer prideda TCP/UDP informaciją.
- Network layer prideda IP informaciją.
- Data Link layer prideda MAC / Ethernet informaciją.

Trumpai:
> Encapsulation = duomenų „supakavimas“ kelionei per tinklą.

---

## Laiško analogija
Frame galima įsivaizduoti kaip voką.
Packet galima įsivaizduoti kaip laiško turinį.

Vokas padeda laiškui judėti iki kito taško.
Laiško viduje yra informacija, kur jis galiausiai turi nukeliauti.

Trumpai:
> Frame perneša packet.

---

## Kodėl duomenys skaidomi į packet’us?
Duomenys tinkle dažniausiai nesiunčiami kaip vienas didelis gabalas.

Vietoj to jie suskaidomi į mažesnius packet’us, nes:
- taip efektyviau naudoti tinklą;
- mažiau rizikos užkimšti tinklą;
- lengviau retransmitinti tik prarastas dalis;
- gavėjo įrenginys gali surinkti dalis atgal į pilną informaciją.

Pavyzdys:
> Nuotrauka iš website nėra siunčiama kaip viena vientisa nuotrauka. Ji suskaidoma į mažesnes dalis, perduodama, tada tavo kompiuteryje sudedama atgal.

---

## IP packet header
IP packet turi header informaciją, kuri padeda packet’ui keliauti per tinklą.

Svarbūs laukai:

### Time To Live / TTL
TTL nurodo, kiek laiko arba kiek hop’ų packet gali keliauti tinkle.

Kam reikia:
> Kad packet neužstrigtų tinkle amžinai, jei nepavyksta pasiekti destination.

---

### Checksum
Checksum naudojamas duomenų vientisumui patikrinti.

Kam reikia:
> Jei duomenys pakeliui pasikeitė arba buvo sugadinti, checksum padeda tai pastebėti.

---

### Source Address
Source Address yra siuntėjo IP adresas.

Kam reikia:
> Kad gavėjas žinotų, kur siųsti atsakymą.

---

### Destination Address
Destination Address yra gavėjo IP adresas.

Kam reikia:
> Kad tinklo įrenginiai žinotų, kur packet turi keliauti.

---

## TCP/IP
TCP yra Transport Layer protokolas, kuris veikia virš IP.

TCP svarbus tada, kai reikia patikimo ryšio:
- ryšys užmezgamas prieš siunčiant duomenis;
- duomenys sekami;
- galima patikrinti, ar jie pasiekė gavėją;
- prarasti duomenys gali būti siunčiami iš naujo.

Trumpai:
> TCP = patikimas, connection-based duomenų perdavimas.

---

## Three-Way Handshake
Three-way handshake yra TCP ryšio užmezgimo procesas.

Jis vyksta trimis žingsniais:

1. SYN
2. SYN/ACK
3. ACK

Supaprastintai:

Client → Server: SYN
Server → Client: SYN/ACK
Client → Server: ACK

Ką tai reiškia:
- Client prašo pradėti ryšį.
- Server patvirtina ir atsako.
- Client patvirtina serverio atsakymą.
- Ryšys užmegztas.

Trumpai:
> Three-way handshake = TCP būdas susitarti, kad abi pusės pasiruošusios bendrauti.

---

## UDP/IP
UDP taip pat yra Transport Layer protokolas, bet jis veikia kitaip nei TCP.

UDP nėra connection-based:
- nėra three-way handshake;
- nėra tokio pat patikimumo kaip TCP;
- nėra garantijos, kad packet’ai pasieks gavėją;
- greitesnis, nes mažiau kontrolės.

Trumpai:
> UDP = greitesnis, bet mažiau patikimas duomenų perdavimas.

UDP dažnai naudojamas ten, kur greitis svarbiau nei absoliutus tikslumas:
- streaming;
- voice chat;
- online games;
- DNS;
- video calls.

---

## TCP vs UDP

| Dalykas | TCP | UDP |
|---|---|---|
| Ryšio tipas | Connection-based | Connectionless |
| Handshake | Yra | Nėra |
| Patikimumas | Aukštesnis | Žemesnis |
| Greitis | Lėtesnis | Greitesnis |
| Retransmission | Taip | Ne taip griežtai / dažnai ne |
| Pavyzdžiai | HTTP/HTTPS, SSH, FTP | DNS, streaming, voice, gaming |

Trumpai:
> TCP = patikimiau.
> UDP = greičiau.

---

## Ports
Portai padeda nustatyti, kuri programa arba service turi gauti tinklo duomenis.

IP adresas nurodo įrenginį.
Portas nurodo service / aplikaciją tame įrenginyje.

Pavyzdys:
- IP = namo adresas
- Port = durys į konkretų kambarį

---

## Dažni portai

| Portas | Protokolas / Service |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3389 | RDP |

Trumpai:
> Portai leidžia vienam įrenginiui turėti daug skirtingų servisų vienu metu.

---

## Kaip tai siejasi su Nmap
Kai naudoju nmap, aš tikrinu:
- ar host gyvas;
- kokie portai atviri;
- kokie servisai klausosi;
- kokie protokolai naudojami;
- kokia potenciali attack surface.

Pavyzdys:
nmap randa portą 22 open
→ tai greičiausiai SSH service
→ tada galima galvoti apie SSH login, version detection, brute force apsaugą, config klaidas ir pan.

Trumpai:
> Nmap padeda pamatyti, kokios „durys“ atviros target įrenginyje.

---

## Kaip tai siejasi su Wireshark / tcpdump
Wireshark ir tcpdump leidžia matyti packet’us ir frame’us realiai.

Juose galima matyti:
- Ethernet / frame informaciją;
- IP / packet informaciją;
- TCP arba UDP informaciją;
- source ir destination adresus;
- source ir destination portus;
- handshake procesą;
- DNS request’us;
- HTTP traffic, jei jis nešifruotas.

Supaprastinta struktūra Wireshark’e:

Ethernet II
→ Internet Protocol
→ Transmission Control Protocol / User Datagram Protocol
→ Application Data

Trumpai:
> Wireshark parodo, kaip teorija atrodo realiame traffic’e.

---

## Svarbiausi sakiniai įsiminti
Packet = Layer 3 duomenų gabalas su IP informacija.
Frame = Layer 2 duomenų gabalas su MAC informacija.
Frame perneša packet per vietinį tinklą.
TCP naudoja three-way handshake.
UDP handshake nenaudoja.
IP adresas nurodo įrenginį.
Portas nurodo service tame įrenginyje.
Nmap padeda rasti atvirus portus ir servisus.
Wireshark / tcpdump leidžia matyti tinklo traffic’ą realiai.

---

## Mano verdiktas
Šitas room yra tinklo pagrindų stuburas.

Jeigu OSI modelis paaiškina sluoksnius teoriškai, tai Packets & Frames pradeda rodyti, kaip tie sluoksniai atrodo praktiškai:
- Layer 2 = frames / MAC;
- Layer 3 = packets / IP;
- Layer 4 = TCP / UDP / ports.

Pagrindinis mental model:
> Data keliauja ne kaip vienas magiškas failas, o kaip daug mažų gabalų, kurie yra supakuojami, adresuojami, perduodami ir surenkami atgal.
