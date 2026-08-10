# Konceptas — `setuid`

## Kas yra `setuid`
`setuid` yra specialus permission bitas ant executable failo.
Jis leidžia programai būti vykdomai ne kaip tas useris, kuris ją paleido, o kaip failo savininkas.

Paprastai:
`vartotojas paleidžia programą`
`↓`
`programa veikia to vartotojo teisėmis`

Su `setuid`:
`vartotojas paleidžia programą`
`↓`
`programa veikia failo ownerio teisėmis`

---

## Svarbiausia mintis
`setuid` nėra komanda.
`setuid` nėra package.
`setuid` yra failo permission mechanizmas.

Tai ne kažkas, ką instaliuoji ar paleidi tiesiogiai.
Tai savybė, kuri būna uždėta ant executable binary failo.

---

## Kodėl tai naudinga
Kartais paprastam useriui reikia atlikti vieną konkretų veiksmą su didesnėmis teisėmis, bet jam negalima duoti pilno root/admin priėjimo.

Tada sukuriama programa, kuri:
`priklauso privilegijuotam useriui`
`turi setuid bitą`
`leidžia atlikti tik konkretų veiksmą`

Tai kontroliuojamas privilege switch.

---

## Effective user ID
Linux'e svarbu atskirti:
`real user ID` = kas tu esi realiai
`effective user ID` = kieno teisėmis procesas šiuo metu veikia

Pvz.:
`tu esi bandit19`
`paleidi setuid binary, kurio owneris bandit20`
`procesas veikia su effective user ID bandit20`

Tai nereiškia, kad tu tapai `bandit20`.
Tai reiškia, kad konkreti programa laikinai veikia su `bandit20` teisėmis.

---

## Kaip atrodo permissionuose
Normalus executable:
`-rwxr-x---`

Setuid executable:
`-rwsr-x---`

Svarbus simbolis:
`s`

Tas `s` owner execute vietoje reiškia:
`setuid bit is set`

---

## Ką reiškia `s`
Permission dalis:
`rwx`

Reiškia:
`r` = read
`w` = write
`x` = execute

Kai yra setuid, owner execute vietoje gali matytis:
`s`

Pvz.:
`rws`

Tai reiškia:
`owner gali execute`
`setuid bitas įjungtas`

---

## Jeigu matai didžiąją `S`
Kartais gali būti:
`rwS`

Didžioji `S` reiškia:
`setuid bitas įjungtas`
`bet execute bitas nėra įjungtas`

Tai dažniausiai blogas arba keistas permission setupas, nes failas turi setuid, bet nėra executable.

---

## Bandit mental model
Tu esi:
`bandit19`

Failas:
`bandit20-do`

Jo owneris:
`bandit20`

Jis turi setuid bitą.

Kai paleidi:
`./bandit20-do kažkokią_komandą`

Įvyksta:
`bandit19 paleidžia binary`
`↓`
`binary veikia kaip bandit20`
`↓`
`binary paleidžia tavo nurodytą komandą kaip bandit20`
`↓`
`gali pasiekti dalykus, kuriuos gali bandit20`

---

## Kodėl tai leidžia skaityti passwordą
Paprastas useris `bandit19` negali skaityti:
`/etc/bandit_pass/bandit20`

Bet `bandit20` gali skaityti savo password failą.

Jeigu `bandit20-do` veikia kaip `bandit20`, tada per jį galima paleisti failo skaitymo komandą su `bandit20` teisėmis.

Schema:
`bandit19`
`↓`
`paleidžia setuid binary`
`↓`
`binary veikia kaip bandit20`
`↓`
`cat perskaito bandit20 password failą`
`↓`
`outputas parodomas bandit19 terminale`

---

## Kodėl tai nėra pilnas userio perėmimas
Setuid binary neduoda automatiškai pilno shello ar pilnos paskyros.
Jis leidžia vykdyti tai, ką ta programa leidžia vykdyti.

Jeigu programa leidžia paleisti bet kokią komandą, tai labai stipru.
Jeigu programa leidžia tik vieną ribotą veiksmą, poveikis mažesnis.

Svarbu:
`setuid` pavojingas tada, kai programa leidžia useriui per daug kontroliuoti, ką ji vykdo.

---

## Kodėl setuid binary yra pavojingi saugume
Jeigu setuid programa turi bugą arba leidžia paleisti savavališkas komandas, galima gauti privilege escalation.

Pvz. blogai:
`setuid root programa leidžia paleisti /bin/sh`

Tada paprastas useris gali gauti shellą su root teisėmis.

Todėl realiam saugume setuid failai visada yra įdomūs:
`find setuid binaries`
`patikrinti ownerį`
`patikrinti ką jie leidžia vykdyti`
`ieškoti command injection`
`ieškoti path hijacking`
`ieškoti netinkamų permissionų`

---

## Root pavyzdys
Jeigu failo owneris yra `root` ir jis turi setuid bitą:
`paprastas useris paleidžia programą`
`↓`
`programa veikia kaip root`

Tai nebūtinai blogai.
Kai kurios legalios sistemos programos taip veikia, nes joms reikia trumpam atlikti privilegijuotą veiksmą.

Bet jeigu tokia programa nesaugi, tai gali tapti root privilege escalation.

---

## Bandit pavyzdys nėra root
Šitam Bandit levelyje esmė nėra root.
Esmė:
`bandit19 → bandit20`

Tai privilege escalation į kitą level userį, ne į root.

Svarbiausias principas tas pats:
`programa veikia failo ownerio teisėmis`

---

## Kaip ieškoti setuid failų
Setuid failų galima ieškoti pagal permission bitą.

Svarbi idėja:
`ieškau executable failų, kurie turi setuid bitą`

Tokių failų auditas realiam penteste yra dažnas Linux privilege escalation žingsnis.

---

## Komandų konceptas
Paleisti programą normaliai:
`programa`

Paleisti failą iš current directory:
`./programa`

Paleisti setuid wrapperį su komanda:
`./wrapper komanda argumentas`

Šitam Bandit tipo atveju:
`./bandit20-do cat /etc/bandit_pass/bandit20`

Kur:
`./bandit20-do` = setuid wrapperis
`cat` = komanda, kurią wrapperis paleidžia
`/etc/bandit_pass/bandit20` = argumentas komandai `cat`

---

## Dažnos klaidos
* Galvoti, kad `setuid` yra komanda
Ne. Tai permission bitas.

* Galvoti, kad setuid visada reiškia root
Ne. Programa veikia kaip failo owneris. Owneris gali būti root, bandit20 ar kitas useris.

* Paleisti executable be `./`
Jeigu failas yra current directory ir nėra PATH'e, reikia `./failas`.

* Maišyti real user ir effective user
Tu lieki tas pats useris, bet procesas gali veikti kito userio teisėmis.

* Manyti, kad setuid automatiškai duoda shellą
Ne. Jis tik pakeičia programos vykdymo teises. Ką gali padaryti, priklauso nuo pačios programos.

---

## Vienos eilutės apibrėžimas
`setuid` yra Linux permission bitas, kuris leidžia executable failui veikti failo savininko teisėmis, o ne to userio, kuris jį paleido.
