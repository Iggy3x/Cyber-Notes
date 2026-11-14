# THM – HTTP in Detail

## 1. Room esmė
Šitas roomas aiškina HTTP pagrindus.
HTTP reiškia:
> HyperText Transfer Protocol
Paprastai:
> HTTP = taisyklių rinkinys, kaip browseris ir web serveris kalbasi tarpusavyje.
Kai atidarai website, browseris siunčia HTTP request į serverį, o serveris grąžina HTTP response.
Šitas roomas dengia:
- HTTP ir HTTPS skirtumą
- URL struktūrą
- HTTP requests
- HTTP responses
- HTTP methods
- HTTP status codes
- headers
- cookies
- basic manual requests
Cybersecurity kontekste HTTP yra vienas svarbiausių protokolų, nes web hacking beveik visada prasideda nuo supratimo:
> ką siunčia clientas ir ką atsako serveris.
---

## 2. HTTP vs HTTPS
HTTP:
> HyperText Transfer Protocol
HTTPS:
> HyperText Transfer Protocol Secure
HTTP siunčia data plain text formatu.
Tai reiškia:
- requestai gali būti matomi
- response’ai gali būti matomi
- login data gali būti pavogta, jei nėra encryption
- attacker’is tinkle teoriškai gali skaityti traffic
HTTPS yra saugesnė HTTP versija.
HTTPS naudoja encryption per TLS.
Paprastai:
> HTTPS = HTTP + encryption.
HTTPS padeda:
- apsaugoti data nuo skaitymo pakeliui
- patvirtinti, kad kalbi su tikru serveriu
- sumažinti man-in-the-middle riziką
Svarbu:
> HTTPS nereiškia, kad website yra saugus. Tai tik reiškia, kad ryšys tarp tavęs ir serverio yra šifruotas.
Website su HTTPS vis tiek gali turėti:
- SQL injection
- XSS
- broken authentication
- IDOR
- logic bugs
- prastą session handling
---

## 3. URL struktūra
URL reiškia:
> Uniform Resource Locator
URL pasako browseriui, kur ir kaip pasiekti resursą.
Pvz:
```txt
http://user:password@tryhackme.com:80/view-room?id=1#task3
```
Dalys:
- `http` = scheme/protocol
- `user:password` = user info/auth dalis
- `tryhackme.com` = host/domain
- `80` = port
- `/view-room` = path
- `?id=1` = query string
- `#task3` = fragment
Paprastai:
> URL = adresas iki konkretaus resurso.
Dažnas realus pavyzdys:
```txt
https://example.com/blog?id=5
```
Čia:
- `https` = naudojamas HTTPS
- `example.com` = host
- `/blog` = path
- `id=5` = parameteris
Web hackinge query parameters labai svarbūs, nes dažnai būtent čia testuoji input:
```txt
?id=1
?user=admin
?page=home
?search=test
```
---

## 4. Request ir response
HTTP veikia request/response principu.
Paprastai:
> Clientas siunčia request, serveris grąžina response.
Clientas dažniausiai yra:
- browseris
- curl
- Burp Suite
- scriptas
- mobile app
Serveris:
- gauna requestą
- apdoroja
- grąžina response
Eiga:
```txt
Browser -> HTTP Request -> Web Server
Browser <- HTTP Response <- Web Server
```
Svarbi mintis:
> Web hacking = mokėjimas skaityti, keisti ir suprasti HTTP requestus bei response’us.
---

## 5. HTTP request struktūra
HTTP request paprastai turi:
- request line
- headers
- optional body
Pvz:
```http
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```
Request line:
```http
GET / HTTP/1.1
```
Reiškia:
- `GET` = method
- `/` = path
- `HTTP/1.1` = protocol version
Headers:
```http
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```
Reiškia:
- `Host` = kurio website/serverio prašoma
- `User-Agent` = kokia programa/browseris siunčia requestą
- `Referer` = iš kur ateita į šitą requestą
Jeigu request siunčia data, dažnai būna body.
Pvz POST login request:
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=admin&password=test
```
---

## 6. HTTP response struktūra
HTTP response paprastai turi:
- status line
- headers
- optional body
Pvz:
```http
HTTP/1.1 200 OK
Server: nginx/1.15.8
Content-Type: text/html; charset=utf-8
Content-Length: 233

<html>
<body>Hello</body>
</html>
```
Status line:
```http
HTTP/1.1 200 OK
```
Reiškia:
- `HTTP/1.1` = protocol version
- `200` = status code
- `OK` = reason phrase
Headers:
- `Server` = web server software
- `Content-Type` = kokio tipo content grąžinamas
- `Content-Length` = kiek data browseris gali tikėtis
Body:
- HTML
- JSON
- image data
- file
- error message
- redirect page
---

## 7. HTTP methods
HTTP method pasako, ką clientas nori padaryti su resursu.
Dažniausi:
- GET
- POST
- PUT
- DELETE
- PATCH
- HEAD
- OPTIONS
---

## 8. GET
GET naudojamas gauti informaciją iš serverio.
Paprastai:
> GET = duok man resursą.
Pvz:
```http
GET /blog?id=1 HTTP/1.1
Host: example.com
```
Naudojamas:
- atidaryti puslapį
- gauti article
- gauti image
- gauti API data
- search query
Svarbu:
- GET request dažnai neturi body
- parameters dažnai matomi URL’e
- GET neturėtų keisti serverio state
Pvz:
```txt
/search?q=tattoo
/product?id=10
/user/profile?id=5
```
Security mintis:
> Jei jautri data siunčiama per GET, ji gali matytis URL, browser history, logs ir proxy.
---

## 9. POST
POST naudojamas siųsti data į serverį.
Paprastai:
> POST = siunčiu data, serveri apdorok.
Pvz:
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=test
```
Naudojamas:
- login forma
- registracija
- comment submit
- file upload
- payment form
- API create action
POST dažnai turi body.
Security testuose POST labai svarbus, nes ten dažnai būna:
- login credentials
- form input
- JSON body
- uploaded files
- hidden fields
- CSRF tokens
---

## 10. PUT
PUT dažniausiai naudojamas atnaujinti arba pakeisti resursą.
Paprastai:
> PUT = pakeisk/atnaujink šitą resursą.
Pvz:
```http
PUT /user/15/email HTTP/1.1
Host: example.com
Content-Type: application/json

{"email":"new@example.com"}
```
Naudojamas:
- update account data
- update API resource
- replace existing object
Security kontekste reikia žiūrėti:
- ar useris turi teisę update’inti tą resource
- ar galima keisti kito userio data
- ar IDOR įmanomas
---

## 11. DELETE
DELETE naudojamas pašalinti resursą.
Paprastai:
> DELETE = ištrink šitą resursą.
Pvz:
```http
DELETE /picture/99 HTTP/1.1
Host: example.com
```
Naudojamas:
- delete post
- delete image
- delete account item
- API delete action
Security kontekste:
> DELETE be geros authorization kontrolės yra pavojingas.
Testuoti reikia:
- ar gali delete’inti tik savo resursus
- ar pakeitus ID galima trinti svetimus resursus
- ar serveris tikrai tikrina permissions
---

## 12. Kiti methods
PATCH:
> partial update.
HEAD:
> kaip GET, bet be response body.
OPTIONS:
> parodo, kokie methods palaikomi.
Pvz:
```http
OPTIONS / HTTP/1.1
Host: example.com
```
Security kontekste OPTIONS gali parodyti leidžiamus methods:
```txt
GET, POST, PUT, DELETE, OPTIONS
```
Jeigu nereikalingi pavojingi methods įjungti, tai gali būti misconfiguration.
---

## 13. Status codes
Status code pasako, kaip serveriui sekėsi apdoroti requestą.
Pagrindinės klasės:
- `1xx` = informational
- `2xx` = success
- `3xx` = redirection
- `4xx` = client error
- `5xx` = server error
Svarbiausia:
> Pirmas status code skaičius pasako bendrą kategoriją.
---

## 14. 2xx Success
2xx reiškia, kad requestas sėkmingas.
Dažni:
- `200 OK`
- `201 Created`
- `204 No Content`
`200 OK`:
> requestas pavyko, serveris grąžino response.
`201 Created`:
> naujas resursas sukurtas.
Pvz:
```txt
POST /users -> 201 Created
```
`204 No Content`:
> requestas pavyko, bet nėra body.
Pvz dažnai po DELETE:
```txt
DELETE /post/5 -> 204 No Content
```
---

## 15. 3xx Redirection
3xx reiškia redirect.
Dažni:
- `301 Moved Permanently`
- `302 Found`
- `304 Not Modified`
`301`:
> resursas visam laikui perkeltas.
`302`:
> laikinas redirect.
`304`:
> browseris gali naudoti cached copy.
Security kontekste redirects svarbūs dėl:
- open redirect
- login redirect abuse
- phishing flow
- auth bypass edge cases
---

## 16. 4xx Client Error
4xx reiškia, kad problema iš client/request pusės.
Dažni:
- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `405 Method Not Allowed`
`400`:
> requestas blogai suformuotas.
`401`:
> reikia autentifikacijos arba login.
`403`:
> esi žinomas, bet neturi teisės.
`404`:
> resursas nerastas.
`405`:
> šitas HTTP method neleidžiamas.
Svarbus skirtumas:
> 401 = kas tu toks?
> 403 = žinau kas tu, bet tau negalima.
---

## 17. 5xx Server Error
5xx reiškia, kad serverio pusėje kažkas nepavyko.
Dažni:
- `500 Internal Server Error`
- `502 Bad Gateway`
- `503 Service Unavailable`
- `504 Gateway Timeout`
`500`:
> generic server crash/error.
`502`:
> proxy/gateway gavo blogą atsakymą iš upstream serverio.
`503`:
> service unavailable, serveris negali aptarnauti requesto.
`504`:
> gateway/proxy nesulaukė atsakymo laiku.
Security kontekste 5xx įdomūs, nes gali rodyti:
- crash
- unhandled input
- SQL error
- backend failure
- stack trace
- debug leak
Bet vien 500 dar nereiškia exploit. Čia tik kvapas, ne lavonas.
---

## 18. Headers
HTTP headers perduoda papildomą informaciją apie requestą arba response’ą.
Paprastai:
> Headers = metadata apie HTTP žinutę.
Request headers pavyzdžiai:
- `Host`
- `User-Agent`
- `Referer`
- `Cookie`
- `Content-Type`
- `Authorization`
- `Accept`
Response headers pavyzdžiai:
- `Server`
- `Set-Cookie`
- `Content-Type`
- `Content-Length`
- `Location`
- `Cache-Control`
- `Strict-Transport-Security`
- `Content-Security-Policy`
---

## 19. Dažni request headers
`Host`:
> nurodo, į kokį host/domain requestas siunčiamas.
Pvz:
```http
Host: tryhackme.com
```
`User-Agent`:
> nurodo client software/browserį.
Pvz:
```http
User-Agent: Mozilla/5.0 Firefox/87.0
```
`Referer`:
> nurodo, iš kur useris/requestas atėjo.
Pvz:
```http
Referer: https://tryhackme.com/
```
`Cookie`:
> siunčia cookies atgal serveriui.
Pvz:
```http
Cookie: session=abc123
```
`Authorization`:
> siunčia auth token arba credentials.
Pvz:
```http
Authorization: Bearer eyJhbGciOi...
```
`Content-Type`:
> nurodo, kokio formato data siunčiama body dalyje.
Pvz:
```http
Content-Type: application/json
```
---

## 20. Dažni response headers
`Server`:
> gali rodyti web server software.
Pvz:
```http
Server: nginx/1.15.8
```
Security mintis:
> Server header gali padėti fingerprintinti technologijas, bet ne visada būna tikslus.
`Set-Cookie`:
> serveris liepia browseriui išsaugoti cookie.
Pvz:
```http
Set-Cookie: session=abc123; HttpOnly; Secure
```
`Content-Type`:
> nurodo response body tipą.
Pvz:
```http
Content-Type: text/html; charset=utf-8
```
`Content-Length`:
> nurodo response body dydį bytes.
`Location`:
> naudojamas redirectui.
Pvz:
```http
Location: /login
```
`Cache-Control`:
> nurodo caching taisykles.
---

## 21. Cookies
Cookies yra mažos data dalys, kurias serveris liepia browseriui išsaugoti.
Paprastai:
> Cookie = mažas client-side saugomas gabalas, kurį browseris siunčia atgal serveriui.
Serveris nustato cookie per:
```http
Set-Cookie: session=abc123
```
Browseris vėliau siunčia:
```http
Cookie: session=abc123
```
Cookies dažnai naudojami:
- session management
- login state
- preferences
- tracking
- cart data
- CSRF flow
Svarbu:
> HTTP yra stateless. Cookies padeda serveriui “prisiminti” userį tarp requestų.
Be cookies serveris kiekvieną requestą matytų kaip atskirą įvykį.
---

## 22. Session cookies
Login sistemos dažnai naudoja session cookie.
Pvz:
```http
Set-Cookie: sessionid=abc123xyz
```
Tada kiekvieną kartą einant į protected page browseris siunčia:
```http
Cookie: sessionid=abc123xyz
```
Serveris pagal sessionid supranta:
- kas useris
- ar jis logged in
- kokias permissions turi
Security kontekste session cookie yra labai jautrus.
Jeigu attacker gauna session cookie, dažnai jis gali apsimesti useriu.
Čia vadinama:
> session hijacking
---

## 23. Cookie flags
Svarbūs cookie flags:
- `HttpOnly`
- `Secure`
- `SameSite`
`HttpOnly`:
> JavaScript negali skaityti cookie.
Padeda prieš XSS session cookie vagystę.
`Secure`:
> cookie siunčiamas tik per HTTPS.
Padeda, kad cookie neišeitų per plain HTTP.
`SameSite`:
> kontroliuoja, kada cookie siunčiamas cross-site requestuose.
Padeda prieš CSRF.
Pvz geresnis cookie:
```http
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax
```
Svarbi mintis:
> Cookie be flags nėra automatiškai exploit, bet tai silpnesnė konfiguracija.
---

## 24. HTTP yra stateless
HTTP stateless reiškia:
> Kiekvienas requestas pagal nutylėjimą yra nepriklausomas.
Serveris automatiškai “neatsimena” ankstesnio requesto, nebent naudojamas mechanizmas kaip:
- cookies
- sessions
- tokens
- authentication headers
Pavyzdys:
1. Prisijungi per login formą
2. Serveris duoda session cookie
3. Browseris siunčia cookie su kitais requestais
4. Serveris atpažįsta tave
Be session/cookie tu po login vis tiek būtum “nežinomas” kitame requeste.
---

## 25. Making requests
HTTP requestus galima daryti ne tik su browseriu.
Įrankiai:
- browser devtools
- curl
- Burp Suite
- netcat
- telnet
- scripts
GET su curl:
```bash
curl http://example.com/
```
GET su headers:
```bash
curl -i http://example.com/
```
`-i` parodo response headers.
POST su curl:
```bash
curl -X POST http://example.com/login -d "username=admin&password=test"
```
POST JSON:
```bash
curl -X POST http://example.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"test"}'
```
Tik headers:
```bash
curl -I http://example.com/
```
Manual HTTP request su netcat:
```bash
nc example.com 80
```
Tada:
```http
GET / HTTP/1.1
Host: example.com

```
Po tuščios eilutės serveris atsako.
Svarbu:
> Tuščia eilutė HTTP requeste reiškia headers pabaigą.
---

## 26. Praktinė vertė white hat mokymuisi
HTTP yra vienas svarbiausių web security pamatų.
Kur tai naudosi:
- Burp Suite
- PortSwigger labs
- login testing
- auth testing
- IDOR
- XSS
- SQL injection
- file upload testing
- API testing
- session handling
- CSRF
- SSRF
- web enumeration
Praktinis web testing mąstymas:
```txt
Koks method?
Koks path?
Kokie parameters?
Kokie headers?
Ar yra cookies?
Ar yra auth token?
Koks status code?
Ar response skiriasi pakeitus input?
Ar serveris leakina info?
```
HTTP request yra tavo pagrindinis objektas.
Ne “puslapis”, ne “mygtukas”, ne “forma”.
Security akimis:
> forma = gražus frontend wrapperis HTTP requestui.
---

## 27. GET vs POST security požiūriu
GET:
- data dažnai matosi URL
- geras read veiksmams
- netinka sensitive data siuntimui
- gali būti cache/log/history problemos
POST:
- data body dalyje
- geresnis form submit
- naudojamas sensitive data siuntimui
- bet nėra automatiškai saugus be HTTPS
Svarbi mintis:
> POST nėra encryption. HTTPS yra encryption.
Jeigu POST eina per HTTP, credentials vis tiek keliauja plain text.
---

## 28. HTTP status codes kaip signalai testavime
Status code padeda suprasti serverio elgesį.
Pvz:
```txt
200 = pavyko
201 = sukurta
301/302 = redirect
400 = blogas requestas
401 = reikia login
403 = draudžiama
404 = nerasta
500 = serverio klaida
503 = service unavailable
```
Testuojant:
- `401` rodo auth requirement
- `403` rodo access control
- `404` gali slėpti ar tikrai nėra resurso
- `500` gali rodyti crash ar input problemą
- `302` dažnai rodo redirect į login
Pavyzdys:
```txt
GET /admin -> 302 /login
GET /admin su cookie -> 200
```
Tai rodo access control/session logic.
---

## 29. Mini troubleshooting modelis
Kai web neveikia arba testuoji targetą:
1. Ar domain resolve’inasi?
2. Ar host pasiekiamas?
3. Ar portas atidarytas?
4. Ar HTTP/HTTPS atsako?
5. Koks status code?
6. Kokie headers?
7. Ar yra redirect?
8. Ar yra cookies?
9. Ar input keičia response?
10. Ar auth reikalinga?
Komandos:
```bash
ping example.com
nmap -p 80,443 example.com
curl -I http://example.com/
curl -i http://example.com/
```
---

## 30. Komandos
GET request:
```bash
curl http://example.com/
```
GET su headers:
```bash
curl -i http://example.com/
```
Tik response headers:
```bash
curl -I http://example.com/
```
Sekti redirectus:
```bash
curl -L http://example.com/
```
POST form data:
```bash
curl -X POST http://example.com/login -d "username=admin&password=test"
```
POST JSON:
```bash
curl -X POST http://example.com/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"test"}'
```
Pridėti custom header:
```bash
curl -H "X-Test: hello" http://example.com/
```
Pridėti cookie:
```bash
curl -b "session=abc123" http://example.com/profile
```
Išsaugoti cookies į failą:
```bash
curl -c cookies.txt http://example.com/login
```
Naudoti cookies iš failo:
```bash
curl -b cookies.txt http://example.com/profile
```
Nmap HTTP service:
```bash
nmap -p 80,443 target
```
Manual request:
```bash
nc example.com 80
```
Tada:
```http
GET / HTTP/1.1
Host: example.com

```
---

## 31. Mano paaiškinimas žmonių kalba
HTTP yra kaip užsakymas bare.
Clientas:
> Duok man `/beer?id=1`.
Serveris:
> 200 OK, štai tavo alus.
Jeigu nėra:
> 404, tokio neturim.
Jeigu reikia login:
> 401, pirma parodyk dokumentą.
Jeigu žino kas tu, bet neleidžia:
> 403, VIP zona ne tau, seni.
Jeigu barmenas susprogo:
> 500 Internal Server Error.
Headers yra papildomos pastabos ant užsakymo:
- koks klientas
- kokio formato nori
- ar turi session cookie
- ar yra token
- kiek content bus grąžinta
Cookies yra kaip apyrankė klube:
> Tu jau patikrintas, gali eiti toliau.
Tik jeigu kažkas pavagia tavo apyrankę, jis gali apsimesti tavimi.
---

## 32. Ką būtina atsiminti
- HTTP = browserio/client ir web serverio komunikacijos protokolas.
- HTTPS = HTTP su TLS encryption.
- HTTPS saugo transportą, bet negarantuoja, kad aplikacija saugi.
- URL turi scheme, host, port, path, query string, fragment.
- HTTP veikia request/response principu.
- Request turi method, path, headers ir kartais body.
- Response turi status code, headers ir kartais body.
- GET dažniausiai naudojamas gauti data.
- POST dažniausiai naudojamas siųsti data.
- PUT dažniausiai naudojamas update/replace.
- DELETE naudojamas trynimui.
- Status codes padeda suprasti serverio atsakymą.
- 2xx = success.
- 3xx = redirect.
- 4xx = client/request problema.
- 5xx = serverio problema.
- Headers yra metadata.
- Cookies padeda išlaikyti session state.
- HTTP yra stateless.
- Session cookie yra labai jautrus dalykas.
- Cookie flags `HttpOnly`, `Secure`, `SameSite` yra svarbūs security.
- Web hacking esmė: suprasti ir manipuliuoti HTTP requestais.
---

## 33. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- HTTP/2 frame struktūrą
- HTTP/3 ir QUIC
- TLS handshake gilias detales
- cipher suites
- HPACK/QPACK compression
- advanced cache poisoning
- full browser security model
- CORS gylį
- CSP bypass technikas
- OAuth/OIDC detales
Dabar svarbiausia:
> Mokėti perskaityti HTTP requestą ir response’ą, suprasti method, path, headers, cookies ir status code.
---

## 34. Mini testas sau
Atsakyk nežiūrėdamas:
1. Ką reiškia HTTP?
2. Ką reiškia HTTPS?
3. Ar HTTPS reiškia, kad website neturi vulnerabilities?
4. Kas yra URL?
5. Kokios pagrindinės URL dalys?
6. Kuo skiriasi request ir response?
7. Kas yra HTTP method?
8. Kam naudojamas GET?
9. Kam naudojamas POST?
10. Kam naudojamas PUT?
11. Kam naudojamas DELETE?
12. Ką reiškia status code `200`?
13. Ką reiškia status code `201`?
14. Ką reiškia status code `301/302`?
15. Ką reiškia status code `401`?
16. Kuo `401` skiriasi nuo `403`?
17. Ką reiškia status code `404`?
18. Ką reiškia status code `500`?
19. Kas yra headers?
20. Kam naudojamas `Host` header?
21. Kam naudojamas `User-Agent` header?
22. Kam naudojamas `Set-Cookie` header?
23. Kuo skiriasi `Cookie` ir `Set-Cookie`?
24. Kodėl HTTP vadinamas stateless?
25. Kam naudojamas session cookie?
26. Ką daro `HttpOnly` cookie flag?
27. Ką daro `Secure` cookie flag?
28. Ką daro `SameSite` cookie flag?
29. Kodėl POST be HTTPS nėra saugus?
30. Kodėl web hackinge svarbiau matyti HTTP requestą nei tik gražų puslapį?
---

## 35. Mano verdiktas
Šitas roomas yra labai svarbus web security pamatui.
Statusas:
```txt
Status: Foundation
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš Burp Suite, PortSwigger, auth testing, IDOR, XSS, SQLi
```
Svarbiausia mintis:
> Web aplikacija iš išorės atrodo kaip puslapis, bet po kapotu tai HTTP requestų ir response’ų sistema.
Jeigu supranti:
- method
- path
- parameters
- headers
- cookies
- status codes
tada web hacking pradeda atrodyti ne kaip magija, o kaip mechanika.
Čia jau nebe vien “atidariau puslapį”.
Čia jau:
> “Ką clientas išsiuntė, ką serveris atsakė, ir kas nutiks, jei pakeisiu requestą?”
Būtent nuo čia prasideda normalus web pentesting mąstymas.
