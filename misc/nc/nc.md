# `nc` / Netcat — trumpas Bandit cheat sheet

# Kas yra `nc`
`nc` reiškia `netcat`.
Tai įrankis, leidžiantis kalbėtis su network servisais per TCP arba UDP.
Paprasčiausias mental model:
`nc = terminalinis būdas prisijungti prie hosto ir porto`
Pvz.:
`nc localhost 30000`
reiškia:
`prisijunk prie localhost ant porto 30000`

---

## Pagrindinė forma
`nc [host] [port]`
Pvz.:
`nc localhost 30000`
Kur:
`localhost` = hostas / serveris  
`30000` = portas

---

## Kas yra hostas
Hostas yra mašina, prie kurios jungiesi.
Pvz.:
`localhost`
arba:
`127.0.0.1`
arba:
`bandit.labs.overthewire.org`
Svarbu:
`localhost` visada reiškia **tą pačią mašiną, kurioje paleidi komandą**.
Jeigu paleidi savo Ubuntu:
`localhost = tavo Ubuntu`
Jeigu paleidi būdamas prisijungęs į Bandit:
`localhost = Bandit serveris`

---

## Kas yra portas
Portas yra konkretaus serviso „durys“ hoste.
Pvz.:
`localhost:30000`
reiškia:
`tas pats serveris, portas 30000`
Jeigu portas atidarytas, ten gali klausytis kažkoks servisas.

---

## Kaip veikia `nc`
Kai paleidi:
`nc localhost 30000`
gali atrodyti, kad niekas nevyksta.
Tai normalu.
Dažnai `nc` nerodo jokio prompto. Jis tiesiog laukia inputo.
Schema:
`nc prisijungia prie porto`
`↓`
`tu įvedi tekstą`
`↓`
`servisas gauna tavo tekstą`
`↓`
`servisas atsako`

---

## Rankinis naudojimas
Paleidi:
`nc localhost 30000`
Tada įklijuoji tekstą/passwordą:
`mano_passwordas`
Spaudi Enter.
Jeigu servisas suprogramuotas atsakyti, jis išveda atsakymą terminale.

---

## Naudojimas su pipe
Galima tekstą paduoti automatiškai:
`echo "tekstas" | nc localhost 30000`
Kas vyksta:
`echo "tekstas"` išveda tekstą  
`|` paduoda tą tekstą į `nc`  
`nc` nusiunčia tekstą servisui  
servisas grąžina atsakymą
Pvz. Bandit tipo logika:
`echo "current_level_password" | nc localhost 30000`

---

## `nc` ir `nmap` skirtumas

`nmap` naudojamas patikrinti, kas atidaryta.
Pvz.:
`nmap localhost -p 30000`
Tai atsako maždaug:
`30000/tcp open`
Bet `nmap` nekalba su servisu kaip vartotojas. Jis labiau scanina.
`nc` jau leidžia realiai prisijungti ir siųsti tekstą.
Schema:
`nmap = ar portas atidarytas?`
`nc = prisijungiu prie porto ir kalbuosi`

---

## TCP servisų mental model
Servisas gali klausytis porto:
`localhost:30000`
Tu su `nc` prisijungi:
`nc localhost 30000`
Tada tavo terminalas tampa kaip raw pokalbio langas su tuo servisu.
Tai nėra SSH.  
Tai nėra web browseris.  
Tai nėra graži programa su menu.
Tai tiesiog:
`input → service → output`

---

## Dažnas WTF momentas
* `nc` nieko nerodo 
Tai nebūtinai erroras.
Jeigu terminalas tiesiog laukia, reiškia ryšys gali būti atidarytas ir servisas laukia tavo inputo.
Tada reikia rašyti tekstą ir spausti Enter.

---

### Neaišku, kada naudoti `nc`

Naudok `nc`, kai užduotis sako kažką kaip:
`connect to port`
`submit password to port`
`send data to localhost:PORT`
`there is a service listening on port`
`communicate with TCP service`

---

## Naudingos komandos
Prisijungti prie TCP porto:
`nc localhost 30000`
Patikrinti portą su nmap:
`nmap localhost -p 30000`
Paduoti tekstą į servisą:
`echo "tekstas" | nc localhost 30000`
Paduoti failo turinį į servisą:
`cat failas.txt | nc localhost 30000`

---

## Bandit tipo pavyzdys
Užduotis:
`Submit the password of the current level to port 30000 on localhost.`
Reiškia:
`esu prisijungęs į Bandit`
`↓`
`localhost = Bandit serveris`
`↓`
`port 30000 = servisas`
`↓`
`nc localhost 30000`
`↓`
`įvedu dabartinio levelio passwordą`
`↓`
`gaunu kito levelio passwordą`

---

## Trumpa santrauka
`nc host port`
`nc` leidžia prisijungti prie porto.
`localhost` reiškia mašiną, kurioje paleidi komandą.
Jeigu `nc` nieko nerodo, tai dažnai reiškia, kad jis laukia inputo.
`nmap` patikrina portus.
`nc` kalbasi su servisais.
Pipe leidžia paduoti inputą automatiškai:
`echo "tekstas" | nc host port`
