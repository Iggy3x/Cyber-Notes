## 1. Room esmė

Šitas roomas yra bazinis tinklų įvadas.

Jo tikslas nėra išmokyti gilaus packet analysis, subnetting ar enterprise networking. Čia labiau pirmas normalus supratimas:

- kas yra network
- kas yra internetas
- kaip įrenginiai atpažįstami tinkle
- kas yra IP adresas
- kas yra MAC adresas
- kuo skiriasi public ir private IP
- kas yra IPv4 ir IPv6
- kam naudojamas `ping`
- kas yra ICMP

Paprastai:

> Network = sujungti įrenginiai arba sistemos, kurios gali keistis informacija.

Cybersecurity kontekste networking yra pamatas, nes beveik viskas vyksta per tinklą:

- web puslapiai
- DNS
- SSH
- VPN
- email
- file sharing
- service enumeration
- exploitai
- traffic analysis
- firewall’ai
- packet’ai

Jeigu nesuprantu tinklo, tada `nmap`, `wireshark`, `tcpdump`, `metasploit`, `burp`, `ports`, `services` ir `routing` atrodo kaip atskiri chaoso gabalai.

Realiai tai viena sistema.
---

## 2. Kas yra network?

Network reiškia sujungtus dalykus.
Ne tik kompiuteriuose.
Network pavyzdžiai gyvenime:
- draugų ratas
- viešojo transporto sistema
- elektros tinklas
- pašto sistema
- kaimynų bendruomenė

Kompiuteriuose network reiškia technologinius įrenginius, kurie gali komunikuoti tarpusavyje.

Pavyzdžiai:

- laptopas
- telefonas
- routeris
- printeris
- kamera
- serveris
- traffic light sistema
- IoT įrenginiai
- cloud serveriai

Network gali būti labai mažas arba milžiniškas:

- 2 įrenginiai tame pačiame tinkle
- namų Wi-Fi
- tattoo shop tinklas
- įmonės tinklas
- universitetų tinklas
- internetas

Svarbiausia mintis:

> Network = connected devices.

---

## 3. Kas yra internetas?

Internetas yra milžiniškas tinklų tinklas.

Paprastai:

> Internet = network of networks.

Internetas nėra vienas serveris ir nėra tas pats kas Google.

Internetas sudarytas iš daugybės mažesnių tinklų:

- namų tinklai
- įmonių tinklai
- ISP tinklai
- duomenų centrai
- cloud infrastruktūra
- routeriai
- backbone jungtys

Svarbus skirtumas:

| Terminas | Reikšmė |
|---|---|
| Internetas | infrastruktūra, jungianti tinklus |
| World Wide Web | svetainių ir web resursų sistema, veikianti per internetą |
| Browseris | programa web turiniui pasiekti |
| Google | viena labai didelė paslauga internete |

Paprastai:

> Internetas yra keliai. Web puslapiai yra vietos, į kurias tais keliais važiuoji.
---
## 4. Private ir public network

Roomas mini du pagrindinius network tipus:

- private network
- public network

## Private network

Private network yra vidinis/lokalus tinklas.

Pavyzdžiai:

- tavo namų Wi-Fi
- darbo shopo tinklas
- THM internal lab network
- įmonės LAN

Private network naudoja private IP adresus.

Dažni private IP ranges:

```txt
192.168.x.x
10.x.x.x
172.16.x.x – 172.31.x.x
```

Pavyzdžiai:

```txt
192.168.1.10
192.168.0.25
10.10.10.10
172.16.5.20
```

## Public network

Public network yra tinklas, pasiekiamas per internetą.

Public IP yra tas adresas, pagal kurį tavo tinklas/įrenginys matomas internete.

Paprastai public IP duoda ISP.

ISP reiškia:

> Internet Service Provider

Pavyzdžiai:

- BT
- Virgin Media
- Sky
- TalkTalk
- Vodafone

Namų tinkle keli įrenginiai gali turėti skirtingus private IP, bet į internetą išeiti per tą patį public IP.

Pavyzdys:

| Įrenginys | Private IP | Public IP |
|---|---|---|
| Laptopas | 192.168.1.77 | 86.157.52.21 |
| Telefonas | 192.168.1.74 | 86.157.52.21 |

Tai reiškia:

- viduje jie skiriasi
- iš interneto pusės jie gali atrodyti kaip vienas public adresas

Čia vėliau susijungia su NAT tema.

---

## 5. IP adresas

IP reiškia:

> Internet Protocol

IP adresas yra loginis įrenginio adresas tinkle.

Paprastai:

> IP = kur tinkle siųsti duomenis.

Pvz.:

```txt
192.168.1.10
8.8.8.8
10.10.10.10
```

IP adresas leidžia įrenginiams rasti vienas kitą tinkle.

White hat praktikoje IP naudojamas beveik visur:

```bash
ping 10.10.10.10
nmap 10.10.10.10
ssh user@10.10.10.10
curl http://10.10.10.10
```

Svarbu:

- IP gali keistis.
- IP nėra fizinis įrenginio ID.
- IP yra loginis adresas.
- IP naudojamas routingui.
- Tas pats IP negali būti aktyvus du kartus tame pačiame tinkle tuo pačiu metu.

---

## 6. IPv4

IPv4 yra senesnė ir vis dar labai plačiai naudojama IP adresavimo versija.

IPv4 adresas atrodo taip:

```txt
192.168.1.10
```

IPv4 turi 4 dalis.

Kiekviena dalis vadinama:

> octet

Pvz.:

```txt
192.168.1.10
 |   | |  |
octet octet octet octet
```

Kodėl “octet”?

Nes kiekviena IPv4 dalis yra 8 bitai.

IPv4 teoriškai turi apie 4.29 milijardo adresų, nes naudoja 32 bitus:

```txt
2^32
```

Problema:

> Šiuolaikiniam internetui IPv4 adresų per mažai.

Dėl to atsirado IPv6.

---

## 7. IPv6

IPv6 yra naujesnė IP adresavimo versija.

Ji sukurta tam, kad išspręstų IPv4 adresų trūkumo problemą.

IPv6 atrodo daug ilgesnis ir baisiau žmogaus akiai:

```txt
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

IPv6 naudoja 128 bitus:

```txt
2^128
```

Tai reiškia, kad IPv6 turi nepalyginamai daugiau galimų adresų nei IPv4.

Praktinis verdiktas šitam etapui:

> IPv6 žinoti reikia, bet dabar nereikia gilintis iki dugno.

Šiuo metu svarbiau suprasti IPv4, nes THM, CTF ir basic enumeration labai dažnai dirbs su IPv4 adresais.

---

## 8. MAC adresas

MAC reiškia:

> Media Access Control

MAC adresas yra tinklo sąsajos identifikatorius.

Paprastai:

> MAC = fizinio network interface identitetas lokaliame tinkle.

Pvz.:

```txt
a4:c3:f0:85:ac:2d
```

MAC adresas dažniausiai būna 12 hexadecimal simbolių, suskirstytų poromis.

Hexadecimal reiškia base-16 sistemą.

Naudojami simboliai:

```txt
0 1 2 3 4 5 6 7 8 9 a b c d e f
```

MAC adresas dažniausiai atrodo taip:

```txt
XX:XX:XX:XX:XX:XX
```

Pirmi 6 simboliai dažnai nurodo gamintoją.

Paskutiniai 6 simboliai yra unikali dalis konkrečiam įrenginiui / tinklo sąsajai.

---

## 9. IP vs MAC

Labai svarbus skirtumas:

| Dalykas | IP adresas | MAC adresas |
|---|---|---|
| Tipas | loginis adresas | fizinės tinklo sąsajos ID |
| Kur naudojamas | tinkle/internete | lokaliame tinkle |
| Ar gali keistis? | taip | techniškai galima spoofinti |
| Paskirtis | rasti kur siųsti traffic | identifikuoti interface lokaliame tinkle |
| Pavyzdys | 192.168.1.10 | a4:c3:f0:85:ac:2d |

Žmonių analogija:

- IP = tavo dabartinis adresas
- MAC = tavo pirštų antspaudai / įrenginio ID

Bet čia atsargiai:

> MAC atrodo “fizinis”, bet saugume negalima aklai juo pasitikėti.

---

## 10. MAC spoofing

MAC spoofing reiškia, kad įrenginys apsimeta turintis kitą MAC adresą.

Paprastai:

> MAC spoofing = apsimetimas kitu įrenginiu lokaliame tinkle.

Roomo pavyzdys:

- viešbutis turi Wi-Fi
- Alice sumokėjo už Wi-Fi
- Bob nesumokėjo
- routeris leidžia Alice MAC adresą
- Bob pakeičia savo MAC į Alice MAC
- sistema gali galvoti, kad Bob yra Alice

Tai parodo svarbią saugumo pamoką:

> Jeigu saugumas paremtas tik MAC adresu, tai yra silpnas saugumas.

Čia labai geras principas:

> Identifikacija nėra tas pats kas saugumas.

MAC address control gali būti apeinamas, jeigu sistema blogai sukonfigūruota.

---

## 11. Ping

`ping` yra vienas iš bazinių network diagnostic įrankių.

Jis naudojamas patikrinti, ar target hostas atsako per tinklą.

Pvz.:

```bash
ping 8.8.8.8
```

Arba:

```bash
ping 10.10.10.10
```

Paprastai:

> ping = “ar tu gyvas?” signalas.

Jeigu target atsako, gausi atsakymus terminale.

Pvz.:

```txt
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms
```

Tai reiškia:

- tavo įrenginys išsiuntė requestą
- targetas atsakė
- ryšys egzistuoja
- matai response laiką

---

## 12. ICMP

Ping naudoja ICMP.

ICMP reiškia:

> Internet Control Message Protocol

Ping logika:

```txt
ICMP Echo Request  -> siunčiamas į targetą
ICMP Echo Reply    <- gaunamas atsakymas iš targeto
```

Paprastai:

> ping siunčia ICMP Echo Request ir laukia ICMP Echo Reply.

Ping matuoja, kiek laiko užtrunka request/response kelionė.

Tas laikas dažniausiai rodomas milisekundėmis:

```txt
time=12.3 ms
```

Kuo mažesnis laikas, tuo greitesnis atsakas.

---

## 13. Svarbi ping interpretacija

Svarbu:

> Jeigu ping neatsako, tai nebūtinai reiškia, kad hostas išjungtas.

Galimos priežastys:

- firewall blokuoja ICMP
- targetas sukonfigūruotas neatsakyti į ping
- blogas IP
- routing problema
- VPN neveikia
- THM target dar neužsikrovė
- tavo tun0 interface neveikia
- network policy blokuoja ICMP
- hostas tikrai išjungtas

Todėl ping yra geras pirmas testas, bet ne galutinis verdiktas.

Praktikoje:

```bash
ping 10.10.10.10
```

Jeigu ping neveikia, dar galima bandyti:

```bash
nmap -Pn 10.10.10.10
```

`-Pn` sako nmap:

> Nelaikyk hosto mirusiu vien dėl to, kad ping neatsako.

---

## 14. Komandos

## Ping IP adresą

```bash
ping 8.8.8.8
```

## Ping su ribotu packet kiekiu Linux’e

```bash
ping -c 4 8.8.8.8
```

`-c 4` reiškia:

> siųsti 4 ping requestus ir sustoti.

## Ping THM targetą

```bash
ping -c 4 10.10.10.10
```

## Pažiūrėti savo IP adresus Linux’e

```bash
ip addr
```

arba trumpiau:

```bash
ip a
```

## Pažiūrėti network interfaces

```bash
ip link
```

Galimi interface pavyzdžiai:

```txt
lo
eth0
wlan0
tun0
```

`lo` = loopback interface.

`eth0` = wired ethernet.

`wlan0` = Wi-Fi interface.

`tun0` = dažnai VPN interface, pvz. TryHackMe OpenVPN.

## Pažiūrėti routing lentelę

```bash
ip route
```

Naudinga kai reikia suprasti, per kur eina traffic.

## Patikrinti ar THM VPN interface atsirado

```bash
ip a | grep tun0
```

Jeigu `tun0` yra, OpenVPN greičiausiai aktyvus.

---

## 15. Praktinė vertė white hat mokymuisi

Šitas roomas labai basic, bet jis svarbus kaip žodyno pradžia.

## Kur tai naudosi realiai?

### 1. TryHackMe VPN

Kai jungi Warhorse prie THM:

```bash
sudo openvpn file.ovpn
```

Tavo laptopas gauna prieigą prie THM private network.

Tada target IP, pvz.:

```txt
10.10.x.x
```

tampa pasiekiamas iš tavo sistemos.

### 2. Nmap

Kai darai scaną:

```bash
nmap 10.10.10.10
```

Tu skenuoji konkretų IP adresą.

Turi suprasti:

- kas yra target IP
- ar host pasiekiamas
- ar ping atsako
- kodėl kartais reikia `-Pn`
- kad IP nėra tas pats kas hostname

### 3. Wireshark / tcpdump

Kai žiūri traffic, matysi:

- source IP
- destination IP
- source MAC
- destination MAC
- protocol
- ICMP
- TCP
- UDP

Jeigu IP/MAC skirtumas neaiškus, packet analizė bus migla.

### 4. Web hacking

Kai eini į:

```txt
http://10.10.10.10
```

Tu realiai jungiiesi prie IP adreso, į web servisą.

Dažniausiai:

```txt
HTTP  = port 80
HTTPS = port 443
```

Web nėra magija. Tai service per networką.

### 5. Troubleshooting

Kai kažkas neveikia, pirma reikia tikrinti sluoksniais:

```txt
Ar VPN veikia?
Ar turiu tun0?
Ar yra route?
Ar ping atsako?
Ar portas atidarytas?
Ar servisas veikia?
Ar URL teisingas?
```

Ne iškart “viskas supista”. Nors kartais tikrai būna. Bet pirma diagnostika.

---

## 16. Mano paaiškinimas žmonių kalba

Tinklas yra kaip kelių ir adresų sistema.

- IP adresas = namo adresas
- MAC adresas = mašinos VIN / fizinis identitetas lokaliame kieme
- Routeris = sankryža / pašto paskirstymo punktas
- Packet = laiškas arba siuntinys
- Ping = “ar esi namie?” signalas
- ICMP reply = “jo, esu”
- Firewall = piktas durininkas

Bet:

> Jeigu kažkas neatsako į durų skambutį, nereiškia, kad jo nėra namie.

Lygiai taip pat:

> Jeigu ping neatsako, nereiškia, kad hostas miręs.

Gali būti, kad jis tiesiog ignoruoja ICMP.

---

## 17. Ką būtina atsiminti

- Network = sujungti įrenginiai/sistemos.
- Internet = daug mažų tinklų, sujungtų į vieną globalų tinklą.
- Private network = vidinis/lokalus tinklas.
- Public network = tinklas/infrastruktūra, pasiekiama per internetą.
- IP = loginis adresas tinkle.
- MAC = tinklo sąsajos identifikatorius.
- IPv4 turi 4 dalis.
- Viena IPv4 dalis vadinama octet.
- IPv4 adresų kiekis ribotas.
- IPv6 sukurtas išspręsti adresų trūkumo problemą.
- MAC adresą galima spoofinti.
- Saugumas vien pagal MAC adresą yra silpnas.
- `ping` naudoja ICMP.
- `ping` tikrina ryšį, bet ne visada duoda galutinį atsakymą.
- Neatsakantis ping nebūtinai reiškia, kad targetas išjungtas.
