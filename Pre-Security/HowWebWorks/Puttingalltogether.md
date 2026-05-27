# THM – Putting It All Together

## 1. Room esmė
Šitas roomas sujungia ankstesnius web pagrindus į vieną bendrą vaizdą.
Pagrindinė mintis:
> Website nėra vienas serveris ir vienas HTML failas. Realiame pasaulyje tarp browserio ir website dažnai stovi DNS, cache, CDN, WAF, load balancer, web serveris, aplikacija ir database.
Šitas roomas dengia:
- DNS/cache logiką
- CDN
- load balancer
- health checks
- databases
- WAF
- web serverius
- virtual hosts
- static vs dynamic content
- backend code
- request kelionę nuo browserio iki website
Cybersecurity kontekste svarbiausia:
> Reikia suprasti ne tik “puslapis atsidarė”, o kokius komponentus requestas galėjo praeiti iki serverio.
---

## 2. Bendras web request kelias
Kai browseryje įvedi website, pvz:
```txt
https://tryhackme.com
```
vyksta ne vienas paprastas veiksmas.
Supaprastinta eiga:
1. Browseris gauna URL
2. Tikrina local cache dėl IP
3. Jei IP nėra cache, klausia DNS
4. DNS randa domain IP
5. Browseris jungiasi prie serverio per 80 arba 443 portą
6. Request gali praeiti per WAF
7. Request gali praeiti per load balancer
8. Load balancer siunčia requestą į vieną backend serverį
9. Web serveris gauna HTTP requestą
10. Web aplikacija gali kalbėti su database
11. Serveris grąžina response
12. Browseris renderina HTML/CSS/JS į matomą puslapį
Paprastai:
> Browseris mato website. Po kapotu vyksta visa infrastruktūros grandinė.
---

## 3. DNS ir cache
Prieš jungiantis prie website, browseriui reikia sužinoti IP adresą.
Domain vardas:
```txt
tryhackme.com
```
turi būti paverstas į IP adresą.
Tai daro DNS:
> DNS = domain vardų vertimas į IP adresus.
Bet pirmiausia sistema dažnai tikrina cache:
- browser cache
- OS cache
- local DNS cache
- recursive DNS resolver cache
Kodėl?
> Kad nereikėtų kiekvieną kartą klausti DNS nuo nulio.
Jeigu IP yra cache:
- greičiau
- mažiau DNS traffic
- mažiau latency
Jeigu IP nėra cache:
- klausiama recursive DNS serverio
- jis gali klausti root serverių
- tada TLD serverių
- tada authoritative DNS serverio
Svarbi mintis:
> DNS yra vienas pirmų žingsnių prieš realų HTTP/HTTPS requestą.
---

## 4. CDN
CDN reiškia:
> Content Delivery Network
Paprastai:
> CDN = geografiškai paskirstyti serveriai, kurie hostina static files ar cached content arčiau vartotojo.
CDN dažnai hostina:
- JavaScript files
- CSS files
- images
- videos
- fonts
- static assets
Kodėl CDN naudinga:
- greičiau kraunasi puslapis
- mažiau apkrovos origin serveriui
- content gaunamas iš arčiau esančio serverio
- geresnis performance globaliems vartotojams
- gali padėti su availability
Pavyzdys:
```txt
User UK -> CDN node London
User Germany -> CDN node Frankfurt
User US -> CDN node New York
```
Vietoj to, kad visi trauktų image iš vieno serverio kitame pasaulio gale.
Security mintis:
> CDN gali slėpti origin serverį ir sumažinti load, bet CDN nėra automatinis aplikacijos saugumas.
Jeigu backend turi SQLi ar IDOR, CDN to stebuklingai neišspręs.
---

## 5. Static content
Static content reiškia turinį, kuris nesikeičia pagal kiekvieną user/requestą.
Paprastai:
> Static content = tas pats failas visiems.
Pavyzdžiai:
- images
- CSS
- JavaScript
- fonts
- videos
- static HTML files
Pvz:
```txt
/logo.png
/style.css
/app.js
```
Static content gerai tinka CDN.
Kodėl?
- failas tas pats
- galima cache’inti
- galima platinti per CDN
- nereikia kiekvieną kartą generuoti serveryje
Svarbi mintis:
> Static file nėra “nesvarbus”. JS failuose kartais būna endpoints, config, hardcoded keys ar debug info.
---

## 6. Dynamic content
Dynamic content reiškia turinį, kuris gali keistis pagal requestą, userį, session ar database data.
Paprastai:
> Dynamic content = serveris sugeneruoja arba pakeičia atsakymą pagal situaciją.
Pavyzdžiai:
- user profile page
- shopping cart
- bank account dashboard
- forum comments
- search results
- admin panel
- personalized homepage
Pvz:
```txt
/user/15/profile
/search?q=tattoo
/orders
```
Dynamic content gali priklausyti nuo:
- logged-in userio
- cookies
- permissions
- database
- query parameters
- POST body
- backend logic
Security mintis:
> Dauguma rimtų web vulnerabilities atsiranda dynamic content ir backend logic vietose.
Pvz:
- SQL injection
- IDOR
- broken access control
- auth bypass
- business logic bugs
---

## 7. Load balancer
Load balancer yra komponentas, kuris paskirsto traffic tarp kelių serverių.
Paprastai:
> Load balancer = traffic reguliuotojas tarp clientų ir backend serverių.
Schema:
```txt
User -> Load Balancer -> Web Server 1
                      -> Web Server 2
                      -> Web Server 3
```
Kodėl naudojamas load balancer:
- vienas serveris gali neatlaikyti traffic
- galima paskirstyti apkrovą
- galima turėti failover
- galima išimti mirusį serverį iš rotacijos
- galima gerinti availability
Load balancing algoritmai gali būti:
- round-robin
- least connections
- weighted
- IP hash
Paprastai:
> Load balancer leidžia website veikti stabiliau, kai yra daug userių arba keli backend serveriai.
---

## 8. Health check
Health check yra load balancer atliekamas patikrinimas, ar backend serveris gyvas ir veikia.
Paprastai:
> Health check = periodinis klausimas serveriui “ar tu dar gyvas ir normaliai atsakai?”
Pvz load balancer tikrina:
```txt
GET /health
```
Jeigu serveris atsako:
```txt
200 OK
```
load balancer laiko jį sveiku.
Jeigu serveris neatsako arba grąžina blogą statusą:
```txt
500
timeout
connection refused
```
load balancer gali nustoti siųsti jam traffic.
Kodėl tai svarbu:
- useriai nesiunčiami į mirusį serverį
- sistema tampa patikimesnė
- galima deployinti serverius po vieną
- galima greičiau aptikti backend problemas
Security/troubleshooting mintis:
> Kartais website neveikia ne todėl, kad “serveris mirė”, o todėl, kad load balancer išėmė backend iš rotacijos dėl failed health checks.
---

## 9. Database
Database saugo informaciją, kurios reikia web aplikacijai.
Paprastai:
> Database = vieta, kur web aplikacija saugo ir skaito data.
Database gali saugoti:
- users
- passwords hashes
- profiles
- comments
- orders
- products
- messages
- permissions
- sessions
- logs
Dažnos database technologijos:
- MySQL
- PostgreSQL
- MSSQL
- MongoDB
- SQLite
- Redis
Web request pavyzdys:
```txt
User atidaro /profile
Web app patikrina session cookie
Web app klausia database userio duomenų
Database grąžina data
Web app sukuria response
Browseris parodo profile
```
Security mintis:
> Jeigu web app neteisingai dirba su input ir database, atsiranda SQL injection ar kitos injection problemos.
Svarbu:
> Clientas nemato tiesioginės database komunikacijos. Jis mato tik serverio grąžintą response.
---

## 10. WAF
WAF reiškia:
> Web Application Firewall
Paprastai:
> WAF = apsauginis sluoksnis tarp user requesto ir web serverio.
Schema:
```txt
User -> WAF -> Load Balancer -> Web Server -> App -> Database
```
WAF gali bandyti blokuoti:
- SQL injection payloads
- XSS payloads
- malicious headers
- suspicious request patterns
- known exploit signatures
- bots
- brute force patterns
- denial of service tipo traffic
Svarbu:
> WAF padeda gintis, bet nėra stebuklinga siena.
WAF gali:
- blokuoti akivaizdžius payloadus
- sumažinti noise
- padėti nuo known attacks
- loginti įtartinus requestus
Bet WAF negali garantuoti:
- kad aplikacija neturi logic bugs
- kad nėra IDOR
- kad authorization gera
- kad backend saugus
- kad nėra neteisingos konfiguracijos
Security mintis:
> WAF yra papildomas sluoksnis, ne pakaitalas saugiam kodui.
---

## 11. Web serveris
Web serveris priima HTTP/HTTPS requestus ir grąžina response.
Paprastai:
> Web serveris = software, kuris klauso 80/443 porto ir aptarnauja web requestus.
Dažni web serveriai:
- Apache
- Nginx
- Microsoft IIS
- Caddy
Web serveris gali:
- serve’inti static files
- perduoti requestus backend aplikacijai
- valdyti virtual hosts
- daryti redirectus
- tvarkyti TLS
- loginti requestus
- taikyti basic access rules
Pvz:
```txt
GET /index.html -> grąžina HTML file
GET /api/user -> perduoda backend app
```
Security mintis:
> Web serverio headers, klaidos, config ir enabled features gali atskleisti technologijas ar misconfig.
Pvz:
```http
Server: Apache/2.4.49
```
Tokia info gali padėti fingerprintinti.
---

## 12. Virtual Hosts
Virtual Hosts leidžia vienam web serveriui hostinti kelias svetaines/domainus.
Paprastai:
> Virtual host = web serverio config, leidžiantis skirtingus domainus aptarnauti iš to paties serverio/IP.
Pvz vienas serveris:
```txt
example.com
shop.example.com
admin.example.com
blog.example.net
```
Visi gali būti ant to paties IP, bet web serveris pagal `Host` header nusprendžia, kurį site aptarnauti.
HTTP request:
```http
GET / HTTP/1.1
Host: admin.example.com
```
Web serveris mato:
```txt
Host: admin.example.com
```
ir parenka tinkamą virtual host config.
Security/pentest mintis:
> Virtual hosts reiškia, kad IP scan ne visada parodo visus website’us. Reikia ieškoti subdomainų ir vhostų.
Pvz:
```txt
target.com
dev.target.com
admin.target.com
test.target.com
```
Visi gali rodyti skirtingą content net per tą patį IP.
---

## 13. Backend code
Backend code veikia serveryje.
Paprastai:
> Backend = serverio pusės logika, kurios clientas tiesiogiai nemato.
Backend gali būti rašomas:
- PHP
- Python
- Node.js
- Ruby
- Java
- C#
- Go
Backend daro:
- login
- permissions checks
- database queries
- file upload handling
- email sending
- payment processing
- business logic
Clientas gauna:
- HTML
- JSON
- status code
- cookies
- response headers
Clientas nemato:
- backend source code
- database credentials
- server-side variables
- internal function logic
Nebent viskas blogai sukonfigūruota ir įvyksta leak.
Svarbi mintis:
> View Page Source nerodo backend code. Jis rodo tik tai, ką serveris jau sugeneravo ir atsiuntė browseriui.
---

## 14. Static vs dynamic web server response
Static pavyzdys:
```txt
GET /logo.png
```
Serveris tiesiog grąžina tą patį failą.
Dynamic pavyzdys:
```txt
GET /profile
Cookie: session=abc123
```
Serveris:
- patikrina session
- randa userį
- klausia database
- sugeneruoja profile page
- grąžina individualų response
Skirtumas:
```txt
Static = failas
Dynamic = serverio sugeneruotas atsakymas pagal requestą/data
```
Security mintis:
> Dynamic responses turi daugiau attack surface, nes į juos įeina input, session, database ir logic.
---

## 15. Request kelias kaip viena grandinė
Supaprastintas pilnas kelias:
```txt
Browser
  -> DNS/cache
  -> CDN arba origin
  -> WAF
  -> Load Balancer
  -> Web Server
  -> Backend Application
  -> Database
  -> Backend Application
  -> Web Server
  -> Browser
```
Ne kiekvienas website turės visus komponentus.
Mažas website gali būti:
```txt
Browser -> Web Server -> Static HTML
```
Didelis website gali būti:
```txt
Browser -> CDN -> WAF -> Load Balancer -> Web Server -> App Servers -> Database Cluster
```
Svarbiausia:
> Kuo didesnė sistema, tuo daugiau tarpinių komponentų gali keisti, filtruoti, cache’inti arba nukreipti requestus.
---

## 16. Kodėl pentesteriui svarbi visa grandinė
Pentesteris turi suprasti, kurioje vietoje kas vyksta.
Pvz:
- DNS rodo į CDN, bet origin serveris gali būti kitur
- WAF blokuoja payloadus, bet backend vis tiek gali būti vulnerable
- load balancer gali duoti skirtingus atsakymus iš skirtingų backendų
- virtual hosts gali slėpti admin/dev panelius
- database klaidos gali pasirodyti per web response
- static JS files gali išduoti API endpoints
- dynamic content gali turėti IDOR
Praktinis mąstymas:
```txt
Ar matau tik CDN/WAF?
Ar žinau origin?
Ar yra subdomainų?
Ar yra virtual hosts?
Ar response skiriasi pagal Host header?
Ar cookies keičia dynamic content?
Ar app kalba su database?
Ar errorai leakina backend info?
```
---

## 17. Basic troubleshooting modelis
Jeigu website neveikia, eik sluoksniais:
1. Ar domain resolve’inasi?
```bash
dig example.com
```
2. Ar host pasiekiamas?
```bash
ping example.com
```
3. Ar 80/443 portai atviri?
```bash
nmap -p 80,443 example.com
```
4. Ar HTTP/HTTPS atsako?
```bash
curl -I https://example.com
```
5. Ar yra redirect?
```bash
curl -IL https://example.com
```
6. Ar atsako skirtingai pagal Host header?
```bash
curl -H "Host: admin.example.com" http://IP_ADDRESS/
```
7. Ar matosi WAF/CDN headers?
```bash
curl -I https://example.com
```
8. Ar response turi server/backend leakų?
```bash
curl -i https://example.com
```
Svarbu:
> Nešok iškart prie “exploit”. Pirma suprask grandinę.
---

## 18. Komandos / įrankiai
DNS lookup:
```bash
dig example.com
```
Trumpas DNS answer:
```bash
dig +short example.com
```
Patikrinti nameservers:
```bash
dig NS example.com
```
Patikrinti HTTP headers:
```bash
curl -I https://example.com
```
Pilnas response su headers:
```bash
curl -i https://example.com
```
Sekti redirectus:
```bash
curl -IL https://example.com
```
Nmap web ports:
```bash
nmap -p 80,443 example.com
```
Service/version detection:
```bash
nmap -sV -p 80,443 example.com
```
Tik IP be DNS:
```bash
curl -I http://IP_ADDRESS/
```
Testuoti virtual host per Host header:
```bash
curl -H "Host: example.com" http://IP_ADDRESS/
```
Testuoti kitą vhost:
```bash
curl -H "Host: admin.example.com" http://IP_ADDRESS/
```
Ieškoti CDN/WAF clues headers:
```bash
curl -I https://example.com | grep -i "server\|cf-\|x-\|via"
```
---

## 19. Praktinė vertė white hat mokymuisi
Šitas roomas svarbus, nes sujungia atskirus gabalus:
- DNS
- HTTP
- web servers
- CDN
- WAF
- load balancing
- databases
- backend/frontend skirtumą
Kur tai naudosi:
- web enumeration
- subdomain discovery
- vhost discovery
- WAF/CDN fingerprinting
- origin server thinking
- HTTP request analysis
- troubleshooting THM labs
- Burp Suite
- PortSwigger
- realų web app pentesting
Praktinis checklist:
```txt
Koks domain?
Koks IP?
Ar čia CDN?
Ar yra WAF?
Ar matau realų origin?
Ar yra subdomainų?
Ar yra virtual hosts?
Ar request eina į static ar dynamic content?
Ar backend kalba su database?
Ar response rodo server technologiją?
Ar yra cookies/session?
Ar klaidos leakina informaciją?
```
---

## 20. Mano paaiškinimas žmonių kalba
Website yra kaip restoranas su visa virtuve, ne tik padavėju.
Browseris:
> Klientas, kuris pateikia užsakymą.
DNS:
> Telefonų knyga, kuri pasako adresą.
CDN:
> Sandėliai arčiau klientų su paruoštais produktais.
WAF:
> Apsauga prie durų, kuri tikrina ar neatėjai su peiliu tarp dantų.
Load balancer:
> Vadybininkas, kuris paskirsto klientus laisviems padavėjams.
Web serveris:
> Padavėjas, kuris priima užsakymą.
Backend app:
> Virtuvė, kur priimami realūs sprendimai.
Database:
> Sandėlis/receptų knyga/klientų sąrašas.
Browser render:
> Klientas gauna patiekalą ant stalo.
Svarbiausia:
> Iš išorės matai tik lėkštę. Pentesteris turi suprasti visą virtuvės grandinę.
---

## 21. Ką būtina atsiminti
- Website requestas dažnai praeina per daug komponentų.
- DNS paverčia domain į IP.
- Cache leidžia greičiau naudoti jau žinomą informaciją.
- CDN hostina/cachina static content arčiau userio.
- Static content dažniausiai nesikeičia pagal userį.
- Dynamic content keičiasi pagal requestą, userį, session ar database.
- Load balancer paskirsto traffic tarp backend serverių.
- Health check tikrina ar backend serveris gyvas.
- Database saugo web aplikacijos data.
- WAF filtruoja įtartinus/malicious web requestus.
- WAF nėra pakaitalas saugiam kodui.
- Web serveris aptarnauja HTTP/HTTPS requestus.
- Virtual hosts leidžia vienam serveriui hostinti kelis domainus.
- Backend code clientui tiesiogiai nematomas.
- View Source rodo tik tai, ką serveris atsiuntė, ne serverio source code.
- Didelėje sistemoje problema gali būti bet kuriame grandinės taške.
---

## 22. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- CDN cache poisoning
- advanced WAF bypass
- load balancer sticky sessions
- reverse proxy internals
- Kubernetes ingress
- service mesh
- database replication
- database sharding
- HAProxy/Nginx config gylį
- TLS termination architektūrą
- origin discovery advanced technikas
Dabar svarbiausia:
> Suprasti bendrą web infrastruktūros grandinę ir kas už ką atsakingas.
---

## 23. Mini testas sau
Atsakyk nežiūrėdamas:
1. Ką daro DNS?
2. Kodėl naudojamas cache?
3. Ką reiškia CDN?
4. Kokį content dažniausiai hostina CDN?
5. Kuo static content skiriasi nuo dynamic content?
6. Ką daro load balancer?
7. Kas yra health check?
8. Kas nutinka, jei backend serveris failina health check?
9. Kam web aplikacijai reikia database?
10. Ką reiškia WAF?
11. Kurioje vietoje grandinėje dažnai stovi WAF?
12. Ar WAF garantuoja, kad aplikacija saugi?
13. Ką daro web serveris?
14. Kas yra virtual host?
15. Kaip `Host` header susijęs su virtual hosts?
16. Ar clientas mato backend code?
17. Kuo skiriasi View Source nuo backend source code?
18. Kodėl subdomainai/vhostai svarbūs enumeration metu?
19. Kodėl CDN gali trukdyti pamatyti origin serverį?
20. Kodėl pentesteriui reikia suprasti visą request grandinę?
---

## 24. Mano verdiktas
Šitas roomas nėra gilus techniškai, bet jis labai gerai sujungia visus ankstesnius web basics.
Statusas:
```txt
Status: Foundation / Connecting Room
Depth: Low-Medium
Practical value: High
Revisit: verta prieš web enumeration, Burp Suite, PortSwigger ir realų web app testing
```
Svarbiausia mintis:
> Website yra grandinė, ne vienas puslapis.
Jeigu supranti:
- DNS/cache
- CDN
- WAF
- load balancer
- web server
- backend app
- database
- browser render
tada web infrastruktūra pradeda atrodyti kaip mechanika, ne migla.
Core rule:
> Kai testuoji website, visada klausk: kuriam grandinės taškui aš dabar kalbu?
