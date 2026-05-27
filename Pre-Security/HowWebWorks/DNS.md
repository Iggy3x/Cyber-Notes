# DNS in Detail

## Room esmė
Šis TryHackMe room aiškina, kaip veikia DNS ir kodėl jis reikalingas pasiekiant interneto servisus.

Pagrindinės temos:
- kas yra DNS
- domain hierarchy
- TLD
- second-level domain
- subdomain
- DNS record types
- DNS request eiga
- recursive DNS server
- root DNS server
- TLD DNS server
- authoritative DNS server
- TTL
- DNS queries

Trumpai:
> DNS leidžia naudoti žmonėms suprantamus domain vardus vietoj IP adresų.

---

## What is DNS?

DNS reiškia Domain Name System.

DNS pagrindinė funkcija:
> Paversti domain name į IP address.

Žmogui lengviau atsiminti:
- google.com
- tryhackme.com
- example.com

Negu atsiminti IP adresus:
- 142.250.187.14
- 104.26.10.229
- 2606:4700:20::681a:be5

Trumpai:
> DNS yra interneto telefonų knyga.

Kai naršyklėje įvedu domain name, kompiuteris turi sužinoti, į kokį IP adresą kreiptis.

Pavyzdys:
> tryhackme.com → DNS lookup → IP address → connection to server

---

## Kodėl DNS reikalingas?

Kompiuteriai tinkle bendrauja naudodami IP adresus.

Žmonės geriau prisimena vardus.

DNS sujungia šiuos du pasaulius:
- žmogus įveda domain name
- DNS suranda IP address
- kompiuteris jungiasi prie IP

Trumpai:
> Žmogus naudoja vardą, kompiuteris naudoja IP.

---

## Domain Hierarchy

Domain vardai turi hierarchiją.

Pavyzdys:
> admin.platform.google.com

Skaitant iš dešinės į kairę:
- com = Top-Level Domain / TLD
- google = Second-Level Domain
- platform = subdomain
- admin = subdomain

Trumpai:
> Domain hierarchy skaitoma nuo dešinės į kairę.

---

## TLD / Top-Level Domain

TLD yra dešiniausia domain name dalis.

Pavyzdžiai:
- .com
- .org
- .net
- .edu
- .gov
- .co.uk
- .lt

Pavyzdys:
> tryhackme.com

Čia:
- tryhackme = second-level domain
- .com = TLD

Trumpai:
> TLD yra paskutinė domain dalis.

---

## gTLD

gTLD reiškia generic Top-Level Domain.

Pavyzdžiai:
- .com
- .org
- .net
- .online
- .club

Dažnai gTLD nurodo bendrą paskirtį.

Pavyzdžiai:
- .com dažnai naudojamas komerciniams puslapiams
- .org dažnai naudojamas organizacijoms

Trumpai:
> gTLD = generic top-level domain.

---

## ccTLD

ccTLD reiškia country code Top-Level Domain.

Tai TLD, susijęs su šalimi arba teritorija.

Pavyzdžiai:
- .uk
- .lt
- .de
- .fr
- .pl
- .co.uk

Trumpai:
> ccTLD = country code top-level domain.

Pavyzdys:
> .co.uk yra ccTLD.

---

## Second-Level Domain

Second-Level Domain yra domain dalis kairėje nuo TLD.

Pavyzdys:
> tryhackme.com

Čia:
- tryhackme = second-level domain
- com = TLD

Pavyzdys:
> google.com

Čia:
- google = second-level domain
- com = TLD

Second-level domain turi apribojimus:
- maksimalus ilgis: 63 characters
- gali naudoti letters a-z
- gali naudoti numbers 0-9
- gali naudoti hyphen -
- negali naudoti underscore _

Trumpai:
> Second-level domain yra pagrindinis domain vardas prieš TLD.

---

## Subdomain

Subdomain yra domain dalis kairėje nuo second-level domain.

Pavyzdys:
> admin.tryhackme.com

Čia:
- admin = subdomain
- tryhackme = second-level domain
- com = TLD

Pavyzdys:
> store.tryhackme.com

Čia:
- store = subdomain
- tryhackme = second-level domain
- com = TLD

Galima turėti kelis subdomain lygius.

Pavyzdys:
> admin.platform.google.com

Čia:
- admin = subdomain
- platform = subdomain
- google = second-level domain
- com = TLD

Subdomain apribojimai:
- maksimalus ilgis: 63 characters
- galima naudoti letters a-z
- galima naudoti numbers 0-9
- galima naudoti hyphen -
- negalima naudoti underscore _

Trumpai:
> Subdomain leidžia sukurti atskirus vardus po tuo pačiu domain.

---

## Maksimalus Domain Name ilgis

Maksimalus pilno domain name ilgis yra 253 characters.

Pavyzdys:
> labai.ilgas.subdomain.example.com

Kiekviena dalis tarp taškų turi savo limitą, bet visas domain name taip pat turi bendrą limitą.

Trumpai:
> Vienos domain dalies limitas dažnai 63 characters, o viso domain name limitas 253 characters.

---

## DNS Record Types

DNS record nurodo, kokią informaciją domain turi grąžinti.

Skirtingi record types naudojami skirtingiems tikslams.

Pagrindiniai record types šiame room:
- A
- AAAA
- CNAME
- MX
- TXT

Trumpai:
> DNS record yra įrašas, kuris atsako į konkretų DNS klausimą.

---

## A Record

A record susieja domain name su IPv4 address.

Pavyzdys:
> example.com → 93.184.216.34

IPv4 pavyzdys:
> 104.26.10.229

Trumpai:
> A record = domain į IPv4.

Naudojimas:
- website hosting
- server IP lookup
- basic DNS resolution

---

## AAAA Record

AAAA record susieja domain name su IPv6 address.

IPv6 pavyzdys:
> 2606:4700:20::681a:be5

Trumpai:
> AAAA record = domain į IPv6.

Kodėl vadinasi AAAA?
> Nes IPv6 adresas yra ilgesnis už IPv4, o AAAA yra tarsi išplėstas A record variantas.

---

## CNAME Record

CNAME reiškia Canonical Name.

CNAME record nurodo, kad vienas domain vardas yra alias kitam domain vardui.

Pavyzdys:
> store.tryhackme.com → shops.shopify.com

Tai reiškia:
- vartotojas prašo store.tryhackme.com
- DNS atsako, kad tai yra alias į shops.shopify.com
- tada reikia išspręsti shops.shopify.com IP adresą

Trumpai:
> CNAME = vienas domain vardas rodo į kitą domain vardą.

Svarbu:
> CNAME paprastai negrąžina IP tiesiogiai. Jis grąžina kitą domain name.

---

## MX Record

MX reiškia Mail Exchange.

MX record nurodo, kurie mail serveriai priima email’us už domain.

Pavyzdys:
> tryhackme.com MX → alt1.aspmx.l.google.com

MX record turi priority value.

Priority nurodo, kuriuo mail serveriu bandyti naudotis pirmiau.

Mažesnis priority number dažniausiai reiškia aukštesnį prioritetą.

Pavyzdys:
- MX 10 mail1.example.com
- MX 20 mail2.example.com

Čia:
- pirmiau bandomas mail1
- jei neveikia, bandomas mail2

Trumpai:
> MX record nurodo, kur siųsti email’us domain’ui.

---

## TXT Record

TXT record leidžia saugoti tekstinę informaciją DNS sistemoje.

TXT record gali būti naudojamas:
- domain ownership verification
- SPF
- DKIM
- DMARC
- email spoofing apsauga
- trečių šalių servisų patvirtinimas
- papildoma domain metadata

Trumpai:
> TXT record = laisvas tekstinis DNS įrašas.

Cyber kontekste TXT records gali būti naudingi reconnaissance metu, nes kartais atskleidžia servisus, verification tokens ar email security setup.

---

## SPF

SPF reiškia Sender Policy Framework.

SPF dažnai saugomas TXT record’e.

SPF nurodo, kurie serveriai turi teisę siųsti email’us domain vardu.

Trumpai:
> SPF padeda kovoti su email spoofing.

---

## DKIM

DKIM reiškia DomainKeys Identified Mail.

DKIM leidžia email’ą pasirašyti cryptographic signature.

Gavėjas gali patikrinti, ar email tikrai susijęs su domain ir ar nebuvo pakeistas.

Trumpai:
> DKIM padeda patikrinti email autentiškumą.

---

## DMARC

DMARC reiškia Domain-based Message Authentication, Reporting and Conformance.

DMARC remiasi SPF ir DKIM rezultatais.

DMARC nurodo, ką daryti su email’ais, kurie nepraeina SPF arba DKIM patikros.

Galimi policy variantai:
- none
- quarantine
- reject

Trumpai:
> DMARC pasako, kaip elgtis su įtartinais email’ais.

---

## DNS Request eiga

Kai naršyklėje įvedu domain name, kompiuteris turi išsiaiškinti IP adresą.

Supaprastinta DNS request eiga:
1. kompiuteris patikrina local cache
2. jei atsakymo nėra, klausia recursive DNS server
3. recursive DNS server tikrina savo cache
4. jei atsakymo nėra, recursive server klausia root DNS server
5. root DNS server nukreipia į TLD DNS server
6. TLD DNS server nukreipia į authoritative DNS server
7. authoritative DNS server grąžina DNS record
8. recursive DNS server grąžina atsakymą klientui
9. atsakymas išsaugomas cache pagal TTL

Trumpai:
> DNS request keliauja per kelis DNS serverių tipus, kol randamas authoritative answer.

---

## Local DNS Cache

Kompiuteris pirmiausia gali tikrinti savo local DNS cache.

Jei neseniai jau lankiausi tame pačiame domain, IP gali būti išsaugotas local cache.

Tai pagreitina procesą.

Trumpai:
> Cache leidžia neklausti DNS serverių kiekvieną kartą iš naujo.

---

## Recursive DNS Server

Recursive DNS server dažniausiai yra DNS serveris, kurį suteikia ISP arba kurį pasirenki pats.

Pavyzdžiai:
- ISP DNS
- Google DNS
- Cloudflare DNS
- Quad9 DNS

Recursive DNS server užduotis:
> Surasti atsakymą už klientą.

Jis pats keliauja per DNS hierarchy:
- root
- TLD
- authoritative

Trumpai:
> Recursive DNS server yra tarpininkas, kuris suranda IP adresą už tave.

---

## Root DNS Server

Root DNS server yra aukščiausias DNS hierarchy lygis.

Jis nežino kiekvieno domain IP adreso, bet žino, kur rasti TLD servers.

Pavyzdys:
> Jei klausi apie tryhackme.com, root server žino, kur yra .com TLD serveriai.

Trumpai:
> Root DNS server nukreipia į tinkamą TLD server.

---

## TLD DNS Server

TLD DNS server atsakingas už konkretų TLD.

Pavyzdžiai:
- .com TLD server
- .org TLD server
- .uk TLD server
- .lt TLD server

TLD DNS server žino, kur rasti authoritative DNS server konkrečiam domain.

Pavyzdys:
> .com TLD server žino, kur yra tryhackme.com nameservers.

Trumpai:
> TLD server nukreipia į domain authoritative DNS server.

---

## Authoritative DNS Server

Authoritative DNS server saugo realius DNS records konkrečiam domain.

Pavyzdys:
> authoritative server for tryhackme.com saugo tryhackme.com A, MX, TXT ir kitus records.

Čia daromi DNS record pakeitimai.

Trumpai:
> Authoritative DNS server turi galutinį atsakymą apie domain DNS records.

---

## Nameserver

Nameserver yra serveris, kuris atsakingas už domain DNS records.

Domain dažnai turi kelis nameservers dėl redundancy.

Pavyzdys:
- ns1.example.com
- ns2.example.com

Trumpai:
> Nameserver yra authoritative DNS server domain’ui arba jo daliai.

---

## TTL / Time To Live

TTL reiškia Time To Live.

TTL nurodo, kiek laiko DNS record gali būti laikomas cache.

TTL dažniausiai matuojamas sekundėmis.

Pavyzdys:
- TTL 300 = cache 5 minutes
- TTL 3600 = cache 1 hour
- TTL 86400 = cache 24 hours

Trumpai:
> TTL pasako, kiek laiko DNS atsakymas gali būti saugomas cache.

Kodėl TTL svarbu:
- mažesnis TTL leidžia greičiau keisti DNS
- didesnis TTL sumažina DNS queries kiekį
- per didelis TTL gali sulėtinti pakeitimų propagaciją
- per mažas TTL gali padidinti DNS serverių apkrovą

---

## DNS Caching

DNS caching reiškia, kad DNS atsakymai laikinai išsaugomi.

Cache gali būti:
- browser cache
- operating system cache
- recursive DNS server cache
- ISP cache

Caching nauda:
- greičiau atidaromi puslapiai
- mažiau DNS queries
- mažesnė apkrova DNS infrastruktūrai

Trumpai:
> DNS cache taupo laiką ir resursus.

---

## DNS Propagation

DNS propagation yra laikas, per kurį DNS pakeitimai pasimato įvairiuose DNS serveriuose ir cache sistemose.

Svarbiausia priežastis:
> TTL ir cache.

Jei record buvo cached, senas atsakymas gali būti naudojamas iki TTL pabaigos.

Trumpai:
> DNS pakeitimai ne visada matomi iškart.

---

## DNS Query

DNS query yra užklausa DNS sistemai.

Pavyzdžiai:
- koks yra A record?
- koks yra MX record?
- koks yra TXT record?
- koks yra CNAME?

Trumpai:
> DNS query yra klausimas DNS serveriui.

---

## nslookup

nslookup yra įrankis DNS queries vykdyti.

Jis leidžia klausti DNS records iš terminalo.

Pavyzdžiai:
- nslookup example.com
- nslookup -type=A example.com
- nslookup -type=AAAA example.com
- nslookup -type=CNAME store.example.com
- nslookup -type=MX example.com
- nslookup -type=TXT example.com

Trumpai:
> nslookup leidžia rankiniu būdu klausti DNS records.

---

## dig

dig yra dar vienas DNS query įrankis.

Jis dažnai duoda detalesnį output nei nslookup.

Pavyzdžiai:
- dig example.com
- dig A example.com
- dig AAAA example.com
- dig MX example.com
- dig TXT example.com
- dig CNAME store.example.com

Trumpai:
> dig yra galingesnis DNS query įrankis.

---

## host

host yra paprastas DNS lookup įrankis.

Pavyzdžiai:
- host example.com
- host -t MX example.com
- host -t TXT example.com

Trumpai:
> host greitai parodo DNS resolution informaciją.

---

## DNS Record Types lentelė

| Record type | Kam naudojamas |
|---|---|
| A | Domain į IPv4 address |
| AAAA | Domain į IPv6 address |
| CNAME | Alias į kitą domain name |
| MX | Mail serveriai domain’ui |
| TXT | Tekstinė informacija, verification, SPF, DKIM, DMARC |

---

## DNS Server Types lentelė

| Server type | Funkcija |
|---|---|
| Recursive DNS Server | Suranda atsakymą už klientą |
| Root DNS Server | Nukreipia į TLD server |
| TLD DNS Server | Nukreipia į authoritative server |
| Authoritative DNS Server | Saugo tikrus domain DNS records |

---

## DNS Request mental model

Kai rašau:
> tryhackme.com

Vyksta:
1. mano kompas klausia: ar žinau IP iš cache?
2. jei ne, klausia recursive DNS server
3. recursive klausia root server
4. root sako: klausk .com TLD server
5. TLD sako: klausk tryhackme.com authoritative server
6. authoritative server grąžina record
7. recursive server cache’ina atsakymą
8. mano kompas gauna IP
9. naršyklė jungiasi prie IP

Trumpai:
> DNS yra kelionė nuo vardo iki IP.

---

## Kaip DNS siejasi su web

Kai naršyklėje įvedu domain:
1. DNS suranda IP address
2. naršyklė jungiasi prie IP
3. jei naudojamas HTTPS, vyksta TLS handshake
4. siunčiamas HTTP request
5. web serveris grąžina response

Trumpai:
> DNS įvyksta prieš HTTP request.

Jei DNS neveikia:
- website gali neatsidaryti
- net jei serveris pats veikia
- nes klientas nežino, į kokį IP kreiptis

---

## Kaip DNS siejasi su cybersecurity

DNS svarbus cyber kontekste, nes jis gali atskleisti:
- subdomains
- mail servers
- hosting providers
- third-party services
- verification records
- email security setup
- infrastructure clues

Recon metu DNS padeda suprasti target infrastructure.

Pavyzdžiai:
- A records gali parodyti server IP
- MX records gali parodyti mail provider
- TXT records gali parodyti SPF, DKIM, DMARC ar verification tokens
- CNAME records gali parodyti third-party services
- subdomains gali parodyti papildomus attack surface taškus

Trumpai:
> DNS dažnai yra vienas pirmų reconnaissance šaltinių.

---

## Subdomain Enumeration

Subdomain enumeration yra bandymas rasti domain subdomains.

Pavyzdžiai:
- admin.example.com
- dev.example.com
- test.example.com
- api.example.com
- staging.example.com
- vpn.example.com

Kodėl svarbu:
- subdomain gali turėti mažiau apsaugotą service
- gali būti senas test serveris
- gali būti admin panel
- gali būti API endpoint
- gali atskleisti infrastruktūrą

Trumpai:
> Subdomains plečia attack surface.

---

## CNAME ir third-party services

CNAME gali parodyti, kad subdomain nukreiptas į third-party platform.

Pavyzdžiai:
- Shopify
- GitHub Pages
- Heroku
- AWS
- Azure
- Cloudflare
- Netlify

Cyber kontekste tai svarbu, nes kartais neteisingai sukonfigūruoti CNAME records gali sukelti subdomain takeover riziką.

Trumpai:
> CNAME gali atskleisti, kur realiai hostinamas service.

---

## MX ir email reconnaissance

MX records parodo, kas tvarko domain email.

Pavyzdžiai:
- Google Workspace
- Microsoft 365
- Proton
- Zoho
- custom mail server

Cyber kontekste:
- padeda suprasti email infrastructure
- padeda vertinti phishing apsaugas
- kartu su TXT records galima matyti SPF, DKIM, DMARC setup

Trumpai:
> MX record parodo domain mail infrastruktūrą.

---

## TXT ir email security

TXT records dažnai naudojami email security.

SPF:
> Kas gali siųsti email’us domain vardu.

DKIM:
> Email signature verification.

DMARC:
> Policy, ką daryti su nepraėjusiais email’ais.

Trumpai:
> TXT records dažnai slepia labai naudingą security informaciją.

---

## DNS problemų troubleshooting

Jei domain neveikia, galima tikrinti:
- ar domain resolves į IP
- ar A record egzistuoja
- ar AAAA record egzistuoja
- ar CNAME rodo kur reikia
- ar MX record veikia email’ui
- ar TXT records teisingi
- ar TTL nėra per ilgas
- ar local cache neturi seno atsakymo
- ar recursive DNS server atsako
- ar authoritative DNS server turi teisingus records

Trumpai:
> DNS problemos dažnai atrodo kaip website problema, bet realiai gali būti vardų rezoliucijos problema.

---

## Dažnos klaidos

### Nėra A record
Domain neturi IPv4 adreso.

Pasekmė:
> Website gali neatsidaryti per IPv4.

### Blogas CNAME
Subdomain rodo į neteisingą target.

Pasekmė:
> Subdomain neveikia arba rodo į blogą servisą.

### Blogas MX
Email’ai gali nepasiekti domain.

Pasekmė:
> Mail delivery problemos.

### Blogas TXT
SPF, DKIM, DMARC ar verification gali neveikti.

Pasekmė:
> Email spoofing rizika arba third-party verification failure.

### Per didelis TTL
DNS pakeitimai ilgai neatsinaujina.

Pasekmė:
> Senas IP arba senas record dar kurį laiką naudojamas.

---

## Praktinis DNS query mąstymas

Kai matau domain, klausiu:
1. koks jo A record?
2. ar turi AAAA record?
3. ar jis turi CNAME?
4. kokie jo MX records?
5. kokie TXT records?
6. koks TTL?
7. kas yra authoritative nameservers?
8. ar yra subdomains?
9. ar DNS records rodo third-party services?

Trumpai:
> DNS query nėra tik IP radimas. Tai infrastruktūros žemėlapis.

---

## Svarbiausi sakiniai įsiminti

DNS reiškia Domain Name System.

DNS paverčia domain name į IP address.

TLD yra dešiniausia domain dalis.

Second-level domain yra pagrindinis domain vardas prieš TLD.

Subdomain yra papildomas vardas kairėje nuo domain.

Vienos domain dalies maksimalus ilgis dažnai yra 63 characters.

Pilno domain name maksimalus ilgis yra 253 characters.

A record rodo į IPv4 address.

AAAA record rodo į IPv6 address.

CNAME record rodo į kitą domain name.

MX record rodo mail servers.

TXT record saugo tekstinę informaciją.

Recursive DNS server suranda atsakymą už klientą.

Root DNS server nukreipia į TLD server.

TLD DNS server nukreipia į authoritative server.

Authoritative DNS server saugo tikrus DNS records.

TTL nurodo, kiek laiko DNS record gali būti cached.

DNS cache pagreitina užklausas.

DNS vyksta prieš HTTP request.

DNS yra labai svarbus reconnaissance šaltinis.

---

## Mini žodynas

### DNS
Domain Name System. Sistema, kuri domain names paverčia į IP addresses.

### Domain Name
Žmogui suprantamas interneto adreso vardas, pvz. tryhackme.com.

### IP Address
Skaitinis arba hexadecimal tinklo adresas, kurį naudoja kompiuteriai.

### TLD
Top-Level Domain. Dešiniausia domain dalis, pvz. .com.

### gTLD
Generic Top-Level Domain, pvz. .com, .org, .net.

### ccTLD
Country Code Top-Level Domain, pvz. .uk, .lt, .de.

### Second-Level Domain
Pagrindinis domain vardas prieš TLD, pvz. tryhackme iš tryhackme.com.

### Subdomain
Papildomas domain vardas kairėje nuo second-level domain, pvz. admin.tryhackme.com.

### A Record
DNS record, kuris domain susieja su IPv4 address.

### AAAA Record
DNS record, kuris domain susieja su IPv6 address.

### CNAME Record
DNS record, kuris domain vardą nukreipia į kitą domain vardą.

### MX Record
DNS record, kuris nurodo mail serverius domain’ui.

### TXT Record
DNS record, skirtas tekstinei informacijai.

### Recursive DNS Server
DNS serveris, kuris suranda atsakymą už klientą.

### Root DNS Server
DNS hierarchy viršus, kuris nukreipia į TLD servers.

### TLD DNS Server
DNS serveris, atsakingas už konkretų TLD.

### Authoritative DNS Server
DNS serveris, kuris saugo realius DNS records konkrečiam domain.

### Nameserver
Serveris, atsakingas už domain DNS records.

### TTL
Time To Live. Nurodo, kiek laiko DNS record gali būti saugomas cache.

### DNS Cache
Laikinas DNS atsakymų saugojimas.

### DNS Query
Užklausa DNS serveriui dėl konkretaus record.

### SPF
TXT record pagrindu veikiantis email sender authorization mechanizmas.

### DKIM
Email signature verification mechanizmas.

### DMARC
Email authentication policy mechanizmas, paremtas SPF ir DKIM.

---

## Mano verdiktas
DNS yra vienas svarbiausių web ir networking pagrindų.

Pagrindinis mental model:
> Žmogus įveda domain vardą, bet kompiuteriui reikia IP adreso. DNS yra sistema, kuri suranda tą IP pagal domain hierarchy ir DNS records.

Labai trumpai:
> DNS = vardas į IP.
> A = IPv4.
> AAAA = IPv6.
> CNAME = alias į kitą domain.
> MX = mail servers.
> TXT = tekstinė/security informacija.
> TTL = kiek laiko cache’inti atsakymą.
> Authoritative server = galutinis DNS records šaltinis.
