# THM – Networking Core Protocols

## 1. Room esmė
Šitas roomas aiškina pagrindinius application layer / core internet protocols.
Pagrindinė mintis:
> Kai IP, routing, TCP/UDP ir ports jau aiškūs, kitas žingsnis yra suprasti, kokios “kalbos” veikia viršuje: DNS, HTTP, FTP ir email protokolai.
Šitas roomas dengia:
- DNS
- WHOIS
- HTTP / HTTPS
- FTP
- SMTP
- POP3
- IMAP
Šitie protokolai dažniausiai yra application layer protokolai.
Jie naudoja transport layer:
- TCP
- UDP
ir standartinius portus.
Cybersecurity kontekste svarbiausia:
> Labai daug enumeration prasideda nuo protokolo supratimo: kas veikia, kokiam porte, kokius commands palaiko, ar siunčia data plaintext, ar reikalauja encryption.
---

## 2. Core protocols mental model
Network pagrindas:
```txt
IP = kur yra hostas
Port = kokia paslauga/procesas tame hoste
Protocol = kokia kalba kalbama su ta paslauga
```
Pvz:
```txt
8.8.8.8:53      -> DNS
example.com:80  -> HTTP
example.com:443 -> HTTPS
server:21       -> FTP
mail:25         -> SMTP
mail:110        -> POP3
mail:143        -> IMAP
```
Svarbu:
> Portas pats savaime nėra garantija, bet dažnai duoda stiprų hintą, kokia paslauga veikia.
Pvz `80/tcp open http` reiškia:
- hostas pasiekiamas
- TCP portas 80 atviras
- tikėtina, kad ten kalba HTTP
---

## 3. DNS
DNS reiškia:
> Domain Name System
Paprastai:
> DNS = vardų vertimo sistema, kuri domain vardus paverčia į IP adresus.
Žmogui lengviau prisiminti:
```txt
tryhackme.com
```
nei:
```txt
104.x.x.x
```
DNS atsako į klausimą:
> Koks IP adresas priklauso šitam domain vardui?
Pvz:
```txt
tryhackme.com -> IP address
```
DNS svarbus, nes browseriui/serveriui galiausiai reikia IP.
Domain vardas yra patogus žmogui.
IP adresas reikalingas network komunikacijai.
---

## 4. DNS veikimo eiga
Kai įvedi:
```txt
https://example.com
```
supaprastinta eiga:
1. Browseris/OS patikrina cache.
2. Jei cache nėra, klausia DNS resolverio.
3. Resolveris gali klausti root DNS serverių.
4. Tada TLD serverių.
5. Tada authoritative DNS serverio.
6. Gaunamas IP adresas.
7. Browseris jungiasi prie to IP.
Paprastai:
> DNS įvyksta prieš HTTP/HTTPS connection, nes pirma reikia žinoti, kur jungtis.
Svarbu:
> Jei DNS neveikia, website gali atrodyti “neveikiantis”, nors IP/portas realiai veikia.
Pvz:
```bash
ping 8.8.8.8
```
veikia, bet:
```bash
ping google.com
```
neveikia.
Tikėtina:
> DNS problema.
---

## 5. DNS records
Dažni DNS record tipai:
- A
- AAAA
- CNAME
- MX
- TXT
- NS
- SOA
A record:
> domain -> IPv4 address
Pvz:
```txt
example.com -> 93.184.216.34
```
AAAA record:
> domain -> IPv6 address
CNAME:
> alias į kitą domain vardą
Pvz:
```txt
www.example.com -> example.com
```
MX:
> mail servers domainui
TXT:
> text records, dažnai SPF/DKIM/verification
NS:
> nameservers domainui
SOA:
> authoritative zone information
Security/enumeration mintis:
> DNS records gali atskleisti mail serverius, subdomainus, hosting/CDN info ir kitus infrastruktūros clues.
---

## 6. DNS portai
DNS dažniausiai naudoja:
```txt
UDP 53
```
TCP 53 irgi naudojamas.
Dažnai:
- UDP 53 = normalios greitos DNS queries
- TCP 53 = zone transfer, dideli atsakymai, fallback
Svarbu:
> DNS nėra vien tik UDP. TCP 53 irgi egzistuoja.
Komandos:
```bash
nslookup example.com
```
```bash
dig example.com
```
Trumpas answer:
```bash
dig +short example.com
```
MX records:
```bash
dig MX example.com
```
NS records:
```bash
dig NS example.com
```
TXT records:
```bash
dig TXT example.com
```
---

## 7. DNS security mintys
DNS pats iš savęs tradiciškai nėra encrypted.
Tai reiškia:
- DNS queries gali būti matomos networke
- DNS gali leakinti kokius domainus useris lanko
- DNS spoofing/cache poisoning yra security temos
- blogas DNS config gali nukreipti userius ne ten
Svarbios temos vėliau:
- DNS over HTTPS
- DNS over TLS
- DNSSEC
- zone transfer misconfig
- subdomain enumeration
Šitam lygy užtenka:
> DNS yra vardų vertimas. Jei DNS blogai, visas web experience gali griūti, nors serveris gyvas.
---

## 8. WHOIS
WHOIS yra protokolas/paslauga domain registracijos informacijai gauti.
Paprastai:
> WHOIS = kas užregistravo domainą ir kokia registracijos info prieinama.
WHOIS gali parodyti:
- registrar
- registration date
- expiry date
- nameservers
- registrant organization, jei nepaslėpta
- contact info, jei neprivatizuota
- domain status
Komanda:
```bash
whois example.com
```
Svarbu:
> Dėl privacy/GDPR daug asmeninės info dažnai būna paslėpta/redacted.
Security/enumeration mintis:
> WHOIS gali padėti suprasti domain savininką, registratorių, nameserverius ir infrastruktūros kontekstą.
---

## 9. WHOIS praktinė vertė
WHOIS naudinga:
- recon
- domain ownership check
- registracijos datos tikrinimui
- nameserver info
- phishing domain analizėje
- ar domain naujas/įtartinas
Pvz phishing tyrime:
```txt
Banko domainas registruotas prieš 12 metų = normaliau
Panašus domainas registruotas vakar = įtartina
```
Svarbu:
> WHOIS nėra “exploit”. Tai public information lookup.
---

## 10. HTTP
HTTP reiškia:
> HyperText Transfer Protocol
Paprastai:
> HTTP = protokolas, kuriuo browseris ir web serveris keičiasi requests/responses.
Dažnas portas:
```txt
TCP 80
```
HTTP veikia request/response principu:
```txt
Client -> HTTP Request -> Server
Client <- HTTP Response <- Server
```
Request pavyzdys:
```http
GET / HTTP/1.1
Host: example.com
```
Response pavyzdys:
```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>Hello</html>
```
Svarbu:
> HTTP yra plaintext, jei nenaudojamas TLS/HTTPS.
---

## 11. HTTPS
HTTPS reiškia:
> HTTP Secure
Paprastai:
> HTTPS = HTTP per TLS encryption.
Dažnas portas:
```txt
TCP 443
```
HTTPS suteikia:
- confidentiality
- integrity
- server authentication per certificate
Tai reiškia:
- traffic šifruotas
- sunkiau skaityti pakeliui
- padeda apsisaugoti nuo MITM
Svarbu:
> HTTPS nereiškia, kad website neturi vulnerabilities. Tai tik reiškia, kad transportas šifruotas.
Website su HTTPS vis tiek gali turėti:
- SQL injection
- XSS
- IDOR
- auth bugs
- logic bugs
- insecure upload
---

## 12. HTTP praktinės komandos
GET request:
```bash
curl http://example.com/
```
Response headers:
```bash
curl -I http://example.com/
```
Headers + body:
```bash
curl -i http://example.com/
```
Sekti redirectus:
```bash
curl -L http://example.com/
```
Manual HTTP per telnet:
```bash
telnet example.com 80
```
Tada:
```http
GET / HTTP/1.1
Host: example.com

```
Svarbu:
> Tuščia eilutė po headers baigia HTTP requestą.
Jei request “kabo”, dažnai pamiršta tuščia eilutė arba blogas Host/header formatas.
---

## 13. FTP
FTP reiškia:
> File Transfer Protocol
Paprastai:
> FTP = protokolas failų siuntimui tarp client ir server.
Dažni portai:
```txt
TCP 21 = control connection
TCP 20 = data connection active mode atveju
```
FTP naudojamas:
- upload files
- download files
- list directories
- manage remote files
Basic FTP komandos:
```txt
USER
PASS
LIST
PWD
CWD
RETR
STOR
QUIT
```
Linux client:
```bash
ftp MACHINE_IP
```
Arba:
```bash
ftp example.com
```
Svarbu:
> Klasikinis FTP siunčia credentials ir data plaintext.
---

## 14. FTP active vs passive trumpai
FTP turi control ir data connection.
Control connection:
```txt
client -> server TCP 21
```
Data connection priklauso nuo mode.
Active mode:
> serveris jungiasi atgal į client data port.
Passive mode:
> client jungiasi į serverio nurodytą data port.
Praktinė mintis:
> Passive FTP dažnai geriau veikia per NAT/firewall, nes client inicijuoja connections į serverį.
Šitam lygy nereikia giliai kapstytis, bet verta žinoti:
> FTP nėra vienas paprastas connection kaip HTTP. Jis turi control ir data kanalus.
---

## 15. FTP security
Klasikinis FTP nėra encrypted.
Tai reiškia:
- username gali keliauti plaintext
- password gali keliauti plaintext
- files gali keliauti plaintext
Saugūs variantai:
- FTPS = FTP su TLS
- SFTP = SSH File Transfer Protocol
Svarbu:
> SFTP nėra “FTP per TLS”. SFTP yra atskiras protokolas, veikiantis per SSH.
Dažnas SFTP portas:
```txt
TCP 22
```
Security mintis:
> Jei matai FTP, galvok apie plaintext credentials, anonymous login, writable directories ir sensitive files.
---

## 16. SMTP
SMTP reiškia:
> Simple Mail Transfer Protocol
Paprastai:
> SMTP = email siuntimo protokolas.
SMTP naudojamas:
- client siunčia email į mail serverį
- mail serveriai perduoda email vienas kitam
Dažni portai:
```txt
TCP 25  = server-to-server SMTP
TCP 587 = mail submission su STARTTLS/auth
TCP 465 = SMTPS / implicit TLS
```
Basic SMTP commands:
```txt
HELO / EHLO
MAIL FROM:
RCPT TO:
DATA
QUIT
```
Svarbu:
> SMTP skirtas siųsti email, ne skaityti inbox.
---

## 17. SMTP manual pavyzdys
Prisijungti:
```bash
telnet MACHINE_IP 25
```
Supaprastintas dialogas:
```smtp
HELO test.local
MAIL FROM:<alice@example.com>
RCPT TO:<bob@example.com>
DATA
Subject: Test

Hello Bob
.
QUIT
```
Taškas vienoje eilutėje:
```txt
.
```
reiškia:
> email body pabaiga.
Svarbu:
> Realūs SMTP serveriai dažnai reikalauja auth/TLS ir neleidžia relaying.
---

## 18. SMTP security
SMTP problemos:
- open relay
- spoofing
- plaintext auth, jei be TLS
- phishing
- weak mail configuration
- missing SPF/DKIM/DMARC
TXT records dažnai naudojami email security:
- SPF
- DKIM
- DMARC
Security mintis:
> Email security nėra vien SMTP. Reikia žiūrėti DNS records, authentication, TLS ir anti-spoofing controls.
---

## 19. POP3
POP3 reiškia:
> Post Office Protocol version 3
Paprastai:
> POP3 = email parsisiuntimo iš mailbox protokolas.
Dažni portai:
```txt
TCP 110 = POP3 plaintext
TCP 995 = POP3S / POP3 over TLS
```
POP3 dažnai:
- prisijungia prie mailbox
- parsisiunčia emails
- gali ištrinti juos iš serverio
- mažiau tinkamas sync keliems devices
Basic POP3 commands:
```txt
USER
PASS
STAT
LIST
RETR
DELE
QUIT
```
Svarbu:
> POP3 labiau “download email” modelis, ne modernus multi-device sync.
---

## 20. POP3 manual pavyzdys
Prisijungti:
```bash
telnet MACHINE_IP 110
```
Dialogas:
```pop3
USER username
PASS password
STAT
LIST
RETR 1
QUIT
```
`STAT`:
> parodo messages count ir mailbox size.
`LIST`:
> parodo messages sąrašą.
`RETR 1`:
> parsiunčia pirmą message.
Svarbu:
> Jei POP3 serveris reikalauja secure connection, naudok POP3S portą 995 su TLS-capable tool, pvz openssl.
Pvz:
```bash
openssl s_client -connect MACHINE_IP:995 -crlf -quiet
```
---

## 21. IMAP
IMAP reiškia:
> Internet Message Access Protocol
Paprastai:
> IMAP = email access/sync protokolas, kai laiškai lieka serveryje.
Dažni portai:
```txt
TCP 143 = IMAP plaintext
TCP 993 = IMAPS / IMAP over TLS
```
IMAP leidžia:
- skaityti email serveryje
- sync tarp kelių devices
- folders
- flags
- read/unread state
- server-side mailbox management
IMAP labiau tinka:
- phone + laptop + webmail sync
- moderniam email naudojimui
Svarbu:
> IMAP sinchronizuoja mailbox. POP3 labiau parsisiunčia laiškus.
---

## 22. IMAP manual pavyzdys
IMAP komandos turi tags.
Pvz:
```bash
telnet MACHINE_IP 143
```
Dialogas:
```imap
A001 LOGIN username password
A002 LIST "" "*"
A003 SELECT INBOX
A004 FETCH 1 BODY[]
A005 LOGOUT
```
Kodėl `A001`, `A002`?
> IMAP naudoja command tags, kad clientas galėtų susieti responses su commands.
IMAPS:
```bash
openssl s_client -connect MACHINE_IP:993 -crlf -quiet
```
---

## 23. SMTP vs POP3 vs IMAP
Trumpai:
```txt
SMTP = siunčia email
POP3 = parsisiunčia email
IMAP = syncina/valdo email serveryje
```
Lentelė:
| Protokolas | Paskirtis | Default portai |
|---|---|---|
| SMTP | Email siuntimas | 25, 587, 465 |
| POP3 | Email parsisiuntimas | 110, 995 |
| IMAP | Email sync/access | 143, 993 |
Svarbu:
> Email sistema dažniausiai naudoja kelis protokolus: SMTP siuntimui, IMAP/POP3 gavimui.
---

## 24. Plaintext vs encrypted protocols
Daug senų/core protokolų pradžioje buvo plaintext.
Plaintext reiškia:
> data matoma skaitomu tekstu networke.
Rizika:
- credentials gali būti sniffinami
- content gali būti matomas
- MITM gali perimti ar pakeisti traffic
Plaintext protokolai:
- HTTP
- FTP
- SMTP be TLS
- POP3 be TLS
- IMAP be TLS
Saugesni variantai:
- HTTPS
- FTPS
- SFTP
- SMTPS / STARTTLS
- POP3S
- IMAPS
Core mintis:
> Jei protokolas siunčia password be encryption, tai nėra saugu nepatikimame networke.
---

## 25. Telnet kaip mokymosi įrankis
Telnet pats yra senas plaintext remote terminal protokolas.
Bet šituose roomuose telnet dažnai naudojamas kaip mokymosi įrankis:
> prisijungti prie TCP porto ir rankomis parašyti application layer commands.
Pvz:
```bash
telnet MACHINE_IP 80
telnet MACHINE_IP 21
telnet MACHINE_IP 25
telnet MACHINE_IP 110
telnet MACHINE_IP 143
```
Svarbu:
> Telnet nėra saugus remote admin protokolas. SSH pakeitė Telnet administravimui.
Bet telnet naudinga suprasti:
- HTTP requestus
- SMTP dialogą
- POP3 commands
- IMAP commands
- kaip protokolai atrodo “po GUI”
---

## 26. OpenSSL kaip secure protocol test tool
Kai reikia kalbėti su TLS portais, telnet netinka.
Naudok:
```bash
openssl s_client -connect host:port -crlf -quiet
```
Pavyzdžiai:
HTTPS:
```bash
openssl s_client -connect example.com:443 -crlf -quiet
```
POP3S:
```bash
openssl s_client -connect MACHINE_IP:995 -crlf -quiet
```
IMAPS:
```bash
openssl s_client -connect MACHINE_IP:993 -crlf -quiet
```
SMTPS:
```bash
openssl s_client -connect MACHINE_IP:465 -crlf -quiet
```
Svarbu:
> Telnet tinka plaintext. OpenSSL s_client tinka TLS/SSL servisams testuoti rankomis.
---

## 27. Ports summary
| Protocol | Purpose | Port |
|---|---|---|
| DNS | domain -> IP | UDP/TCP 53 |
| WHOIS | domain registration info | TCP 43 |
| HTTP | web plaintext | TCP 80 |
| HTTPS | web encrypted | TCP 443 |
| FTP control | file transfer control | TCP 21 |
| FTP data | file transfer data active mode | TCP 20 |
| SMTP | mail transfer | TCP 25 |
| SMTP submission | authenticated mail submit | TCP 587 |
| SMTPS | SMTP over TLS | TCP 465 |
| POP3 | receive/download mail | TCP 110 |
| POP3S | POP3 over TLS | TCP 995 |
| IMAP | mailbox sync/access | TCP 143 |
| IMAPS | IMAP over TLS | TCP 993 |
| SSH/SFTP | secure shell/file transfer | TCP 22 |
| Telnet | plaintext remote terminal | TCP 23 |
---

## 28. Nmap praktinė vertė
Nmap padeda pamatyti, kokie core protocols veikia hoste.
Basic scan:
```bash
nmap MACHINE_IP
```
Service detection:
```bash
nmap -sV MACHINE_IP
```
Specific ports:
```bash
nmap -sV -p 21,22,25,53,80,110,143,443,465,587,993,995 MACHINE_IP
```
Pavyzdys:
```txt
21/tcp open ftp
25/tcp open smtp
80/tcp open http
110/tcp open pop3
143/tcp open imap
```
Interpretacija:
- 21 -> tikrinti FTP
- 25 -> SMTP
- 80 -> HTTP
- 110 -> POP3
- 143 -> IMAP
Svarbu:
> Atviras portas yra kvietimas suprasti servisą, ne automatinis exploit.
---

## 29. Wireshark/tcpdump praktinė vertė
Šitie protokolai gerai matosi packet capture.
Plaintext protokolai gali rodyti:
- HTTP request lines
- FTP USER/PASS
- SMTP commands
- POP3 USER/PASS
- IMAP LOGIN
Jeigu protokolas encrypted:
- matysi TCP connection
- matysi TLS handshake
- nematysi turinio paprastu tekstu
Tcpdump HTTP:
```bash
sudo tcpdump -i any port 80 -A
```
Tcpdump DNS:
```bash
sudo tcpdump -i any port 53
```
Tcpdump mail:
```bash
sudo tcpdump -i any 'port 25 or port 110 or port 143' -A
```
Svarbu:
> Plaintext protocols packet capture’e atrodo kaip tekstas. Encrypted protocols atrodo kaip šifruota data.
---

## 30. Praktinė vertė white hat mokymuisi
Šitas roomas svarbus, nes moko ne tik ports, o protokolų elgesį.
Kur naudosi:
- service enumeration
- nmap interpretation
- manual protocol interaction
- Wireshark/tcpdump analysis
- web basics
- mail server testing basics
- FTP checks
- DNS recon
- phishing/email investigation basics
Checklist:
```txt
Koks portas atviras?
Koks service?
Ar service plaintext?
Ar reikalauja login?
Ar leidžia anonymous?
Ar matosi banner?
Ar palaiko TLS?
Ar galima kalbėti su service rankomis?
Ar DNS/WHOIS duoda papildomų clues?
```
---

## 31. Basic legal test flow
Tik legaliose lab aplinkose.
1. Skenuoji target:
```bash
nmap -sV MACHINE_IP
```
2. Žiūri atvirus portus.
3. Pagal portą/protokolą pasirenki įrankį:
```txt
80/443 -> curl/browser
21 -> ftp
25 -> telnet/ncat/openssl pagal TLS
110/143 -> telnet arba openssl TLS portams
53 -> dig/nslookup
```
4. Bandai suprasti service elgesį.
5. Užsirašai:
```txt
service, port, version, plaintext/encrypted, auth required, interesting response
```
Svarbu:
> Manual protocol interaction yra mokymasis, ne brute forcingas.
---

## 32. Komandos
DNS:
```bash
dig example.com
dig +short example.com
dig MX example.com
dig NS example.com
dig TXT example.com
nslookup example.com
```
WHOIS:
```bash
whois example.com
```
HTTP:
```bash
curl http://MACHINE_IP/
curl -I http://MACHINE_IP/
curl -i http://MACHINE_IP/
```
Manual HTTP:
```bash
telnet MACHINE_IP 80
```
```http
GET / HTTP/1.1
Host: MACHINE_IP

```
HTTPS:
```bash
curl -I https://example.com/
openssl s_client -connect example.com:443 -crlf -quiet
```
FTP:
```bash
ftp MACHINE_IP
```
SMTP:
```bash
telnet MACHINE_IP 25
```
POP3:
```bash
telnet MACHINE_IP 110
```
POP3S:
```bash
openssl s_client -connect MACHINE_IP:995 -crlf -quiet
```
IMAP:
```bash
telnet MACHINE_IP 143
```
IMAPS:
```bash
openssl s_client -connect MACHINE_IP:993 -crlf -quiet
```
Nmap:
```bash
nmap -sV MACHINE_IP
nmap -sV -p 21,22,25,53,80,110,143,443,465,587,993,995 MACHINE_IP
```
---

## 33. Mano paaiškinimas žmonių kalba
Core protocols yra kaip skirtingos įstaigos tame pačiame mieste.
DNS:
> Telefonų knyga. Pasako adresą pagal vardą.
WHOIS:
> Registrų centras. Pasako domain registracijos info.
HTTP:
> Web užklausa bare: “duok puslapį”.
FTP:
> Sandėlis failams įkelti/parsisiųsti.
SMTP:
> Paštininkas, kuris išsiunčia laišką.
POP3:
> Nueini į paštą, pasiimi laiškus į rankas.
IMAP:
> Pašto dėžutė lieka serveryje, o tavo telefonas/laptopas tik syncina vaizdą.
Encryption:
> Užklijuotas vokas. Be jo visi pakeliui gali skaityti.
Svarbiausia:
> Protokolas pasako, kaip kalbėtis su paslauga. Portas tik parodo, prie kurių durų belstis.
---

## 34. Ką būtina atsiminti
- DNS verčia domain vardus į IP.
- DNS dažniausiai naudoja UDP 53, bet TCP 53 irgi egzistuoja.
- WHOIS rodo domain registration info.
- HTTP naudoja TCP 80 ir yra plaintext.
- HTTPS naudoja TCP 443 ir TLS encryption.
- FTP naudoja TCP 21 control connection.
- Klasikinis FTP siunčia data/credentials plaintext.
- SFTP nėra tas pats kas FTPS.
- SMTP siunčia email.
- POP3 parsisiunčia email.
- IMAP syncina mailbox tarp devices.
- POP3 portai: 110 plaintext, 995 secure.
- IMAP portai: 143 plaintext, 993 secure.
- SMTP portai: 25, 587, 465.
- Telnet gali būti naudojamas mokymuisi kalbėti su plaintext TCP servisais.
- OpenSSL s_client naudojamas rankomis kalbėti su TLS servisais.
- Plaintext protokolai packet capture’e gali rodyti credentials.
- Encrypted protokolai slepia content, bet ne visą metadata.
---

## 35. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- DNSSEC gylį
- DNS cache poisoning praktines atakas
- zone transfer exploitation
- SMTP relay abuse gylį
- SPF/DKIM/DMARC pilną konfigūraciją
- IMAP folders/flags advanced commands
- FTP active/passive troubleshooting gylį
- TLS certificate chain gylį
- ALPN/SNI detales
- STARTTLS downgrade attacks
Dabar svarbiausia:
> Suprasti ką daro DNS, WHOIS, HTTP, FTP, SMTP, POP3 ir IMAP; kokius portus naudoja; kurie yra plaintext; kaip su jais basic lygiu kalbėti.
---

## 36. Mini testas sau
Atsakyk nežiūrėdamas:
1. Ką daro DNS?
2. Koks DNS default portas?
3. Kodėl DNS gali naudoti ir TCP?
4. Ką rodo A record?
5. Ką rodo MX record?
6. Ką rodo TXT record?
7. Kam naudojamas WHOIS?
8. Koks WHOIS portas?
9. Kuo HTTP skiriasi nuo HTTPS?
10. Kokie HTTP ir HTTPS portai?
11. Ar HTTPS reiškia, kad website neturi vulnerabilities?
12. Kam naudojamas FTP?
13. Koks FTP control portas?
14. Kodėl klasikinis FTP nesaugus?
15. Kuo SFTP skiriasi nuo FTPS?
16. Kam naudojamas SMTP?
17. Kokie dažni SMTP portai?
18. Kam naudojamas POP3?
19. Kokie POP3/POP3S portai?
20. Kam naudojamas IMAP?
21. Kokie IMAP/IMAPS portai?
22. Kuo POP3 skiriasi nuo IMAP?
23. Kodėl telnet tinka mokytis plaintext protokolų?
24. Kada vietoj telnet naudotum openssl s_client?
25. Kodėl plaintext credentials pavojingi?
26. Ką pirmiausia darytum pamatęs `21/tcp open ftp`?
27. Ką pirmiausia darytum pamatęs `25/tcp open smtp`?
28. Ką pirmiausia darytum pamatęs `110/tcp open pop3`?
29. Ką reiškia manual HTTP requeste tuščia eilutė po headers?
30. Kodėl protokolo supratimas svarbiau nei tik porto numerio žinojimas?
---

## 37. Mano verdiktas
Šitas roomas yra labai praktiškas, nes perkelia tave nuo “žinau kas yra IP/portas” prie “suprantu kas kalba per tą portą”.
Statusas:
```txt
Status: Foundation / Core Protocols
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš service enumeration, nmap, Wireshark/tcpdump, web/email/FTP rooms
```
Svarbiausia mintis:
> Portas atidaro duris, bet protokolas pasako, kaip kalbėti su tuo, kas už durų.
Core mental model:
```txt
DNS   = vardas -> IP
WHOIS = domain registration info
HTTP  = web request/response
FTP   = file transfer
SMTP  = email sending
POP3  = email download
IMAP  = email sync/access
TLS   = encryption layer, kad turinys nebūtų plaintext
```
Jeigu supranti šituos core protocols, `nmap` output pradeda atrodyti ne kaip portų sąrašas, o kaip realių paslaugų žemėlapis.
