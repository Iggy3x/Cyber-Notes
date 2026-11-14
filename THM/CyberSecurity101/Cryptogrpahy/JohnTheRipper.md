### John The Ripper ###

# Kodel geri hash atsparus nulauzimui? #
Kriptografine hash funkcija yra *vienakrypte funkcija* (one-way function)
Pagrindines savybes, kurios daro hash stipru:
* Lengva apskaiciuoti hash (ivestis) --> greita aporeacija
* labai sunku rasti orginalia ivesti turint tik hash --> *preimage resistance*
* Sunku rasti kita ivesti, kuri duoda ta pati hash --> *second preimage resistance* 
* Sunku rasti dvi skirtingas ivestis, lurios duoda ta pati hash --> *collision resistance*

Butent *preimage ataka* yra tai ko mes siekiame password cracking.
Kol kas nera zinomo algoritmo, kuris tai padarytu per polimonini laika - todel geri hashai lieka stiprus

# P vs NP (kodel hash crakinamas dazniausiai neimanomas "protingu budu" # 
*P* (Polynomial time)
Problemos, kurias galima ispresti per laika, kuris auga polynomialiai (pvz: n^2, n^3)
Pvz: rusiuoti 1milijona elementu - tunka sekundes-minutes.

*NP* (non-deteministic Polynomial time)
Problmos, kurias galima patikrinti labai greitai,jei jau turi sprendima
bet rasti sprendima dazniausiai labai sunku. (kolas nera zinomo greito algoritmo)
Pvz:SAT problema, kelioniu prekeivio problema

Hashinimas (skaiciuoti hash is teksto) --> priklauso P klasei -> labai greita
Atvirkstinis veiksmas ( rasti teksta pahal hash) - manoma, kad priklauso NP ar net sunkesnems klasems ( kai kuriems hash - dar blogiau nei NP)

Kol P ≠ NP ( o dauguma mokslininku mano, kad taip ir liks) nera greito budo atkepti stipru hash klasikiniame PC
Todel geri hash algoritmai lieka stiprus

# Kaip John The Ripper apeina sia apsauga? # 
John *nemegina spresti NP-sunkios problemos matematiskai*
Jis tiesiog *speja slaptazodzius* ir tikrina ar ju hash sutampa su taikiniu
1. Speja kandidatini slaptazodi
2. Apskaiciuoja jo hash ( naudodamas ta pati algoritma)
3. Palygina su taikiniu 
4. Jai sutampa - radom (cracked) 

Populiariausios atakos rusys Johne:
* Dictionary attack   ( zodynas pvz rockyou.txt)
* Brute-force 	(visos kombinacijos-leta)
* mask attack	(pvz. ?u?1?1?1?1d?d?D?d - Anna2025)
* Rules-based	(pakeitimai:a-@, prideti 123,kapitalizacija ir kt. 

# John The Ripper - pagrindine sintakse #
< john [options] [hash_file] >

# Daznaisiai naudojami variantai # 

1. Paprasčiausias bandymas (John bando pats atpažinti formatą)
john hash.txt
 
2. Žodyno ataka – pats populiariausias variantas
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

3. Aiškiai nurodant hash tipą (kai auto-detect stringa)
john --format=raw-md5    --wordlist=rockyou.txt md5_hashes.txt
john --format=bcrypt     --wordlist=rockyou.txt bcrypt_hashes.txt
john --format=NTLM       --wordlist=rockyou.txt ntlm_hashes.txt

4. Parodyti visus palaikomus formatus
john --list=formats

5. Parodyti ką jau nulaužėme
john --show hash.txt

# Populiariausi hash formatai # 

|Hash tipas	|--format=	|Prasideda			|Greitis Johne		|Pastaba
|MD5		|raw-md5	|teisiog 32 hex simboliai	|labai greitas		|Silpnas lengvai crakinamas
|SHA-1		|raw-sha1	|40 hex simboliu		|greitas		|Silpnas
|NTLM		|NTLM		|32 hex simboliai		|greitas		|windows psw
|md5crypt	|md5crypt	|$1$...				|vidutiniskai letas	|senesni linux
|sha512crypt	|sha512crypt	|$6$...				|letas			|modernus linux /etc/shadow
|bcrypt		|bcrypt		|$2a&,$2b$,$2y$			|labai letas		|stiprus, turi coast factor

# Salt # 
Beveik visi siuoliakiniai hash metodai naudoja unikalia druska kiekvienam vartotojui
tas pats psw "password123" duoda skirtingus hash skirtingiems zmonems
John automatiskai naudoja salt`a jei formatas teisingai atpazintas.

# Cost/work factor (bcrypt, Argon2, PBKDF2, scrypt) #

Sie algoritmai tycia daromi leti:
* bcrypt coast=10 -> 0.06-0.1s per bandyma ( vienas branduolys)
* cost=12 -> ~0.2-0.5 s
* cost=14 -> ~1-2s

Net jei slaptazodis silpnas, bet cost didelis, craking uztruka labai ilgai.


# Tipinis Linux scenarijus TryHackMe#
* <unshadow passwd.txt shadow.txt > combined_hashes.txt >
* <john --wordlist=/usr/share/wordlists/rockyou.txt combined_hashes.txt>
* <john --show combined_hashes.txt>



## Kodėl ir kaip naudojamas unshadow?

* /etc/passwd → čia yra vartotojų vardai, UID, GID, home katalogas, shell ir kt. (vieša informacija)
* /etc/shadow → čia yra hash'inti slaptažodžiai + salt + kiti parametrai (labai jautrūs duomenys, prieigos teisės root)

John the Ripper negali dirbti su atskirais passwd ir shadow failais – jam reikia vieno failo su "username:hash" formatu.

Todėl darome:
<unshadow passwd.txt shadow.txt > combined_hashes.txt>
Ši komanda:
- sujungia abu failus
- sukuria eilutes tokio formato:   username:$6$... (arba kitas hash formatas)
- išsaugo rezultatą faile combined_hashes.txt

Po to jau galima normaliai paleisti John:
<john --wordlist=/usr/share/wordlists/rockyou.txt combined_hashes.txt>
Ir vėliau pažiūrėti rezultatus:
<john --show combined_hashes.txt>
______________________________________________________________________________________________________

# Cracking Windows Passwords Hashes # 

Windows password hashes - tai yra hashed password versija, kuri yra saugoma Windows sistemoje vietoj plaintext passwords.

Jeigu puolejas gauna tokius hashus, jis kartais gali bandyti juos crackinti naudodamas:
* Dictionary attack 
* rule-based attack
* brute-force

Tam dazniausiai reikia *privileged access* nes tokie hashai paprastam useriui lengvai neprieinami.

# NT HASH /NTLM  #

*NT HASH* yra pagrindine Windows password hash reiksme moderniose Windows sistemose

Daznai vadinama NLTM hash, bet techniskai tai nera visiskai tas pats. 
`svarbu suprast`:
* NT hash = password hash reiksme
* NTLM = WIndows authentication protocol / mechanism

Praktikoje sie terminai daznai maisomi, todel labai daznai isgirsi "ntlm hahs" nors tiksliau butu "NT hash" 

# LM HASH ir NT HAHS #

Senesnese windows versijose buvo naudojami tiek LM hash tiek NT hash

*LM hash*
* Senas legacy hash
* silpnas
* daug lengviau crackinamas
* moderniose sistemose jo saugojimas paprastai neberekomenduojamas ir daznai buna isjungtas.

*NT HASH*
* naujesnis windows password hash
* pagrindinis moderniose sistemose
* stipresnis uz LM, bet vistiek pazeidziamas, jei psw silpnas

# SAM-Security Account manager # 

SAM saugo local account informacija windows sistemoje
Tai apima:
* Username
* account info
* psw-related data/hashes
Trumpai:
* SAM = local accounts
* jei puolejas gali isdumpinti SAM, jis gali gauti local account hashus

# NTDS.dit #
Kai kalba apie *Domain enviroment*, tada svarbi duomenu baze yra:
*NTDS.dit*

Ji saugo *active directory domain account* informacija domeno kontroleryje

Trumpai:
* NTDS.dit=domain account
* randama Domain Controller
* Tai svarbus saltinis domain credential duomenims

# Kaip galima gauti siuos hashus #
Windows psw hashai gali buti gauti is:
* SAM database - local acc
* NTDS.dit database - domain acc
Tam dazniausiai reikia aukstensiu privilegiju ir naudojami credential-dumping metoai ar irankiai. Tokie hashai yra vertingi credential artefaktai, todel MS turi apsaugas, skirtas ju vagystei apsunkinti.

# Ar visada reikia crakinti hash ?# 

Ne. 
Jeigu turi galiojanti NT hash, tau ne visada reikia atkurti tikra psw.
Kartais gali atlikti:
*PASS-THE-HASH*
Tai reiskia, kad naudoji pati hash autentikacijai, nezinodamas plaintext psw.
Taigi
* hash cracking = bandai atkurti tikra psw.
* pass-the-hash = naudoji hash tiesiogiai autentikacijai

# Kada hash cracking vis dar naudingas # 
Hash cracking vis dar naudingas kai:
* psw silpnas
* tau reikia plaintext psw
* psw gali buti naudojamas kitose sistemose
* nori patikrinti psw policy stipruma
Jeigu psw silpnas, crackinimas gali greit duoti rezultata

# John the ripper kontekstas # 

Praktinė detalė:
Nors žmonės dažnai sako NTLM hash, John the Ripper dažnai naudoja formatą:
<john --format=NT hash.txt>
o ne:
<john --format=NTLM hash.txt>
Taigi:
* pasaulyje dažnai sakoma NTLM hash
* John formate dažnai naudojamas NT 

# Santrauka # 

Svarbiausi dalykai:
* NT hash = pagrindinė Windows password hash reikšmė
* NTLM = authentication protocol / mechanism
* LM hash = senas ir silpnas legacy hash
* SAM = local account hashai
* NTDS.dit = domain account hashai
* Pass-the-Hash leidžia naudoti hashą tiesiogiai
* Hash cracking leidžia bandyti atkurti tikrą passwordą

# Atmintine # 

* NT hash = Windows password hash reikšmė
* NTLM = Windows authentication protocol
* LM hash = senas, silpnas legacy hash
* SAM = local accounts
* NTDS.dit = domain accounts
* Pass-the-Hash = naudojamas pats hashas
* Hash cracking = bandoma atkurti plaintext password
* John format for NT/“NTLM” hashes = --format=NT
________________________________________________________________________________________

# Craking Hashes from /etc/shadow # 

/etc/shadow failas Linux sistemoje saugo *user account password ir hashus ir password aging information*

Tai gali apimti:

* password hash
* last password change
* min/max psw age
* expiraton/inactivity laukus

Kiekvienas useris paprastai turi viena eilute, o `shadow(5)` nurodo, kad kiekviena eilute turi *9* `:` atsirus laukus. Sis failas neturi buti prienamas paprastiems useriams. 

# etc/passwd ir etc/shadow #

Moderniose linux sistemose tikras password hash dazniausiai laikomas ne `etc/passwd` o `etc/shadow`.

`etc/shadow` faile password field daznai buna:
*X*

Tai reiskia, kad encrypted password/hash yra laikomas `etc/shadow` , `passwd(5)`
tai apraso tiesiogiai:
`/etc/passwd = user account info`
`/etc/shadow = password hashes + aging info`

# Unshadowing #

Kai sistema naudoja shadow passwords, John the Ripper daznai reikia *suungto info* is:
* /etc/passwd
* /etc/shadow

Tam naudojamas *unshadow* tool, kuris priklauso John toolset`ui. Openwall FAQir examples nurodo, kad jei Unix psw failas yra shadowd, reikia paimti abu failus ir sujungti juos su *unshadow*
Basic syntax:
<unshadow [path_to_passwd] [path_to_shadow]>

#Example Usage #

<unshadow local_passwd local_shadow > Unshadowed.txt >
Si komanda:
* paima *passwd* ir *shadow* duomenis
* sujungia juos i John tinkama formata
* isaugo rezultata i *unshadowed.txt*

# Pastaba apie failus #

Naudojant *ushadow*, galima naudoti:
* visa */etc/passwd* ir */etc/shadow*
* arba tik konkretaus userio atitinkamas eilutes

Svarbiausia kad *passwd* entry ir *shadow* entry priklausytu tam paciam useriui. Net *pwck(8)* tikrina, ar kiekvienas *passwd* irasas turi atitinkama *shadow* irasa ir atvirksciai.

#PVZ#

FILE 1 — local_passwd
<root:x:0:0::/root:/bin/bash>
FILE 2 — local_shadow
<root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3KEVsZ9rOVLB/ldTeEL/OIhJZ4GMFMGA0:18576::::::>

Cia:
* pirmas failas duoda account info
* antras failas duoda hash ir aging laukus
* *unshadow* juos sujungia i John suprantama formata.

# Craking #

Jeigu hash prasideda:
<$6$>
Tai reiskia *sha512crypt* tipo unix *crypt(3)*hash, todel gali naudoti:
<john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt>
Taigi logika tokia:
* jei John pats atpazysta formata *--format=* gali nereikti
* jei nori tikslumo arba autodetect stringa, nurodai *--format=sha512crypt* ranka

# Santrauka # 
Kai linux sistema naudoja shadow password mdeli:
1. account informacija yra /etc/passwd
2. password hashai laikomi /etc/shadow
3. unshadow sujungia abu failus
4. gautą rezultatą paduodi John
5. jei reikia, nurodai konkretų formatą, pvz. sha512crypt

# ATMINTINE# 

* /etc/passwd = user account info
* /etc/shadow = password hashes + password aging info
* shadowed systemose tikras hash dažniausiai yra /etc/shadow
* unshadow passwd shadow > unshadowed.txt
* john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
* jei hash yra $6$:
* john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
_____________________________________________________________________________________________________________________________________________________________________________________

# Single Crack Mode # 
Tai yra vienas is John crakinimo metodu.

Tai yra rezimas, nuo krio daznai verta pradeti, nes John cia naudoja *jau turima info apie acc* vietoj paprasto isorinio wordlisto. Openwall teisiai raso, kad single crack mode naudoja *login names,GECOS/ Full name field ir home directory names* kaip kandidatu baze.

Trumpai:
* wordlist mode = john ima zodziu is wordlist
* single mode = john ima zinoma acc info ir is jos kura spejimus.

# World Mangling # 

Singel crack mode  remiasi tuo ka John vadina "word mangling"
Word Mangling reiskia, kad John paima bazini zodi, pvz username ir pradeda ji keisti pagal tam tikras taisykles, kad sugeneruotu tiketinus psw variantus. 
 PVZ:
Jeigu username yra:
`Markus`
John gali isbandyti tokius variantus:
* Markus1
* Markus2
* Markus3
* Markus!
* Markus$
* Markus
* MArkus
* MARkus
* MARKus

Esme tokia:
John nemeto atsistiktiniu zodziu.
Jis bando kurti *tiketinus silpno psw variantus*, paremtus tuo, ka jau zino apie useri. 

# Mangling Rules # 

Kad tai veiktu, John naudoja vadinamas *manglin rules* 
Sitos tasiykles nusako kaip bazini zodis gali  buti keiciamas. 

PVZ:
* Prideti skaiciu gale
* Prideti specialu simboli
* keisti diziasias/mazasias raides
* daryti mazus zodzio pakeitimus
Tau veikia todel, kad zmones labai daznai naudoja tingius psw, paremtus:
* Savo vardu
* Username
* sistemos pavadinimu
* paprastais simbolu ar skaiciu priedais

Trumpai:
*Single crack mode exploitina silpnus, nuspejamus psw iprocois*

# GECOS #

John single crack mode gali naudoti ne tik username, bet ir papildoma acc info is *GECOS.field*

Kas yra GECOS?
Unix/Linux sistemose `/etc/passwd` irasai yra atskirti dvitaskiais `:` `passwd(5)` apraso si faila kaip user login accounta faila , o penktas laukas tradiciskai naudojamas kaip aprasomas fieldas. `chfn(1)` aiskiai rodo kad siame lauke gali buti saugoma vadinamoki finger informacija, pvz:
* real name
* Office / room number
* work phone
* Home phone

Pvz:
<user:x:1000:1000:Full Name,Room,WorkPhone,HomePhone:/home/user:/bin/bash>
Cia penktas field yra GECOS / finger info laukas. John gali naudoti tokias reiksmes kaip:
* Full name
* Home directory name
Kad sugeneruotu papildomus kandidatus single mode metu. Openwall tai mini tiesiogiai.

# Kodel Single mode gali buti greitas # 
Single crack mode daznai buna greitesnis uz wordlist mode, nes acc info paprastai naudojama daugiau pries to acc hasha, o ne pries visus pasaulio hashus is eiles. Del to John gali taikyti didesni manglin rules kieki ir vistiek islikti efektyvus. Openwall butent taip ir apraso si rezima. 

Dar viena idomi detale: openwall mini kad sekmingai atspeti slaptazodziai po to dal gali buti patiktinami ir pries kitus uzkrautus hash. Jiei keli useriai turi ta pati psw.

# Using SINGLE CRACK MODE # 

Single crack mode naudojamas taip:
<john --single --format=[format] [path_to_file]>
Kur: 
* <--single> - jungia single crack mode
* <--format=[format]> - nurodo hash formata
* <[path_to_file]> - failas su John kraunamais hash irasais
PVZ:
<john --single --format=raw-sha256 hashes.txt>


# SVARBI PASTABA APIE FAILO FORMATA # 
Kad single mode normaliai veiktu, John turi gauti ne vien tik hasha, bet ir bent kazkiek *acc konteksto*, nes kitaip jis netures is ko generuoti kandidatu. Kadangi Openwall sako, jog single mode naudoja login names, GECOS ir home directory names, praktikoje mokymosi uzduotyse failas daznai rusiuojamas taip, kad hash butu susietsa su userio vardu.

Todel vietoj:
*1efee03cdcb96d90ad48ccc7b8666033*
Single mode uzduotys daznai daroma taip:
*mike:1efee03cdcb96d90ad48ccc7b8666033*
Tai leidzia John naudoti *mike* kaip baze kandidatams generuoti
Svarbiausia praktine mintis:
* ne komandoje 
* faile
Tai reiskia kad *joker:* ar *mike:* eina i failo turini ne i command line kaip failo vardas.

# PRAKTINE LOGIKA # 
Jeigu uzduoyje parasyta, kad hash priklauso useriui Joker,tada failai turi buti tokio tipo irasas:
<Joker:<hash>
Po to komanda atrodo taip:
<john --single --format=raw-md5 hash07.txt>

Logika:
1. Failo viduje surasai `username:hash`
2. Nurodai John ta faila
3. John naudoja username kaip kandidatu baze single mode metu. 

# Santrauka # 
Single Crack Mode yra John režimas, kuris bando crackinti hashus naudodamas account informaciją, o ne paprastą wordlist.

Jis gali naudoti:

* login names
* GECOS / Full Name fields
* home directory names
* mangling rules

Workflow:

* paruoši failą taip, kad John turėtų userio vardą kartu su hashu
* nurodai teisingą formatą
* paleidi John su --single
* John generuoja tikėtinus password variantus iš account infor

# Atmintine # 

Single Crack Mode = hash cracking using account information

John gali naudoti:
- login name
- GECOS / Full Name
- home directory name

word mangling = bazinio žodžio keitimas pagal taisykles

single mode komanda:
* john --single --format=[format] [file]

* praktikoje failas dažnai ruošiamas su:
<username:hash>
______________________________________________________________________________



# Custom Rules # 
Custom rules leidzia John the Ripper modifikuoti zodzius is *wordlsit*, kad is vieno bazinio zodzio sugeneruotu daugiau tiketinu password variantu. John wordlsit rules yea eilutes is praprastu komandu kurios keicia kiekviena wordlsit zodi pagal aprasytas taisykles. 

Trumpai:
* Turi baziny zodi is wordlist.
* John ji *mangling`a*
* Is vieno zodzio padaro daug kandidatu.

# Kodel Custom Rules yra naudingos? #
Daugelis zmoniu kurias passwordus pagal nuspejamus modelius.
Pvz:
`polopassword`
Paverciamas i
`Polopassword1!`
Tai atrodo "sudetingiau",bet modelis islieka nuspejamas
John wordlist mode dokumentacija aiskiai sako, kad taisykles naudojamos zodziams modify or mangle, kad butu gaminami kiti tiketini password variantai.

# Kur rasomos CUstom Rules? #

Custom rules rasomos John kongifuracijos faile, paprastai `john.conf` arba `john.ini`, priklausomai nuo platformos ir diegimo budo. Openwall dokumentacija aiskina, kad John naudoja konfiguracijos faila savo "home directory", o tiksli vieta priklauso nuo idiegimo budo. 
Praktiškai gali sutikti tokius variantus:

`/opt/john/john.conf`
`/etc/john/john.conf`
John run / source direktorijoje esantį john.conf

Tai reiškia: neapsiženyk su vienu path’u. Pasitikrink kur guli tavo configas.

# Kaip pavadinama taisykle # 
Custom wordlist rule sekcija rasoma taip:
<[List.Rules:RuleName]>
Pvz:
<[List.Rules:PoloPassword]>
Sitas pavadinimas yra svarbus, nes butent juo paskui kivesi taisykles komandoje su <--rules=RuleName>. Openwall options ir cinfig dikumentacija tai rodo teisiai.

# Kaip veikia taisykles # 
John taisykles yra *konadu seka vienoje eiluteje*. Kekviena komanda kazka daro su baziniu zdziu. RULES dokumentacija teisiai sako kad kiek iena taisykle susidea is vienos ar daugiau paprastu konadu, surasytu vienoje eiluteje. 
Common Custom rules:
* c → kapitalizuoja žodį
* A0"..." → prideda tekstą į pradžią
* Az"..." → prideda tekstą į galą

Trumpa logika:
* c iš polopassword gali padaryti Polopassword
* Az"1!" gale pridės 1!
* A0"!" pradžioje pridėtų !

# Svarbi pastaba apie brackets ir "Regex" mastyma # 

John rules nera paprastas regex rasymas.
Bracket`ai, klases ir preprocessor sintakse Johne turi savo logika. OPenwall RULES dokumentacija aiskiai sako, kad taisukles turi savo komandu sintakse ir preprocessor, kuris gali generuoti daug taisykliu is vienos source eilutes.
Todėl toks užrašas:
<cAz"[0-9] [!£$%@]">
THM pavyzdys greičiausiai yra blogai arba bent jau klaidinančiai parašytas, nes John rules sintaksėje [] čia veikia kaip preprocessor character lists, ne kaip „tiesiog regex-looking tekstas“. Openwall taisyklių dokumentacija duoda oficialų pavyzdį l$[0-9], kur $X reiškia „append character X“, o [0-9] preprocessor išplečia į 10 atskirų taisyklių.
Svarbiausia mintis:
* c = capitalize.
* $X = pridėti vieną simbolį gale.
* ^X = pridėti vieną simbolį pradžioje.
* AN"STR" = įterpti stringą STR į poziciją N; Az"STR" reiškia pridėti stringą gale, o A0"STR" — pradžioje.
* [0-9], [A-Z], [!@#] ir panašūs dalykai yra preprocessor ranges/lists, kurie generuoja daug taisyklių iš vienos source eilutės.

Kaip rašyti teisingai

Jei nori tik vieno konkretaus varianto:

iš polopassword padaryti tik Polopassword1!

rašai:

c$1$!

arba:

cAz"1!"

Abi logikos yra suprantamos John’ui:

c kapitalizuoja žodį.
$1$! prideda simbolius po vieną.
Az"1!" prideda konkretų stringą 1! gale.

Jei nori visų variantų, pvz.:

Polopassword1!
Polopassword2!
Polopassword7$
Polopassword9@

tada geresnė taisyklė yra:

c$[0-9]$[!£$%@]

Kodėl būtent taip:

c → kapitalizuoja pirmą raidę.
$[0-9] → pridės vieną skaitmenį, o preprocessor išplės tai į 10 taisyklių.
$[!£$%@] → po to pridės vieną simbolį iš sąrašo, ir preprocessor sugeneruos visas kombinacijas.

Tai yra daug švaresnė logika nei Az"[0-9] [!£$%@]".

# Kada naudoti Az, o kada $#

Naudok:
<Az"STR"> kai nori pridėti fiksuotą tekstą, pvz.:
<Az"123">
arba
<cAz"1!">

<$X> kai nori pridėti vieną simbolį, ypač kai jis ateina iš preprocessor range:

<$[0-9]>
<$[!@#$]>

Trumpai:
* fiksuotas stringas → Az"..."
* char-by-char variacijos → $...

Teisingi pavyzdžiai
Jei sekcijoje turi:
<[List.Rules:PoloPassword]>
<c$[0-9]$[!£$%@]>

# Kaip kviesti Custom RUles # 
Wordlist mode taisykles ijungiamos su:
<john --wordlist=wordlist.txt --rules=RuleName hash.txt>
Logika:
* --wordlist=... → duoda bazinius žodžius
* --rules=RuleName → pasako, kokias taisykles taikyti
* hash.txt → hash failas

Taip pat verta žinoti, kad be konkretaus pavadinimo galima įjungti numatytas wordlist taisykles su <--rules>, kurios skaitomos iš <[List.Rules:Wordlist]>.
 
# Jau esancios taisykles # 

John jau turi nemažai paruoštų taisyklių. Config dokumentacija aiškiai sako, kad wordlist ir single mode taisyklės laikomos atskirose [List.Rules:Wordlist] ir [List.Rules:Single] sekcijose, o CHANGES rodo, kad šitie numatytieji taisyklių rinkiniai buvo tobulinami dėl greičio ir mažesnio dublikatų kiekio.
Tai praktiškai reiškia:
* jei stringi su sintakse
* jei nežinai, kaip aprašyti patterną
* pirma pažiūrėk jau esančias taisykles john.conf faile

# SANTRAUKA

Custom rules John’e leidžia modifikuoti wordlist žodžius pagal tavo aprašytas taisykles.
Jos naudojamos wordlist mode metu ir aprašomos [List.Rules:...] sekcijose John config faile. Taisyklės kviečiamos su --rules, o jų tikslas — iš bazinių žodžių sugeneruoti daugiau tikėtinų password variantų.

# ATMINTINE # 

Custom rules = taisyklės, kurios manglina wordlist žodžius

Kur rašomos:
[List.Rules:RuleName]

Kaip kviečiamos:
--rules=RuleName

Naudojamos su:
john --wordlist=wordlist.txt --rules=RuleName hash.txt

Dažni tikslai:
- kapitalizuoti pirmą raidę
- pridėti tekstą pradžioje
- pridėti tekstą gale
- generuoti tikėtinus password patternus
________________________________________________________________________________________________________


# John su ZIP / RAR / SSH key — trumpas konceptas #
John dažnai nedirba tiesiogiai su tokiais failais kaip:
* .zip
* .rar
* id_rsa

Todėl pirma failą reikia paversti į John suprantamą formatą su specialiu toolu:
* ZIP → zip2john
* RAR → rar2john
* SSH private key → ssh2john

Po to tą outputą paduodi john, kuris bando rasti slaptažodį su wordlistu.

Bendra logika
1. Konvertuoji failą į hash formatą
[tool] [failas] > [hash_failas]
2. Crackini su John
john --wordlist=/usr/share/wordlists/rockyou.txt [hash_failas]
3. Parodai rastą slaptažodį
john --show [hash_failas]
4. Panaudoji slaptažodį pagal failo tipą
ZIP → unzip
RAR → unrar
SSH key → ssh -i
Pavyzdžiai
*ZIP*
`zip2john secure.zip > zip_hash.txt`
`john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt`
`john --show zip_hash.txt`
`unzip secure.zip`
*RAR*
`/opt/john/rar2john secure.rar > rar_hash.txt`
`john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt`
`john --show rar_hash.txt`
`unrar x secure.rar`
*SSH key*
`python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt`
`john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt`
`john --show id_rsa_hash.txt`
`chmod 600 id_rsa`
`ssh -i id_rsa user@IP`
`Esmė vienu sakiniu`

Konvertuoji failą → crackini su John → panaudoji rastą slaptažodį su tam failui skirtu įrankiu.

Mini note versija

John ne visada crackina failą tiesiogiai, todėl naudojami konvertavimo įrankiai:

zip2john
rar2john
ssh2john

Jie paverčia failą į hash formatą, kurį supranta john.

Tada:

john --wordlist=/usr/share/wordlists/rockyou.txt [hash_failas]

Kai slaptažodis randamas:

john --show [hash_failas]

Po to slaptažodis panaudojamas su normaliu įrankiu:

ZIP → unzip
RAR → unrar
SSH key → ssh -i
______________________________________________________________________________________

