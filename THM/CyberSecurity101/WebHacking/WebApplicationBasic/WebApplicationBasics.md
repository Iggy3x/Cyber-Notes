# Web Application Basics — pagrindiniai konceptai

## Web Application Overview

Web application — tai programa, kuri veikia per naršyklę.

Paprastai ji susideda iš kelių dalių:

- **Front end** — tai, ką mato ir su kuo sąveikauja vartotojas naršyklėje.
- **Back end** — serverio pusė, kurios vartotojas tiesiogiai nemato.
- **Database** — vieta, kur saugomi ir keičiami duomenys.
- **Infrastructure** — serveriai, tinklai, DNS, CDN, load balanceriai ir kiti komponentai.
- **Security components** — pvz. WAF, kuris bando filtruoti pavojingas užklausas.

---

## Front End

Front end yra web aplikacijos dalis, kurią matome naršyklėje.

Tai yra kaip aplikacijos „paviršius“:

- puslapio tekstas;
- mygtukai;
- formos;
- paveikslėliai;
- meniu;
- dizainas;
- interaktyvūs elementai.

Front end dažniausiai sudaro:

- **HTML**
- **CSS**
- **JavaScript**

Svarbu suprasti: front end kodas dažnai pasiekia vartotojo naršyklę, todėl jo negalima laikyti slapta ar saugia vieta.

---

## HTML

**HTML — HyperText Markup Language**

HTML yra pagrindinė web puslapio struktūra.

Jis nusako, kas bus puslapyje:

- antraštės;
- tekstas;
- nuorodos;
- formos;
- lentelės;
- paveikslėliai;
- mygtukai.

Paprastai tariant:

> HTML pasako naršyklei, kokie elementai turi būti rodomi.

Pvz. login forma, kuri turi username ir password laukus, dažniausiai bus aprašyta HTML struktūroje.

---

## CSS

**CSS — Cascading Style Sheets**

CSS apibūdina, kaip web puslapis atrodo vizualiai.

Jis atsakingas už:

- spalvas;
- šriftus;
- dydžius;
- išdėstymą;
- tarpus;
- animacijas;
- responsive dizainą.

Paprastai tariant:

> HTML pasako, kas yra puslapyje, o CSS pasako, kaip tai atrodo.

CSS nėra saugumo logika. Jis skirtas dizainui.

---

## JavaScript

**JavaScript / JS**

JavaScript yra front end kalba, kuri leidžia puslapiui būti interaktyviam.

JS gali:

- reaguoti į mygtuko paspaudimą;
- tikrinti formos laukus;
- siųsti užklausas į serverį;
- keisti puslapio turinį be reload;
- dirbti su cookies, localStorage ir sessionStorage;
- vykdyti logiką vartotojo naršyklėje.

Paprastai tariant:

> HTML yra struktūra, CSS yra išvaizda, o JavaScript yra elgesys.

Svarbus web hacking dalykas:

> JavaScript veikia kliento pusėje, todėl vartotojas gali jį matyti, analizuoti ir manipuliuoti.

Dėl to slapta informacija neturėtų būti laikoma JS kode.

---

## Back End

Back end yra serverio pusė, kurios vartotojas tiesiogiai nemato.

Ji atsakinga už:

- login sistemą;
- vartotojų autorizaciją;
- duomenų apdorojimą;
- bendravimą su database;
- API endpointus;
- sesijų valdymą;
- verslo logiką.

Pvz. kai vartotojas prisijungia prie puslapio, front end tik parodo formą, bet back end patikrina:

- ar username egzistuoja;
- ar password teisingas;
- ar vartotojas turi teisę prieiti prie tam tikro puslapio;
- kokius duomenis jam galima parodyti.

Svarbu:

> Tikra saugumo logika turi būti back end pusėje, ne vien front end.

---

## Database

Database yra vieta, kur web aplikacija saugo duomenis.

Ten gali būti saugoma:

- vartotojų paskyros;
- slaptažodžių hash’ai;
- el. pašto adresai;
- užsakymai;
- žinutės;
- produktai;
- sesijų duomenys;
- teisės/rolės;
- aplikacijos nustatymai.

Web aplikacija dažnai siunčia užklausas į database per back end.

Pvz.:

1. Vartotojas įveda login duomenis.
2. Front end nusiunčia juos į serverį.
3. Back end patikrina duomenis database.
4. Serveris grąžina atsakymą: login successful arba login failed.

Web hacking kontekste database svarbi dėl tokių pažeidžiamumų kaip:

- SQL Injection;
- data leakage;
- weak access control;
- insecure direct object references;
- netinkamai saugomi slaptažodžiai.

---

## Infrastructure

Infrastructure — tai visi papildomi komponentai, kurie leidžia web aplikacijai veikti.

Tai gali būti:

- web serveris;
- application serveris;
- DNS;
- CDN;
- load balancer;
- reverse proxy;
- firewall;
- WAF;
- cloud servisai;
- logging/monitoring sistemos;
- containeriai;
- Kubernetes;
- storage sistemos.

Paprastai tariant:

> Infrastructure yra visas techninis pagrindas po web aplikacija.

Web hacking metu svarbu suprasti, kad pažeidžiamumas gali būti ne tik pačiame kode, bet ir infrastruktūroje.

Pvz.:

- blogai sukonfigūruotas serveris;
- atviri nereikalingi portai;
- senos programų versijos;
- public admin panel;
- neteisingai sukonfigūruotas cloud bucket;
- silpni TLS/SSL nustatymai.

---

## WAF

**WAF — Web Application Firewall**

WAF yra saugumo komponentas, kuris stovi tarp vartotojo ir web aplikacijos.

Jo tikslas — filtruoti pavojingas HTTP užklausas.

WAF gali bandyti blokuoti:

- SQL Injection;
- XSS;
- command injection;
- path traversal;
- suspicious payloads;
- automated scanning;
- brute force bandymus.

Paprastai tariant:

> WAF yra kaip apsauginis prie klubo durų. Jis bando nepraleisti įtartinų užklausų.

Bet svarbu:

> WAF nėra magiškas skydas. Jis gali padėti, bet blogo kodo pilnai neišgelbės.

Web hacking kontekste WAF svarbus todėl, kad kartais testuojant aplikaciją užklausos gali būti blokuojamos arba pakeičiamos.

---

## Bendras vaizdas

Labai supaprastintas web aplikacijos kelias:

1. Vartotojas naršyklėje atidaro puslapį.
2. Naršyklė parodo front end: HTML, CSS, JavaScript.
3. Vartotojas atlieka veiksmą, pvz. spaudžia login.
4. JavaScript arba forma siunčia HTTP request į back end.
5. Back end apdoroja request.
6. Back end kreipiasi į database.
7. Database grąžina duomenis.
8. Back end grąžina response.
9. Front end parodo rezultatą vartotojui.

---

## Mano trumpas mental modelis

- **HTML** — ką matau.
- **CSS** — kaip atrodo.
- **JavaScript** — ką puslapis daro naršyklėje.
- **Back end** — kur vyksta tikra logika.
- **Database** — kur laikomi duomenys.
- **Infrastructure** — ant ko visa aplikacija stovi.
- **WAF** — filtras prieš pavojingas užklausas.

---

## Web hacking požiūris

Kai žiūriu į web aplikaciją, turiu klausti:

- Kas vyksta front end?
- Kokius requestus naršyklė siunčia į serverį?
- Kokie endpointai egzistuoja?
- Ar front end neslepia svarbios informacijos?
- Ar back end teisingai tikrina teises?
- Ar inputai tinkamai validuojami?
- Ar galima manipuliuoti requestais?
- Ar database gali būti pasiekta per pažeidžiamumą?
- Ar infrastruktūroje nėra blogos konfigūracijos?
- Ar WAF kažką blokuoja arba išduoda apie sistemą?

---

## Mano verdiktas

Šitas skyrius yra bazė. Čia dar nėra „hacking“ pilna prasme, bet tai yra žemėlapis, be kurio paskui viskas atrodo kaip magija.

Svarbiausia suprasti ne tik terminus, bet ryšį tarp jų:

> Naršyklė rodo front end, bet tikras darbas vyksta tarp requestų, back end logikos ir database.

Web hacking prasideda tada, kai pradedu stebėti, keisti ir testuoti tuos requestus.


 ____________________________________________________________________


# URL — Uniform Resource Locator

## Kas yra URL?

URL — Uniform Resource Locator**

URL yra web adresas, kuris leidžia pasiekti konkretų resursą internete.

Tas resursas gali būti:

- web puslapis;
- nuotrauka;
- video;
- failas;
- API endpointas;
- kitas online turinys.

Paprastai tariant:

> URL pasako naršyklei, kur internete eiti ir kokio resurso prašyti.

---

## URL anatomija

Pavyzdys:

http://user:password@tryhackme.com:80/view-room?id=1#task3


Šitas URL susideda iš kelių dalių:

http://          → scheme / protocol
user:password   → user info / credentials
tryhackme.com   → host / domain
:80             → port
/view-room      → path
?id=1           → query string
#task3          → fragment


Kiekviena URL dalis padeda naršyklei ir serveriui suprasti, kur reikia kreiptis ir kokio resurso prašyti.

---

## Scheme / Protocol

Pavyzdys:

http://
https://

**Scheme** nurodo, kokį protokolą naudoti prisijungimui prie resurso.

Dažniausi:

* **HTTP** — HyperText Transfer Protocol
* **HTTPS** — HyperText Transfer Protocol Secure

**HTTPS** yra saugesnis, nes ryšys tarp naršyklės ir serverio yra šifruotas.

Tai reiškia, kad trečiai pusei daug sunkiau skaityti arba keisti siunčiamus duomenis.

Paprastai tariant:

> HTTP yra paprastas ryšys, HTTPS yra šifruotas ryšys.

Dauguma modernių web aplikacijų privalo naudoti HTTPS, ypač jei yra:

* login forma;
* slaptažodžiai;
* mokėjimai;
* asmeniniai duomenys;
* sesijos/cookies.

---

## User Info / Credentials

Pavyzdys:

user:password@

Kai kurie URL gali turėti vartotojo prisijungimo informaciją.

Pilnas pavyzdys:

http://user:password@example.com

Čia:

* `user` — vartotojo vardas;
* `password` — slaptažodis.

Šitas metodas šiandien laikomas nesaugiu ir beveik nebenaudojamas.

Kodėl blogai?

* URL gali likti browser history;
* URL gali patekti į logs;
* URL gali būti nukopijuotas ar nutekintas;
* credentials gali matytis ekrane;
* tai prastas saugumo dizainas.

Paprastai tariant:

> Login duomenų dėjimas į URL yra senas ir nesaugus metodas.

---

## Host / Domain

Pavyzdys:

tryhackme.com

**Host** arba **domain** nurodo, į kurį serverį / svetainę naršyklė turi kreiptis.

Tai viena svarbiausių URL dalių.

Paprastai tariant:

> Domain pasako, kokią svetainę bandai pasiekti.

Pvz.:


tryhackme.com
google.com
github.com

Kiekvienas domain turi būti unikalus ir registruotas per domain registratorių.
---

## Domain saugumo perspektyva

Iš security pusės labai svarbu tikrinti domain vardą.

Atakose dažnai naudojami fake domainai, kurie atrodo panašiai į tikrus.

Pvz.:

tryhackme.com
tryhacl<me.com
tryhackrne.com
tryhackme-login.com


Toks metodas vadinamas **typosquatting**.

Tikslas — apgauti žmogų, kad jis manytų, jog eina į tikrą svetainę.

Tai dažnai naudojama:

* phishing atakose;
* fake login puslapiuose;
* malware platinime;
* credential harvesting.

Paprastai tariant:

> Visada žiūrėk ne tik į puslapio išvaizdą, bet ir į domain vardą.

---

## Port

Pavyzdys:

:80
:443

**Port** nurodo, į kokią paslaugą serveryje reikia kreiptis.

Serveris gali turėti daug servisų, veikiančių per skirtingus portus.

Dažni web portai:

* `80` — HTTP
* `443` — HTTPS

Paprastai tariant:

> IP / domain pasako, į kokį namą eini, o port pasako, pro kurias duris beldiesi.

Pvz.:

http://example.com:80
https://example.com:443

Jeigu URL porte nieko nenurodyta, naršyklė dažniausiai naudoja default portą pagal protokolą:

* HTTP → 80
* HTTPS → 443

---

## Path

Pavyzdys:

/view-room

**Path** nurodo konkretų resursą serveryje.

Tai gali būti:

* puslapis;
* failas;
* endpointas;
* aplikacijos route.

Pvz.:


/about
/login
/admin
/api/users
/view-room

Paprastai tariant:

> Path pasako, kokio konkretaus dalyko prašai iš svetainės.

Security perspektyva:

Path'ai turi būti tinkamai apsaugoti.

Pvz. vartotojas neturėtų galėti pasiekti:

/admin
/internal
/backup
/config

jeigu jis neturi tam teisės.

Svarbu:

> Vien tai, kad path nėra matomas meniu, nereiškia, kad jis saugus.

Serveris turi tikrinti autorizaciją back end pusėje.

---

## Query String

Pavyzdys:

?id=1

**Query string** prasideda nuo klaustuko `?`.

Ji dažnai naudojama perduoti papildomus parametrus serveriui.

Pvz.:

/search?q=tattoo
/view-room?id=1
/products?category=books
/user?id=25

Query string gali turėti kelis parametrus:

/products?category=books&page=2&sort=price

Čia:

* `category=books`
* `page=2`
* `sort=price`

Paprastai tariant:

> Query string yra papildomi duomenys, kuriuos naršyklė siunčia serveriui.

Security perspektyva:

Vartotojas gali lengvai keisti query string.

Pvz.:

/user?id=25

gali būti pakeista į:

/user?id=26

Todėl serveris turi tikrinti:

* ar vartotojas turi teisę matyti tą resursą;
* ar inputas validus;
* ar parametrai nėra naudojami nesaugiai;
* ar nėra injection galimybės.

Su query string gali būti susiję pažeidžiamumai:

* SQL Injection;
* XSS;
* IDOR;
* command injection;
* open redirect;
* parameter tampering.

---

## Fragment

Pavyzdys:

#task3

**Fragment** prasideda nuo hash simbolio `#`.

Jis dažniausiai naudojamas nukreipti naršyklę į konkrečią puslapio vietą.

Pvz.:

/page#section1
/docs#install
/room#task3

Paprastai tariant:

> Fragment padeda naršyklei nušokti į konkrečią puslapio dalį.

Svarbus dalykas:

> Fragment dažniausiai nėra siunčiamas į serverį.

Tai reiškia, kad serveris paprastai nemato `#task3`.

Fragment dažniausiai naudojamas naršyklės / front end pusėje.

Security perspektyva:

Kadangi JavaScript gali skaityti fragment reikšmę, blogai parašytas front end kodas gali naudoti fragment nesaugiai.

Tai gali būti susiję su:

* DOM-based XSS;
* client-side routing problemomis;
* neteisingu duomenų apdorojimu JavaScript'e.

Bet fragment nėra tas pats kaip query string.

---

## Full mental model

Pavyzdys:

https://tryhackme.com:443/room?id=1#task3

Galima galvoti taip:

https        → naudok saugų HTTPS ryšį
tryhackme.com → eik į šitą domain
:443         → naudok HTTPS portą
/room        → prašyk šito resurso / endpointo
?id=1        → perduok parametrą id=1 serveriui
#task3       → naršyklėje parodyk konkrečią puslapio sekciją
---

## Web hacking požiūris į URL

Kai matau URL, turiu klausti:

* Koks domain?
* Ar domain tikras, ar panašus į phishing?
* Ar naudojamas HTTP ar HTTPS?
* Ar URL yra port numeris?
* Koks path?
* Ar path atrodo kaip sensitive vieta?
* Ar yra query string?
* Ar galiu keisti parametrus?
* Ar serveris tikrina autorizaciją?
* Ar yra ID parametras?
* Ar galima testuoti IDOR?
* Ar inputas gali būti injection taškas?
* Ar fragment naudojamas JavaScript logikoje?
* Ar URL rodo API endpointą?

---

## Labai trumpas variantas

* **Scheme** — kokiu protokolu jungiamės.
* **User info** — login info URL'e, šiandien bloga praktika.
* **Host / Domain** — į kokią svetainę einame.
* **Port** — į kokį servisą serveryje kreipiamės.
* **Path** — kokio resurso prašome.
* **Query string** — papildomi parametrai serveriui.
* **Fragment** — konkreti vieta puslapyje, dažniausiai browserio pusėje.

---

## Mano verdiktas

URL atrodo paprastas dalykas, bet web hacking'e jis yra vienas iš pirmų žemėlapių.

Per URL dažnai matosi:

* aplikacijos struktūra;
* endpointai;
* parametrai;
* galimi IDOR taškai;
* injection taškai;
* admin/internal keliai;
* technologijų užuominos;
* blogos konfiguracijos.

Svarbiausia mintis:

> URL nėra tik adresas. Web security požiūriu URL yra attack surface žemėlapio pradžia.

_____________________________________________________________________________

# HTTP Messages

## Kas yra HTTP message?

**HTTP messages** yra struktūruotos žinutės, kuriomis apsikeičia:

- **client** — dažniausiai naršyklė, mobile app arba API klientas;
- **web server** — serveris, kuriame veikia web aplikacija.

Svarbu:

> HTTP message nėra tas pats, kas network packet.

HTTP message yra application-layer duomenys. Keliaudama per tinklą ji gali būti suskaidyta į TCP segmentus ir IP paketus.

---

## Dvi HTTP messages rūšys

### HTTP Request

**HTTP request** siunčia client į serverį.

Request tikslas — paprašyti resurso arba atlikti veiksmą.

Pvz.:

- atidaryti puslapį;
- prisijungti;
- nusiųsti formą;
- gauti vartotojo duomenis;
- pakeisti account nustatymus;
- ištrinti objektą.

---

### HTTP Response

**HTTP response** siunčia serveris clientui kaip atsakymą į request.

Response pasako:

- ar request pavyko;
- kokie duomenys grąžinami;
- kokio tipo turinys grąžinamas;
- ar reikia redirectinti;
- ar vartotojas turi teisę prieiti;
- ar įvyko klaida.

---

# HTTP Message struktūra

HTTP/1.1 žinutė dažniausiai turi šias dalis:

1. Start line
2. Headers
3. Empty line
4. Body

Ne kiekviena žinutė privalo turėti body.

---

## Start Line

Start line yra pirma HTTP message eilutė.

Ji pasako, ar tai yra request, ar response, ir kaip žinutę reikia interpretuoti.

---

### Request start line

Request start line dažniausiai turi:

*METHOD PATH HTTP_VERSION*

Pavyzdys:

*GET /products?id=55 HTTP/1.1*

Čia:

* `GET` — method;
* `/products?id=55` — resursas ir parametrai;
* `HTTP/1.1` — HTTP versija.

---

### Response start line

Response start line dažniausiai turi:

*HTTP_VERSION STATUS_CODE STATUS_MESSAGE*

Pavyzdys:

HTTP/1.1 200 OK

Čia:

* `HTTP/1.1` — HTTP versija;
* `200` — status code;
* `OK` — trumpas statuso aprašymas.

---

## Headers

Headers yra papildoma informacija apie request arba response.

Jie naudojami `key: value` formatu.

Pavyzdys:

http
Host: example.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Cookie: session=abc123

Headers gali nurodyti:

* kokį host nori pasiekti;
* kokį content tipą siunčia ar gauna client;
* kokį browserį naudoja client;
* cookies ir session informaciją;
* autorizacijos duomenis;
* caching taisykles;
* security nustatymus;
* kalbą;
* redirect informaciją.

Paprastai tariant:

> Start line pasako pagrindinį veiksmą, o headers duoda papildomą kontekstą ir instrukcijas.

---

## Empty Line

HTTP/1.1 formate tuščia eilutė atskiria headers nuo body.

Pavyzdys:

POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=ignelis&password=test

Tuščia eilutė po headers parodo:

> Headers baigėsi. Toliau prasideda body.

Svarbu:

> HTTP/2 ir HTTP/3 techniškai naudoja kitokį, binary framing formatą, todėl ten šitas „tuščios eilutės“ vaizdas nėra toks pats kaip HTTP/1.1.

Bet mokantis Burp Suite, browser DevTools ir basic web hacking, HTTP/1.1 formatą suprasti būtina.

---

## Body

Body yra vieta, kur gali būti siunčiami tikrieji duomenys.

### Request body

Request body dažnai naudojamas siunčiant duomenis į serverį.

Pvz. login forma:

POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=ignelis&password=test

Body čia yra:

username=ignelis&password=test

---

### Response body

Response body yra turinys, kurį serveris grąžina clientui.

Pvz. HTML puslapis:

HTTP/1.1 200 OK
Content-Type: text/html

<html>
  <body>Hello</body>
</html>

Body čia yra HTML kodas.

Response body gali būti:

* HTML;
* JSON;
* CSS;
* JavaScript;
* paveikslėlis;
* failas;
* API duomenys;
* klaidos tekstas.

---

# Paprastas request pavyzdys

GET /products?id=55 HTTP/1.1
Host: shop.example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123

Šiame requeste:

GET /products?id=55 HTTP/1.1  → start line
Host: shop.example.com         → header
User-Agent: Mozilla/5.0        → header
Accept: text/html              → header
Cookie: session=abc123         → header

Šitas GET request neturi body.

---

# Paprastas response pavyzdys

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 32

<h1>Product page</h1>

Šiame response:

HTTP/1.1 200 OK               → start line
Content-Type: text/html       → header
Content-Length: 32            → header
<h1>Product page</h1>         → body

---

# Kodėl HTTP messages svarbios web hacking'e?

HTTP messages yra pagrindinė vieta, kur vyksta web aplikacijos komunikacija.

Supratęs requestus ir response gali:

* matyti, ką naršyklė siunčia serveriui;
* keisti parametrus;
* tikrinti authorization;
* analizuoti cookies;
* rasti API endpointus;
* testuoti input validation;
* pastebėti IDOR;
* matyti serverio klaidas;
* suprasti, ką blokuoja WAF;
* analizuoti redirectus;
* ieškoti informacijos leakage.

---

# Web hacking mental modelis

Request = ko client prašo arba ką siunčia serveriui
Response = ką serveris grąžina clientui

Dar paprasčiau:

Browser: "Duok man /profile?id=55"
Server:  "Gerai, čia 200 OK ir profile duomenys."

---

# Mano verdiktas

HTTP messages yra web hacking pagrindas.

URL pasako:

> Kur einu ir ko prašau.

HTTP request pasako:

> Ką konkrečiai siunčiu serveriui.

HTTP response parodo:

> Ką serveris man leidžia gauti arba ką jis apie save išduoda.

_____________________________________________________________________________



# HTTP Request — Request Line and Methods

## Kas yra HTTP Request?

HTTP request yra žinutė, kurią client siunčia web serveriui, kad galėtų sąveikauti su web aplikacija ir atlikti veiksmą.

Client gali būti:

- web browser;
- mobile app;
- API client;
- scriptas;
- Burp Suite;
- curl.

HTTP request yra pirmas kontaktas tarp client ir web serverio.

Paprastai tariant:

> Client sako serveriui, ko nori: gauti puslapį, prisijungti, sukurti objektą, pakeisti duomenis ar kažką ištrinti.

---

## HTTP Request struktūra

Pavyzdys:

GET /user/login.html HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Connection: keep-alive

HTTP request sudaro:

GET /user/login.html HTTP/1.1  → Request line
Host: tryhackme.com            → Header
User-Agent: Mozilla/5.0        → Header
Accept: */*                    → Header

---

# Request Line

Request line yra pirma HTTP request eilutė.

Ji pasako serveriui:

* kokį veiksmą client nori atlikti;
* kokio resurso prašo;
* kokia HTTP versija naudojama.

Formatas:

METHOD PATH HTTP_VERSION

Pavyzdys:

GET /user/login.html HTTP/1.1

Čia:

GET               → HTTP method
/user/login.html  → URL path
HTTP/1.1          → HTTP version

---

# HTTP Methods

HTTP method nurodo, kokį veiksmą client nori atlikti su resursu.

Pvz.:

GET /products/55

reiškia:

> Client nori gauti produkto nr. 55 informaciją.

---

## GET

`GET` naudojamas duomenims gauti iš serverio.

Jis neturėtų keisti serverio duomenų.

Pvz.:

GET /products?id=55 HTTP/1.1

Naudojamas:

* atidaryti puslapį;
* gauti produktą;
* atlikti paiešką;
* pažiūrėti profilį;
* gauti API duomenis.

Security:

* nerodyti jautrių duomenų GET parametruose;
* nedėti slaptažodžių ar tokenų į URL;
* tikrinti, ar user turi teisę matyti prašomą resursą.

---

## POST

`POST` naudojamas siųsti duomenis į serverį.

Dažnai naudojamas:

* login formoms;
* naujo vartotojo sukūrimui;
* komentarui parašyti;
* failui įkelti;
* formos duomenims nusiųsti.

Pvz.:

POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=ignelis&password=test

Security:

* validuoti visą inputą;
* sanitizuoti arba saugiai apdoroti duomenis;
* saugotis SQL Injection;
* saugotis XSS;
* nenaudoti vien front end validacijos.

---

## PUT

`PUT` dažniausiai naudojamas pilnai pakeisti arba atnaujinti resursą.

Pvz.:

PUT /api/users/55 HTTP/1.1

Dažnai reiškia:

> Pakeisk visą user nr. 55 informaciją naujais duomenimis.

Security:

* tikrinti authorization;
* tikrinti, ar user gali keisti būtent šį objektą;
* validuoti siunčiamus duomenis;
* nepasitikėti vien tuo, ką client atsiunčia.

---

## DELETE

`DELETE` naudojamas ištrinti resursą.

Pvz.:

DELETE /api/orders/55 HTTP/1.1

Dažnai reiškia:

> Ištrink order nr. 55.

Security:

* tik autorizuoti useriai gali trinti resursus;
* serveris turi tikrinti ownership;
* reikia saugotis IDOR;
* svarbiems veiksmams kartais reikalingas papildomas patvirtinimas.

---

## PATCH

`PATCH` naudojamas dalinai atnaujinti resursą.

Pvz. vietoje viso user profilio pakeitimo gali pakeisti tik vardą:

PATCH /api/users/55 HTTP/1.1

Pvz. body:

json
{
  "display_name": "Ignelis"
}

Paprastai tariant:

> PUT keičia visą objektą, PATCH keičia tik dalį objekto.

Security:

* validuoti laukus;
* neleisti keisti laukų, kurių user neturi teisės keisti;
* tikrinti authorization;
* saugotis mass assignment problemų.

---

## HEAD

`HEAD` panašus į `GET`, bet serveris grąžina tik headers, be response body.

Pvz.:

HEAD /file.zip HTTP/1.1

Naudinga, kai nori patikrinti:

* ar resursas egzistuoja;
* content type;
* content length;
* paskutinio pakeitimo datą;
* caching informaciją.

---

## OPTIONS

`OPTIONS` parodo, kokie HTTP methods yra leidžiami konkrečiam resursui.

Pvz.:

OPTIONS /api/users HTTP/1.1

Response gali turėti:

Allow: GET, POST, OPTIONS

Tai padeda client suprasti, ką galima daryti su konkrečiu endpointu.

Security:

* OPTIONS nebūtinai turi būti išjungtas;
* svarbiausia, kad serveris realiai apsaugotų jautrius methods;
* vien `Allow` headeris nėra garantija, kad serveris tikrai apsaugotas.

---

## TRACE

`TRACE` yra diagnostinis HTTP method.

Jo tikslas — paprašyti serverio grąžinti tą patį requestą atgal.

Jis gali būti naudojamas debuggingui, kad matytum, ką serveris realiai gavo.

Security:

* TRACE dažnai išjungiamas;
* jis gali padidinti informacijos nutekėjimo riziką;
* jis nėra skirtas parodyti leidžiamus methods.

> OPTIONS parodo galimus methods. TRACE atspindi requestą.

---

## CONNECT

`CONNECT` dažniausiai naudojamas proxy serveriuose.

Jis prašo proxy sukurti tunelį iki kito serverio.

Pavyzdys:

CONNECT example.com:443 HTTP/1.1

Tai dažnai naudojama, kai browseris jungiasi prie HTTPS puslapio per proxy.

Svarbu:

> CONNECT pats savaime nesukuria HTTPS. Jis padeda proxy sukurti tunelį, per kurį tada gali keliauti HTTPS/TLS srautas.

---

# URL Path

URL path nurodo, kokio resurso client prašo.

Pvz. URL:

https://tryhackme.com/api/users/123

Path yra:

/api/users/123

Šitas path gali reikšti konkretų user objektą.

Security požiūris:

Attacker dažnai bandys keisti path, kad rastų neapsaugotus resursus.

Pvz.:

/admin
/api/users/123
/backup.zip
/.git
/internal

Serveris turi:

* tikrinti authorization;
* validuoti įvestį;
* tinkamai apdoroti path;
* neleisti pasiekti jautrių failų;
* nepasitikėti vien tuo, kad user nemato nuorodos meniu.

Svarbu:

> Paslėptas path nėra apsaugotas path.

---

# HTTP Versions

## HTTP/0.9 — 1991

Pirmoji HTTP versija.

* palaikė tik `GET`;
* neturėjo headers;
* buvo labai paprasta.

---

## HTTP/1.0 — 1996

Pridėjo:

* headers;
* status codes;
* geresnį skirtingų content tipų palaikymą;
* caching galimybes.

---

## HTTP/1.1 — 1997

Labai plačiai naudojama versija.

Pridėjo arba pagerino:

* persistent connections;
* chunked transfer encoding;
* Host header;
* caching;
* daugiau request kontrolės.

---

## HTTP/2 — 2015

Pagerino greitį ir efektyvumą.

Pagrindinės savybės:

* multiplexing;
* header compression;
* binary framing;
* request prioritisation.

Multiplexing reiškia, kad keli requestai gali vykti per vieną connection vienu metu.

---

## HTTP/3 — 2022

HTTP/3 paremtas QUIC protokolu, kuris veikia virš UDP.

Pagrindinės idėjos:

* greitesnis connection setup;
* mažiau problemų praradus vieną paketą;
* geresnis veikimas nestabiliuose tinkluose;
* modernus šifravimo modelis.

Svarbu:

> HTTP/3 nėra tiesiog HTTP/2 su kitu numeriu. Jis naudoja kitą transporto pagrindą — QUIC vietoje TCP.

---

# Web Hacking mental modelis

Kai matau HTTP request, žiūriu:

Method   → ką client bando padaryti?
Path     → su kokiu resursu?
Headers  → kokia papildoma informacija siunčiama?
Cookie   → kokia session / identity naudojama?
Body     → kokie duomenys siunčiami?

Pvz.:

GET /api/users/55 HTTP/1.1
Cookie: session=abc123

Čia reikia klausti:

* kas yra user 55?
* ar session abc123 turi teisę jį matyti?
* kas nutiks pakeitus 55 į 56?
* ar čia gali būti IDOR?

---

# Trumpa atmintinė


GET      → gauti duomenis
POST     → nusiųsti duomenis / sukurti veiksmą
PUT      → pilnai pakeisti resursą
PATCH    → dalinai pakeisti resursą
DELETE   → ištrinti resursą
HEAD     → gauti tik headers
OPTIONS  → paklausti, kokie methods galimi
TRACE    → diagnostikai, atspindi requestą
CONNECT  → proxy tuneliui į kitą serverį

_________________________________________________________________

# HTTP Requests — Headers and Body

## Request Headers

HTTP request headers leidžia clientui perduoti papildomą informaciją web serveriui apie requestą.

Headers yra rašomi `key: value` formatu.

Pvz.:

Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Cookie: session=abc123

Paprastai tariant:

> Request line pasako, ką client nori padaryti, o headers duoda papildomą kontekstą apie requestą.

---

# Common Request Headers

## Host

Pavyzdys:

Host: tryhackme.com

`Host` nurodo, kuriam web serveriui / domain skirtas requestas.

Tai ypač svarbu, nes vienas IP adresas gali hostinti kelis skirtingus websites.

Pvz. tas pats serveris gali turėti:

site1.com
site2.com
admin.site2.com

Todėl `Host` headeris padeda serveriui suprasti, kurio website client prašo.

Security požiūris:

* gali būti aktualu testuojant virtual hosts;
* kartais galima rasti paslėptus subdomainus arba vidinius hostus;
* blogai sukonfigūruoti serveriai gali turėti Host Header Injection problemų.

---

## User-Agent

Pavyzdys:

User-Agent: Mozilla/5.0

`User-Agent` perduoda informaciją apie clientą, kuris siunčia requestą.

Tai gali būti:

* browseris;
* OS;
* mobile app;
* scriptas;
* crawleris;
* toolas, pvz. `curl`.

Pvz.:

User-Agent: Mozilla/5.0
User-Agent: curl/8.0
User-Agent: sqlmap

Security požiūris:

* kai kurios aplikacijos elgiasi skirtingai pagal User-Agent;
* scanneriai kartais išsiduoda per User-Agent;
* User-Agent galima keisti rankiniu būdu;
* loguose dažnai matosi User-Agent.

Svarbu:

> Serveris neturėtų aklai pasitikėti User-Agent, nes clientas gali jį lengvai pakeisti.

---

## Referer

Pavyzdys:

Referer: https://www.google.com/

`Referer` nurodo URL, iš kurio vartotojas atėjo į dabartinį puslapį.

Pvz. jei žmogus spaudė linką iš Google, requeste gali būti:

Referer: https://www.google.com/

Svarbus niuansas:

> HTTP standarte naudojamas žodis `Referer`, nors gramatiškai teisingai būtų `Referrer`.

Tai senas istorinis typo, kuris liko standarte.

Security požiūris:

* Referer gali nutekinti jautrią informaciją, jeigu URL turi tokenus ar slaptus parametrus;
* serveris neturėtų aklai pasitikėti Referer;
* Referer kartais naudojamas CSRF apsaugose, bet vien jo nepakanka.

---

## Cookie

Pavyzdys:

Cookie: user_type=student; room=introtowebapplication; room_status=in_progress

`Cookie` headeris siunčia serveriui informaciją, kurią serveris anksčiau paprašė browserio išsaugoti.

Cookies dažnai naudojami:

* session valdymui;
* login būsenai;
* user preferences;
* tracking;
* aplikacijos state saugojimui.

Pvz.:

Cookie: session=abc123

Tai gali reikšti:

> Šitas requestas priklauso vartotojui, kurio session ID yra `abc123`.

Security požiūris:

* session cookies yra labai jautrūs;
* pavogtas session cookie gali leisti perimti paskyrą;
* cookies turi turėti saugumo nustatymus: `HttpOnly`, `Secure`, `SameSite`;
* cookie reikšmių nereikia laikyti saugiomis vien todėl, kad jos ateina iš browserio.

---

## Content-Type

Pavyzdys:

Content-Type: application/json

`Content-Type` nurodo, kokio formato duomenys yra request body dalyje.

Pvz.:

Content-Type: application/json
Content-Type: application/x-www-form-urlencoded
Content-Type: multipart/form-data
Content-Type: application/xml

Paprastai tariant:

> Content-Type pasako serveriui, kaip interpretuoti body duomenis.

Jeigu Content-Type neteisingas, serveris gali nesuprasti duomenų arba juos apdoroti neteisingai.

Security požiūris:

* svarbu teisingai validuoti body pagal formatą;
* blogas input handling gali vesti į SQL Injection, XSS, XXE ar kitas problemas;
* clientas gali keisti Content-Type, todėl serveris turi tikrinti realų turinį, ne tik headerį.

---

# Request Body

HTTP request body yra vieta, kur client siunčia duomenis serveriui.

Body dažniausiai naudojamas su methods kaip:

* `POST`
* `PUT`
* `PATCH`

Pvz. kai useris:

* siunčia login formą;
* registruojasi;
* uploadina failą;
* keičia profilį;
* siunčia API duomenis.

Svarbu:

> GET requestai dažniausiai body neturi. Duomenys dažniausiai keliauja URL query stringe.

---

# Common Request Body Formats

## URL Encoded

Content-Type:

Content-Type: application/x-www-form-urlencoded

Šitame formate data siunčiama kaip `key=value` poros.

Kelios poros atskiriamos `&` simboliu.

Pvz.:

name=Aleksandra&age=27&country=US

Pilnas request pavyzdys:

POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US

Čia body yra:

name=Aleksandra&age=27&country=US
Specialūs simboliai yra percent-encoded.

Pvz.:

space → %20
@     → %40
/     → %2F

Security požiūris:

* validuoti visus laukus;
* nepasitikėti client-side validacija;
* saugotis SQL Injection;
* saugotis XSS;
* tikrinti authorization.

---

## Form Data / Multipart Form Data

Content-Type:

Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

`multipart/form-data` leidžia siųsti kelis atskirus data blokus viename requeste.

Kiekvienas blokas atskiriamas specialiu `boundary` stringu.

Dažnai naudojama:

* failų uploadui;
* paveikslėlių uploadui;
* formoms su tekstu ir failais.

Pilnas request pavyzdys:

POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

Aleksandra
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
------WebKitFormBoundary7MA4YWxkTrZu0gW--

Security požiūris:

* tikrinti failo tipą;
* netikėti vien failo extension, pvz. `.jpg`;
* riboti failo dydį;
* neleisti uploadinti executable failų;
* saugoti uploadus saugioje vietoje;
* saugotis path traversal per filename;
* saugotis malicious file upload.

---

## JSON

Content-Type:

Content-Type: application/json

JSON reiškia **JavaScript Object Notation**.

Data siunčiama kaip `name: value` poros.

Pvz.:

{
  "name": "Aleksandra",
  "age": 27,
  "country": "US"
}

Pilnas request pavyzdys:

POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
  "name": "Aleksandra",
  "age": 27,
  "country": "US"
}

JSON dažnai naudojamas:

* API requestuose;
* moderniose web aplikacijose;
* mobile apps;
* JavaScript front end komunikacijoje su back end.

Security požiūris:

* validuoti JSON laukus;
* neleisti useriui keisti laukų, kurių jis neturi teisės keisti;
* saugotis mass assignment;
* tikrinti authorization;
* saugotis injection per JSON laukus.

Pvz. įtartinas JSON:
{
  "name": "Aleksandra",
  "role": "admin"
}

Jeigu paprastas useris gali pats sau atsiųsti `"role": "admin"` ir serveris tai priima — čia problema.

---

## XML

Content-Type:

Content-Type: application/xml

XML data struktūruojama su tags.

Tags turi opening ir closing dalis.

Pvz.:

<name>Aleksandra</name>

XML gali turėti nested struktūrą:

<user>
  <name>Aleksandra</name>
  <age>27</age>
  <country>US</country>
</user>

Pilnas request pavyzdys:

POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
  <name>Aleksandra</name>
  <age>27</age>
  <country>US</country>
</user>

Security požiūris:

* XML parseriai gali būti pavojingi, jei blogai sukonfigūruoti;
* reikia saugotis XXE;
* validuoti XML struktūrą;
* riboti input dydį;
* neleisti parseriui skaityti lokalių failų ar išorinių entities.

XXE pavyzdys kaip konceptas:

> Jeigu XML parseris leidžia external entities, attacker gali bandyti priversti serverį skaityti failus arba daryti requestus į vidinius resursus.

---

# Headers vs Body

Trumpai:
Headers = papildoma informacija apie requestą
Body    = tikri siunčiami duomenys

Pvz.:
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Cookie: session=abc123

username=ignelis&password=test

Čia:
POST /login HTTP/1.1                  → request line
Host: example.com                     → header
Content-Type: application/x-www-form-urlencoded → header
Cookie: session=abc123                → header
username=ignelis&password=test        → body

---

# Web Hacking mental modelis

Kai matau request headers ir body, klausiu:

* Kokie headers siunčiami?
* Ar yra Cookie?
* Ar yra session token?
* Koks Content-Type?
* Ar body yra form-urlencoded, JSON, XML ar multipart?
* Ar galiu keisti body laukus?
* Ar serveris validuoja inputą?
* Ar galiu pakeisti ID?
* Ar galiu pakeisti role/status/isAdmin lauką?
* Ar yra file upload?
* Ar galima testuoti SQL Injection, XSS, IDOR, XXE ar file upload vuln?

---

# Trumpa atmintinė

Host           → kuriam domain/server skirtas requestas
User-Agent     → kas siunčia requestą
Referer        → iš kurio URL atėjo requestas
Cookie         → browserio saugoma info, dažnai session
Content-Type   → kokio formato body duomenys

x-www-form-urlencoded → key=value&key2=value2
multipart/form-data   → formos + failų uploadas
application/json      → JSON API duomenys
application/xml       → XML struktūra

---

# Mano verdiktas

Headers ir body yra viena svarbiausių vietų web hacking'e.

URL parodo, kur requestas eina.

Method parodo, ką requestas bando daryti.

Headers parodo papildomą kontekstą.

Body parodo, kokius duomenis client siunčia serveriui.

Svarbiausia mintis:

> Viskas, kas ateina iš cliento — URL, headers, cookies, body — turi būti laikoma nepatikima ir tikrinama serverio pusėje.
______________________________________________________________________________

# HTTP Responses — Status Line and Status Codes

## Kas yra HTTP Response?

Kai client siunčia HTTP request į web serverį, serveris grąžina **HTTP response**.

Response pasako:

- ar request pavyko;
- ar įvyko klaida;
- ar reikia eiti į kitą URL;
- ar trūksta authentication;
- ar serveris pats susidūrė su problema.

Paprastai tariant:

> Request yra klausimas / veiksmas iš cliento, o response yra serverio atsakymas.

---

# Status Line

Pirma HTTP response eilutė vadinama **Status Line**.

Ji turi 3 pagrindines dalis:

```text
HTTP_VERSION STATUS_CODE REASON_PHRASE
````

Pvz.:

```http
HTTP/1.1 200 OK
```

Čia:

```text
HTTP/1.1  → HTTP version
200       → status code
OK        → reason phrase
```

---

## HTTP Version

Version parodo, kokia HTTP versija naudojama.

Pvz.:

```text
HTTP/1.1
HTTP/2
HTTP/3
```

---

## Status Code

**Status code** yra trijų skaitmenų numeris, kuris pasako requesto rezultatą.

Pvz.:

```text
200 → pavyko
404 → nerasta
500 → serverio klaida
```

Paprastai tariant:

> Status code yra greitas serverio atsakymas, kas nutiko su requestu.

---

## Reason Phrase

**Reason phrase** yra trumpas žmogui suprantamas status code paaiškinimas.

Pvz.:

```http
200 OK
404 Not Found
500 Internal Server Error
```

Svarbu:

> Status code yra svarbiausia dalis. Reason phrase tik padeda žmogui lengviau suprasti reikšmę.

---

# Status Code kategorijos

HTTP status codes suskirstyti į 5 pagrindines grupes.

---

## 100–199 — Informational Responses

Šitie kodai reiškia, kad serveris gavo dalį requesto ir procesas dar tęsiamas.

Tai kaip serverio signalas:

> „Gavau, tęsk toliau.“

Pvz.:

```text
100 Continue
```

---

## 200–299 — Successful Responses

Šitie kodai reiškia, kad request pavyko.

Serveris suprato requestą, jį apdorojo ir grąžino tinkamą atsakymą.

Pvz.:

```text
200 OK
201 Created
204 No Content
```

Paprastai tariant:

> 2xx = viskas gerai.

---

## 300–399 — Redirection Messages

Šitie kodai reiškia, kad resursas yra kitoje vietoje arba client turi būti nukreiptas kitur.

Pvz.:

```text
301 Moved Permanently
302 Found
304 Not Modified
```

Paprastai tariant:

> 3xx = eik kitur arba naudok kitą vietą / cache.

---

## 400–499 — Client Error Responses

Šitie kodai reiškia, kad problema greičiausiai yra requeste.

Pvz.:

* blogas URL;
* nėra authentication;
* nėra authorization;
* blogas inputas;
* resursas neegzistuoja.

Pvz.:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
```

Paprastai tariant:

> 4xx = client kažką blogai atsiuntė arba neturi teisės.

---

## 500–599 — Server Error Responses

Šitie kodai reiškia, kad serveris susidūrė su problema bandydamas apdoroti requestą.

Pvz.:

```text
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

Paprastai tariant:

> 5xx = serverio pusėje kažkas sprogo.

---

# Common Status Codes

## 100 Continue

```http
HTTP/1.1 100 Continue
```

Serveris gavo pirmą requesto dalį ir leidžia clientui tęsti.

Dažniausiai aktualu, kai requestas turi didesnį body.

---

## 200 OK

```http
HTTP/1.1 200 OK
```

Request pavyko.

Serveris grąžino prašomą resursą arba duomenis.

Pvz.:

```text
GET /profile → 200 OK
```

---

## 301 Moved Permanently

```http
HTTP/1.1 301 Moved Permanently
```

Resursas visam laikui perkeltas į naują URL.

Client turėtų naudoti naują URL ateityje.

Pvz.:

```text
http://example.com → 301 → https://example.com
```

---

## 404 Not Found

```http
HTTP/1.1 404 Not Found
```

Serveris nerado prašomo resurso.

Tai gali reikšti:

* blogas URL;
* puslapis ištrintas;
* neteisingas path;
* resursas neegzistuoja.

Pvz.:

```text
GET /does-not-exist → 404 Not Found
```

---

## 500 Internal Server Error

```http
HTTP/1.1 500 Internal Server Error
```

Serveris susidūrė su klaida ir negalėjo apdoroti requesto.

Tai dažniausiai serverio pusės problema.

Pvz.:

* aplikacijos crash;
* blogas serverio kodas;
* database problema;
* netikėta exception.

---

# Web Hacking mental modelis

Kai matau HTTP response status code, klausiu:

```text
2xx → Ar request pavyko?
3xx → Kur mane redirectina?
4xx → Ar trūksta auth, ar blogas requestas?
5xx → Ar serveris leakina klaidas?
```

Status codes padeda suprasti, kaip serveris reaguoja į mano requestus.

Pvz.:

```text
GET /admin → 403 Forbidden
GET /admin123 → 404 Not Found
GET /api/users/55 → 200 OK
GET /api/users/56 → 200 OK
```

Čia jau galima pradėti galvoti apie:

* access control;
* hidden paths;
* IDOR;
* authentication;
* authorization;
* server-side klaidas;
* information leakage.

---

# Trumpa atmintinė

```text
1xx → informacija / tęsk toliau
2xx → sėkmė
3xx → redirect / resursas kitur
4xx → client/request problema
5xx → serverio problema
```

Common:

```text
100 Continue              → tęsk requestą
200 OK                    → pavyko
301 Moved Permanently     → visam laikui perkelta
404 Not Found             → nerasta
500 Internal Server Error → serverio klaida
```

---

# Mano verdiktas

Status codes yra kaip serverio kūno kalba.

Request parodo, ką client bando padaryti.

Response status code parodo, kaip serveris į tai sureagavo.

Svarbiausia mintis:

> Web hacking'e status codes padeda suprasti, ar serveris leidžia, blokuoja, redirectina, neranda, ar pats lūžta.

```
```
________________________________________________________________________________

````markdown id="6zq6rf"
# HTTP Response — Headers and Body

## Response Headers

Kai web serveris atsako į HTTP requestą, jis grąžina HTTP response.

HTTP response gali turėti **response headers**.

Headers yra `key: value` poros, kurios suteikia papildomos informacijos apie response ir pasako clientui / browseriui, kaip su tuo response elgtis.

Pvz.:

```http
HTTP/1.1 200 OK
Date: Thu, 06 Aug 2026 15:30:00 GMT
Content-Type: text/html; charset=utf-8
Server: nginx
Cache-Control: max-age=600
````

Paprastai tariant:

> Response headers pasako ne tik ką serveris grąžina, bet ir kaip clientas turėtų tą atsakymą apdoroti.

---

# Important / Common Response Headers

## Date

Pavyzdys:

```http id="e11po5"
Date: Thu, 06 Aug 2026 15:30:00 GMT
```

`Date` headeris parodo, kada serveris sugeneravo response.

Tai gali būti naudinga:

* caching logikai;
* debuggingui;
* logų palyginimui;
* suprasti, kada response buvo sukurtas.

---

## Content-Type

Pavyzdys:

```http id="bb1f8p"
Content-Type: text/html; charset=utf-8
```

`Content-Type` pasako clientui, kokio tipo turinį serveris grąžina.

Pvz.:

```http id="w7dpkt"
Content-Type: text/html
Content-Type: application/json
Content-Type: image/png
Content-Type: text/css
Content-Type: application/javascript
```

`charset=utf-8` nurodo simbolių encodingą, kad browseris galėtų teisingai parodyti tekstą.

Paprastai tariant:

> Content-Type pasako browseriui, ar jis gauna HTML, JSON, paveikslėlį, failą ar kitą turinį.

Security požiūris:

* neteisingas `Content-Type` gali sukelti browserio interpretavimo problemų;
* kai kuriais atvejais browseris gali bandyti “atspėti” turinio tipą;
* tam dažnai naudojamas papildomas headeris `X-Content-Type-Options: nosniff`.

---

## Content-Length

Pavyzdys:

```http id="t0a1ma"
Content-Length: 348
```

`Content-Length` parodo, kiek baitų yra response body dalyje.

Tai padeda clientui žinoti, kiek duomenų tikėtis.

Svarbu:

> Rašosi `Content-Length`, ne `Content-Lenght`.

---

## Server

Pavyzdys:

```http id="fa6yox"
Server: nginx
```

`Server` headeris parodo, kokia serverio programinė įranga atsakė į requestą.

Pvz.:

```http id="699uos"
Server: nginx
Server: Apache
Server: Microsoft-IIS/10.0
```

Tai gali padėti debuggingui, bet security požiūriu gali išduoti informaciją attacker’iui.

Security požiūris:

* per daug detalus `Server` headeris gali išduoti technologijas;
* blogiau, jeigu rodo tikslią versiją;
* attacker gali pagal tai ieškoti žinomų pažeidžiamumų.

Pvz. mažiau saugu:

```http id="8gsdi9"
Server: Apache/2.4.49
```

Nes tiksli versija gali padėti taikyti konkrečius exploitus.

---

# Other Common Response Headers

## Set-Cookie

Pavyzdys:

```http id="zxlv7m"
Set-Cookie: sessionId=38af1337es7a8
```

`Set-Cookie` headeris siunčia cookie iš serverio į clientą.

Browseris išsaugo cookie ir vėliau siunčia ją atgal serveriui su requestais:

```http id="4c6yaj"
Cookie: sessionId=38af1337es7a8
```

Cookies dažnai naudojami:

* session valdymui;
* login būsenai;
* user preferences;
* tracking;
* aplikacijos state saugojimui.

Security flags:

```http id="ixf49s"
Set-Cookie: sessionId=38af1337es7a8; HttpOnly; Secure; SameSite=Lax
```

Svarbūs cookie nustatymai:

* `HttpOnly` — JavaScript negali pasiekti cookie per `document.cookie`;
* `Secure` — cookie siunčiama tik per HTTPS;
* `SameSite` — padeda mažinti CSRF riziką.

Security požiūris:

> Session cookies yra labai jautrios. Jei attacker pavagia session cookie, jis gali apsimesti tuo vartotoju.

---

## Cache-Control

Pavyzdys:

```http id="rv6f7j"
Cache-Control: max-age=600
```

`Cache-Control` pasako clientui arba proxy, kaip cache’inti response.

Pvz.:

```http id="hrsmvz"
Cache-Control: max-age=600
```

reiškia:

> Šį response galima laikyti cache 600 sekundžių.

Sensitive duomenims galima naudoti:

```http id="w2pk2l"
Cache-Control: no-store
```

`no-store` reiškia:

> Nesaugoti šito response cache’e.

Svarbus skirtumas:

```text id="xf01wk"
no-cache  → gali būti saugoma, bet prieš naudojant turi būti patikrinta su serveriu
no-store  → nesaugoti išvis
```

Security požiūris:

* login puslapiai;
* account puslapiai;
* bankiniai duomenys;
* private API response;
* medical/personal data;

neturėtų būti saugomi cache’e.

---

## Location

Pavyzdys:

```http id="to1fku"
Location: /index.html
```

`Location` headeris dažniausiai naudojamas su redirection status codes, pvz. `301`, `302`, `303`, `307`, `308`.

Pvz.:

```http id="fp5853"
HTTP/1.1 302 Found
Location: /login
```

Tai reiškia:

> Client turi eiti į `/login`.

Security požiūris:

`Location` gali būti susijęs su **open redirect** pažeidžiamumu.

Pvz.:

```text id="6gycu3"
/redirect?next=https://evil.com
```

Jeigu serveris aklai paima `next` parametrą ir įdeda į `Location`:

```http id="dxycta"
HTTP/1.1 302 Found
Location: https://evil.com
```

tada attacker gali naudoti tikrą domainą redirectui į malicious puslapį.

Svarbu:

> Useris tiesiogiai nekeičia response headerio. Jis keičia request parametrus, o pažeidžiamas serveris pagal juos sugeneruoja pavojingą `Location` headerį.

Apsauga:

* validuoti redirect URL;
* leisti tik vidinius path, pvz. `/dashboard`;
* nenaudoti pilnų išorinių URL, jei nereikia;
* naudoti allowlist;
* niekada aklai nepasitikėti `next`, `returnUrl`, `redirect` parametrais.

---

# Response Body

Response body yra vieta, kur serveris įdeda tikrą turinį, kurį siunčia clientui.

Response body gali būti:

* HTML puslapis;
* JSON API data;
* CSS;
* JavaScript;
* image;
* video;
* failas;
* klaidos pranešimas;
* XML;
* plain text.

Pvz. HTML response:

```http id="z049ap"
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8

<html>
  <body>
    <h1>Hello</h1>
  </body>
</html>
```

Body čia yra:

```html id="3kfp6i"
<html>
  <body>
    <h1>Hello</h1>
  </body>
</html>
```

Pvz. JSON response:

```http id="eh0gug"
HTTP/1.1 200 OK
Content-Type: application/json

{
  "name": "Aleksandra",
  "role": "student"
}
```

Body čia yra JSON duomenys.

---

# Response Body Security

Jeigu serveris į response body įdeda user-generated content, jis turi būti saugiai apdorotas.

Pvz. useris parašo komentarą:

```html id="4yc3aw"
<script>alert(1)</script>
```

Jeigu serveris tą tekstą įdeda į HTML puslapį neescape’intą, gali atsirasti XSS.

Security požiūris:

* escape’inti HTML output;
* validuoti input;
* naudoti saugius template engine nustatymus;
* nedėti nepatikrinto user input tiesiai į HTML;
* naudoti tinkamą `Content-Type`;
* naudoti `Content-Security-Policy`, kai reikia papildomos apsaugos.

Svarbu:

> XSS dažnai atsiranda ne todėl, kad useris kažką įvedė, o todėl, kad serveris arba JavaScript nesaugiai tą įvestį įdėjo į response / DOM.

---

# Headers vs Body

Trumpai:

```text id="nqytwp"
Response headers = metadata ir instrukcijos apie response
Response body    = tikras turinys, kurį serveris grąžina
```

Pvz.:

```http id="klyvd7"
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Server: nginx
Cache-Control: no-store

<h1>Account page</h1>
```

Čia:

```text id="9xfpe2"
HTTP/1.1 200 OK                  → status line
Content-Type: text/html          → response header
Server: nginx                    → response header
Cache-Control: no-store          → response header
<h1>Account page</h1>            → response body
```

---

# Web Hacking mental modelis

Kai matau HTTP response, žiūriu:

* koks status code?
* koks `Content-Type`?
* ar yra `Set-Cookie`?
* ar cookie turi `HttpOnly`, `Secure`, `SameSite`?
* ar `Server` headeris leakina technologiją ar versiją?
* ar sensitive content turi tinkamą `Cache-Control`?
* ar yra `Location` redirectas?
* ar redirectas gali būti manipuliuojamas?
* ar response body rodo klaidas?
* ar user input grįžta į HTML neescape’intas?
* ar yra XSS rizika?

---

# Trumpa atmintinė

```text id="vmiqcc"
Date             → kada serveris sugeneravo response
Content-Type     → kokio tipo turinys grąžinamas
Content-Length   → response body dydis baitais
Server           → serverio software info
Set-Cookie       → serveris nustato cookie browseriui
Cache-Control    → kaip cache’inti response
Location         → kur redirectinti clientą
```

Security:

```text id="8h0xga"
Server version leak       → padeda fingerprintinti technologijas
Weak cookies              → session hijacking rizika
Bad Cache-Control         → sensitive data gali likti cache
Unsafe Location redirect  → open redirect
Unsafe response body      → XSS
```

---

# Mano verdiktas

Response headers ir body parodo, kaip serveris atsako ir kokias instrukcijas duoda clientui.

Web hacking'e response svarbus ne mažiau nei requestas.

Requeste matai, ką tu siunti serveriui.

Response matai, ką serveris leidžia, išduoda, cache’ina, redirectina arba blogai apdoroja.

Svarbiausia mintis:

> Response headers dažnai išduoda serverio elgesį, o response body dažnai išduoda aplikacijos klaidas.

_______________________________________________________________________________



````markdown id="cvf4s2"
# HTTP Security Headers

## Kas yra Security Headers?

HTTP security headers padeda pagerinti web aplikacijos saugumą duodant browseriui papildomas taisykles, kaip elgtis su puslapiu ir jo turiniu.

Jie gali padėti sumažinti riziką nuo tokių atakų kaip:

- Cross-Site Scripting (XSS);
- clickjacking;
- MIME sniffing;
- nesaugus HTTP naudojimas;
- referrer informacijos nutekėjimas.

Svarbu:

> Security headers nėra magiškas fixas blogam kodui. Jie yra papildomas apsaugos sluoksnis.

Security headers galima pasitikrinti su:

```text
https://securityheaders.io
````

---

# Content-Security-Policy (CSP)

`Content-Security-Policy` arba **CSP** yra security headeris, kuris leidžia web administratoriui nurodyti, iš kokių šaltinių browseris gali krauti turinį.

CSP gali padėti sumažinti XSS riziką.

Pvz. jei attacker bando į vulnerable puslapį injectinti scriptą iš kito domaino, CSP gali neleisti browseriui to scripto paleisti.

Paprastai tariant:

> CSP pasako browseriui: „krauk scriptus, CSS, image ir kitą content tik iš šitų leidžiamų vietų.“

---

## CSP pavyzdys

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

Čia naudojami keli directives:

```text id="qivfl8"
default-src
script-src
style-src
```

---

## default-src

```http id="yb0kig"
default-src 'self'
```

`default-src` nurodo default taisyklę turiniui, jei nėra nurodyta konkretesnė taisyklė.

`'self'` reiškia:

> Leisti krauti turinį tik iš to paties origin / website.

Pvz. jei puslapis yra:

```text id="kf2j17"
https://example.com
```

tai `'self'` reiškia:

```text id="0dc5un"
https://example.com
```

---

## script-src

```http id="fnbkst"
script-src 'self' https://cdn.tryhackme.com
```

`script-src` nurodo, iš kur galima krauti JavaScript failus.

Šitam pavyzdyje leidžiama krauti scriptus iš:

```text id="82uizo"
tos pačios svetainės
https://cdn.tryhackme.com
```

Security požiūris:

* riboja, iš kur gali būti kraunami scriptai;
* gali sumažinti XSS žalą;
* padeda blokuoti neleistinus external scriptus.

---

## style-src

```http id="k8g3gj"
style-src 'self'
```

`style-src` nurodo, iš kur galima krauti CSS stylesheets.

Šitam pavyzdyje CSS leidžiama krauti tik iš tos pačios svetainės.

---

## CSP security mintis

CSP padeda prieš XSS, bet:

> CSP neturi būti vienintelė XSS apsauga.

Vis tiek reikia:

* validuoti input;
* escape’inti output;
* nenaudoti nesaugaus JavaScript;
* vengti inline scriptų;
* naudoti saugų templating.

CSP yra papildomas saugumo sluoksnis, ne pagrindinis vaistas.

---

# Strict-Transport-Security (HSTS)

`Strict-Transport-Security`, arba **HSTS**, liepia browseriui visada jungtis prie svetainės per HTTPS.

Pvz.:

```http id="w66j3f"
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

Paprastai tariant:

> HSTS pasako browseriui: „prie šitos svetainės junkis tik per HTTPS.“

Tai padeda apsisaugoti nuo situacijų, kai vartotojas netyčia bando jungtis per nesaugų HTTP.

---

## max-age

```http id="m5fp9z"
max-age=63072000
```

`max-age` nurodo, kiek sekundžių browseris turi prisiminti HSTS taisyklę.

Pvz.:

```text id="piayrk"
63072000 sekundžių = maždaug 2 metai
```

---

## includeSubDomains

```http id="tx4wnv"
includeSubDomains
```

`includeSubDomains` reiškia, kad HSTS taisyklė galios ir visiems subdomainams.

Pvz.:

```text id="w03xnk"
example.com
shop.example.com
admin.example.com
api.example.com
```

Security požiūris:

> Prieš įjungiant includeSubDomains reikia būti tikram, kad visi subdomainai tvarkingai veikia per HTTPS.

Kitaip galima netyčia sulaužyti dalį svetainės.

---

## preload

```http id="crjhga"
preload
```

`preload` leidžia svetainei būti įtrauktai į browserių HSTS preload sąrašus.

Tai reiškia, kad browseris gali enforce’inti HTTPS dar prieš pirmą vartotojo apsilankymą.

Security požiūris:

* labai stipri apsauga;
* bet reikia atsargiai, nes blogai sukonfigūravus galima prisidaryti problemų;
* norint preload, svetainė turi atitikti tam tikras taisykles.

---

# X-Content-Type-Options

`X-Content-Type-Options` headeris liepia browseriui nebandyti spėlioti failo tipo.

Pvz.:

```http id="rn10xv"
X-Content-Type-Options: nosniff
```

---

## nosniff

```http id="yk4n14"
nosniff
```

`nosniff` nurodo browseriui:

> Naudok `Content-Type` headerį ir nebandyk pats atspėti MIME type.

Tai svarbu, nes be šito kai kurie browseriai gali bandyti interpretuoti failą kitaip, nei serveris nurodė.

Pvz. serveris grąžina failą kaip text/plain, bet browseris gali bandyti jį interpretuoti kaip JavaScript arba HTML.

Security požiūris:

* padeda sumažinti MIME sniffing riziką;
* gali padėti apsisaugoti nuo tam tikrų XSS scenarijų;
* ypač naudinga kartu su teisingu `Content-Type`.

---

# Referrer-Policy

`Referrer-Policy` kontroliuoja, kiek informacijos browseris siunčia apie puslapį, iš kurio useris atėjo.

Kai vartotojas spaudžia linką, browseris gali nusiųsti `Referer` headerį į naują puslapį.

Pvz.:

```http id="r419aw"
Referer: https://example.com/account/settings
```

Tai gali būti problema, jeigu URL turi jautrią informaciją.

Pvz.:

```text id="o93oid"
https://example.com/reset-password?token=secret123
```

Jeigu toks URL nutekėtų per `Referer`, būtų blogai.

---

## Referrer-Policy pavyzdžiai

```http id="mzwddm"
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```

---

## no-referrer

```http id="3y8k0w"
Referrer-Policy: no-referrer
```

`no-referrer` reiškia:

> Niekada nesiųsti Referer informacijos.

Tai labiausiai privatus variantas.

---

## same-origin

```http id="vwmgb3"
Referrer-Policy: same-origin
```

`same-origin` reiškia:

> Siųsti Referer tik tada, kai requestas eina į tą patį origin.

Pvz.:

```text id="ku91cf"
https://example.com/page1 → https://example.com/page2
```

Referer siunčiamas.

Bet:

```text id="vpmgbx"
https://example.com/page1 → https://other-site.com
```

Referer nesiunčiamas.

---

## strict-origin

```http id="ohum5b"
Referrer-Policy: strict-origin
```

`strict-origin` siunčia tik origin, ne pilną URL path.

Pvz. vietoje:

```text id="pkhmle"
https://example.com/account/settings
```

bus siunčiama tik:

```text id="ifcd0v"
https://example.com
```

Taip pat jis nesiunčia referrer informacijos, kai vyksta downgrade iš HTTPS į HTTP.

Pvz.:

```text id="f8zm31"
https://example.com → http://other-site.com
```

Referer nesiunčiamas.

---

## strict-origin-when-cross-origin

```http id="13d87f"
Referrer-Policy: strict-origin-when-cross-origin
```

`strict-origin-when-cross-origin` yra dažnas modernus default pasirinkimas.

Jis veikia taip:

* same-origin requestams siunčia pilną URL;
* cross-origin requestams siunčia tik origin;
* HTTPS → HTTP downgrade atveju referrer nesiunčia.

Pvz. same-origin:

```text id="72sf9n"
https://example.com/account/settings → https://example.com/profile
```

Gali siųsti pilną referrer:

```text id="z4fvkg"
https://example.com/account/settings
```

Pvz. cross-origin:

```text id="64qnco"
https://example.com/account/settings → https://other-site.com
```

Siunčia tik origin:

```text id="t7j8lg"
https://example.com
```

Pvz. HTTPS į HTTP:

```text id="2vluy0"
https://example.com/account/settings → http://other-site.com
```

Referrer nesiunčiamas.

---

# Web Hacking mental modelis

Kai matau security headers, klausiu:

* Ar yra CSP?
* Ar CSP per daug laisvas?
* Ar leidžia `unsafe-inline`?
* Ar leidžia `*`?
* Ar yra HSTS?
* Ar HSTS turi `includeSubDomains`?
* Ar yra `X-Content-Type-Options: nosniff`?
* Kokia `Referrer-Policy`?
* Ar cookies turi `HttpOnly`, `Secure`, `SameSite`?
* Ar yra clickjacking apsauga, pvz. `X-Frame-Options` arba CSP `frame-ancestors`?

---

# Trumpa atmintinė

```text id="3m8cb6"
Content-Security-Policy  → riboja, iš kur browseris gali krauti content
Strict-Transport-Security → enforce'ina HTTPS
X-Content-Type-Options   → neleidžia browseriui spėlioti MIME type
Referrer-Policy          → kontroliuoja, kiek Referer info nuteka
```

Common CSP directives:

```text id="w21bxg"
default-src → default taisyklė contentui
script-src  → iš kur galima krauti JavaScript
style-src   → iš kur galima krauti CSS
'self'      → tas pats origin kaip dabartinė svetainė
```

HSTS directives:

```text id="t7n92n"
max-age           → kiek laiko browseris prisimena HTTPS taisyklę
includeSubDomains → taikyti ir subdomainams
preload           → galimybė įtraukti į browserių preload sąrašą
```

Referrer-Policy:

```text id="dzqgs7"
no-referrer                  → nesiųsti Referer visai
same-origin                  → siųsti tik tam pačiam origin
strict-origin                → siųsti tik origin, be path
strict-origin-when-cross-origin → full URL same-origin, tik origin cross-origin, nieko downgrade
```

---

# Mano verdiktas

Security headers yra browserio saugumo instrukcijos.

Jos padeda sumažinti žalą nuo XSS, clickjacking, MIME sniffing, HTTP downgrade ir referrer leakage.

Svarbiausia mintis:

> Security headers neužtikrina, kad aplikacija saugi, bet jų nebuvimas dažnai rodo prastą saugumo higieną.
________________________________________________________________________________


