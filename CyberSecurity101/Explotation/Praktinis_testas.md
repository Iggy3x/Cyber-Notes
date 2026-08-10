# METASPLOIT / ENUMERATION / LOGIN TESTING – BENDRAS KONSEKTAS

## 1. Metasploit modulis != laisva komanda

Kai naudoju Metasploit modulį, aš nebe esu paprastoje shell komandinėje eilutėje.
Aš jau esu konkretaus modulio kontekste.

Workflow:
- use <module>
- show options
- set <option> <value>
- run

Svarbu:
- Metasploit moduliai turi savo required options
- jei trūksta privalomo parametro, modulis nepasileis
- run nepakeičia normalaus options sukonfigūravimo

Pvz:
- RHOSTS = target host/IP
- PORTS = kuriuos portus tikrinti
- run = paleidžia modulį su jau nustatytais parametrais

Jei gaunu klaidą apie "validate RHOSTS", tai reiškia:
- modulis nežino, kurį hostą skanuoti

"run 8000" nereiškia "scan port 8000".
Portas paprastai turi būti nurodomas per modulio options, o ne kaip laisvas argumentas po run.


## 2. Hostas != portas != servisas

Reikia aiškiai atskirti tris dalykus:

- hostas = target mašina / IP
- portas = konkretus įėjimo taškas toje mašinoje
- servisas = kas realiai tame porte klausosi

Pvz:
- RHOSTS atsako į klausimą "kur?"
- PORTS arba RPORT atsako į klausimą "kuris portas?"
- service enumeration atsako į klausimą "kas ten realiai sukasi?"


## 3. Port scan pasako tik tiek, kad durys atidarytos

Port scan paskirtis:
- rasti atidarytus portus
- kartais parodyti service guess

Port scan nepasako pilnai:
- kokia aplikacija ten veikia
- koks web serveris
- koks title
- kokie endpointai
- kokia versija
- koks login puslapis ar panelė

Kitaip tariant:
- open port != pilnai suprastas servisas


## 4. Jei žinau, kad portas atidarytas, kitas etapas yra service enumeration

Kai jau žinau, kad portas atidarytas, turiu nuspręsti, ko man reikia toliau:

- tik patvirtinti service tipą?
- pamatyti bannerį?
- pamatyti HTTP atsakymą?
- identifikuoti aplikaciją?
- patikrinti prisijungimą?

Jei klausimas yra "kas stovi už porto?", tai jau nebe grynas port scanning.
Tai jau:
- service enumeration
- banner grabbing
- web enumeration
- tiesioginė sąveika su servisu


## 5. "http-alt" nėra pilnas atsakymas

Kai Nmap rodo:
- 8000/tcp open http-alt

tai paprastai reiškia:
- portas 8000 atidarytas
- greičiausiai ten HTTP tipo servisas

Bet tai dar nėra pilna identifikacija.
"http-alt" dažnai yra tik:
- service label
- spėjimas / fingerprint pagal portą ar atsaką

Tai dar neatsako pilnai:
- kas konkrečiai sukasi ant to porto


## 6. Norint suprasti, kas runnina ant porto, reikia kalbėtis su pačiu servisu

Jei vien portscan neužtenka, reikia tiesiogiai bendrauti su tuo portu / servisu.

Tam tinka:
- telnet
- nc
- curl
- browseris
- nmap service/version detection

Svarbi mintis:
- portscan parodo, kad durys atidarytos
- tiesioginė sąveika parodo, kas kalba už tų durų

Telnet naudinga, kai nori:
- ranka prisijungti prie porto
- pamatyti tekstinį atsakymą
- pasiųsti paprastą requestą
- gauti bannerį ar HTTP response


## 7. Reikia atskirti užduoties tipą

Cyber užduotyse labai svarbu atskirti, kokio tipo užduotis dabar vyksta.

Pvz:
- host discovery
- port discovery
- service identification
- web enumeration
- login brute force
- hash cracking

Didelė klaida:
- naudoti senos užduoties logiką naujai užduočiai

Pvz:
- iš pradžių 8000 portas buvo web service identifikavimui
- vėliau užduotis jau buvo apie SMB password
- jei palieki seną portą ar seną kontekstą, pradedi šaudyti pro šalį


## 8. Password gavimas gali reikšti du visiškai skirtingus dalykus

### A. Offline cracking
Turiu hashą.
Tada naudoju:
- John the Ripper
- Hashcat

Logika:
- turiu suhashintą reikšmę
- bandau atspėti originalų slaptažodį offline

### B. Online login testing
Neturiu hash'o, bet turiu:
- hostą
- portą
- service
- userį
- wordlistą

Tada naudoju:
- Metasploit login modulį
- kitą bruteforce / login testing įrankį

Logika:
- aš necrackinu hash'o
- aš tiesiogiai bandau slaptažodžių kandidatus prieš gyvą prisijungimo servisą


## 9. John ir Metasploit smb_login nėra tas pats

John:
- dirba su hash failais
- offline cracking
- neina į SMB portą ir nesijunginėja kaip klientas

Metasploit smb_login:
- jungiasi prie gyvo SMB serviso
- tikrina username/password kombinacijas
- daro online authentication testing

Taisyklė:
- jei turi hashą -> galvok apie John
- jei turi servisą ir userį -> galvok apie login testing


## 10. PASS_FILE != USERPASS_FILE

### PASS_FILE
- failas su slaptažodžių sąrašu
- naudojamas, kai vartotoją jau žinai
- pvz: SMBUser = penny, PASS_FILE = passwords.txt

Logika:
- tas pats useris
- daug password kandidatų

### USERPASS_FILE
- failas su user/password poromis
- naudojamas, kai turi pilnas credential poras
- pvz:
  admin password123
  guest guest
  penny secret

Logika:
- failas jau turi ir userį, ir passwordą

### USER_FILE + PASS_FILE
- atskiras userių sąrašas
- atskiras passwordų sąrašas

Svarbu:
- nereikia be reikalo vienu metu naudoti kelių credential input būdų
- tai apsunkina logiką ir gali klaidinti


## 11. Minimalus config dažnai yra geriausias config

Metasploit moduliuose geriau naudoti minimalų reikalingą config.

Jei žinau vartotoją:
- SMBUser + PASS_FILE

Jei turiu user/password porų failą:
- USERPASS_FILE

Jei turiu atskirus userių ir passwordų failus:
- USER_FILE + PASS_FILE

Kuo mažiau nereikalingų parametrų:
- tuo aiškiau suprasi, kas vyksta
- tuo lengviau debuginti
- tuo mažiau painiavos


## 12. Labai svarbu nepainioti service ir porto

Jei užduotis klausia apie SMB password, turiu galvoti:
- koks portas priklauso SMB?
- ar aš tikrai kalbu su SMB servisu?

Jei palieku RPORT iš ankstesnės web užduoties, modulis gali būti techniškai sukonfigūruotas, bet logiškai neteisingas.

Svarbiausia:
- modulis turi būti nukreiptas į teisingą servisą
- teisingas service svarbiau už bet kokį random komandų mušimą


## 13. Kaip išvalyti Metasploit option reikšmę

Jei nori nunulinti vieną option:
- unset OPTION_NAME

Pvz:
- unset USERPASS_FILE
- unset PASS_FILE
- unset SMBUser

Jei nori viską modulyje grąžinti į default:
- unset all

Arba:
- back
- use <module>

Tai naudinga, kai modulis "užsiteršia" nuo ankstesnių bandymų.


## 14. Dažniausios priežastys, kodėl kažkas neveikia

Kai kažkas neveikia, patikrink šituos 4:

1. Ar tas pats modulis vis dar tinkamas šitai užduočiai?
2. Ar nepalikau seno PORT / RHOST / USER / FILE nustatymo iš ankstesnio žingsnio?
3. Ar naudoju tinkamą input tipą (PASS_FILE vs USERPASS_FILE)?
4. Ar dirbu su teisingu servisu, o ne su senos užduoties šešėliu?


## 15. Pagrindinis mental model

Mąstyk ne:
- "kokią komandą čia dar pakalt?"

Mąstyk:
- kokio tipo informacijos man dabar trūksta?
- kokio sluoksnio užduotis dabar vyksta?
- ar man reikia rasti portą?
- ar man reikia suprasti service?
- ar man reikia gauti response?
- ar man reikia brute-force'inti loginą?
- ar man reikia crackinti hashą?

Kai pradedi taip mąstyti, nebespaudinėji komandų aklai.
Tada jau pradedi realiai suprasti, ką darai.
