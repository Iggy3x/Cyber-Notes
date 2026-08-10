### Intro ### 

# METASPLOIT: METERPRETER

## Kas yra Meterpreter?

**Meterpreter** yra pažangus Metasploit payload'as, naudojamas penetration testing procese.

Jis veikia kaip agentas ant target sistemos ir leidžia attacker'iui / pentesteriui bendrauti su ta sistema per command-and-control tipo architektūrą.

Paprasčiau:

> Meterpreter = payload'as, kuris po exploit'o suteikia interaktyvią sesiją target sistemoje.

Per Meterpreter galima:
- naršyti target failų sistemą
- vykdyti komandas
- rinkti informaciją apie sistemą
- matyti procesus
- migruoti tarp procesų
- naudoti specialias Meterpreter komandas, kurios nėra paprastas Windows/Linux shell

---

## Meterpreter versijos

Meterpreter turi daug skirtingų versijų/payload'ų.

Skirtingos versijos priklauso nuo:
- target operacinės sistemos
- architektūros: x86 / x64
- ryšio tipo
- naudojamo payload formato
- norimos funkcijos

Pvz. Windows target'ui bus naudojamas vienoks Meterpreter payload'as, Linux target'ui — kitoks.

---

## Kaip Meterpreter veikia?

Meterpreter veikia ant target sistemos, bet nėra normaliai „įdiegtas“ kaip programa.

Svarbus principas:

> Meterpreter dažniausiai veikia RAM atmintyje ir nerašo savęs į diską.

Tai reiškia:
- target sistemoje nebūtinai atsiras aiškus `.exe` failas
- antivirusui sunkiau aptikti jį paprastu failų skenavimu
- Meterpreter dažniau matomas kaip procesas, o ne kaip atskiras failas diske

RAM = Random Access Memory, laikina darbinė kompiuterio atmintis.

---

## Kodėl veikimas RAM'e svarbus?

Daug antivirusų pirmiausia tikrina naujus failus diske.

Pvz. jeigu atsisiunti `.exe` failą, antivirusas jį gali nuskenuoti.

Meterpreter bando išvengti tokio paprasto aptikimo, nes jis gali veikti atmintyje be aiškaus failo diske.

Bet svarbu:

> Tai nereiškia, kad Meterpreter yra nematomas.

Daug modernių antivirusų ir EDR sprendimų Meterpreter vis tiek atpažįsta pagal elgesį, atmintį, procesų veiklą ar komunikaciją.

---

## IDS ir IPS

Meterpreter taip pat bando išvengti tinklo lygio aptikimo.

Terminai:

**IDS** = Intrusion Detection System  
Sistema, kuri stebi tinklą ir bando aptikti įtartiną veiklą.

**IPS** = Intrusion Prevention System  
Sistema, kuri ne tik aptinka, bet gali ir blokuoti įtartiną veiklą.

Meterpreter naudoja šifruotą komunikaciją su attacker'io sistema, todėl tinklo apsaugoms sunkiau matyti, kas tiksliai vyksta.

Pvz. jeigu komunikacija eina per šifruotą kanalą, paprastas IDS/IPS gali matyti ryšį, bet nebūtinai matys jo turinį.

---

## Ar Meterpreter stealthy?

Taip, Meterpreter turi stealth savybių:
- gali veikti RAM'e
- nebūtinai palieka aiškų failą diske
- naudoja šifruotą komunikaciją
- gali veikti prisidengęs kitu procesu

Bet realybė tokia:

> Modernūs antivirusai, EDR ir saugumo įrankiai dažnai Meterpreter aptinka.

Tai nėra magiškas nematomumo apsiaustas. Greičiau senas geras „bandau nepritraukti dėmesio“ metodas.

---

## getpid komanda

Meterpreter komanda:

`getpid`

Parodo, kokiu proceso ID šiuo metu veikia Meterpreter sesija.

Pvz.

`meterpreter > getpid`

Rezultatas:

`Current pid: 1304`

Tai reiškia, kad Meterpreter šiuo metu veikia procese, kurio PID yra `1304`.

---

## Kas yra PID?

**PID** = Process ID / Process Identifier

Tai unikalus numeris, kurį operacinė sistema suteikia kiekvienam veikiančiam procesui.

Pvz. Windows ar Linux sistemoje kiekviena programa/procesas turi savo PID.

PID naudojamas tam, kad sistema galėtų:
- atpažinti procesą
- valdyti procesą
- sustabdyti procesą
- stebėti proceso veiklą

---

## ps komanda Meterpreter'yje

Meterpreter komanda:

`ps`

Parodo target sistemoje veikiančių procesų sąrašą.

Pvz. jeigu `getpid` parodo PID `1304`, o `ps` sąraše PID `1304` yra `spoolsv.exe`, tai reiškia, kad Meterpreter veikia tame procese.

Svarbu:

> Procesas gali vadintis `spoolsv.exe`, bet Meterpreter gali veikti jo viduje.

Tai nereiškia, kad failas vadinsis `meterpreter.exe`.

---

## spoolsv.exe pavyzdys

THM pavyzdyje Meterpreter veikia procese:

`spoolsv.exe`

`spoolsv.exe` yra Windows Print Spooler procesas.

Jis susijęs su printerių/spausdinimo paslaugomis.

Pavyzdžio esmė:

> Meterpreter gali veikti kaip dalis kito proceso, todėl ne visada matysi akivaizdų „meterpreter.exe“.

---

## tasklist komanda Windows sistemoje

Komanda:

`tasklist /m /fi "pid eq 1304"`

Naudojama Windows command prompt'e.

Ji parodo informaciją apie konkretų procesą ir jo naudojamus modulius/DLL failus.

---

## Komandos išskaidymas

`tasklist`

Parodo veikiančių procesų sąrašą Windows sistemoje.

`/m`

Parodo modulius / DLL failus, kuriuos naudoja procesas.

DLL = Dynamic-Link Library  
Tai Windows bibliotekos failas, kurį procesas gali naudoti savo darbui.

`/fi`

Reiškia filter.

Naudojama, kai nori filtruoti `tasklist` rezultatą pagal tam tikrą sąlygą.

`"pid eq 1304"`

Filtro sąlyga.

Reiškia:

> parodyk tik tą procesą, kurio PID yra lygus 1304.

`eq` = equals / lygu.

Visa komanda reiškia:

> Parodyk procesą su PID 1304 ir išvardink jo naudojamus modulius/DLL failus.

---

## Kodėl THM rodo tasklist /m /fi?

THM nori parodyti, kad net pažiūrėjus į procesą giliau, nebūtinai pamatysi kažką akivaizdaus kaip `meterpreter.dll`.

Pvz. komanda rodo daug normalių Windows DLL failų:
- ntdll.dll
- kernel32.dll
- USER32.dll
- ADVAPI32.dll
- WS2_32.dll
- ir t.t.

Iš pirmo žvilgsnio viskas gali atrodyti normaliai.

Todėl šita dalis parodo Meterpreter stealth principą:

> Meterpreter gali veikti proceso viduje taip, kad paprastas procesų ar DLL sąrašas neparodys aiškaus „čia Meterpreter“.

---

## Svarbus saugumo realybės punktas

Nors Meterpreter turi stealth savybių, modernios apsaugos sistemos gali jį aptikti.

Aptikimas gali vykti pagal:
- atminties analizę
- proceso elgesį
- neįprastą tinklo komunikaciją
- exploit požymius
- antivirus/EDR signatūras
- suspicious parent-child process ryšius

Todėl negalima galvoti:

> „Jeigu nėra failo diske, tai niekas nemato.“

Mato. Tik ne visada paprastu būdu.

---

## Mano trumpas verdiktas

Meterpreter yra Metasploit payload'as, kuris po sėkmingo exploit'o suteikia interaktyvią kontrolę target sistemoje.

Jis dažnai veikia RAM'e, gali būti susietas su kitu procesu ir naudoja šifruotą komunikaciją, todėl yra stealthier nei paprastas `.exe` failas.

Bet tai nėra nematomas įrankis — modernūs antivirusai ir EDR sprendimai dažnai jį atpažįsta.

Svarbiausia suprasti ne tik komandą, o logiką:

> Meterpreter nėra tiesiog failas. Jis yra sesija/procesas/agentas, veikiantis target sistemoje po exploit'o.

## Mini recap: Meterpreter stealth logika

Meterpreter dažnai veikia RAM atmintyje, o ne kaip paprastas `.exe` failas diske. Tai sumažina paprasto antivirusinio failų skenavimo efektyvumą, nes nėra aiškaus failo, kurį būtų lengva pagauti.

`getpid` parodo PID — Process ID — proceso numerį, kuriame šiuo metu veikia Meterpreter sesija. PID nėra IP adresas.

Jeigu `getpid` rodo `1304`, o `ps` sąraše PID `1304` yra `spoolsv.exe`, tai reiškia, kad Meterpreter veikia tame procese arba jo atmintyje. Todėl procesų sąraše nebūtinai matysi `meterpreter.exe`.

Windows komanda:

`tasklist /m /fi "pid eq 1304"`

reiškia:
- `tasklist` — parodyti procesus
- `/m` — parodyti proceso naudojamus modulius / DLL
- `/fi` — naudoti filtrą
- `"pid eq 1304"` — rodyti tik procesą, kurio PID lygus 1304

Svarbu: Meterpreter turi stealth savybių, bet modernūs AV/EDR sprendimai dažnai jį aptinka pagal elgesį, atmintį, tinklo ryšius ar signatūras.


## PID vs IP vs ID

**PID** = Process ID  
Tai proceso numeris operacinės sistemos viduje.

Pvz. `1304` gali būti proceso PID.

**IP** = Internet Protocol address  
Tai įrenginio/tinklo adreso numeris.

Pvz. `10.10.45.23` gali būti target IP.

**ID** = bendras identifikatorius  
Tai gali reikšti daug ką, priklausomai nuo konteksto:
- user ID
- session ID
- process ID
- service ID
- object ID

Svarbu: PID ir IP nėra tas pats.

Trumpai:
- IP padeda rasti mašiną tinkle
- PID padeda rasti procesą mašinos viduje

________________________________________________________________________

# METASPLOIT: METERPRETER FLAVOURS

## Kas yra Meterpreter flavours?

**Meterpreter flavours** reiškia skirtingas Meterpreter payload versijas.

Meterpreter nėra vienas universalus payload'as viskam. Yra daug skirtingų variantų, kurie priklauso nuo:
- target operacinės sistemos
- CPU architektūros
- programavimo/platformos tipo
- ryšio tipo
- ar payload'as yra staged, ar inline/stageless

Paprasčiau:

> Meterpreter flavour = konkretus Meterpreter payload variantas konkrečiam targetui ir konkrečiam ryšio būdui.

Pvz.:
- `windows/x64/meterpreter/reverse_tcp`
- `linux/x64/meterpreter_reverse_tcp`
- `php/meterpreter_reverse_tcp`
- `android/meterpreter/reverse_https`

---

## Payload tipai: inline ir staged

Metasploit payload'ai dažnai skirstomi į dvi pagrindines grupes:
- **inline payloads**
- **staged payloads**

Meterpreter payload'ai taip pat turi staged ir inline/stageless versijas.

---

## Inline payload

**Inline payload** dar vadinamas:
- single payload
- stageless payload

Inline payload siunčiamas į targetą vienu žingsniu.

Tai reiškia:

> Visas payload'as nueina į targetą iš karto.

Privalumai:
- paprasčiau suprasti
- nereikia antro atsisiuntimo etapo
- gali būti patikimiau, jeigu ryšys nestabilus

Minusai:
- payload'as dažnai būna didesnis
- didesnį payload'ą gali būti sunkiau panaudoti, jeigu exploit turi mažai vietos

---

## Staged payload

**Staged payload** siunčiamas į targetą dviem etapais.

Pirmas etapas vadinamas:

**stager**

Stager yra maža pradinė payload dalis.

Jo darbas:

> Prisijungti atgal prie attacker'io ir atsisiųsti likusią payload dalį.

Antras etapas yra pagrindinis payload'as, pvz. Meterpreter.

Procesas atrodo taip:

1. Į targetą nueina mažas stager
2. Stager susisiekia su attacker'iu
3. Tada atsisiunčia pilną Meterpreter payload'ą
4. Atsidaro Meterpreter session

Privalumai:
- pradinis payload mažesnis
- gerai tinka exploitams, kur mažai vietos payload'ui

Minusai:
- reikia, kad antras ryšio etapas pavyktų
- daugiau tinklo veiklos
- gali būti lengviau pastebimas, jeigu apsauga seka callback'us

---

## Kaip atskirti staged ir stageless pagal pavadinimą?

Dažnai Metasploit payload pavadinime skirtumas matosi iš slash `/`.

Pavyzdys staged:

`windows/x64/meterpreter/reverse_tcp`

Čia yra:

`platform/arch/meterpreter/stager_type`

Šitas variantas dažniausiai reiškia staged payload.

Pavyzdys stageless / inline:

`windows/x64/meterpreter_reverse_tcp`

Čia `meterpreter_reverse_tcp` eina kaip vienas payload pavadinimas.

Svarbus pattern:

- `meterpreter/reverse_tcp` = dažnai staged
- `meterpreter_reverse_tcp` = dažnai stageless / inline

Nereikia aklai remtis vien pavadinimu, bet šitas pattern labai padeda.

---

## msfvenom payload sąrašas

Norint pamatyti galimus Meterpreter payload'us, galima naudoti:

`msfvenom --list payloads | grep meterpreter`

Komandos dalys:

`msfvenom --list payloads`

Parodo visus Metasploit payload'us.

`|`

Pipe simbolis. Jis perduoda pirmos komandos output'ą antrai komandai.

`grep meterpreter`

Išfiltruoja tik tas eilutes, kuriose yra žodis `meterpreter`.

Visa komanda reiškia:

> Parodyk visus payload'us, bet iš jų palik tik Meterpreter payload'us.

---

## Platformos, kurioms yra Meterpreter payload'ai

THM pavyzdyje rodomos Meterpreter versijos skirtingoms platformoms:
- Android
- Apple iOS
- Java
- OSX / macOS
- Linux
- PHP
- Python
- Windows

Tai reiškia, kad Meterpreter gali būti pritaikytas skirtingoms aplinkoms.

Pvz.:
- jeigu target yra Windows mašina — rinktumeisi Windows Meterpreter
- jeigu target yra Linux serveris — rinktumeisi Linux Meterpreter
- jeigu pažeidžiamumas yra PHP web aplikacijoje — gali reikėti PHP payload'o
- jeigu target aplinkoje yra Python — gali būti naudojamas Python payload'as

---

## Pagal ką pasirinkti Meterpreter flavour?

Sprendimas dažniausiai priklauso nuo 3 pagrindinių faktorių:
- target operacinės sistemos
- target sistemoje esančių komponentų
- galimo network connection tipo

---

## 1. Target operacinė sistema

Pirmas klausimas:

> Kokia target sistema?

Pvz.:
- Windows?
- Linux?
- macOS?
- Android?
- iOS?

Jeigu target yra Windows, Linux payload'as neveiks.

Jeigu target yra Linux, Windows payload'as neveiks.

Payload'as turi atitikti target sistemą.

---

## 2. Target sistemoje esantys komponentai

Antras klausimas:

> Kas target sistemoje yra įdiegta arba palaikoma?

Pvz.:
- Ar yra Python?
- Ar yra PHP?
- Ar aplikacija veikia Java aplinkoje?
- Ar tai web serveris?
- Ar architektūra x86, x64, ARM?

Pvz. jeigu target yra PHP web aplikacija, gali būti logiškiau naudoti PHP Meterpreter payload'ą.

Jeigu target neturi Python, Python payload'as tau nepadės.

Svarbu suprasti:

> Payload'as turi tikti ne tik OS, bet ir aplinkai, kurioje jis bus paleistas.

---

## 3. Galimas network connection tipas

Trečias klausimas:

> Kokį ryšį galiu turėti su target sistema?

Svarbūs variantai:
- reverse TCP
- bind TCP
- reverse HTTP
- reverse HTTPS
- IPv4
- IPv6
- named pipe
- kiti specifiniai ryšio būdai

Network situacija dažnai nulemia, kuris payload'as realiai veiks.

---

## Reverse connection

**Reverse payload** reiškia, kad target prisijungia atgal prie attacker'io.

Schema:

`Target ---> Attacker`

Tai dažnai naudojama, nes target tinkle gali būti firewall, kuris blokuoja incoming connections, bet leidžia outgoing connections.

Pvz. payload:

`windows/x64/meterpreter/reverse_tcp`

Reiškia:

> Windows x64 Meterpreter, kuris jungiasi atgal į attacker mašiną per TCP.

---

## Bind connection

**Bind payload** reiškia, kad target atidaro portą ir laukia, kol attacker prisijungs.

Schema:

`Attacker ---> Target`

Pvz. payload:

`java/meterpreter/bind_tcp`

Reiškia:

> Java Meterpreter payload'as, kuris klauso target pusėje ir laukia attacker'io prisijungimo.

Bind gali būti sunkiau naudoti, jeigu firewall blokuoja incoming connection į target.

---

## Reverse vs Bind trumpai

Reverse:

`Target jungiasi pas attacker'į`

`Target ---> Attacker`

Bind:

`Attacker jungiasi pas targetą`

`Attacker ---> Target`

Dažnai praktiškiau naudoti reverse payload, nes daug tinklų leidžia outbound traffic, bet blokuoja inbound traffic.

---

## HTTP ir HTTPS reverse payload'ai

Kai kuriais atvejais raw TCP ryšiai gali būti blokuojami arba atrodyti įtartinai.

Tada gali būti naudojami:
- `reverse_http`
- `reverse_https`

HTTPS privalumas:

> Komunikacija atrodo panašesnė į įprastą šifruotą web traffic.

Tai nereiškia, kad ji nematoma, bet tinklo apsaugoms sunkiau matyti turinį, jeigu šifruotas traffic nėra dešifruojamas ir tikrinamas.

---

## Exploit gali riboti payload pasirinkimą

Svarbu:

> Ne visada gali rinktis bet kokį payload'ą.

Jeigu naudoji Meterpreter kaip standalone payload'ą su `msfvenom`, gali turėti daug pasirinkimų.

Bet jeigu naudoji konkretų Metasploit exploit modulį, payload pasirinkimas gali būti ribotas.

Pvz. THM rodo:

`use exploit/windows/smb/ms17_010_eternalblue`

Metasploit automatiškai parenka default payload:

`windows/x64/meterpreter/reverse_tcp`

Tai reiškia:

> EternalBlue exploit modulis pagal nutylėjimą naudoja Windows x64 Meterpreter reverse TCP payload'ą.

---

## show payloads komanda

Kai esi konkrečiame exploit modulyje, gali pažiūrėti suderinamus payload'us:

`show payloads`

Ši komanda parodo payload'us, kurie yra compatible su tuo exploit moduliu.

Pvz.:

`msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads`

Tai parodys ne visus Metasploit payload'us, o tik tuos, kurie tinka tam konkrečiam exploitui.

Svarbu:

`msfvenom --list payloads`

rodo bendrą payload sąrašą.

`show payloads`

exploit modulyje rodo tik suderinamus payload'us tam moduliui.

---

## Default payload

Kai kurie Metasploit exploit moduliai turi default payload.

Pvz.:

`[*] Using configured payload windows/x64/meterpreter/reverse_tcp`

Tai reiškia, kad Metasploit automatiškai pasirinko payload'ą, kuris dažniausiai tinka tam exploitui.

Bet tu vis tiek gali pasižiūrėti kitus variantus su:

`show payloads`

Ir pakeisti payload'ą su:

`set payload <payload_name>`

Pvz.:

`set payload windows/x64/meterpreter/reverse_https`

---

## Praktinė logika renkantis payload'ą

Kai renkiesi Meterpreter flavour, galvok taip:

1. Kokia target OS?
2. Kokia architektūra? x86, x64, ARM?
3. Koks exploit naudojamas?
4. Kokie payload'ai compatible su tuo exploit?
5. Kokį ryšį leidžia tinklas?
6. Ar geriau reverse, ar bind?
7. Ar reikia TCP, HTTP, HTTPS, IPv6?
8. Ar target turi reikalingą runtime, pvz. Python/PHP/Java?

---

## Trumpas pavyzdys

Jeigu target:

- Windows 64-bit
- SMB pažeidžiamumas
- Firewall gali blokuoti incoming connection
- Bet outgoing connection į attacker leidžiamas

Tada logiškas pasirinkimas gali būti:

`windows/x64/meterpreter/reverse_tcp`

Jeigu TCP blokuojamas arba labiau tinka web traffic:

`windows/x64/meterpreter/reverse_https`

---

## Kas tavo notes buvo gerai

Tavo notes teisingai pagavo:
- payload'ai skirstomi į inline ir staged
- staged payload eina per du etapus
- Meterpreter turi daug versijų
- versijos priklauso nuo OS/platformos
- pasirinkimą lemia OS, komponentai ir network connection
- exploit gali riboti payload pasirinkimą

---

## Ką reikėjo pataisyti / papildyti

Svarbiausi pataisymai:
- `Staed` turi būti **staged**
- staged payload pirmiausia siunčia **stager**
- stager atsisiunčia likusią payload dalį
- inline payload = single / stageless payload
- reikia suprasti reverse ir bind skirtumą
- `msfvenom --list payloads | grep meterpreter` rodo galimus Meterpreter payload'us
- `show payloads` exploit modulyje rodo tik compatible payload'us tam exploitui
- exploit modulis gali turėti default payload
- ne visada gali rinktis bet kokį payload'ą

---

## Mini testas

1. Kuo skiriasi staged ir inline/stageless payload?
2. Kas yra stager?
3. Ką daro komanda `msfvenom --list payloads | grep meterpreter`?
4. Pagal kokius 3 pagrindinius dalykus renkiesi Meterpreter flavour?
5. Kuo skiriasi reverse ir bind payload?
6. Kuo skiriasi šitie du payload'ai?
   - `windows/x64/meterpreter/reverse_tcp`
   - `windows/x64/meterpreter_reverse_tcp`
7. Ką parodo `show payloads`, kai esi konkrečiame exploit modulyje?

---

## Mano trumpas verdiktas

Meterpreter turi daug flavours, nes target sistemos ir tinklo sąlygos skiriasi.

Nėra vieno geriausio Meterpreter payload'o viskam.

Payload pasirinkimas priklauso nuo:
- target OS
- architektūros
- target aplinkos
- galimo tinklo ryšio tipo
- exploit modulio suderinamumo

Svarbiausia suprasti:

> Payload'as turi tikti targetui, exploitui ir network situacijai.

Jeigu vienas iš tų trijų netinka, sesija gali tiesiog neatsidaryti.


___________________________________________________________________

# METASPLOIT: METERPRETER COMMANDS

## Help komanda

Kai turi aktyvią Meterpreter sesiją, prompt atrodo taip:

`meterpreter >`

Norint pamatyti galimas komandas, naudojama:

`help`

arba:

`?`

Šita komanda parodo Meterpreter help meniu su visomis prieinamomis komandomis.

Svarbu:

> Kiekviena Meterpreter versija gali turėti skirtingas komandas.

Pvz. Windows Meterpreter turės vienas komandas, Linux Meterpreter gali turėti kitas, Android dar kitas.

Todėl pradėjus naują Meterpreter sesiją visada verta paleisti:

`help`

---

## Kas yra Meterpreter commands?

Meterpreter commands yra built-in įrankiai, kurie leidžia dirbti su target sistema per Meterpreter sesiją.

Svarbu:

> Daug Meterpreter komandų veikia target sistemoje be papildomų scriptų ar `.exe` failų įkėlimo.

Tai reiškia, kad jos yra integruotos į Meterpreter aplinką.

---

## Trys pagrindinės Meterpreter įrankių grupės

Meterpreter suteikia tris pagrindines įrankių kategorijas:

- Built-in commands
- Meterpreter tools
- Meterpreter scripting

Paprasčiau:

- **Built-in commands** — bazinės Meterpreter komandos
- **Meterpreter tools** — papildomi įrankiai ir funkcijos
- **Meterpreter scripting** — scriptai / post modules / automatizuotos funkcijos

---

## Help meniu komandų kategorijos

Paleidus `help`, komandos gali būti suskirstytos į skirtingas kategorijas.

Pvz. Windows Meterpreter versijoje gali matyti:

- Core commands
- File system commands
- Networking commands
- System commands
- User interface commands
- Webcam commands
- Audio output commands
- Elevate commands
- Password database commands
- Timestomp commands

Svarbu:

> Šitos kategorijos priklauso nuo Meterpreter versijos ir target sistemos.

Pvz. Windows payload `windows/x64/meterpreter/reverse_tcp` gali rodyti daugiau Windows-specifinių komandų negu Linux ar PHP Meterpreter payload.

---

# Core Commands

Core commands yra pagrindinės Meterpreter komandos, skirtos sesijos valdymui ir darbui su Meterpreter aplinka.

## Dažniausios core komandos

### `background`

Nusiunčia dabartinę Meterpreter sesiją į background.

Tai naudinga, kai nori grįžti į Metasploit konsolę, bet nenori uždaryti sesijos.

Paprasčiau:

> Sesija lieka gyva, bet tu grįžti į `msfconsole`.

---

### `bg`

`bg` yra trumpesnis `background` alias.

Reiškia tą patį kaip:

`background`

---

### `exit`

Uždaro Meterpreter sesiją.

Svarbu:

> `exit` baigia sesiją, o `background` tik paslepia ją į background.

---

### `guid`

Parodo sesijos GUID.

**GUID** = Globally Unique Identifier.

Tai unikalus sesijos identifikatorius.

---

### `help`

Parodo help meniu.

Naudojama:

`help`

arba:

`?`

---

### `info`

Parodo informaciją apie Post module.

Post modules yra Metasploit moduliai, naudojami po exploit'o, kai jau turi sesiją.

Pvz. informacijos rinkimui, privilege escalation bandymams, sistemų enumeracijai ir pan.

---

### `irb`

Atidaro interaktyvų Ruby shell dabartinėje sesijoje.

Ruby yra kalba, kuria parašyta didelė Metasploit dalis.

Pradedančiajam šita komanda nėra pirmo būtinumo.

---

### `load`

Įkelia Meterpreter extension'us.

Extension'ai prideda papildomų funkcijų prie Meterpreter.

---

### `migrate`

Leidžia perkelti Meterpreter sesiją į kitą procesą.

Pvz. jeigu Meterpreter veikia nestabiliame procese, galima bandyti migruoti į stabilesnį procesą.

Svarbu:

> `migrate` gali padėti išlaikyti sesiją, bet taip pat gali būti rizikinga, nes netinkama migracija gali nulaužti sesiją.

---

### `run`

Paleidžia Meterpreter scriptą arba Metasploit Post module.

Pvz. gali būti naudojama post-exploitation automatizacijai.

---

### `sessions`

Leidžia peržiūrėti ir persijungti tarp sesijų.

Pvz. jeigu turi kelias Meterpreter arba shell sesijas, su `sessions` gali jas valdyti.

---

# File System Commands

File system commands leidžia dirbti su target failų sistema.

## Dažniausios file system komandos

### `pwd`

Parodo dabartinį katalogą target sistemoje.

Reiškia:

> Print Working Directory

---

### `cd`

Pakeičia katalogą.

Pvz. naudojama judėti per target failų sistemą.

---

### `ls`

Parodo failus ir katalogus dabartinėje vietoje.

Windows Meterpreter sesijoje dažnai veikia ir:

`dir`

---

### `cat`

Parodo failo turinį ekrane.

Naudojama greitai perskaityti tekstinį failą.

---

### `edit`

Leidžia redaguoti failą.

Svarbu:

> Reikia atsargiai, nes pakeitus target failus galima sugadinti sistemą arba palikti aiškius pėdsakus.

---

### `rm`

Ištrina nurodytą failą.

Svarbu:

> Failų trynimas target sistemoje yra destruktyvus veiksmas. Laboratorijoje suprasti galima, realiame penteste reikia leidimo ir atsargumo.

---

### `search`

Ieško failų target sistemoje.

Naudinga, kai ieškai konkretaus failo pagal pavadinimą ar pattern.

---

### `upload`

Įkelia failą arba katalogą iš attacker mašinos į target sistemą.

Kryptis:

`Attacker ---> Target`

---

### `download`

Atsisiunčia failą arba katalogą iš target sistemos į attacker mašiną.

Kryptis:

`Target ---> Attacker`

---

# Networking Commands

Networking commands leidžia matyti ir valdyti target tinklo informaciją.

## Dažniausios networking komandos

### `arp`

Parodo target sistemos ARP cache.

**ARP** = Address Resolution Protocol.

ARP susieja IP adresus su MAC adresais lokaliame tinkle.

Paprasčiau:

> ARP cache gali parodyti, su kokiais kitais įrenginiais target neseniai bendravo lokaliame tinkle.

---

### `ifconfig`

Parodo target sistemos network interfaces.

Pvz. gali parodyti:
- IP adresus
- tinklo adapterius
- subnet informaciją

Windows aplinkoje tai panašu į `ipconfig`.

---

### `netstat`

Parodo target sistemos tinklo ryšius.

Gali padėti matyti:
- aktyvius connection'us
- listening portus
- su kuo target bendrauja

---

### `portfwd`

Leidžia forwardinti lokalų portą į remote service per Meterpreter sesiją.

Paprasčiau:

> Gali per Meterpreter tunelį pasiekti servisą, kuris tiesiogiai tau nebūtų pasiekiamas.

Tai naudinga pivoting situacijose, kai target turi prieigą prie vidinio tinklo.

---

### `route`

Leidžia peržiūrėti ir modifikuoti routing table.

Meterpreter/Metasploit kontekste tai svarbu pivotingui.

Paprasčiau:

> `route` padeda nurodyti Metasploit'ui, per kurią sesiją pasiekti kitus vidinius tinklus.

---

# System Commands

System commands leidžia rinkti informaciją apie target sistemą ir valdyti procesus.

## Dažniausios system komandos

### `sysinfo`

Parodo informaciją apie target sistemą.

Pvz.:
- OS
- kompiuterio pavadinimą
- architektūrą
- Meterpreter tipą

---

### `getuid`

Parodo, kokio vartotojo teisėmis veikia Meterpreter.

Pvz. gali parodyti:

`NT AUTHORITY\SYSTEM`

arba paprastą user account.

Svarbu:

> `getuid` atsako į klausimą: “kas aš esu target sistemoje?”

---

### `getpid`

Parodo proceso ID, kuriame veikia Meterpreter.

**PID** = Process ID.

Svarbu:

> `getpid` parodo ne IP adresą, o proceso numerį.

---

### `ps`

Parodo veikiančių procesų sąrašą target sistemoje.

Naudinga norint:
- matyti procesus
- rasti stabilų procesą migracijai
- suprasti, kas veikia sistemoje

---

### `kill`

Nutraukia procesą pagal PID.

Pvz. galima nužudyti procesą, kurio PID žinomas.

Atsargiai:

> Nužudžius netinkamą procesą galima nulaužti sistemą arba prarasti sesiją.

---

### `pkill`

Nutraukia procesus pagal pavadinimą.

Pvz. vietoj PID naudoji proceso vardą.

---

### `execute`

Paleidžia komandą arba programą target sistemoje.

Tai leidžia vykdyti system-level veiksmus per Meterpreter.

---

### `shell`

Numeta tave į įprastą target sistemos command shell.

Windows atveju dažniausiai gausi `cmd`.

Linux atveju gali gauti `/bin/sh` ar panašų shell.

Svarbu suprasti skirtumą:

- `meterpreter >` = Meterpreter komandos
- `C:\Windows\system32>` arba `$` = target sistemos shell komandos

---

### `reboot`

Perkrauna remote kompiuterį.

---

### `shutdown`

Išjungia remote kompiuterį.

---

### `clearev`

Išvalo Windows event logs.

Svarbu:

> Šita komanda yra labai jautri. Realiame penteste logų valymas be leidimo yra bloga praktika ir gali būti laikoma veiksmų slėpimu. THM rodo komandą mokymosi tikslais, bet realybėje su logais elgiamasi pagal scope ir taisykles.

---

# Other Commands

Šitos komandos help meniu gali būti po skirtingomis kategorijomis, priklausomai nuo Meterpreter versijos.

---

## User activity / desktop commands

### `idletime`

Parodo, kiek sekundžių remote user buvo idle.

Paprasčiau:

> Parodo, kiek laiko vartotojas neaktyvus.

---

### `screenshot`

Padaro interactive desktop screenshot.

Svarbu:

> Veiks tik tada, jeigu target turi aktyvią desktop aplinką ir sesija turi tinkamas teises.

---

### `screenshare`

Leidžia realiu laiku stebėti remote user's desktop.

Svarbu:

> Ne visose sistemose veiks. Serveriai ar lab mašinos dažnai neturi normalios GUI aplinkos.

---

## Keystroke commands

### `keyscan_start`

Pradeda fiksuoti klavišų paspaudimus.

---

### `keyscan_dump`

Parodo surinktą keystroke buffer.

---

### `keyscan_stop`

Sustabdo klavišų fiksavimą.

Svarbu:

> Keylogging yra labai jautri funkcija. Realiame penteste tokie veiksmai turi būti aiškiai leidžiami scope. Be leidimo tai yra nelegalu ir etiškai šūdina teritorija.

---

## Audio / webcam commands

### `record_mic`

Įrašo garsą iš default mikrofono nurodytą sekundžių skaičių.

---

### `webcam_list`

Parodo galimas webcams.

---

### `webcam_snap`

Padaro snapshot iš nurodytos webcam.

---

### `webcam_stream`

Paleidžia video stream iš webcam.

---

### `webcam_chat`

Pradeda video chat.

Svarbu:

> Šitos komandos veiks tik tada, jeigu target turi webcam/mic, tinkamą desktop aplinką ir sesija turi reikiamas teises.

---

## Privilege / credential commands

### `getsystem`

Bando pakelti privileges iki local SYSTEM.

Windows sistemoje `NT AUTHORITY\SYSTEM` yra labai aukštas teisių lygis.

Svarbu:

> `getsystem` ne visada veiks. Tai priklauso nuo target konfigūracijos, pažeidžiamumų ir sesijos teisių.

---

### `hashdump`

Ištraukia SAM database hash'us.

**SAM** = Security Account Manager.

Windows SAM database saugo lokalios sistemos account password hash'us.

Svarbu:

> Hashdump yra credential access veiksmas. Realiame penteste tam reikia aiškaus leidimo. THM tai rodo edukaciniame lab kontekste.

---

# Kodėl ne visos komandos veiks?

Nors `help` meniu gali rodyti daug komandų, tai nereiškia, kad visos jos būtinai veiks.

Priežastys:
- target neturi webcam
- target neturi microphone
- target neturi GUI / desktop environment
- Meterpreter versija nepalaiko konkrečios funkcijos
- sesija neturi pakankamai teisių
- antivirus/EDR gali blokuoti veiksmą
- exploit/session yra nestabili
- OS/platforma nepalaiko tos komandos

Pvz.:

> Jeigu target yra lab serveris be desktop aplinkos, `screenshot`, `screenshare` ar `webcam_snap` gali neveikti.

---

# Svarbiausia logika

Meterpreter komandos nėra vienas didelis chaotiškas sąrašas. Jas galima mąstyti pagal paskirtį:

- Session control: `background`, `sessions`, `exit`
- File system: `pwd`, `cd`, `ls`, `cat`, `upload`, `download`
- Network info: `arp`, `ifconfig`, `netstat`, `route`, `portfwd`
- System info/processes: `sysinfo`, `getuid`, `getpid`, `ps`, `migrate`
- Shell access: `shell`, `execute`
- Privilege/credentials: `getsystem`, `hashdump`
- User interface: `screenshot`, `screenshare`, `webcam_*`
- Sensitive monitoring: `keyscan_*`, `record_mic`

---

# Praktinis workflow po Meterpreter session

Kai gauni Meterpreter session, pirmos logiškos komandos dažnai būtų:

1. `help`
2. `sysinfo`
3. `getuid`
4. `getpid`
5. `ps`
6. `pwd`
7. `ls`
8. `ifconfig`
9. `netstat`

Kodėl?

- `help` parodo, ką ši sesija moka
- `sysinfo` parodo target sistemos info
- `getuid` parodo tavo teises
- `getpid` parodo, kokiame procese sėdi Meterpreter
- `ps` leidžia matyti procesus
- `pwd` ir `ls` padeda orientuotis failų sistemoje
- `ifconfig` ir `netstat` padeda suprasti tinklo situaciją

---

# Mini testas

1. Ką daro `help` Meterpreter sesijoje?
2. Kodėl skirtingos Meterpreter versijos gali turėti skirtingas komandas?
3. Kuo skiriasi `background` nuo `exit`?
4. Ką parodo `getuid`?
5. Ką parodo `getpid`?
6. Kuo skiriasi `upload` ir `download`?
7. Ką parodo `ifconfig`?
8. Ką parodo `netstat`?
9. Kodėl ne visos help meniu rodomos komandos būtinai veiks?
10. Kodėl `clearev`, `keyscan_*` ir `hashdump` yra jautrios komandos?

---

# Mano trumpas verdiktas

Šitoje dalyje svarbiausia ne iškalti visas komandas mintinai, o suprasti kategorijas.

Meterpreter komandos padeda:
- valdyti sesiją
- judėti per failus
- rinkti informaciją apie sistemą
- matyti tinklą
- valdyti procesus
- pereiti į normalų shell
- naudoti papildomas post-exploitation funkcijas

Svarbiausia taisyklė:

> Nauja Meterpreter sesija prasideda nuo `help`, `sysinfo`, `getuid`, `getpid` ir `ps`.

Taip pirmiausia supranti, kur esi, kas esi, kokias teises turi ir kokiame procese sėdi tavo Meterpreter.

 ______________________________________________________________________

# METASPLOIT: POST-EXPLOITATION WITH METERPRETER

## Kas yra post-exploitation?

**Post-exploitation** yra etapas po sėkmingo exploit'o, kai jau turi prieigą prie target sistemos.

Šito etapo tikslas nėra vien „įlindau ir valio“.

Tikslas yra suprasti:
- kokioje sistemoje esi
- kokio vartotojo teisėmis veiki
- kokie procesai veikia
- ar sesija stabili
- ar galima kelti privileges
- ar galima rasti jautrios informacijos
- ar galima judėti toliau tinkle
- kokius įrodymus / flags / konfigūracijas galima rasti

Paprasčiau:

> Exploitation = patekimas į sistemą.  
> Post-exploitation = ką darai jau patekęs į sistemą.

Meterpreter turi daug komandų, kurios palengvina post-exploitation procesą.

---

## Help komanda

Meterpreter sesijoje prompt atrodo taip:

`meterpreter >`

Komanda:

`help`

parodo visų galimų Meterpreter komandų sąrašą.

Tą patį dažnai galima padaryti su:

`?`

Svarbu:

> Skirtingos Meterpreter versijos gali turėti skirtingas komandas.

Pvz. Windows Meterpreter gali turėti vienas komandas, Linux Meterpreter kitas, Android dar kitas.

Todėl gavus Meterpreter sesiją visada verta pradėti nuo:

`help`

---

## Pagrindinė logika po Meterpreter session

Kai gauni Meterpreter sesiją, pirmi klausimai turėtų būti:

1. Kokia sistema?
2. Kokio vartotojo teisėmis veikiu?
3. Kokiame procese veikia Meterpreter?
4. Kokie procesai veikia target sistemoje?
5. Ar sesija stabili?
6. Ar yra naudingi failai?
7. Ar galiu atsidaryti normalų shell?

Dažnos pradinės komandos:

- `help`
- `sysinfo`
- `getuid`
- `getpid`
- `ps`
- `pwd`
- `ls`
- `search`
- `shell`

---

# GETUID

## Ką daro `getuid`?

Komanda:

`getuid`

parodo, kokio vartotojo teisėmis šiuo metu veikia Meterpreter.

Pvz.:

`Server username: NT AUTHORITY\SYSTEM`

Tai reiškia, kad Meterpreter veikia su labai aukštomis Windows teisėmis.

---

## Kodėl `getuid` svarbu?

`getuid` atsako į klausimą:

> Kas aš esu target sistemoje?

Tai labai svarbu, nes nuo vartotojo teisių priklauso, ką gali daryti.

Pvz. gali būti:
- paprastas vartotojas
- admin lygio vartotojas
- `NT AUTHORITY\SYSTEM`
- web serverio vartotojas
- service account

Jeigu esi paprastas user, tavo galimybės ribotos.

Jeigu esi `NT AUTHORITY\SYSTEM`, turi labai aukštą prieigos lygį Windows sistemoje.

---

## Trumpai

`getuid` = parodo vartotoją, kurio teisėmis veikia Meterpreter.

Svarbu:

> `getuid` padeda suprasti tavo privilege level.

---

# PS

## Ką daro `ps`?

Komanda:

`ps`

parodo target sistemoje veikiančių procesų sąrašą.

Ji rodo informaciją kaip:
- PID
- PPID
- proceso pavadinimą
- architektūrą
- session
- user
- path

---

## Kas yra PID?

**PID** = Process ID.

Tai unikalus proceso numeris operacinėje sistemoje.

Pvz. jeigu procesas turi PID `1304`, sistema tą procesą identifikuoja pagal šitą numerį.

Svarbu:

> PID nėra IP adresas. PID yra proceso numeris sistemos viduje.

---

## Kas yra PPID?

**PPID** = Parent Process ID.

Tai proceso, kuris paleido dabartinį procesą, PID.

Paprasčiau:

> PID = pats procesas.  
> PPID = proceso „tėvas“.

Pvz. jeigu `cmd.exe` turi PPID `1304`, reiškia `cmd.exe` buvo paleistas iš proceso, kurio PID yra `1304`.

---

## Kodėl `ps` svarbu?

`ps` naudinga, nes gali:
- pamatyti visus veikiančius procesus
- rasti, kuriame procese sėdi Meterpreter
- rasti stabilų procesą migracijai
- pamatyti, kokio vartotojo teisėmis veikia procesai
- suprasti target sistemos veiklą

Svarbu:

> `ps` duoda PID informaciją, kurios reikia `migrate` komandai.

---

# MIGRATE

## Kas yra `migrate`?

Komanda:

`migrate`

leidžia perkelti Meterpreter sesiją iš vieno proceso į kitą.

Pvz.:

`migrate 716`

reiškia:

> Perkelk Meterpreter sesiją į procesą, kurio PID yra 716.

---

## Kodėl migruoti į kitą procesą?

Migracija gali būti naudinga dėl kelių priežasčių.

### 1. Stabilumas

Jeigu Meterpreter veikia nestabiliame procese, tas procesas gali užsidaryti ir tu prarasi sesiją.

Migruojant į stabilesnį procesą, sesija gali tapti stabilesnė.

Pvz. stabilūs Windows procesai dažnai gali būti:
- `explorer.exe`
- `services.exe`
- kai kurie `svchost.exe` procesai

Svarbu:

> Ne visi procesai yra saugūs migracijai. Blogas pasirinkimas gali užmušti sesiją.

---

### 2. Sąveika su vartotojo procesu

Jeigu target sistemoje veikia vartotojo programa, pvz.:
- `notepad.exe`
- `word.exe`
- browser procesas
- kitas interactive user procesas

Meterpreter migracija į tokį procesą gali leisti geriau sąveikauti su tuo procesu.

THM pavyzdyje minima, kad migruojant į tokį procesą gali būti galima naudoti keystroke capture funkcijas, jeigu Meterpreter versija tai palaiko.

---

## Keyscan komandos

Kai kurios Meterpreter versijos turi šitas komandas:

- `keyscan_start`
- `keyscan_dump`
- `keyscan_stop`

Jos leidžia Meterpreter veikti kaip keylogger tipo mechanizmui.

Komandos:
- `keyscan_start` pradeda fiksuoti klavišų paspaudimus
- `keyscan_dump` parodo surinktus paspaudimus
- `keyscan_stop` sustabdo fiksavimą

Svarbu:

> Keylogging yra labai jautrus veiksmas. Realiame penteste tam reikia aiškaus leidimo scope. Be leidimo tai būtų nelegalu.

---

## Kaip migruoti?

Pirmiausia su `ps` susirandi norimą procesą ir jo PID.

Tada naudoji:

`migrate <PID>`

Pvz.:

`migrate 716`

Pavyzdinė logika:

1. Paleidi `ps`
2. Randi norimą procesą
3. Pasiimi jo PID
4. Paleidi `migrate PID`
5. Patikrini su `getpid`, ar Meterpreter jau sėdi naujame procese

---

## Migracijos rizika

Migruojant reikia būti atsargiam.

Svarbus THM punktas:

> Gali prarasti privileges, jeigu migruosi iš aukštesnių teisių proceso į žemesnių teisių procesą.

Pvz. jeigu Meterpreter veikia kaip `NT AUTHORITY\SYSTEM`, bet migruoji į procesą, paleistą paprasto vartotojo arba web serverio account'o, gali prarasti aukštesnes teises.

Ir blogiausia:

> Gali nepavykti jų susigrąžinti.

Todėl prieš migraciją visada verta patikrinti:
- iš kokio vartotojo proceso migruoji
- į kokio vartotojo procesą migruoji
- ar architektūra tinka
- ar procesas stabilus
- ar tikrai reikia migruoti

---

## Migrate trumpai

`migrate` = perkelia Meterpreter sesiją į kitą procesą pagal PID.

Naudojama dėl:
- sesijos stabilumo
- sąveikos su kitu procesu
- galimybės naudoti tam tikras funkcijas

Rizika:
- gali prarasti privileges
- gali nulaužti sesiją
- gali pasirinkti netinkamą procesą

---

# HASHDUMP

## Ką daro `hashdump`?

Komanda:

`hashdump`

parodo Windows SAM database turinį, tiksliau — lokalios sistemos vartotojų password hash'us.

**SAM** = Security Account Manager.

SAM database Windows sistemoje saugo lokalių vartotojų paskyrų informaciją, įskaitant password hash'us.

---

## Kas yra NTLM?

Windows password hash'ai dažnai saugomi NTLM formatu.

**NTLM** = New Technology LAN Manager.

Svarbu:

> Hash'as nėra slaptažodis plaintext formatu.

Pvz. vietoj tikro slaptažodžio matai ilgą hash reikšmę.

---

## Ar hash galima iššifruoti?

Tiksliau sakant:

> Hash'ai nėra iššifruojami kaip užšifruotas tekstas.

Hash funkcija yra vienkryptė.

Tai reiškia, kad iš hash'o matematiškai tiesiogiai neatgauni slaptažodžio.

Bet galima bandyti atrasti plaintext slaptažodį per:
- wordlist attack
- brute force
- rainbow tables
- online hash databases

Todėl geriau sakyti ne „iššifruoti hash'ą“, o:

> bandyti nulaužti / atspėti slaptažodį, kuris sugeneravo tą hash'ą.

---

## Kam dar naudingi hash'ai?

Hash'ai gali būti naudojami ne tik password cracking.

Jie gali būti naudojami ir **Pass-the-Hash** atakose.

**Pass-the-Hash** esmė:

> Kai kuriose situacijose gali autentifikuotis naudodamas hash'ą, net nežinodamas plaintext slaptažodžio.

Tai ypač svarbu Windows/Active Directory aplinkose.

---

## Hashdump trumpai

`hashdump` = ištraukia lokalių Windows vartotojų NTLM hash'us iš SAM database.

Svarbu:
- reikia pakankamų privilegijų
- hash'ai nėra plaintext slaptažodžiai
- hash'ai nėra tiesiog „iššifruojami“
- juos galima bandyti crackinti
- juos galima naudoti Pass-the-Hash atakoms

---

## Etinis / scope punktas

`hashdump` yra credential access veiksmas.

Realiame penteste tai leidžiama tik tada, jeigu:
- tai aiškiai įtraukta į scope
- klientas davė leidimą
- žinai, kaip saugiai tvarkyti credentials
- laikaisi duomenų apsaugos taisyklių

THM lab kontekste tai mokymosi dalis.

---

# SEARCH

## Ką daro `search`?

Komanda:

`search`

leidžia ieškoti failų target sistemoje.

Pvz.:

`search -f flag2.txt`

reiškia:

> Ieškok failo pavadinimu `flag2.txt`.

---

## Kodėl `search` naudinga?

CTF kontekste `search` dažnai naudojama ieškoti:
- flag failų
- proof failų
- user.txt
- root.txt
- specifinių užduoties failų

Realiame penteste `search` gali padėti rasti:
- konfigūracijos failus
- password failus
- backup failus
- user-generated dokumentus
- duomenų bazės config'us
- API keys
- SSH keys
- `.env` failus
- scriptus su credentials

---

## Search trumpai

`search` = ieško failų target sistemoje.

Pvz.:

`search -f flag2.txt`

Svarbu:

> CTF'e ieškai flagų. Realiame penteste ieškai jautrios konfigūracijos, credentials ar kitų įrodymų pagal scope.

---

# SHELL

## Ką daro `shell`?

Komanda:

`shell`

atidaro įprastą target sistemos command-line shell.

Windows atveju dažniausiai gausi:

`C:\Windows\system32>`

Linux atveju gali gauti:

`$`

arba:

`/bin/sh`

---

## Meterpreter shell vs system shell

Svarbu suprasti skirtumą:

`meterpreter >`

reiškia, kad esi Meterpreter aplinkoje ir naudoji Meterpreter komandas.

Pvz.:
- `getuid`
- `getpid`
- `ps`
- `migrate`
- `hashdump`
- `search`

`C:\Windows\system32>`

reiškia, kad esi Windows command shell ir naudoji Windows komandas.

Pvz.:
- `whoami`
- `dir`
- `ipconfig`
- `net user`
- `tasklist`

Linux shell atveju naudotum Linux komandas.

---

## Kaip grįžti iš system shell į Meterpreter?

Kai esi system shell, gali naudoti:

`CTRL+Z`

Tai leidžia grįžti atgal į Meterpreter kontekstą / backgroundinti shell channel.

THM tekste mintis yra tokia:

> `shell` numeta į normalų target command shell, o `CTRL+Z` padeda grįžti į Meterpreter.

Svarbu nesupainioti:

- `shell` = iš Meterpreter į target system shell
- `CTRL+Z` = grįžimas iš system shell atgal į Meterpreter valdymą

---

# GETSYSTEM

## Ką daro `getsystem`?

Komanda:

`getsystem`

bando pakelti privileges iki `NT AUTHORITY\SYSTEM`.

`NT AUTHORITY\SYSTEM` yra vienas aukščiausių Windows local privilege lygių.

Paprasčiau:

> `getsystem` bando iš paprastesnio vartotojo tapti SYSTEM lygio vartotoju.

---

## Ar `getsystem` visada veikia?

Ne.

`getsystem` priklauso nuo:
- target sistemos konfigūracijos
- esamų teisių
- Windows versijos
- patch lygio
- saugumo nustatymų
- ar tinkamas privilege escalation metodas įmanomas

Svarbu:

> `getsystem` nėra magiškas mygtukas. Kartais veikia, kartais ne.

---

# Praktinis post-exploitation workflow su Meterpreter

Gavus Meterpreter sesiją, galima mąstyti taip:

1. `help`
   - pažiūri, kokias komandas turi ši Meterpreter versija

2. `sysinfo`
   - sužinai target OS, architektūrą ir basic info

3. `getuid`
   - sužinai, kokio vartotojo teisėmis veiki

4. `getpid`
   - sužinai, kokiame procese veikia Meterpreter

5. `ps`
   - pamatai procesus ir PID'us

6. `migrate <PID>`
   - jei reikia, migruoji į stabilesnį/tinkamesnį procesą

7. `search`
   - ieškai failų, flagų, config'ų, credentials

8. `shell`
   - atsidarai normalų target system shell, kai reikia vykdyti OS komandas

9. `hashdump`
   - jei turi leidimą ir pakankamas teises, ištrauki SAM hash'us

10. `getsystem`
   - jei reikia ir įmanoma, bandai privilege escalation iki SYSTEM

---

# Komandų santrauka

## `help`

Parodo galimas Meterpreter komandas.

Naudoti pradžioje, nes skirtingos Meterpreter versijos turi skirtingas galimybes.

---

## `getuid`

Parodo, kokio vartotojo teisėmis veikia Meterpreter.

Atsako į klausimą:

> Kas aš esu target sistemoje?

---

## `getpid`

Parodo proceso ID, kuriame veikia Meterpreter.

Atsako į klausimą:

> Kokiame procese sėdi Meterpreter?

---

## `ps`

Parodo veikiančių procesų sąrašą.

Naudinga PID informacijai ir migracijai.

---

## `migrate <PID>`

Perkelia Meterpreter sesiją į kitą procesą.

Naudinga stabilumui arba sąveikai su procesu.

Rizika:
- gali prarasti privileges
- gali prarasti sesiją
- gali pasirinkti netinkamą procesą

---

## `hashdump`

Ištraukia Windows SAM database hash'us.

Naudinga credential access fazėje, bet labai jautru.

Hash'ai:
- nėra plaintext slaptažodžiai
- nėra tiesiog iššifruojami
- gali būti crackinami
- gali būti naudojami Pass-the-Hash atakoms

---

## `search`

Ieško failų target sistemoje.

CTF:
- flags
- proof files
- user.txt
- root.txt

Pentest:
- config files
- credentials
- backup files
- keys
- `.env` failai

---

## `shell`

Atidaro normalų target system shell.

Meterpreter komandos veikia prie:

`meterpreter >`

Windows shell komandos veikia prie:

`C:\Windows\system32>`

Grįžti į Meterpreter galima su:

`CTRL+Z`

---

## `getsystem`

Bando pakelti privileges iki `NT AUTHORITY\SYSTEM`.

Ne visada veikia.

---

# Ką tavo notes pagavo gerai

Tavo notes teisingai pagavo:
- Meterpreter turi post-exploitation komandas
- `help` rodo galimas komandas
- `getuid` rodo userį / privilege level
- `ps` rodo procesus ir PID
- `migrate` naudoja PID
- migracija gali padėti stabilumui arba sąveikai su procesu
- migracija gali prarasti privileges
- `hashdump` susijęs su SAM database
- SAM saugo Windows user password hash'us
- hash'ai gali būti crackinami / naudojami tolesnėms atakoms
- `search` padeda ieškoti juicy failų
- `shell` atidaro normalų system shell

---

# Ką reikėjo pataisyti / patikslinti

Svarbūs pataisymai:

- Rašosi **post-exploitation**, ne `post explotation`.
- Rašosi **Meterpreter**, ne `meterpeter`.
- Rašosi **hashdump**, ne `hasdump`.
- `getuid` rodo userį, kurio teisėmis veikia Meterpreter, ir padeda suprasti privilege level.
- `ps` rodo procesus; PID iš `ps` naudojamas migracijai.
- `migrate` nėra „kitas procesas“. `migrate` yra komanda, kuri perkelia Meterpreter į kitą procesą.
- Migracija gali padėti naudoti keyscan funkcijas, bet pati `migrate` nėra keyloggeris.
- Hash'ai nėra „iššifruojami“. Jie crackinami / lyginami su žinomomis reikšmėmis.
- `shell` atidaro normalų target OS shell.
- `CTRL+Z` padeda grįžti iš system shell į Meterpreter valdymą.
- `getsystem` yra privilege escalation bandymas iki SYSTEM, ne shell komanda.

---

# Mini testas

1. Ką daro `getuid`?
2. Ką parodo `ps`?
3. Kodėl PID svarbus `migrate` komandai?
4. Kuo `migrate` gali būti naudinga?
5. Kokia pagrindinė migracijos rizika dėl privileges?
6. Ar `migrate` pati yra keyloggeris?
7. Ką daro `hashdump`?
8. Kodėl neteisinga sakyti „iššifruoti hash'ą“?
9. Kam gali būti naudojami NTLM hash'ai be password cracking?
10. Ką daro `search -f flag2.txt`?
11. Kuo skiriasi `meterpreter >` nuo `C:\Windows\system32>`?
12. Kam naudojamas `CTRL+Z` po `shell` komandos?

---

# Mano trumpas verdiktas

Šita dalis yra apie tai, ką darai po to, kai Meterpreter jau veikia target sistemoje.

Svarbiausia schema:

> `getuid` pasako kas esi.  
> `getpid` pasako kur sėdi.  
> `ps` parodo procesų žemėlapį.  
> `migrate` leidžia persėsti į kitą procesą.  
> `search` padeda rasti įdomius failus.  
> `hashdump` leidžia ištraukti Windows hash'us, jei turi pakankamas teises.  
> `shell` leidžia pereiti į normalų target OS command shell.

Čia jau realus post-exploitation pagrindas. Ne magija, o sistemingas klausimų rinkinys:

> Kur aš esu?  
> Kas aš esu?  
> Ką galiu matyti?  
> Ką galiu pasiekti?  
> Ar mano sesija stabili?  
> Kokią informaciją galiu surinkti pagal scope?
_____________________________________________________________________



