# `openssl` — trumpas Bandit cheat sheet

# Kas yra `openssl`
`openssl` yra SSL/TLS/crypto įrankių rinkinys.
Jis gali dirbti su:
`SSL/TLS connections`
`certificates`
`keys`
`encryption`
`hashing`
`testing secure services`

Bandit kontekste svarbiausia:
`openssl` leidžia prisijungti prie servisų, kurie naudoja SSL/TLS encryption.

---

## Svarbus mental model
`openssl` yra toolbox.
Pats `openssl` vienas dažniausiai nieko konkretaus nedaro.
Reikia subkomandos.

Pvz.:
`openssl s_client`
reiškia:
`naudok OpenSSL kaip SSL/TLS klientą`

Schema:
`openssl`
`↓`
`subkomanda`
`↓`
`options`
`↓`
`target / input`

---

## Kas yra `s_client`
`s_client` yra OpenSSL subkomanda.
Ji veikia kaip SSL/TLS klientas.
Tai panašu į `nc`, bet su TLS supportu.

Mental model:
`nc = plain TCP klientas`
`openssl s_client = SSL/TLS TCP klientas`

Pvz. jeigu servisas laukia encrypted connection, `nc` gali netikti.
Tada reikia `openssl s_client`.

---

## Kas yra SSL/TLS
SSL/TLS = encrypted/saugus ryšio sluoksnis.
Paprastai:
`plain TCP` = tekstas eina tiesiai
`SSL/TLS` = pirma sukuriamas encrypted kanalas, tada per jį siunčiamas tekstas

Pvz. web pasaulyje:
`HTTP` = plain
`HTTPS` = HTTP over TLS

Bandit kontekste:
`nc host port` = plain TCP
`openssl s_client ...` = TCP + TLS handshake

---

## Kas yra TLS handshake
TLS handshake yra pradinis susitarimas tarp kliento ir serverio.
Jo metu:
`klientas prisijungia`
`serveris pateikia certificate info`
`abi pusės susitaria dėl encryption`
`sukuriamas saugus kanalas`
`tada galima siųsti tekstą per TLS`

Dėl to paleidus `openssl s_client` matai daug certificate/TLS outputo.
Tai normalu.

---

## Kas yra `-connect`
`s_client` turi žinoti, prie kurio hosto ir porto jungtis.
Tam naudojamas:
`-connect`

Forma:
`-connect host:port`

Pvz.:
`-connect localhost:30001`

Svarbu:
su `nc` host ir port eina atskirai:
`nc localhost 30000`

su `openssl s_client` targetas eina kaip vienas gabalas:
`host:port`

---

## Pagrindinė forma
`openssl s_client -connect host:port`

Kur:
`openssl` = OpenSSL toolbox
`s_client` = SSL/TLS client režimas
`-connect` = nurodyk targetą
`host:port` = kur jungtis

Bandit tipo target:
`localhost:30001`

---

## `openssl s_client` ir `nc` skirtumas
`nc`:
`prisijungia prie TCP porto`
`siunčia plaintext`
`tinka paprastiems servisams`

`openssl s_client`:
`prisijungia prie TCP porto`
`padaro TLS handshake`
`siunčia tekstą per encrypted kanalą`
`tinka SSL/TLS servisams`

Schema:
`nc → plain TCP`
`openssl s_client → TLS TCP`

---

## Kada naudoti `openssl s_client`
Naudok, kai užduotis sako kažką kaip:
`using SSL/TLS encryption`
`SSL service`
`TLS service`
`encrypted connection`
`HTTPS-like connection`
`connect securely to port`

Bandit pavyzdys:
`submit password to port 30001 on localhost using SSL/TLS encryption`

Tai reiškia:
`plain nc nebeužtenka`
`reikia TLS kliento`

---

## Kas yra `localhost` su OpenSSL
`localhost` visada reiškia tą mašiną, kurioje paleidi komandą.

Jeigu paleidi savo Ubuntu:
`localhost = tavo Ubuntu`

Jeigu paleidi būdamas prisijungęs į Bandit:
`localhost = Bandit serveris`

Bandit užduotyse dažnai reikia komandą leisti pačiame Bandit levelyje.
Tada:
`localhost:30001`
reiškia:
`Bandit serverio portas 30001`

---

## Ką matai prisijungus
Paleidus `openssl s_client`, gali matyti:
`CONNECTED`
`certificate chain`
`server certificate`
`SSL handshake info`
`cipher info`
`Verify return code`

Tai normalu.
Svarbiausia: po prisijungimo gali įvesti tekstą/passwordą.
Tada servisas atsako.

---

## Helpful note: `DONE`, `RENEGOTIATING`, `KEYUPDATE`
`openssl s_client` nėra visiškai toks pats kaip `nc`.
Jis turi connected commands — specialias komandas, kurios veikia jau prisijungus.

Dėl to užduotis perspėja:
`Getting DONE, RENEGOTIATING or KEYUPDATE? Read CONNECTED COMMANDS section in the manpage.`

Mintis:
kai kurie dalykai gali būti interpretuojami kaip TLS control komandos, ne paprastas tekstas.

---

## Kur ieškoti help
Bendras OpenSSL help:
`man openssl`

Konkrečiai apie s_client:
`man openssl-s_client`

Greitas help:
`openssl s_client -help`

Ieškoti naudinga:
`s_client`
`-connect`
`CONNECTED COMMANDS`

---

## Bandit tipo eiga
Užduotis:
`submit password to localhost port 30001 using SSL/TLS encryption`

Reiškia:
`esu prisijungęs į Bandit`
`↓`
`localhost = Bandit serveris`
`↓`
`port 30001 = TLS servisas`
`↓`
`openssl s_client prisijungia prie localhost:30001`
`↓`
`įvedu current level passwordą`
`↓`
`gaunu next level passwordą`

---

## Dažnos klaidos
* Naudoti `nc`
Jeigu portas laukia SSL/TLS, `nc` kalba per plikai.

* Rašyti host ir port kaip su `nc`
Su `openssl s_client` targetas eina kaip:
`host:port`

* Išsigąsti certificate outputo
Daug teksto po connect yra normalu.

* Galvoti, kad `openssl` automatiškai žino targetą
Nežino. Tam reikia `-connect`.

* Supainioti `localhost`
Jeigu komandą paleidi Bandit serveryje, `localhost` yra Bandit serveris, ne tavo Ubuntu.

---

## Naudingos komandos
Žiūrėti s_client help:
`openssl s_client -help`

Žiūrėti manualą:
`man openssl-s_client`

Prisijungimo forma:
`openssl s_client -connect host:port`

Bandit target forma:
`openssl s_client -connect localhost:30001`

---

## Trumpa santrauka
`openssl` yra SSL/TLS toolbox.
`s_client` yra TLS kliento režimas.
`-connect` nurodo targetą.
Target forma:
`host:port`

`nc` tinka plain TCP.
`openssl s_client` tinka SSL/TLS TCP.

Bandit kontekste:
`using SSL/TLS encryption`
reiškia:
`naudok TLS-aware klientą, ne paprastą nc`
