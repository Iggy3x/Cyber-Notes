# THM – Networking Secure Protocols

## 1. Room esmė
Šitas roomas aiškina, kaip nesaugūs/plaintext protokolai tampa saugesni naudojant encryption, authentication ir tunneling.
Pagrindinė mintis:
> Plaintext protocol leidžia matyti data pakeliui. Secure protocol prideda encryption, integrity ir dažnai authentication, kad traffic nebūtų lengvai skaitomas ar pakeičiamas.
Šitas roomas dengia:
- TLS
- HTTPS
- SMTPS
- POP3S
- IMAPS
- SSH
- SFTP
- FTPS
- VPN
Cybersecurity kontekste svarbiausia:
> Saugus protokolas nepadaro aplikacijos automatiškai saugios, bet apsaugo traffic kelionėje tarp client ir server.
---

## 2. Plaintext vs secure protocols
Plaintext reiškia:
> Data keliauja skaitomu tekstu.
Plaintext protokolai:
- HTTP
- FTP
- SMTP be TLS
- POP3 be TLS
- IMAP be TLS
- Telnet
Rizikos:
- credentials gali būti sniffinami
- content gali būti matomas
- attacker gali stebėti traffic
- MITM gali pakeisti data
Secure variantai:
- HTTP -> HTTPS
- FTP -> FTPS arba SFTP
- SMTP -> SMTPS / SMTP su STARTTLS
- POP3 -> POP3S
- IMAP -> IMAPS
- Telnet -> SSH
Svarbu:
> Secure protokolai dažniausiai prideda encryption, bet nepašalina visų application vulnerabilities.
---

## 3. TLS
TLS reiškia:
> Transport Layer Security
Paprastai:
> TLS = encryption protocol, kuris apsaugo data tarp client ir server.
TLS suteikia:
- confidentiality
- integrity
- authentication
Confidentiality:
> pašaliniai negali lengvai skaityti traffic.
Integrity:
> galima aptikti, jei data buvo pakeista pakeliui.
Authentication:
> clientas gali patikrinti, ar kalba su tikru serveriu.
TLS dažnai naudojamas su:
- HTTPS
- SMTPS
- POP3S
- IMAPS
- FTPS
Svarbu:
> TLS nėra atskira aplikacija. Tai saugumo sluoksnis, kuris apsaugo kitų protokolų traffic.
---

## 4. SSL vs TLS
SSL reiškia:
> Secure Sockets Layer
TLS yra modernesnis SSL įpėdinis.
Praktikoje žmonės dar sako “SSL certificate”, bet dažniausiai realiai kalba apie TLS.
Svarbu:
> SSL yra senas/deprecated. Šiuolaikinė saugi komunikacija turėtų naudoti TLS.
Praktinis wording:
- “SSL certificate” = dažnas buitinės kalbos terminas
- “TLS certificate” = techniškai tikslesnis terminas
---

## 5. Ką TLS apsaugo
TLS apsaugo traffic in transit.
Paprastai:
> TLS saugo data kelionėje tarp client ir server.
TLS padeda nuo:
- passive sniffing
- credential theft over network
- traffic tampering
- basic MITM, jei certificate validacija gera
TLS neapsaugo nuo:
- SQL injection
- XSS
- IDOR
- weak passwords
- stolen session cookies iš pačio browserio
- infected endpoint
- blogos authorization
- server-side vulnerabilities
Svarbu:
> HTTPS/TLS reiškia “ryšys šifruotas”, ne “website saugus”.
---

## 6. TLS certificates
TLS certificate leidžia serveriui įrodyti savo tapatybę.
Certificate dažnai turi:
- domain name
- public key
- issuer / Certificate Authority
- validity dates
- signature
- subject alternative names
CA reiškia:
> Certificate Authority
CA yra trusted entity, kuri pasirašo certificate.
Paprastai:
> Browseris pasitiki CA. Jei CA pasirašė certificate, browseris gali pasitikėti, kad domain priklauso tam serveriui.
Jei certificate blogas:
- expired
- wrong domain
- self-signed
- untrusted CA
browseris rodys warning.
Svarbu:
> Certificate leidžia patikrinti identity, bet pats vienas neištaiso vulnerable website.
---

## 7. TLS handshake supaprastintai
TLS handshake metu client ir server susitaria dėl saugaus ryšio.
Labai supaprastinta eiga:
1. Client jungiasi prie serverio.
2. Client ir server susitaria dėl TLS version/cipher suite.
3. Server pateikia certificate.
4. Client patikrina certificate.
5. Abi pusės susitaria dėl session keys.
6. Toliau traffic šifruojamas symmetric encryption.
Svarbu:
> Asymmetric crypto dažnai naudojama identity/key exchange, o bulk data šifruojama greitesne symmetric crypto.
Šitam lygy nereikia eiti į matematiką.
Svarbiausia:
> TLS leidžia dviem pusėms susikurti saugų šifruotą kanalą per nesaugų networką.
---

## 8. HTTPS
HTTPS reiškia:
> HyperText Transfer Protocol Secure
Paprastai:
> HTTPS = HTTP per TLS.
Default portas:
```txt
TCP 443
```
HTTP default portas:
```txt
TCP 80
```
HTTPS suteikia:
- šifruotą web traffic
- server certificate validation
- apsaugą nuo plaintext sniffing
- apsaugą nuo traffic tampering pakeliui
Pvz:
```txt
https://example.com
```
Svarbu:
> Browserio spynelė reiškia, kad connection encrypted ir certificate validus. Ji nereiškia, kad website patikimas ar nevulnerable.
---

## 9. HTTPS praktinės komandos
Patikrinti headers:
```bash
curl -I https://example.com
```
Pilnas response su headers:
```bash
curl -i https://example.com
```
Sekti redirectus:
```bash
curl -IL http://example.com
```
Patikrinti TLS certificate:
```bash
openssl s_client -connect example.com:443 -servername example.com
```
Su quiet:
```bash
openssl s_client -connect example.com:443 -servername example.com -quiet
```
Svarbu:
> `-servername` prideda SNI, kad serveris žinotų, kurio domain certificate pateikti, jei ant vieno IP yra keli HTTPS virtual hosts.
---

## 10. SNI
SNI reiškia:
> Server Name Indication
Paprastai:
> SNI leidžia clientui TLS handshake metu pasakyti, prie kokio hostname jungiasi.
Kodėl reikia?
> Vienas IP gali hostinti daug HTTPS websites. Serveriui reikia žinoti, kurį certificate parodyti.
Pvz:
```bash
openssl s_client -connect 1.2.3.4:443 -servername example.com
```
Svarbu:
> Be SNI gali gauti neteisingą certificate arba default site.
---

## 11. Secure email protocols
Email protokolai turi plaintext ir secure variantus.
Plaintext:
```txt
SMTP  25 / kartais 587 su STARTTLS
POP3  110
IMAP  143
```
Secure:
```txt
SMTPS 465
POP3S 995
IMAPS 993
```
Svarbu:
> “S” dažnai reiškia secure/TLS variantą.
Bet yra du modeliai:
- implicit TLS
- STARTTLS
Implicit TLS:
> connection nuo pradžių šifruotas.
STARTTLS:
> pradedama plaintext, tada komanda upgrade’ina connection į TLS.
---

## 12. SMTPS
SMTPS = SMTP secured with TLS.
Dažni portai:
```txt
TCP 465 = SMTPS / implicit TLS
TCP 587 = SMTP submission su STARTTLS
TCP 25  = server-to-server SMTP, gali naudoti STARTTLS
```
SMTP paskirtis:
> email siuntimas.
Svarbu:
> SMTP nėra skirtas inbox skaitymui. Tam naudojamas POP3 arba IMAP.
Manual TLS test:
```bash
openssl s_client -connect MACHINE_IP:465 -crlf -quiet
```
SMTP su STARTTLS test:
```bash
openssl s_client -starttls smtp -connect MACHINE_IP:587 -crlf -quiet
```
---

## 13. POP3S
POP3S = POP3 secured with TLS.
Default portas:
```txt
TCP 995
```
POP3 plaintext:
```txt
TCP 110
```
POP3 paskirtis:
> parsisiųsti email iš mailbox.
POP3 basic commands:
```txt
USER
PASS
STAT
LIST
RETR
QUIT
```
POP3S test:
```bash
openssl s_client -connect MACHINE_IP:995 -crlf -quiet
```
Svarbu:
> POP3S apsaugo credentials ir email content nuo plaintext sniffing.
---

## 14. IMAPS
IMAPS = IMAP secured with TLS.
Default portas:
```txt
TCP 993
```
IMAP plaintext:
```txt
TCP 143
```
IMAP paskirtis:
> email access/sync tarp serverio ir devices.
IMAP basic commands su tags:
```imap
A001 LOGIN username password
A002 LIST "" "*"
A003 SELECT INBOX
A004 FETCH 1 BODY[]
A005 LOGOUT
```
IMAPS test:
```bash
openssl s_client -connect MACHINE_IP:993 -crlf -quiet
```
Svarbu:
> IMAP geriau tinka multi-device sync nei POP3.
---

## 15. STARTTLS
STARTTLS yra mechanizmas upgrade’inti plaintext connection į TLS.
Paprastai:
> STARTTLS = pradžioje kalbam plaintext, tada susitariame pereiti į encrypted TLS.
Naudojama su:
- SMTP
- IMAP
- POP3
Pvz SMTP:
```bash
openssl s_client -starttls smtp -connect MACHINE_IP:587 -crlf -quiet
```
Svarbu:
> STARTTLS nėra tas pats kas implicit TLS. Implicit TLS šifruoja nuo pirmo momento, STARTTLS pradeda plaintext ir tada upgrade’ina.
Security mintis:
> Jei STARTTLS neteisingai enforced, gali atsirasti downgrade rizikų.
---

## 16. SSH
SSH reiškia:
> Secure Shell
Paprastai:
> SSH = saugus remote terminal access.
Default portas:
```txt
TCP 22
```
SSH pakeitė Telnet kaip saugesnis remote admin protokolas.
Telnet:
- plaintext
- credentials matomi
- nesaugu
SSH:
- encrypted
- gali naudoti password arba keys
- suteikia secure remote shell
SSH naudojama:
- remote Linux admin
- server management
- file transfer per SFTP/SCP
- port forwarding
- tunneling
Sintaksė:
```bash
ssh user@MACHINE_IP
```
Su portu:
```bash
ssh user@MACHINE_IP -p 2222
```
---

## 17. SSH authentication
SSH gali naudoti:
- password authentication
- public key authentication
Password auth:
> prisijungi su username/password.
Public key auth:
> naudoji private key client pusėje ir public key server pusėje.
Key-based auth dažnai saugesnis, jei:
- private key saugomas saugiai
- key turi passphrase
- password login išjungtas arba ribojamas
Basic key generation:
```bash
ssh-keygen
```
Private key:
```txt
~/.ssh/id_rsa
```
Public key:
```txt
~/.ssh/id_rsa.pub
```
Authorized keys serveryje:
```txt
~/.ssh/authorized_keys
```
Svarbu:
> Private key niekada nedalinamas. Public key gali būti įkeltas į serverį.
---

## 18. SSH host key
Pirmą kartą jungiantis per SSH gali matyti:
```txt
The authenticity of host ... can't be established.
Are you sure you want to continue connecting?
```
Tai susiję su serverio host key.
Host key padeda patikrinti:
> ar jungiesi prie to paties serverio kaip anksčiau.
Jei host key pasikeičia be priežasties, gali būti:
- serveris perinstaliuotas
- IP dabar priklauso kitam serveriui
- galimas MITM
Svarbu:
> THM labuose host key warning dažnai normalu dėl naujai deploy’intų machines, bet realiame production reikia atsargumo.
---

## 19. SCP
SCP reiškia:
> Secure Copy Protocol
Paprastai:
> SCP = failų kopijavimas per SSH.
Upload:
```bash
scp file.txt user@MACHINE_IP:/home/user/
```
Download:
```bash
scp user@MACHINE_IP:/home/user/file.txt .
```
Su portu:
```bash
scp -P 2222 file.txt user@MACHINE_IP:/home/user/
```
Svarbu:
> `ssh` naudoja `-p` portui, `scp` naudoja `-P` didžiąją P.
---

## 20. SFTP
SFTP reiškia:
> SSH File Transfer Protocol
Paprastai:
> SFTP = secure file transfer per SSH.
Default portas:
```txt
TCP 22
```
Prisijungti:
```bash
sftp user@MACHINE_IP
```
SFTP komandos:
```txt
ls
pwd
cd
get file.txt
put file.txt
exit
```
Svarbu:
> SFTP nėra tas pats kas FTPS. SFTP veikia per SSH. FTPS yra FTP su TLS.
---

## 21. FTPS
FTPS reiškia:
> FTP Secure / FTP over TLS
Paprastai:
> FTPS = klasikinis FTP protokolas, apsaugotas TLS.
FTPS gali būti:
- explicit FTPS
- implicit FTPS
Explicit FTPS:
> pradedama kaip FTP, tada naudojama AUTH TLS upgrade.
Implicit FTPS:
> TLS nuo pradžių.
Portai gali skirtis:
```txt
TCP 21 = explicit FTPS control
TCP 990 = implicit FTPS control
```
Svarbu:
> FTPS vis dar turi FTP control/data connection modelį. Tai gali būti sudėtingiau per firewalls/NAT.
---

## 22. SFTP vs FTPS
Trumpai:
```txt
SFTP = file transfer per SSH
FTPS = FTP su TLS
```
Lentelė:
| Savybė | SFTP | FTPS |
|---|---|---|
| Bazė | SSH | FTP + TLS |
| Default portas | 22 | 21/990 |
| Channels | vienas SSH connection | FTP control + data channels |
| Firewall/NAT | paprasčiau | gali būti sudėtingiau |
| Tas pats kaip FTP? | ne | taip, su TLS |
Svarbu:
> SFTP ir FTPS nėra sinonimai.
---

## 23. VPN
VPN reiškia:
> Virtual Private Network
Paprastai:
> VPN = encrypted tunnel per nepatikimą networką.
VPN leidžia:
- saugiai prisijungti prie private network
- šifruoti traffic tarp client ir VPN serverio
- pasiekti internal resources
- dirbti remote
- sukurti virtualų network interface
THM atveju OpenVPN leidžia tavo Warhorse prisijungti prie THM lab network.
Po prisijungimo dažnai atsiranda:
```txt
tun0
```
Linux patikrinti:
```bash
ip a
ip route
```
Svarbu:
> VPN nėra magija. Jis sukuria tunnel ir routes į kitą networką.
---

## 24. VPN kaip tunnel
Tunnel reiškia:
> tavo traffic supakuojamas ir šifruojamas per kitą connection.
Supaprastinta:
```txt
Tavo laptopas -> encrypted VPN tunnel -> VPN server -> private network
```
Be VPN:
```txt
Tavo laptopas negali pasiekti THM target 10.10.x.x
```
Su VPN:
```txt
Tavo laptopas gauna route į THM network per tun0
```
Praktinis troubleshooting:
```bash
ip a | grep tun0
ip route
ping -c 4 MACHINE_IP
nmap MACHINE_IP
```
Svarbu:
> Jei VPN neveikia, target IP gali būti nepasiekiamas net jei target yra gyvas.
---

## 25. VPN security mintys
VPN apsaugo traffic tarp client ir VPN serverio.
Bet:
- VPN provider gali matyti dalį traffic metadata
- endpoint vis tiek gali būti infected
- VPN nepadaro website saugiu
- VPN neapsaugo nuo phishing
- VPN neapsaugo nuo blogų passwords
- split tunneling gali keisti kas eina per VPN
Svarbu:
> VPN yra secure tunnel, ne absoliutus invisibility cloak.
Cyber praktikoje VPN svarbus:
- remote work
- lab access
- internal network testing
- segmentation
- encrypted transport
---

## 26. Secure protocols ports summary
| Protocol | Purpose | Port |
|---|---|---|
| HTTPS | HTTP over TLS | TCP 443 |
| SMTPS | SMTP over TLS | TCP 465 |
| SMTP STARTTLS | Mail submission | TCP 587 |
| POP3S | POP3 over TLS | TCP 995 |
| IMAPS | IMAP over TLS | TCP 993 |
| SSH | Secure remote shell | TCP 22 |
| SFTP | File transfer over SSH | TCP 22 |
| FTPS explicit | FTP over TLS | TCP 21 |
| FTPS implicit | FTP over TLS | TCP 990 |
| OpenVPN | VPN | UDP/TCP 1194 default |
| WireGuard | VPN | UDP 51820 common |
Svarbu:
> Portai gali būti pakeisti. Default port yra hintas, ne įrodymas.
---

## 27. OpenSSL praktika
`openssl s_client` leidžia rankomis testuoti TLS services.
HTTPS:
```bash
openssl s_client -connect example.com:443 -servername example.com
```
SMTPS:
```bash
openssl s_client -connect MACHINE_IP:465 -crlf -quiet
```
SMTP STARTTLS:
```bash
openssl s_client -starttls smtp -connect MACHINE_IP:587 -crlf -quiet
```
POP3S:
```bash
openssl s_client -connect MACHINE_IP:995 -crlf -quiet
```
IMAPS:
```bash
openssl s_client -connect MACHINE_IP:993 -crlf -quiet
```
Svarbu:
> Telnet tinka plaintext servisams. OpenSSL s_client tinka TLS servisams.
---

## 28. Nmap praktika
Service/version detection:
```bash
nmap -sV MACHINE_IP
```
Secure protocol ports:
```bash
nmap -sV -p 22,443,465,587,993,995,990 MACHINE_IP
```
TLS certificate info scripts:
```bash
nmap --script ssl-cert -p 443 MACHINE_IP
```
TLS ciphers:
```bash
nmap --script ssl-enum-ciphers -p 443 MACHINE_IP
```
SSH info:
```bash
nmap -sV -p 22 MACHINE_IP
```
Svarbu:
> Nmap gali parodyti ar portas open ir kokia service/version, bet saugumo vertinimui reikia suprasti protokolą.
---

## 29. Wireshark/tcpdump praktinė vertė
Plaintext traffic:
> gali matytis commands, credentials, content.
TLS/SSH/VPN traffic:
> content šifruotas, bet metadata vis dar gali matytis.
Matysi:
- IPs
- ports
- timing
- packet sizes
- TLS handshake
- certificate info kartais
Nematysi:
- HTTP body per HTTPS
- FTP credentials per FTPS
- email content per IMAPS/POP3S
- SSH session commands
Tcpdump HTTPS:
```bash
sudo tcpdump -i any port 443
```
Tcpdump SSH:
```bash
sudo tcpdump -i any port 22
```
Svarbu:
> Encryption slepia content, bet ne visą network metadata.
---

## 30. Praktinė vertė white hat mokymuisi
Šitas roomas svarbus, nes moko atskirti:
- plaintext vs encrypted
- protocol vs secure wrapper
- SSH vs Telnet
- FTP vs FTPS vs SFTP
- HTTP vs HTTPS
- VPN as tunnel
Kur naudosi:
- nmap service enumeration
- Wireshark/tcpdump analysis
- web testing
- email protocol testing
- SSH access
- file transfer
- THM VPN troubleshooting
- basic security assessment
Checklist:
```txt
Ar service plaintext ar encrypted?
Koks portas?
Ar naudojamas TLS?
Ar certificate validus?
Ar galima prisijungti saugiai?
Ar yra weak/deprecated protocol?
Ar service leidžia password auth?
Ar VPN route veikia?
Ar tai SFTP ar FTPS?
```
---

## 31. Basic legal test flow
Tik legaliose lab aplinkose.
1. Skenuoji:
```bash
nmap -sV MACHINE_IP
```
2. Žiūri secure protocol portus:
```txt
22, 443, 465, 587, 993, 995, 990
```
3. Jei TLS service:
```bash
openssl s_client -connect MACHINE_IP:PORT -crlf -quiet
```
4. Jei HTTPS:
```bash
curl -I https://MACHINE_IP/
```
5. Jei SSH:
```bash
ssh user@MACHINE_IP
```
6. Jei SFTP:
```bash
sftp user@MACHINE_IP
```
7. Jei VPN:
```bash
ip a
ip route
```
Svarbu:
> Tikslas nėra brute force. Tikslas yra suprasti kokia service, ar encrypted, ir kaip ji elgiasi.
---

## 32. Mano paaiškinimas žmonių kalba
Plaintext protocol yra kaip atvirukas:
> visi pakeliui gali perskaityti.
TLS/SSH/VPN yra kaip užklijuotas ir užrakintas vokas:
> turinys paslėptas, bet vis dar gali matytis kam ir kada siunčiama.
HTTPS:
> saugesnis web.
SMTPS/POP3S/IMAPS:
> saugesnis email transportas.
SSH:
> saugus remote terminalas.
SFTP:
> failų pernešimas per SSH.
FTPS:
> senas FTP su TLS šalmu.
VPN:
> užšifruotas tunelis į kitą networką.
Svarbiausia:
> Encryption saugo kelionę, bet ne visada saugo tai, kas laukia kelionės gale.
---

## 33. Ką būtina atsiminti
- TLS apsaugo traffic in transit.
- SSL yra senas, TLS yra modernus variantas.
- HTTPS = HTTP over TLS.
- HTTPS portas yra TCP 443.
- HTTPS nereiškia, kad website neturi vulnerabilities.
- Certificate padeda patikrinti server identity.
- CA pasirašo certificates.
- SNI padeda vienam IP hostinti kelis HTTPS domainus.
- SMTPS/POP3S/IMAPS yra email protokolų secure variantai.
- STARTTLS upgrade’ina plaintext connection į TLS.
- SSH pakeitė Telnet kaip secure remote shell.
- SSH default portas yra TCP 22.
- SSH gali naudoti password arba key auth.
- Private SSH key negalima dalintis.
- SCP kopijuoja failus per SSH.
- SFTP veikia per SSH.
- FTPS yra FTP su TLS.
- SFTP ir FTPS nėra tas pats.
- VPN sukuria encrypted tunnel.
- THM OpenVPN dažnai sukuria `tun0`.
- Secure protocol slepia content, bet metadata gali likti matoma.
---

## 34. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- TLS cipher suite matematiką
- certificate chain gylį
- OCSP/CRL detales
- TLS 1.2 vs 1.3 handshake pilną mechaniką
- SSH key exchange algoritmus
- SSH agent forwarding rizikas
- advanced VPN protocols
- IPsec gylį
- WireGuard internals
- STARTTLS downgrade attack praktikas
- mTLS enterprise konfiguracijas
Dabar svarbiausia:
> Suprasti, kurie protokolai yra plaintext, kurie secure, ką duoda TLS/SSH/VPN, ir kaip basic lygiu testuoti secure services.
---

## 35. Mini testas sau
Atsakyk nežiūrėdamas:
1. Ką reiškia TLS?
2. Kuo TLS skiriasi nuo SSL?
3. Kokius 3 pagrindinius dalykus suteikia TLS?
4. Ar HTTPS reiškia, kad website neturi vulnerabilities?
5. Koks HTTPS default portas?
6. Kas yra TLS certificate?
7. Kas yra CA?
8. Kas yra SNI?
9. Kodėl `openssl s_client` naudinga?
10. Kuo implicit TLS skiriasi nuo STARTTLS?
11. Kas yra SMTPS?
12. Kokie SMTPS/SMTP submission portai?
13. Kas yra POP3S?
14. Koks POP3S portas?
15. Kas yra IMAPS?
16. Koks IMAPS portas?
17. Ką reiškia SSH?
18. Koks SSH default portas?
19. Kodėl SSH geriau už Telnet?
20. Kuo SSH password auth skiriasi nuo key auth?
21. Kur laikomas private SSH key?
22. Kuo SCP skiriasi nuo SFTP?
23. Kas yra SFTP?
24. Kas yra FTPS?
25. Kuo SFTP skiriasi nuo FTPS?
26. Kas yra VPN?
27. Ką VPN reiškia kaip tunnel?
28. Kas yra `tun0` THM kontekste?
29. Ar VPN padaro tave “nematomą”?
30. Kodėl encrypted traffic vis tiek turi matomą metadata?
---

## 36. Mano verdiktas
Šitas roomas uždaro core networking protokolų bloką saugumo kampu.
Statusas:
```txt
Status: Foundation / Secure Protocols
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš Wireshark, tcpdump, Nmap, web testing, SSH usage, VPN troubleshooting
```
Svarbiausia mintis:
> Secure protocol pirmiausia saugo data kelionėje. Jis nesutvarko blogos aplikacijos, blogų slaptažodžių ar blogos autorizacijos.
Core mental model:
```txt
HTTP  -> HTTPS  = web + TLS
SMTP  -> SMTPS  = email sending + TLS
POP3  -> POP3S  = email download + TLS
IMAP  -> IMAPS  = email sync + TLS
Telnet -> SSH   = remote shell + encryption
FTP   -> FTPS   = FTP + TLS
FTP   -> SFTP   = file transfer over SSH
VPN   = encrypted tunnel į kitą networką
```
Jeigu šitą supranti, tada `nmap` output su secure services nebėra vien portų sąrašas. Tai tampa klausimu:
> “Koks protokolas, ar jis šifruotas, kaip jis autentifikuoja, ir ką aš galiu legaliai patikrinti?”
