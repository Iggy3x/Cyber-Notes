# THM – How Websites Work

## 1. Room esmė
Šitas roomas aiškina, kaip veikia website’ai iš vidaus.
Pagrindinė mintis:
> Website nėra tik gražus puslapis. Tai client-side ir server-side dalių bendravimas.
Kai atidarai svetainę:
- browseris siunčia requestą
- serveris grąžina HTML/CSS/JavaScript/assets
- browseris renderina puslapį
- JavaScript gali keisti puslapį dinamiškai
- user input gali būti siunčiamas atgal serveriui arba naudojamas client-side
Šitas roomas dengia:
- kaip website’ai veikia
- client-side vs server-side
- HTML pagrindus
- JavaScript pagrindus
- source code peržiūrą
- sensitive data exposure
- HTML injection
Cybersecurity kontekste šitas roomas svarbus, nes moko:
> Web puslapį reikia žiūrėti ne tik akimis, bet ir per source code, requests, inputs ir browserio elgesį.
---

## 2. Client-side ir server-side
Web aplikacija dažniausiai turi dvi puses:
- client-side
- server-side
Client-side:
> Tai dalis, kuri veikia tavo browseryje.
Pvz:
- HTML
- CSS
- JavaScript
- puslapio struktūra
- buttons
- forms
- frontend validation
- DOM manipulation
Server-side:
> Tai dalis, kuri veikia serveryje.
Pvz:
- backend code
- database queries
- authentication
- authorization
- session management
- API logic
- file handling
- payment logic
Svarbi mintis:
> Viskas, kas yra client-side, gali būti matoma arba manipuliuojama vartotojo.
Tai reiškia:
- HTML gali būti peržiūrėtas
- JavaScript gali būti peržiūrėtas
- hidden fields gali būti pakeisti
- frontend validation gali būti apeita
- browser requestai gali būti modifikuojami
Todėl saugumo taisyklė:
> Nepasitikėk client-side.
Serveris visada turi tikrinti:
- input
- permissions
- session
- role
- ownership
- business logic
---

## 3. Kaip browseris gauna website
Kai įvedi URL, vyksta maždaug tokia eiga:
1. Browseris gauna domain/IP
2. Browseris siunčia HTTP/HTTPS requestą
3. Web serveris apdoroja requestą
4. Serveris grąžina response
5. Browseris parsisiunčia HTML
6. Browseris parsisiunčia CSS, JS, images ir kitus assets
7. Browseris sukonstruoja puslapį
8. JavaScript gali papildomai keisti puslapį arba daryti API requests
Paprastai:
> Serveris atsiunčia instrukcijas, browseris iš jų pastato puslapį.
Tai reiškia, kad puslapis nėra “gatavas paveikslėlis”.
Jis sudarytas iš:
- HTML struktūros
- CSS stiliaus
- JavaScript funkcionalumo
- media/assets
- data iš backend/API
---

## 4. HTML
HTML reiškia:
> HyperText Markup Language
HTML nėra programavimo kalba.
HTML yra markup language.
Paprastai:
> HTML = puslapio struktūra ir turinys.
HTML nurodo browseriui:
- kur yra heading
- kur yra paragraph
- kur yra image
- kur yra link
- kur yra form
- kur yra button
- kokie elementai egzistuoja puslapyje
Pavyzdys:
```html
<h1>Hello World</h1>
<p>This is a paragraph.</p>
<a href="https://tryhackme.com">TryHackMe</a>
```
Elementai dažniausiai turi opening ir closing tag:
```html
<p>Hello</p>
```
Čia:
- `<p>` = opening tag
- `Hello` = content
- `</p>` = closing tag
Kai kurie tag’ai gali būti self-closing / void elements:
```html
<img src="cat.jpg" alt="cat">
<br>
<hr>
```
Svarbi mintis:
> HTML pasako kas yra puslapyje, bet ne kaip tai turi atrodyti ar elgtis.
---

## 5. Dažni HTML tag’ai
Heading:
```html
<h1>Main title</h1>
<h2>Section title</h2>
```
Paragraph:
```html
<p>Text here</p>
```
Link:
```html
<a href="https://example.com">Click me</a>
```
Image:
```html
<img src="image.jpg" alt="Description">
```
Button:
```html
<button>Click</button>
```
Input:
```html
<input type="text" name="username">
```
Form:
```html
<form action="/login" method="POST">
  <input type="text" name="username">
  <input type="password" name="password">
  <button type="submit">Login</button>
</form>
```
Comment:
```html
<!-- This is a comment -->
```
Security mintis:
> HTML comments kartais netyčia paliekami su jautria info.
Pvz blogai:
```html
<!-- TODO: remove test password admin:password123 -->
```
---

## 6. HTML attributes
HTML elementai gali turėti attributes.
Paprastai:
> Attributes = papildoma informacija apie HTML elementą.
Pvz:
```html
<a href="https://example.com">Link</a>
```
Čia `href` yra attribute.
Image:
```html
<img src="cat.jpg" alt="Cat image">
```
Čia:
- `src` = kur yra image file
- `alt` = alternatyvus tekstas
Input:
```html
<input type="text" name="search" value="test">
```
Čia:
- `type` = input tipas
- `name` = parameterio vardas
- `value` = reikšmė
Security kontekste attributes svarbūs, nes jie gali rodyti:
- endpointus
- form action path
- hidden fields
- input names
- IDs
- JavaScript event handlers
Pvz:
```html
<input type="hidden" name="role" value="user">
```
Tai galima pamatyti ir bandyti keisti requeste.
---

## 7. CSS trumpai
CSS reiškia:
> Cascading Style Sheets
CSS atsakingas už puslapio išvaizdą.
Paprastai:
> HTML = struktūra, CSS = stilius.
CSS kontroliuoja:
- spalvas
- fontus
- layout
- margins
- padding
- responsive dizainą
- animations
Pvz:
```css
h1 {
  color: red;
}
```
Šitam roomui CSS nėra pagrindinė tema, bet verta žinoti:
> CSS daro puslapį gražų, bet pagrindinis web security darbas dažniausiai vyksta per HTML, JavaScript, HTTP ir backend logiką.
---

## 8. JavaScript
JavaScript yra programavimo kalba, kuri dažnai veikia browseryje.
Paprastai:
> JavaScript = puslapio elgesys ir interaktyvumas.
JavaScript gali:
- keisti HTML
- reaguoti į button clicks
- validuoti forms
- siųsti requests į API
- keisti DOM
- rodyti/paslėpti elementus
- skaityti kai kuriuos cookies/local storage
- manipuliuoti puslapio turiniu
Pavyzdys:
```html
<p id="demo">Old text</p>
<script>
document.getElementById("demo").innerHTML = "New text";
</script>
```
Čia JavaScript suranda elementą su ID `demo` ir pakeičia jo turinį.
Svarbi mintis:
> JavaScript client-side dalyje nėra patikimas saugumo mechanizmas.
Kodėl?
Nes useris gali:
- peržiūrėti JS
- modifikuoti JS runtime
- išjungti JS
- keisti requestus per Burp
- apeiti frontend validation
---

## 9. DOM
DOM reiškia:
> Document Object Model
Paprastai:
> DOM = browserio sukonstruotas puslapio medis, kurį JavaScript gali skaityti ir keisti.
HTML source yra tai, ką serveris atsiuntė.
DOM yra tai, ką browseris realiai sukonstravo ir naudoja.
JavaScript gali pakeisti DOM po puslapio užkrovimo.
Pvz:
```js
document.getElementById("demo").innerHTML = "Hack the Planet";
```
Tai keičia DOM elementą.
Security kontekste DOM svarbus dėl:
- DOM-based XSS
- HTML injection
- client-side routing
- dynamic content
- hidden elements
- frontend-only logic
Svarbi mintis:
> Source code ir live DOM gali skirtis.
Todėl verta tikrinti:
- View Page Source
- DevTools Elements tab
- Network tab
- JavaScript files
---

## 10. Frontend validation
Frontend validation reiškia, kad browseris patikrina input prieš siunčiant į serverį.
Pvz:
```html
<input type="number" min="1" max="10">
```
Arba JavaScript:
```js
if (password.length < 8) {
  alert("Password too short");
}
```
Tai naudinga UX, bet ne saugumui.
Svarbi taisyklė:
> Frontend validation yra patogumui, ne security.
Attacker gali:
- pakeisti HTML
- išjungti JavaScript
- siųsti requestą tiesiai su curl/Burp
- pakeisti hidden fields
- apeiti browserio limitus
Todėl server-side validation būtina.
Jeigu serveris pasitiki frontend validation, tai yra blogas dizainas.
---

## 11. View Page Source
Vienas pirmų web testing veiksmų:
> Peržiūrėti page source.
Browseryje dažnai:
```txt
Right click -> View Page Source
```
arba:
```txt
Ctrl + U
```
Ko ieškoti source code:
- HTML comments
- hidden links
- hidden fields
- JavaScript files
- API endpoints
- test credentials
- internal notes
- debug info
- version info
- admin paths
- hardcoded secrets
Pvz įtartini dalykai:
```html
<!-- admin portal: /admin-dev -->
```
```html
<!-- temp password: test123 -->
```
```html
<input type="hidden" name="isAdmin" value="false">
```
```html
<script src="/static/app.js"></script>
```
Svarbi mintis:
> Source code nėra exploit pats savaime, bet dažnai duoda kelią į kitą testą.
---

## 12. DevTools
Browser DevTools yra vienas svarbiausių web analizės įrankių.
Atidaroma dažnai:
```txt
F12
```
arba:
```txt
Right click -> Inspect
```
Svarbūs tab’ai:
- Elements
- Console
- Network
- Sources
- Application/Storage
Elements:
> rodo live DOM.
Console:
> leidžia matyti JS errors ir vykdyti JavaScript.
Network:
> rodo HTTP requests ir responses.
Sources:
> rodo JavaScript/CSS/source files.
Application/Storage:
> rodo cookies, local storage, session storage.
Security praktikoje DevTools padeda rasti:
- endpoints
- request parameters
- cookies
- tokens
- JS files
- hidden values
- API responses
- frontend-only restrictions
---

## 13. Sensitive Data Exposure
Sensitive Data Exposure reiškia, kad jautri informacija netyčia pasiekiama ten, kur neturėtų būti.
THM pavyzdyje minima, kad source code gali turėti HTML comments su temporary login credentials ar hidden links, todėl vienas pirmų veiksmų vertinant web aplikaciją yra source code peržiūra. :contentReference[oaicite:1]{index=1}
Paprastai:
> Sensitive data exposure = jautri info palikta matomoje/pasiekiamoje vietoje.
Pavyzdžiai:
- credentials HTML comments
- API keys JavaScript faile
- internal endpoints
- debug messages
- stack traces
- backup files
- exposed config
- tokens local storage
- version info
- `.env` leak
- directory listing
Blogas pavyzdys:
```html
<!-- dev login: admin / Password123 -->
```
Blogas JS pavyzdys:
```js
const API_KEY = "sk_live_123456";
```
Svarbi taisyklė:
> Nieko jautraus nedėti į client-side code.
Nes viskas, ką gauna browseris, gali būti pamatyta vartotojo.
---

## 14. Ką laikyti sensitive data
Sensitive data gali būti:
- passwords
- API keys
- session tokens
- JWT tokens
- private endpoints
- admin paths
- internal IPs
- database errors
- user personal data
- email addresses
- reset tokens
- debug output
- source maps
- cloud bucket names
- backup file paths
Ne viskas iškart yra critical, bet viskas gali padėti atakai.
Pvz:
> Vienas hidden admin path gali nuvesti į login panelį, o login panelis į brute force/auth testą.
Security mąstymas:
> Maži leak’ai dažnai susideda į didelį kelią.
---

## 15. HTML Injection
HTML Injection yra vulnerability, kai user input nėra tinkamai filtruojamas/sanitizuojamas ir tada atvaizduojamas puslapyje kaip HTML. :contentReference[oaicite:2]{index=2}
Paprastai:
> HTML Injection = gali įkišti savo HTML į puslapį.
Pvz vulnerable puslapis turi input:
```txt
What's your name?
```
Jeigu įvedi:
```html
<h1>Ignelis</h1>
```
Ir puslapis tai atvaizduoja kaip didelį heading, reiškia input interpretuojamas kaip HTML.
Dar pavyzdys:
```html
<a href="https://example.com">Click me</a>
```
Jeigu atsiranda clickable link, HTML injection veikia.
Kodėl tai blogai?
Attacker gali:
- keisti puslapio išvaizdą
- įdėti fake login formą
- klaidinti userį
- įdėti linkus
- paveikti DOM
- kartais judėti link XSS, jeigu leidžiamas script execution
Svarbu:
> HTML Injection nėra visada tas pats kas XSS, bet gali būti žingsnis link XSS.
---

## 16. HTML Injection vs XSS
HTML Injection:
> Injectini HTML markup.
Pvz:
```html
<h1>Owned</h1>
```
XSS:
> Injectini JavaScript, kuris vykdomas aukos browseryje.
Pvz:
```html
<script>alert(1)</script>
```
Skirtumas:
- HTML Injection keičia markup/content
- XSS vykdo script code
- XSS dažniausiai pavojingesnis
- HTML Injection gali būti naudojamas phishing UI ar content manipulation
Svarbi mintis:
> Jei input leidžia HTML, reikia tikrinti ar leidžia ir JavaScript/event handlers.
Pvz:
```html
<img src=x onerror=alert(1)>
```
Bet šitą testuoti tik legaliose lab aplinkose.
---

## 17. Input sanitisation
Input sanitisation reiškia pavojingo arba nepageidaujamo input išvalymą.
Paprastai:
> Sanitisation = iš input pašalinami arba neutralizuojami pavojingi simboliai/kodas.
Pvz user įveda:
```html
<h1>Hello</h1>
```
Saugiai atvaizduojant tai turėtų būti parodyta kaip tekstas, ne HTML elementas:
```txt
<h1>Hello</h1>
```
Tam naudojamas escaping/encoding.
Pvz simboliai:
```txt
<  -> &lt;
>  -> &gt;
"  -> &quot;
'  -> &#x27;
&  -> &amp;
```
Svarbi taisyklė:
> User input turi būti traktuojamas kaip data, ne kaip code.
---

## 18. Input validation vs sanitisation vs encoding
Validation:
> Patikrinu ar input atitinka taisykles.
Pvz:
- email turi būti email formato
- age turi būti number
- username max 20 simbolių
- date turi būti valid date
Sanitisation:
> Išvalau arba pašalinu pavojingą input dalį.
Pvz:
- pašalinu `<script>`
- blokuoju neleistinus tag’us
Encoding/Escaping:
> Paverčiu specialius simbolius į saugų tekstinį formatą.
Pvz:
```html
&lt;script&gt;alert(1)&lt;/script&gt;
```
Praktinis security mąstymas:
> Dažnai geriausia yra ne “bandyti išvalyti blogį”, o teisingai encode’inti output pagal kontekstą.
Kontekstai:
- HTML body
- HTML attribute
- JavaScript string
- URL
- CSS
Kiekvienam kontekstui reikia tinkamo encoding.
---

## 19. Kodėl client-side negalima pasitikėti
Client-side yra vartotojo kontrolėje.
Useris gali:
- keisti HTML per DevTools
- keisti JavaScript
- keisti form fields
- siųsti custom requests
- modifikuoti cookies/local storage
- išjungti frontend validation
- keisti hidden inputs
Pavyzdys:
```html
<input type="hidden" name="price" value="100">
```
Attacker gali pakeisti:
```html
<input type="hidden" name="price" value="1">
```
Jeigu serveris tuo tiki, sistema pažeidžiama.
Taisyklė:
> Client-side gali padėti user experience, bet security sprendimai turi būti server-side.
---

## 20. Praktinė vertė white hat mokymuisi
Šitas roomas yra web security pamatas.
Kur tai naudosi:
- Burp Suite
- PortSwigger labs
- source code review
- HTML injection
- XSS pradžia
- auth testing
- hidden field manipulation
- JavaScript analysis
- API endpoint discovery
- sensitive data exposure checks
- frontend validation bypass
Praktinis checklist:
```txt
Ar source code turi comments?
Ar yra hidden links?
Ar yra JS files su endpoints?
Ar yra hardcoded keys/secrets?
Ar form fields siunčia sensitive values?
Ar hidden fields galima keisti?
Ar input atsispindi puslapyje?
Ar input interpretuojamas kaip HTML?
Ar frontend validation dubliuojama server-side?
Ar cookies/tokens saugiai laikomi?
```
---

## 21. Basic legal test flow
Tik legaliose lab aplinkose arba su leidimu.
1. Atidaryti website
2. View Page Source
3. Inspect Elements
4. Patikrinti Network tab
5. Rasti forms/inputs
6. Įvesti paprastą tekstą
7. Pažiūrėti ar tekstas atsispindi response/DOM
8. Pabandyti saugų HTML testą:
```html
<h1>test</h1>
```
9. Pažiūrėti ar renderina kaip HTML ar kaip tekstą
10. Jei renderina kaip HTML, užsirašyti HTML Injection possibility
11. Toliau neeskaluoti be leidimo
Svarbi mintis:
> Pirmas tikslas yra suprasti elgesį, ne iškart “laužti”.
---

## 22. Komandos / įrankiai
Atidaryti source:
```txt
Ctrl + U
```
DevTools:
```txt
F12
```
Inspect:
```txt
Right click -> Inspect
```
Curl gauti HTML:
```bash
curl http://example.com/
```
Curl su headers:
```bash
curl -i http://example.com/
```
Išsaugoti HTML į failą:
```bash
curl http://example.com/ -o page.html
```
Ieškoti comments HTML faile:
```bash
grep -n "<!--" page.html
```
Ieškoti žodžių:
```bash
grep -ni "password\|admin\|key\|secret\|token" page.html
```
Ieškoti JS failų HTML’e:
```bash
grep -n ".js" page.html
```
Parsiųsti JS failą:
```bash
curl http://example.com/static/app.js -o app.js
```
Ieškoti sensitive žodžių JS faile:
```bash
grep -ni "api\|key\|secret\|token\|admin\|debug" app.js
```
---

## 23. Mano paaiškinimas žmonių kalba
Website yra kaip tattoo shop front desk ir back room.
Client-side:
> Tai ką klientas mato salėje.
Server-side:
> Tai kas vyksta už durų: kalendorius, mokėjimai, klientų info, sprendimai.
HTML:
> Baldai ir sienos.
CSS:
> Dekoras, spalvos, apšvietimas.
JavaScript:
> Mygtukai, kurie kažką padaro, durys kurios atsidaro, forma kuri sureaguoja.
Backend:
> Tikras sprendimų priėmimas.
Jeigu salėje ant lapuko palikai:
```txt
admin password: test123
```
Tai nėra “paslėpta”.
Tai tiesiog padėta ant stalo.
Tas pats su source code:
> Jeigu browseris tai gavo, useris gali tai pamatyti.
---

## 24. Ką būtina atsiminti
- Website turi client-side ir server-side dalis.
- Client-side veikia browseryje.
- Server-side veikia serveryje.
- HTML kuria puslapio struktūrą.
- CSS kuria stilių.
- JavaScript kuria elgesį/interaktyvumą.
- DOM yra browserio sukonstruotas puslapio modelis.
- View Page Source rodo serverio atsiųstą HTML.
- DevTools Elements rodo live DOM.
- Source ir DOM gali skirtis.
- Viskas client-side gali būti matoma ir manipuliuojama.
- Frontend validation nėra security.
- Serveris turi validuoti input ir permissions.
- Sensitive data neturi būti client-side kode.
- HTML comments gali išduoti credentials, links ar debug info.
- HTML Injection atsiranda, kai user input atvaizduojamas kaip HTML.
- User input turi būti traktuojamas kaip data, ne code.
- Sanitisation/encoding padeda išvengti injection problemų.
---

## 25. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- React/Vue/Angular internals
- full DOM-based XSS chains
- CSP bypass
- advanced browser security model
- prototype pollution
- DOM clobbering
- build tools
- webpack source maps gylį
- SSR/CSR hydration problemas
- OAuth frontend flows
Dabar svarbiausia:
> Suprasti, ką browseris gauna, ką jis renderina, ką galima matyti source/DevTools, ir kodėl client-side nėra saugumo riba.
---

## 26. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kuo skiriasi client-side ir server-side?
2. Kodėl client-side negalima pasitikėti?
3. Ką reiškia HTML?
4. Ar HTML yra programavimo kalba?
5. Kam naudojamas HTML?
6. Kam naudojamas CSS?
7. Kam naudojamas JavaScript?
8. Kas yra DOM?
9. Kuo skiriasi View Page Source ir DevTools Elements?
10. Kodėl frontend validation nėra security?
11. Kokios info ieškotum source code?
12. Kodėl HTML comments gali būti pavojingi?
13. Kas yra Sensitive Data Exposure?
14. Kodėl API key neturi būti JavaScript faile?
15. Kas yra HTML Injection?
16. Kuo HTML Injection skiriasi nuo XSS?
17. Kas yra input sanitisation?
18. Kuo validation skiriasi nuo sanitisation?
19. Kodėl output encoding svarbus?
20. Ką pirmiausia patikrintum testuodamas paprastą web puslapį?
---

## 27. Mano verdiktas
Šitas roomas yra bazinis, bet labai svarbus web hacking pamatui.
Statusas:
```txt
Status: Foundation
Depth: Low
Practical value: High
Revisit: verta prieš Burp Suite, XSS, HTML Injection, source review ir PortSwigger pradžią
```
Svarbiausia mintis:
> Website nėra tik vaizdas ekrane. Tai HTML, JavaScript, browserio DOM, HTTP requestai ir server-side logika.
Jeigu supranti:
- ką browseris gauna
- ką JavaScript keičia
- kur gali slėptis sensitive data
- kodėl input turi būti tikrinamas
- kaip atsiranda HTML Injection
tada web security pradeda atrodyti kaip mechanika, ne magija.
Core rule:
> Jeigu tai atėjo į browserį, laikyk kad useris gali tai pamatyti ir pakeisti.
