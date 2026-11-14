### Cyptography ###

# Cryptgraphy Basics # 

Kas yra Cryptogrpahy?
Tai metodai ir algoritmai, naudojami apsaugoti informacija nuo neautorizuotos prieigos.

Pagrindinis tikslas-apsaugoti duomenis net jei uzpuolikas gali matyti komunikacija.

Cryptography saugo keturis pagridinius dalykus:

|Principas	|Reiksme|
|Confidentialy	|Tik leidziami zmones gali matyti duomenis
|Integrity	|Duomenys nebuvo pakeisti
|Authentication	|Zinome su kuo komunikuojame
|Authenticity	|Galime patvirtinti kas sukure ar pasirase duomenis

# Kur naudojama Cryptography? #

Mes ja naudojame, kasdien, net to nepastebedami.

Login sistemos
Kai jungiesi prie svetaines (pvz THM.com):

username-->password-->encrypted-->server

Prisijungimo duomenys yra uzsifruoti pries siunciant, todel tinklo stebetojas negali ju perskaityti
Taip niekas tinklo viduje negali ju perskaityti.

# SSH #

Kai prisijungi prie serverio:

SSH client <-> encrypted tunnel <-> server

Visa komunikacija vyksta per encrypted tunnel, todel niekas negali klausytis sesijos ar matyti komandu.


# Online banking / HTTPS #

Kai lankaisi svetaineje su *HTTPS*, narsykle patikrina serverio *certificate*. Tai leidzia isitikinti kad komunikacija vyksta su serveriu o ne su atakeriu. 
HTTPS naudoja TLS encryption, kuri apsaugo visa komunikacija tarp narsykles ir serverio. 

Tai daroma naudojant:
*TLS / HTTPS encryption*

# File integrity #

Kai parsisiunti faila, naudojamas *hash* patikrinti ar failas nebuvo pakeistas.Kai parsisiunti faila gali buti pateiktas *hash* (SHA256)


PVZ:
Original file hash
=
download file hash

Jei sutampa, failas nebuvo pakeistas, ar sugadintas

________________________________________________________________________________

### Cryptography ir saugumo standartai ###

Daug industriju privalo naudoti cryptography, nes tai reikalauja saugumo standartai ir istatymai. 

# Kreditiniu koreliu duomenys #

Reguliuoja standartas:

*PCI DSS (payment card industry data security standard)*

Reikalavimai:

* DUomenys turi buti enreypted at rest ( kai saugomi)
* Duomenys turi buti encrypted in transit ( kai siunciami) 

Tai uztrikrina kad kreditiniu kortelu informacija nebutu pavogta.

# Medicininiai duomenys#

Medicininiai duomenys yra labai jautrus, todel ju saugojima reguliuoja istatymai.

|Standard	|Salis
|HIPAA		|USA
|HITECH		|USA
|GDPR		|EU
|DPA		|UK

Sie istatumai reikalauja apsaugoti pacientu duomenis naudonatis sauguo priemones,iskaitant *Cryptography*


# Svarbiausia mintis: #
 
Cryptography dazniausiai yra:
* nematoma vartotojui
* bet kritiskai svarbi interneto saugumui

Be cryptography neveiktu:
* HTTPS
* Secure login
* online banking
* secure messaging
* saugus failu atsisiuntimas

# Santrauka: #
*Cryptography yra moklas apie saugu duomenu perdavima ir saugojima.

Ji naudojama:
* apsaugoti komunikacija
* patikrinti duomenu autentiskuma
* uztikrinti duomenu integraluma
* patvirtinti indetiteta

Todel cryptography yra viena svarbiausiu modernios kibenetikos saugumo infrastrukturu daliu.

_______________________________________________________________________________

### Plaintext --> Ciphertext ###

Norint suprasti *Cryptography*, reikia zinoti pagrindinius terminus, kurie apraso kaip informacija transformuojama is kaitmos formos i uzsifruota ir atgal.

* Pagrindiniai terminai:

*PLAINTEXT*
Tai orginalus duomenys pries sifravima

Tai gali buti:
* tekstas
* dokumentas
* nuotrauka
* video 
* bet kokie skaitmeniniai duomenys

Svarbu suprasti: kompiuteriuose visi duomenys galiausuau yra binary (bitai) todel plaintext gali buti bet kokia informacija
PVZ:
Hello
^Plaintext


* Ciphertext

Tai uzsifruota informacija po encryption proceso.
Ji atrodo kaip atsitiktine duomenu seka ir neturi buti suprantama be rakto
PVZ:
*XJ92A7L1*
^ Tai yra ciphertext.
Gera kriptografija uztrikrina, kad is cphertext negalima nustatyti orginalaus plaintext turinio.

* Cipher
Tai matematinis algoritmas, kuris naudojamas konvertuoti:
<plaintext--ciphertext> 
ir atgal
<ciphertext--plaintext>

Cipher apibrezia taisykles kaip duomenys transformuojami.

Pavyzdiai:

* Caesar cipher
* AES
* DES
* RSA

Svarbus Cryptograhy princpas:
*CIPHER ALGORITMAS NERA SLAPTAS* 

Sistema turi buti saugi net jei attacker zino algortima


* Key

tai bitu seka (skaiciu rinkinys), kuria cipher naudoja sifravimui ir issifravimui.
Key kontroliuoja kaip tiksliai cipher transformuoja duomenis.

Schema:
< Plaintext + cipher + key = ciphertext >

Be teisingo key:
* ciphertext negali buti sifruojamas
* plaintext negali buti atsakytas

Svarbi taisykle:
< Algorithm (cipher) = public>
<Key = secret>

(Iskyrus public key asymmetric cryptography) 

__________________________________________________________________________

### Encryption ###

Encryption - tai procesas, kurio metu plaintext paverciamas i ciphertext

Procesas:

<plaintext + key + cipher = ciphertext>
PVZ:
HELLO --> encryption --> XJ92A7L1

Encryption tikslas- padaryti informacija nesuprantama be rakto.

__________________________________________________________________________

### Decryption ###

Decryption - tai atvirkstinis procesas

<ciphertext + key + cipher = plaintext>
PVZ:
<XJ92A7L1 + decryption = HELLO >

Be teisingo key plaintext turi buti praktiskai neimanoma atkurti.

Kriptografijoje tai vadinamas:
*computationally infeasible* 

Tai reiskia:
Teoriskai imanoma, bet praktiskai reikalautu neimai daug laiko ir skaiciamo resursu


* VISA SCHEMA
Plaintext
	|
	|Encryption (cipher +key)
Ciphertext
	|
	|Decryption (cipher+key) 
Plaintext

* Kerckhoffs`s priciple
Vienas svarbiausiu kriptografijos principu.
*Sistema turi buti saugi net jei attacker zino viska apie algortima, iskyrus rakta*

Todel:
* algoritmai yra viesi
* saugumas remiasi key slaptumu

___________________________________________________________________________

### SVARBU PRISIMINTI ###

* Plaintext- orginalus duomenys
* Ciphertext - uzsifruoti duomenys
* Cipher - algoritmas kuris transformuoja duomenis
* Key - Slaptas parametras kuris leidzia sifruoti ir isifruoti
< encryption -plaintext-ciphertext>
< Decryption-ciphertext-plaintext>

_______________________________________________________________________

### Historical Cipher  ###

Istoriniai sifrai buvo ankstyvi metodai, naudojami apsaugoti zmones pries atsirandant modernai kriptografijai. Jie dazniausiai veike transformuojant raides pagal tam tikras taisykles.

Vienas paprasciausiu ir zinomiausiu yra *Caesar Cipher*

* Caesar cipher

Paprastas substitution cipher, kuriame kiekviena raide pakeiciama kita, paslinkta per tam tikra skaiciu poziciju abeceleje.

PVZ:
Plaintext:
TRYHACKME
KEY:
3
Tai reiskia:

kiekviena raide paslenkama 3 pozicijas i desine abeceleje.

* Abeceles poslinkis
Orginali ABC:
ABCDEFGHIJKLMNOPQRSTUVWXYZ
Paslinkta +3:
DEFGHIJKLMNOPQRSTUVWXYZABC

* Raidziu transfromacija
|Plaintext	|+3 shift	|Ciphertext
|T		|W		|W
|R		|U		|U	
|Y		|B		|B	
|H		|K		|K
|A		|D		|D
|C		|F		|F	
|K		|N		|N
|M		|P		|P
|E		|H		|H

Ciphertext:
WUBKDFNPH

* Wrap-around pricipas

Kai poslinkis pasieka Z, skaiciavimas apsisuka i abc pradzia

Sis mechanizmas matematiskai aprasojamas naudojant modulat arithmetic.


* Matematine formule

Raidems priskiriami skaiciai:

A=0
B=1
C=3
...
Z=25

Sifrafimas:
C = (P + K ) mod 26

kur:
* P = plaintext raidės indeksas
* K = key (shift)
* C = ciphertext raidės indeksas

* Decryption

Norint atkurti orginalia zinute:

<P = (C + K) mod 26>
PVZ:
<W-3=T>

* Kodel Caesar Cipher nesaugus

Sis sifras yra labai silpnas nes:
* yra tik 25 galimi raktai
* galima isbandyts visus ( brute force attack) 
* galima naudoti frequency analysis

PVZ:

Anglu kalboje dazniausiai pasitaikanti raide yra E, todel galima speti sifravima

* Kodel jis vis dar mokomas:

Nors Caesar cipher nera saugus, jis padeda suprasti pagrindinius kriptografijos principus:

* plaintext -> ciphertext transformacija
* key naudojima
* substitution principa
* modular arithemtic
* encryption ir decryption logika

* Svarbu prisiminti:

Caesar cipher yra substitution cipher, kuriame kiekviena raide pakeiciama pagal nustatyta poslinki:
Pagrindine formule
<C = (P + K) mod 26> 
________________________________________________________________________

### Types of Encryption ###

Sifravimo metodai skistomi i dvi pagrindines kategorijas:
1. Symmetric encryption
2. Asymmetric encryption

Abi technologjis yra modernios kriptografijos pagrindsa ir daznai naudojamos kartu realiose sistemose.

* Symmetric encryption
(simetrinis sifravimas) naudoja ta pati rakta tiek sifravimui, tiek isifravimui.

Schema:
<plaintext + secret key → encryption → ciphertext>
<ciphertext + same key → decryption → plaintext>

* Pagrindine ideja:
Abi komunikacijos puses turi turetu ta pati slapta rakta
Todel jis vadinamas:
shared secret key
arba 
private key cryptography

* Problema - Key distribution

Didziausia summetric encryption problema yra kaip saugiai perduoti rakta kitai pusei.

Pvz:
TU uzsifruoji dokumenta slaptazodziu ir siunti kolegai.
Taciau jei slaptazodis isiustas tuo paciu kanalu ( email) attacker gali gauti:
* Uzsifruota faila
* Slaptazodi
Todel raktas turi buti perduodamas per kita saugu kanala.
Tai vadinama key exange problem.

* Symmetric Encryption Algorithms 

*DES*
Data Encrption Standard
* Sukurtas 1977 
* key size: 56 bits

Didejant kompiuteriu galiai DES tapo nesaugus.
1999 metais DES buvo nulauztas per maziau nei 24 valandas.

* 3DES 
Triple DES
DES algoritmas naudantmas tris kartus is eiles.

Key size:
*168 bits*

Taciau realus saugumas apie :
*112 bits*

3DES buvo laikinas sprendimas DES pakeitimui.
2019 metais 3DES buvo oficialiai deprecated. 

* AES
Advanced encyrption standard

Priimtas 2001 metais.

Galimi raktu dydziai:
*128bits*
*192bts*
*256bits*

AES sidnien yra pagrinidnis symmetric encryption standartas.

Naudojamas:
* TLS
* VPN
* disk encrypton
* secure messaging



# ASSYMETRIC ENCRYPTION #
Naudoja du skirtingus raktus:
1. Public key
2. Private key

Schema:

<plaintext + public key → encryption → ciphertext>
<ciphertext + private key → decryption → plaintext>

* Pagrindine ideja
Public key galima dalintis su visais.
Private key turi likti slaptas
Todel si technologija vadinamas
*PUBLIC KEY CRYPTOGRPAHY* 

* Kaip tai veikia

Jei nori kazkas nusiusti saugia zinute:
1. paimi jo public key
2. uzsifruoji zinute
3. tik tas zmogus gali ja issifruoti su private key

# ASYMMETRIC CRYPTOGRAPHY ALGORITHMS #

* RSA

Vienas zinomiausiu public key algoritmu.

Tipiniai raktu dydziai:
*2048bits*
*3072bits*
*4096bits*

Minimalus rekomenduojamas siandien:
*2048bits*

# DIFFIE-HELLMAN #

NERA ENCRYPTION ALGORITMAS

Jis naudojamas:
* secure key exange 

Tai leidzia dviem salims sukurti bendra slapta rakta per nesaugu kanala.

Sis raktas veliau naudojama symmetric encryption

# EEC (ELLIPTIC CURVE CRYPTOGRAPHY)

Naudoja elipsinius kreiviu matematika
DIdelis privalmas:
tas pats saugumo lygis su mazesniu raktu.

PVZ:
<ECC 256 bit ≈ RSA 3072 bit>

TOdel ECC daznai naudojamas:
* mobile devices
* TLS
* modern cryptography systems

# KODEL ASSYMETRIC ENCRYPTION LETESNIS #

Assymetric cryptography remiasi sudetingomis matematinemis problemomis.

Pvz:
lengva:
*padauginti du didelius pirminius skaičius*

labai sunku:
*is ju sandaugos rasti tuos su skaicius*

tai vadinamas:
*One-way mathematical problem*

# KAIP TAI NAUDOJAMA REALIOSE SISTEMOSE #

Praktiskai naudojamas aieju metodu deriys.

Procesas daznaiausiai:
1. Assymetric cryptography naudojama secure key exange
2. Sukuriama shared symmetric key
3. Tolimesneje komunikacija vyksta naudojant symmetric encrypion

Nes symmetric encryption yr daug greitesnis

# SVARBIAUSIA PRISIMINTI #

Symmetric Encryptio 
* tas pats raktas encryptio decryption
* labai greitas
* problema:key exchange

Assymetric encryption
* public key+private key
* lengva key distribution
* letesnis

__________________________________________________________________________________

### BASIC MATHS ###

Moderni kriptografija remiasi matematika. Daugelis kriptografiniu algoritmu naudoja matematines operacijas, kurios leidziai saugiai trnasformuoti duomenis

Du labai svarbus pagrindai:
* XOR operation
* Modulo operation

Sios operacijos naudojamos daugelyje kriptografiniu algoritmu


# XOR Operation #

*Exlusive OR* yra logine operacija naudojama dvejetaineje (binary) aritmetikoje

Rezultatas:
* 1 jei bitai skirtinig
* 0 jei bitai vienodi

XOR daznai zymimas:
< ⊕ > 

Arba programavime:
< ^ >

# XOR TRUTH TABLE #
|A	|B	|A ⊕ B
|0	|0	|0
|0	|1	|0
|1	|0	|1
|1	|1	|0

Truth table parodo visus galimus operacijos rezultatus!

# XOR EXAMPLE #

<1010>
<1100>
Skaiciuojame bit po bito:
<1 ⊕ 1 = 0>
<0 ⊕ 1 = 1>
<1 ⊕ 0 = 1>
<0 ⊕ 0 = 0>
Rezultatas:
<0110>

# SVARBIOS XOR SAVYBES #

Sios savybes yra priezastis, kodel XOR naudojamas kriptografijoje.

* Self-inverse property
< A ⊕ A = 0 >
Tas pats skaicius XOR su savimi visada duoda 0.

* Identity property
< A ⊕ 0 = A >
XOR su 0 nekeicia reiksmes.

* Commutative property
< A ⊕ B = B ⊕ A >
Operaciju tvarka nesvarbi

* Associative property
< (A ⊕ B) ⊕ C = A ⊕ (B ⊕ C) >
Operacijas galima grupuoti skirtingai

# XOR NAUDOJIMAS KRIPTOGRAFIJOJE #

Xor gali bui naudojamas kaip paprastas symmetric encryption metodas.
Tarkime:
<P = plaintext>
<K = secret key>
Ciphertext:
<C = P ⊕ K>

# Decryption # 

Norint atukurti plaintext:
<C ⊕ K>
Kadangi: 
<C = P ⊕ K>
Gauname:
<C ⊕ K>
<= (P ⊕ K) ⊕ K>
<= P ⊕ (K ⊕ K)>
<= P ⊕ 0>
<= P>

Tai veikia del XOR savybes:
<K ⊕ K = 0>

# PASTABA # 
Jei XOR naudojamas su trumpu raktu, toks sriftas yra nesaugus.
Teoriskai saugus viariantas yra *One-time pad*,kai:
<key lenght = plaintext lenght>
ir raktas naudojamas tik viena karta!

# MODULo OPERATION #

Modulo operacja grazina dalybos liekana

Zymejimas:
* < % >
arba 
* < mod > 

# MODULO PVZ #

* < 25 % 5 = 0 >
nes
* < 25 = 5 x 5 + 0 > 
_______________________
* < 23 % 6 = 5 >
nes
* < 23 = 3 x 6 + 5 >
_______________________
* < 23 % 7 = 2 >
* < 23 = 3 × 7 + 2 >

# MODULO REZULTATO SAVYBES # 

Modulo rezultatas visada bus intervale:
 * < 0 ≤ result < divisor > 
PVZ:
* < 23 % 7 = 2 > 
2 yra tarp 0 ir 6

# Modulo nera reversible # 

Jei turi lygti:
* < x mod 5 = 4 > 
galimu sprendimu yra begalybe:
* 4
* 9
* 14
* 19
* 24
* ...
Todel modulo rezultatas neleidzia atkurit orginalaus skaiciaus.

# Svarbi savybe kriptografijoje #

Modluo leidzia skaicius sumazinti iki riboto intervalo, net jei skaiciai tampa labai dideli.
PVZ:
* < a mod n >
rezultatas visada bus tarp:
* < 0 ir n-1 >

# Modular Exponentation # 

Kriptografijoje daznai naudojama operacija:
* < a^b mod n >
Tai vadinama modular exponentation

Si iperacija naudojama algoritmuose:
* RSA
* DIffie-Hellman
* ECC

PVZ:
* < 9^12 mod 29 > 
Tokies skaiciavimai leidzia naudoti labai dideslius skaicius, bet rezultatas visada lieka ribotas. 

# SVARBU PRISIMINTI # 
*XOR* 
* binary logine operacija
* naudojama encryption algoritmuose
* leidzia atlikti paprasta symmetric encryption

*MODULO*
* grazina dalybos liekana
* dideliu skaicius naudojimas matematikoje
* butinas modernioje kriptografijoje

_______________________________________________________________________

### Public key cryptography basics ###

Kai bendraujame internetu, kyla keli svarbus saugumo klausimai:
* Ar tikrai bendrauju su tuo zmogumi ar serveriu kuris sako kas yra?
* Ar zinute nebuvo pakeista pakeliui per tinkla?
* Ar kazkas kitas negali jos perskaityti?
Kadangi internetas yra nepatikima terpe ( insecure channel) zinutes keliauja per daugybe serveriu ir tinklo irenginiu. Todel reikia mechanizmu kurie uztikrina saugu bendravima.

Kriptografija padeda uztikrinti keturis pagrindinius saugumo principus.


# Pagrindiniai saugumo principai #

*Authentication* 

Authentication reiskia tapatybes patvirtinima

Tai leidzia isitikinti, kad bendrauji su tikru asmeniu ar serveriu, o ne su kazkuo,kas apsimeta juo. 
PVZ:
* SSH patikrina serverio rakta
* HTTPS patikrina serverio sertifikata

*Authenticity* 
Reiskia, kad duomenys tikrai kile is deklaruoto saltinio.
PVZ:
* Failas pasirasytas programos kurejo
* Zinute pasirasyta privaciu raktu
Tai daznai uztikrina *digital signatures*

# Integrity #

Reiskia, kad duomenys nebuvo pakeisti.

Jeigu kas nors pakeistu zinute pakeliui, sistema turetu tai aptikti.

Tam naudojami:
* hash funkcijos
* digital signatures

# Confidentiality #

Confidentiality reiskia duomenu slaptuma
Tik numatytas gavejas gali perskaityti informacija
Tam naudojamas *Encryption* 

# Symmetric vs Assymetric Cryptography # 

# Symmetric vs Assymetric Cryptography # 

Naudoja vena bendra slapta rakta:
< Encrypt -- Key >
< Decrypt -- tas pats key >

Problema:
kaip saugiai perduoti rakta?
Jeigu kazkas perims rakta --> gali skaityti visus duomenis
Todel symmetric encryption daznai naudoja tik po to kai raktas jau suagiai pasikeistas.

PVZ:
*AES* naudojamas:
* HTTPS
* VPN
* disk encryption

# ASSYMETRIC ENCRYPTION # 

Naudoja du skirtingus raktus

* Public key
* Private key

Publoc key gali zinoti visi.
Private key turi zinoti tik savininkas

Principas:
* Encrypt su public key
* Decrypt su private key

Todel galima saugiai siusti duomenis net per nesaugu tinkla. 

# Kur cia atsiranda RSA ir DiffieHellma?# 
Sie algoritmai turi du skirtingus tiklsus

*RSA* 
Naudoja:
* encryption 
* digital signatures
* authenticaton
* key transport
RSA ideja:
*PADAUGINTI DU DIDELIUS PRIME SKAICIUS* 
Bet labai sunku:
*IS PRODUKTO ATSPETI TUOS PRIME* 
PVZ:
< p x q = n >
znoti:
p
q

lengva apskaiciuoti --> n

Bet jaigu zinai tik n
labai sunku rasti p ir q.  

Butent ant to ir stovi RSA saugumas.

# Diffie-Hellman # 
Tai daro kita dalyka.
Jis nesifruoja duomenu
Jis sukuria bendra slapta rakta tarp dvieju zmoniu.
Net jei visi mato komunikacija.

Procesas:
Abi puses sutaria del:
*p prime number*
*g generator*

Tai yra public values.

< Alice pasirenka secrete:>
< *a* >
Apskaiciuoja:
< A = g^a mod p >
ir isiuncia *A*


< Bob pasirenka secret:>
< *b* >
apskaiciuoja:
< B = g^b mod p >
ir siuncia *B*

Tada:

< Alice skaiciuoja:>
< shared key = B^a mod p >
< Bob skaiciuoja:> 
< sharekd key = A^b mod p >
Ir gaunasi tas pats skaicius.
< g^(ab) mod p >
*TAI TAMPA SHARED SECRET KEY*

# Kodel ta saugu?# 

Stebetojas mato:
* p
* g
* A
* B

bet zino:

* a
* b 

kad gauti shared key --> reiketu ispresti *discrete logarithm problema*
Tai praktiskai neimanom su dideliais skaiciais.

# Kaip naudoti realybeje # 

Realus internetas daro taip:
1. Diffie-Hellman
- sukuria shared key
2. AES	
- naudoja ta key sifravimui
3. RSA arba certificates
- patvirtina serverio tapatybe

Todel TLS/HTTPS naudoja visus situos dalykus kartu.

# SVARBI  MITNIS! # 

*RSA->encryption+signatures*
*Diffie-hellman --> key exange *  
*AES --> actual encryption*


### COmmon use of Asymetric Encryption ###

# Analogy #

Isivaizduok kad nori saugiai susitarti su draugu del slapto komunikacijos kodo.

Tu turi:

* Secret code 
* instrukcijas kaip naudoti ta koda

Problema:

Kaip tas instrukcijasnusiusti draugui taip kad niekas kitas ju npepamatytu, net jei zinute keliauja per nesaugu tinkla?


*SPRENDIMAS*

Paprasai draugo atsiusti spyna

*SVARBUS PRINCIPAS*

* spyna gali naudoti bet kas
* bet atrakinti gali tik tas kas turi rakta

Tu turi tvirta deze kuria galima uzrakinti ta spyna

Procesas:
1. Draugas tau atsiuncia spyna
2. Tu i deze idedi:
	* secret code
	* instrukcijas
3. Uzrakini deze ta spyna
4. Nusiunti deze draugui

Kadangi tik draugas turi rakta, tik jis gali:
* atrakinti deze
* perskaityti instrukcijas

PO TO

Dabar jus abu zinot sercet code.
Tolimesne komunikacija gali vykti naudojant si koda pvz:
* synnetric encryption

Net jei kazkas stebi komunikacija, jis negali jos perskaityti. 

# Analogy vs Cryptography #

|Analogy	|Cryptographic system
|Secret code	|Symmertric encryption cipher+key
|Lock		|Public key
|Locks key	|Private key


# Svarbi Kriptografijos mintis # 

Asymmetric encryption dazniausiai naudojama ne visam duomenu sifravimos, o saugiam raktu perdavimui.

Procesas realiose sistemos:

1. Public key cryptography naudojama perduoti symmetric key
2. Symmetric encryption naudojama sifruoti isai komunikacijai

Taip daroma todel,kad:

* Asymmetric encryption yra letesne
* symmetric encryption yra daug greitesne


# Realus pavyzdys#

HTTPS/TLS veikia mazdaug taip:
1. Serveris pateikia public key (certificate)
2. Klientas sugeneruoja symmetric key
3. Klientas uzsifruoja su server public key
4. Serveris ja isifruoja su private key
5. Tolimesne komunikacija vyksta su AES symmetric encryption

# ISVADA #

*Asymmetric encryption solces the key distribution problem* 
Didziausia symmetric encryptonproblema yra:
* KAIP SAUGIAI PERDUOTI RAKTA? 
Public key cryptography leidzia tai padaryti net per nesaugu tinkla.

________________________________________________________________________

### RSA (Rivet-Shamir-Adlemen)

*RSA* yra asymmetric encryption algoritmas, leidziantis saugiai perduoti duomenis per nesaugu komunikacijos kanala.

Nesaugus kanalas reiskia kad:
* komunikacija gali buti stebima
* duomenys gali buti perimti 
* uzpuolikas gali bandyti juos isifruoti

RSA leidzia:

* SIfruoti duomenis
* kurti skaitmeninius parasus
* patvirtinti tapatybe

# Matematine ideja uz RSA # 

Rsa saugumas remiasi viena svarbia matematikos problema:
*DIDELIU SKAICIU FAKTORIZAVIMAS*

Labai lengva:
`padauginti du didelius pirminius skaičius`

Labai sunku:
`is produkto rasti tuos du pirminius skaicius`

PVZ:

Lengva padauginti:
`113 x 127 = 14351`
Taciau jei tau duotas tik skaicius:
`14351`
Rasti kokie du pirminiai skaiciai ji sudaro jau yra daug sunkiau. 

# Dideliu skaiciu pavyzdys #

Prime number 1
`982451653031`
Prime number 2
`169743212279`
Produktas:
`982451653031 × 169743212279 =`
`166764499494295486767649`

Padauginti siuos skaicius gana lengva. 
Taciau jei turi tik :
`166764499494295486767649`
Rasti pirminius faktorius tampa labai sunku. 

# Realiose sistemose # 
Tikrame *RSA*:
* p ir q turi simtus skaitmenu
* daznai apie 300 skaitmenu kiekvienas

Todel:
< n = p x q >

Tampa milzinisku skaiciumi. 
Tokio skaiciaus faktorizavimas yra praktiskai neimanomas su dabartine technologija. 


# RSA veikimo principas # 

1. Pasirenkami du pirminiai skaiciai
	* p = 157
	* q = 199

2. Apskaiciuojamas n
	* n = p x q
	* n = 157 x 199	
	* n = 31243

3. Skaiciuojama Euler funkcija
	* φ(n) = (p − 1)(q − 1)
Siame pavyzdyje:
	* φ(n) = (157 − 1)(199 − 1)
	* φ(n) = 156 × 198
	* φ(n) = 30888
4. Parenkamas viesas eksponentas
Pasirenkamas skaicius e , kuris:
* yra mažesnis už φ(n)
* yra coprime su φ(n)
Tai reiskia:
* < gcd(e, φ(n)) = 1 > 
Siame pavyzdyje:
< e = 163 >

5.Apskaiciuojamas privatus eksponentas
Randamas skaicius d, kuris tenkina:
* < e × d ≡ 1 mod φ(n) >
Siame pavyzdyje:
* < d = 379 > 
Nes:
* < 163 x 379 = 61777 > 
* < 61777 mod 30888 = 1 >  

# RSA raktai # 

Public key:

* < ( n, e ) > 
Siame pvz:
* < 31243 , 163 > 
Private key:
* < ( n, d) >
Siame pvz:
* < ( 31243, 379) >

# Sifravimas # 

Tarkime Alice nori issiusti zinute:
< m = 23 > 
Ciphertext skaiciuojamas:
< c = m^e mod n >
Siame pvz:
< c = 13^163 mod 31243 > 
< c = 16341 > 
Alice siuncia:
< c = 16341 > 

# Desifravimas # 

Bob gauna ciphertext ir skaiciuoja:

< m = c^d mod n > 

Siuo atveju :

< m = 16341^379 mod 31243 > 

Bob gauna orginalia zinute.


# Kodel tai veikia? # 

Matematinis irodymas remiasi:

* modular arithmetic
* Euler teorema

Sis irodymas yra sudetingas ir paprastai nepatenka i ivadinius kursus

# RSA CTF uzduotyse # 

Crypto CTF uzduotyse RSA pasitaiko gan daznai
Dazniausiai pateikiami keli parametrai o reikia rasti likusius:

*p  – prime number*
*q  – prime number*
*n  – p × q*
*e  – public exponent*
*d  – private exponent*
*m  – plaintext*
*c  – ciphertext*

Raktai: 

*Public key = (n, e)*
*Private key = (n, d)*

# Formules #

`Encryption` 
< c = m^e mod n >
`Decryption` 
< m = c^d mod n >

# Kodel RSA luzta CTF uzduotyse? # 

Reliame pasaulyje RSA saugumas remiasi tuo, kad:

< p ir q yra labai dideli > 

CTF uzduotyse daznai:
 
* p ir q yra mazi
* n gali faktorizuoti

Kai randi 
* p
* q

gali apskaiciuoti 
* φ(n)
* d

Ir tada isifruoti zinute.

_____________________________________________________________________________________

###  Diffie-Hellman Key exange ###
Tai yra key exange algoritmas, kuris leidzia dviem salims sukurti bendra slapta rakta (shared secret) net jei ju komunikacija vyksta per nesaugu kanala. 

Tai reiskia kad:
* komunikacija gali stebeti trecios salys
* taciau jos negali zinoti bendro slapto rakto

Sukurtas shared key veliau naudojamas symmetric encryption tolimesnei komunikacijai,

# Pagrindine ideja #

DIffie-Hellman leidzia:
*Dviem salims sukurti ta pati slapta rakta*
*net jei visas ju pokalbis yra viesas* 
Tai imanoma del matematiniu savybiu, susijusiu su modula exponentation. 

# Intuityvus paaiskinimas # 

Galima isivaizduoti procesa taip:
1. Yra bendras viesas elementas
2. Kekviena puse turi savo slapta komponenta
3. Sie komponentai yra kombinuojami
4. Galiausiai abi uses gauna ta pati rezultata

`Svarbios savybes`:
* Labai lengva kombinuoti siuos elementus
* Labai sunku atskirti juos atgal

Taip sukuriamas bendras slaptas raktas.

# Matematinis procesas # 

1. Viesi parametrai
Alice ir Bob susitaria del dvieju viesu reiksmiu:

<p = large prime number > 
<g = generator > 

Salyga:

< 0 < g < p >

Sios reiksmes yra public ir gali buti zinomos visiems.
Realiuose Diffie-Hellman algoritmuose p yra labai didels pirminis skaicius, daznai simtu ar tukstanciu bitu ilgio.

Pavyzdys su mazesniais skaiciais, lengviau skaiciuot:
< p=29 >
< g=3 >
Sie skaiciai yra viesi parametrai kuriuos zino:
* ALice 
* Bob 
* ir net stebetojas
Taciau tai nesukelia saugumo probleos, nes slaptumas remiasi ne p ir g o privaciais raktais. 

# Privatus raktai  
Kiekviena puse pasirenka sapta skaiciu.
Alice:
< a = 13 >
Bob:
< b =15 >

Sie skaiciai yra private keys ir niekada neatskelidziami. 

# Viesu raktu skaiciavimas #  

Alice apskaiciuoja savo public key:
< A = g^a mod p >
< A = 3^13 mod 29 >
< A = 19 > 

Bob apskaiciuoja savo public key:
< B = g^b mod p > 
< B = 3^15 mod 29 > 
< B = 26 >

Sie skaiciuoja yra public keys. 

# Key exange # 

Alice ir Bob apsikeicia savo  public keys. 

Alice siuncia:
< A = 19 > 
Bob siuncia:
< B = 26 > 

Sie skaiciai gali buti *VIESAI MATOMI* 
 
# Shared secret skaiciavimas # 

Alice apskaiciuoja:
< shared key = B^a mod p > 
< shared key = 26^13 mod 29 > 
< shared key = 10 > 

Bob apskaiciuoja:
< shared key = A^b mod p >
< shared key = 19^15 mod 29 >
< shared key = 10 >

Abi puses gauna ta pati rezultata.

`SHARED SECRET = 10 `

Matematiskai tai yra:
<g^(ab) mod p > 

# Kodel tai veikia # 

Alice skaicuoja :
<(g^b)^a mod p>
Bob skaiciuoja 
<(g^a)^b mod p>

Kadangi:
<(g^b)^a = g^(ab)>
<(g^a)^b = g^(ab)>
abi puses gauna ta pati rezultata.

# Kodel stebetojas negali rasti rakto # 

Stebetojas mato tik
< p >
< g > 
< A > 
< B >
Taciau nezino:
< a >
< b >
Norint apskaiciuoti slapta rakta reiktu ispresti problema vadinama:
*DISCRETE LOGARITHM PROBLEM* 
Su dideliais skaiciais si problema yra praktiskai neisprendziama.

# Diffie-Hellman realiose sistemose # 
Naudojamas:
`key agreement` 
Tai reiskia:
`sukurti bendra symmetric encryption rakta`
po to sis raktas naudojamas su:
`AES`
`Ar kitu symmetric encryption algoritmu`

# Diffie-Hellman ir RSA # 

Sie algoritmai daznai naudojami kartu. 
Ju paskirtis skirtinga:
*Diffie-Hellman*
`key exhange`

*RSA*
`authentication`
`digital signatures`
`key transport`

Todel moderniuose protokoluose, pvz:
`TLS/HTTPS`
naudojami keli algoritmai vienu metu.

#ISVADA#
*Diffie-Hellman → sukuria shared key*
*RSA → patvirtina tapatybę*
*AES → šifruoja duomenis*

______________________________________________________________________________

### SSH ###

# SSH Authentication # 

*SSH* (Secure shell) leidziai saugiai prisijungti prie nuotolinio serverio per sifruota risi.

SSH naudoja:
* Encryption-kad niekas negaletu skaityti komunikacijos
* Authentication-kad abi puses zinotu su kuo kalba. 

SSH autentikacija vyksta dviem pagrindiniais etapais:

* Server authentication
* Client authentication

1. Server authentication
Kai pirma karta jungiesi prie serverio:
< ssh user@ip >
SSH klientas parodo serverio *public key fingerpirnt*  ir klausia ar tu juo pasitiki

Pvz:gali pamatyti kazka panasaus:
< The authenticity of host can't be established>
<ED25519 key fingerprint is ...>
Tai reiskia:
* serveris pateikia savo *public key*
* tavo kompiuteris tikrina ar sis serverio raktas jau zinomas.

Jei tai pirmas prisijungimas, tu turi *patvirtiniti serverio identiteta*

Jey sutinki, serverio raktas irasomas i:
<~/.ssh/known_hosts>

___ 

# Kam tai reikalinga? # 

Tai apsaugo nuo :
<Man-in-the-Middle attack>
Jei veliau serverio public key staiga pasikeis, SSH apie tai ispes.

Tai labai svarbu, nes gali reiksti:
* serveris tikrai pakeistas
* arba kazkas bando apsimesti serveriu.

2. Client Authentication

Kai jau zinome, kad serveris tikras, serveris turi patikrinti *kas tu esi*
Dazniausiai naudojami du metodai:

*PASSWORD AUTHETICANTION*
Prisijungi naudojant:
<username+password>

Tai veikia, bet nera geriausia praktika, nes slaptazodziai:
* Gali buti silni
* Gali but reuse`inti
* Gali but brute-force`inami

*KEY-BASED AUTHENTICATION*

Tai saugesnis metodas.
Naudojama:
* *private key* kliento puseje
* *public key* serverio puseje

Principas:
* Klientas turi private key
* serveris turi atitinkama public key
* serveris patikrina ar klientas tikrai turi private key

`Kaip serveris patikrina private key jo nematydamas`
Serveris niekada nepraso tavo private key
Vietoj to jis daro *challenge-response authentication*

Procesas:

1. Serveris randa tavo *public key* faile <authorized_key>
2. serveris sugeneruoja *challenge* ( atsitiktine tvarka) 
3. tavo ssh klientas ta challenge *pasiraso su private key*
4. serveris patikrina parasa naudodamas *public key*

Jei parasas teisingas->autentikacija sekminga.

!MINTIS!
Serveris netikrina tavo private key tiesiogiai.
Jis tik tikrina ar tu gali irodyti kad ji turi.

Tai vienas pagrindiniu public-key cryptography principu.

# SSH Key pair # 

SSH key pair susideda is:

< private key -- lieka tavo PC > 
< public key -- ikeliamas i serveri > 

Dazniausiai kei par generuojamas su:
<ssh-keygen -t ed25519 > 

Sugeneruojami du failai:
<id_ed25519       → private key>
<id_ed25519.pub   → public key>

!SVARBU!

<ssh-keygen> sugeneruoja raktus.

Komanda: 
<cat id_ed25519.pub> 
Tik *parodo public key* turini
CMD:
<cat id_ed25519> 
tik *parodo private key* turini

<cat> nieko negeneruoja


# SSH key algorithms # 

SSH palaiko kelis algortimus. Siame etape svarbu bent atpazinti ju pavadinimus. 

*DSA*
Digital Signature Algorithm.
Senas algortimas, naudojamas skaitmeniniams parasams. 

*ECDSA* 
Elliptic Curve Digital Signature Algorithm
Naudoja elliptic curve cryptography, todel leidzia naudoti mazesnius raktus tam paciam saugumo lygiui, 

*Ed25519*
Modernus ir labai saugus algoritmas.
Dazniausiai rekomenduojamas siandien.

*ECDSA-SK / Ed25519-SK*
Variantas, naudonatys *hardware security keys*(pvz: Yubikey)
jie apsaugo private key dar geriau, nes jis gali buti laikomas fiziniame irenginyje.

# Private key # 
Private key yra labai jautrus failas.
ji galima sulyginti su *password*
Jei kazkas gauna tavo private key,jis gali bandyti prisijungti prie serveriu, kurie pasitiki tavo public key. 

Todel private key:
* niekada nesharinamas
* nesiunciamas internetu
* nelaikomas nesaugiose vetose

# Passphrase # 
Kuriant SSH key galima prideti *passphrase* 
Svarbu suprasti:
Passphrase *neautetifikuoja taves serveryje*

Jis tik *decryptina tavo private key tavo PC*
Tai reiskia:
* Passphrase niekada nesiunciama serveriui
* serveris jos nemato
* ji naudojama tik tavo sistemoje atrakinti private key

`Kodel passphrase svarbi`
Jei kazkas pavogs tavo private key faila, passphrase gali apsaugoti nuo tiesioginio nadojimo
Taciau:
* silpna passphrase galima lauzti
* pvz su *John the ripper*
TOdel:
* private key turi buti saugomas
* passphrase turi buti stipri

# File permissions #
Private key turi teisigas teises.
SSH klientas daznai atsisakys naudoti private key, jei jo permission per laisvos
Tipiskai:
<chmod 600 id_ed25519> 
Tai reiskia :
* Tik savininkas gali skaityt ir rasyti
* Niekas kitas negali prie jo prieti

`known_hosts`
Failas:
<~/.ssh/known_hosts>
Saugo serveriu public keys, kuriuos jau esi patvirtines.
Tai leidzia SSH klientui atpazinti:
* ar jungiesi prie to paties serverio
* ar serverio key staiga nepasikeite

`authorized_keys`
Serverio puseje public key laikomas faile:
<~/.ssh/known_hosts>
Jei tavo public key yra siame faile, serveris gali priimti tavo login per SSH key authentication. 
Tai reiskia:*authorized_keys = sąrašas public keys, kuriems leidžiama jungtis*

# Prisijungimas su SSH key #

Jei nori nurodyti konkretu private key:
<ssh -i privateKeyFileName user@host>
Pvz:
<ssh -i id_ed25519 user@10.10.10.10>

# Pantesting/CTF panaudojimas #
SSH keys yra labai naudingi pentesting ir CTF metu
Jei turi shell serveryje ir gali irasyti savo public key i:
<~/.ssh/authorized_keys>
tada gali prisijungti per SSH ir gauti:
* stabilesni shell
* normalu terminala 
* tab completion 
* maziau reverse shell problemu

`Kodel tai naudiga`
Reverse shells daznai buna nestabilus:
* ctrl+c sugadina sesija
* nera tab completion
* blogas terminalas
* shell luzta
SSH leidzia gauti daug patogesn prieiga. 

`Persistence/Backdoor`
Jei attacker arba pentesteris ideda savo public key i *authorized_keys*, jis gali veliau vel prisingti prie ssh
Tai tampa:*persistence*
arba praktiskai:*backdoor access*
Todel *authorized_keys* failas yra abai jautri cvieta

# Labai svarbi saugumo taisykle *
Jei private keu kompromituotas, laikyk tai labai rimti incidentu
Nes:*private key compromise = galimas neatorizuotas prisijungimas*
Todel reikia:
* saugoti private key
* naudoti stirpia passphrase
* tureti teisingas permissions
* nesharinti rakto

#Santrauka#
*Server authetication*
Serveris irodo savo tapatybe per savo *public key*

*Client authentication* 
Klientas irodo, kad turi *private key*, serveriui naudojant atitikama *public key*

*known_hosts*
Saugo serveriu *public keys*, kuriais pasitiki klientas

*authorized_key* 
Saugo *client public key*, kuriais paisitiki serveris.

#Prisiminti#
<Private key → lieka pas tave>
<Public key  → gali būti dalinamas>
<known_hosts → serverių raktai kuriuos tu pasitvirtinai>
<authorized_keys → klientų raktai kuriems serveris leidžia prisijungti>

______________________________________________________________________________

### Digital signatures and Certificates # 

*Digital Signatures* 
Digital signatures yra kriptografinis mechanizmas, leidziantis patikrinti:
1. Authenticity - kas sukure ar pasirase dokumenta
2. Integrity - ar dokumentas nebuvo pakeistas
Digital signatures naudoja *asymmetric cryptography*

Procesas remiasi *public-private key pora*:
* Private key - naudojamas sukurti signature
* Public key - naudojamas signature patikrinti
Kadangi private keu turi tik savinnka, parasas irodo dokumento autoriu.
Daugelyje saliu *digital signatures turi tokia pacia teisine verte kaip fiziniai parasai* 

`Kaip veikia Digital SIgnature` 

Svarbu suprasti:*nepasirasomas visas dokumentas tiesiogiai*
Vietoj to naudojamas *HASH*

Procesas:
1. Dokumentas paverciamas *hash* naudojant hash funkcija (pvz:SHA-256)
2. Sis hash pasiroasomas su private key
3. Rezultatas yra digital signature

Taileidzia patikrinti:
* Integrity
* Authenticity

`Dazna supaprastinta interpretacija`
*Digital signature = encryptinti dokumenta su private key*

Taciau tai nera tikslu:
*encryptinamas dokumento hash su private key* 
Tai yra:
* Daug efektyviau
* standartinis metodas realiose sistemose

# Certificates # 
Tai yra viena svarbiausiu *public key cryptography aplikaciju*
Ju paskirtis:
*Susieti public key su konkrecia oranizacija ar serveriu*
Be certiciates nebutu galima patikimai zinoti:
*Kam priklauso public key*

# Certificates naudojimas HTTP # 

Kai atidarai svetaine:
*https://example.com*
Tavo narsykle turi buti tikra, kad kalba su tikru serveriu.

Serveris pateikia *TLS certificate*, kuriame yra:
* serverio domain name
* serverio public key
* issuer (kas pasirase certificate)
* validity period 
* digital signature

# Certificate Authorities (CA) # 
Certifikatus pasiraso *certificite authorities*
Tai organizacijos, kurios patvirtina serveriu identiteta.
PVZ:
* DigiCert
* GlobalSign
* Let`s encrypt
* Sectigo

Operacines sistemos ir narsykles turi *is anksto idiegta patikimu Root CA sarasa*

# Chain of Trust # 
Certificates veikia per *pasitikejimo grandine*
Grandine atrodo taip:
<Server Certificate>
      ↓
<Intermediate CA>
      ↓
<Root CA>

# ROOT CA # 
Tai yra auksciausias pastikejimo lygis. 
Root CA certificate yra:
*self-signed*
Tai reiskia jis pasiraso pats save.
Kadangi operacine sistema ir narsykle jau *pasitiki root ca* ji pasitiki ir visais certificates, kuriuos pasiraso si grandine

# Certificates Verification procesas # 
Kai narsykle jungiasi prie HTTPS serverio:
* serveris pateikia certificate
* narsykle patikrina kas ji pasirase
* narsykle patikrina ar issuer yra patikimas CA
* narsykle patikrina certificate galiojimo laika
* narsykle patikrina ar domain name sutampa

Jei visa grandine galioja - narsykle pasitiki serveriu

`Kas nutinka jei certificate negalioja` 
Narsykle rodo HTTPS warning
Tai gali reiskti:
* certificate expired 
* certificate self-signed
* domain mismatch
* certificate chain invalid

tokiu atveju rysys laikomas *nepatikimu*

# Santrauka# 
*Digital signature*
* private key - sukuria signature
* public key - patikrina signature
Uztikrina:
* Authenticity
* Integrity

# Certificates # 
Certificates susieja:
*Public keu -- serverio ar organizacijos identiteta*

# Chain of trust # 
Pasitikejimas atsiranda per:
*Rot CA - Intermediate CA - Server certificates*
Jei Root CA yra patikima, narsykle pasitiki visa certificate grandine

# Kritine pastaba# 

Certificates *neifruoja duomenu* 
Jie naudojami
* Identito patikrinimui
O realu duomenu sifravima TLS rysyje daro:
* Symmetric encrypton (PVZ:AES)
_______________________________________________________________________________

### PGP and GPG ### 

# What is PGP # 
PGP ( Pretty good privacy)  yra kriptografine sistema, naudojama:
* Failu sifravimui 
* zinuciu sifravimui
* digital signatures kurimui
* duomenu intergalumo patikrinimui

PGP remiasi *public key cryptography*
Tai leidzia:
* encryptinti duomenis naudojant gavejo public key
* decryptinti naudojant private key
* pasirasyti duomenis naudojant private key
* patikrinti parasa naudojant public key

# OpenPGP # 

PGP siandien dazniausiai reiskia *OpenPGP standarta*
OpenPGP yra kriptografinis standartas, kuris apibrezia:
* Encryption metodus
* key formatus
* signature mechanizmus
* key exange prcesus

# GPG #
GnuPG (GPG) yra *open-source programine OpenPGP standarto implementacija*
Trumpai:
* OpenPGP - standartas 
* GPG - programine implementacija
* PGP - istorinis pavadinimas / sistema

# Kur naudojamas GPGP # 
Naudojama:
* Email encryption
* file encryption
* digital signatures
* software package signing 
* git comit signing
PVZ:
* Linux package repositories
* software release
* secure email communication

# GPG Encryption procesas
PGP/GPG nenaudoja tik asymmetric encryption
Jis naudoja *hybrid encryption modeli*
Procesas:
1. Sugeneruojamas *random symmetric session key*
2. Zinute arba failas sifruoja naudojant:
	`symmetric encryption`
	PVZ:AES
3. Session key uzsifruojamas naudojant:
	`receiver public key`
4. Gaveja:
	`naudoja private key - decryption session key
	`naudoja session key - decrypt message

# Kodel naudojamas hybrid encryption # 
Assymetric encryption yra:
*labai letas*
TOdel jis naudojamas tik:
*session key apsaugai*
O realus duomenu sifravimas vyksta naudojant:
*symmetric encryption* 

# DIgital Signatures su GPG # 
GPGP taip pat gali kurti *digital signatures*
Procesas:
1. Sukuriamas dokumento hash
2. Hash pasirasomas su *private key*
3. Gavejas patikrina signature naudodamas *public key*
Tai leidzia patikrinti:
* authenticity 
* integrity

# Private key protection # 
PGP/GPG private keys gali buti apsaugoti *passphrase* 
Passphrase:
*uzsifruoja private key lokaliai*
Svarbu:
*passphrase niekada nera siunciama tinklu*
Ji naudojama tik:
*Lokaliai decryptinti private key*

# CTF kontekstas * 
CTF uzduotyse daznai reikia:
*Decryptint GPGP ecrypted failus*
Kartais pateikiamas:
*private key*
*encrypted file*
Jei private kei apsuagotas passphrase, galima bandyti ji nulauzti naudojant:
*John the ripper*
*gpg2john*
Siame pvz:
*private key nera apsaugotas passphrase*
todel ji galima naudoti tiesiogiai

# Sauntrauka #

PGP
*pretty goof privacy*
*kriptografine sistema*

OpenPGP
*kriptografinis standartas*

GPG
*open-source OpenGPG implementacija*

# ENcryption modelis # 
*Hybrid encryption*
naudojama:
*symmetric encryption -- duomenims*
*assymetric encryption - session key apsaugai*

# Private key # 
*Gali buti apsaugotas passphrase*

_______________________________________________________________________________

### Practical Example (GPG) ###

Kai turi sugeneruota *GPGkey pair* gali dalintis savo public key su kitai zmonemis
Kai tavo kontaktai nori tau isiusti suagia zinute, jie:
*Naudoja tabo public key - encryptinti zinute* 
Kadangi tik tu turi private key, tik tu gali:
*Decryptint zinute*

# Backup svarba # 
Kadangi GPG keys yra labai svarbus reikia butinai turetu ju backup kopija saugioje vietioje.
Jei prarasi private key:
*Negalesi decryptinti anksciau gautu zinuciu*

# Key import (naujas PC)
Jei persikeli i nauja pc, gali importuoti savo key is backup.
Tam naudojama CMD:
<gpg --import backup.key>
Si komanda importuoja rakta i GPG keyring, kad ji butu galima naudoti.

# Message Decryption # 
Kai gauni uzsifruota faila,pvz:
*confidential_message.gpg*
ji gali decryptint nauodamas:
<gpgp --decrypt confidential_message.gpg>
GPG tada:
1. Naudoja tavo private key
2. Papraso passphrase ( jei ky apsaugota)
3. Decryptina zinute.

# Conclusion # 
* Cryptography yra mokslas apie metodus, skirtus apsaugoti komunikacija ir duomenis naudojant matematinius algoritmus

* Cryptanalysis yra mokslas, kuris tiria metodus, kaip sulauzyti arba apeiti kriptografines sistemas

Vienas paprasciausiu budu bandyti sulauzyti apsauga yra bandyti ivairius slaptazodzius arba raktus, kol randamas teisingas.

# KEy definitions # 
*Cryptography*
Cryptography – mokslas apie komunikacijos ir duomenų apsaugą naudojant šifrus, kodus ir kriptografinius algoritmus.
*Cryptanalysis*
Cryptanalysis – metodų tyrimas, kaip sulaužyti arba apeiti kriptografines sistemas nežinant slapto rakto.
*Brute-Force Attack*
Brute-Force Attack – metodas, kai bandomos visos galimos rakto arba slaptažodžio kombinacijos.
*Dictionary Attack*
Dictionary Attack – metodas, kai bandomi žodžiai iš žodyno arba jų kombinacijos, vietoj visų galimų simbolių kombinacijų.


_______________________________________________________________________________________
## HASHING ###

1. Kas yra hashing? 
Hashing yra procesas, kuris paima bet kokio dydzio duomenis ir pavericia juos i fiksuoto dydzio reiksme, vadinama hash values arba digest. 

Schema:

Input --> Hash Functon --> Fixed-length hash

PVZ:

Password --> MD5 --> 5f4dcc3b5aa765d61d8327deb882cf99

Svarbu suprasti:
* Input gali buti bet kokio dydzio output visada yra fiksuoto dydzio.
* Hash dydis priklauso nuo algoritmo, ne nuo input dydzio. 
PVZ:
MD5->128bit
SHA1->160bit
SHA256->256bit

Net jei input yra:
1 simbolis
arba
1gb failas

HASH VISTIEK BUS TOKIO PAT DYDZIO


2. Hash Function

Hash funkcija yra matematine funkcija kuri:

* Paima input duomenis 
* Sugeneruoja fixed-lenght hash

Svarbios savybes:

Deterministic
* tas pats input -> Tas pats hash
PVZ:
hello → 2cf24dba5...
hello → 2cf24dba5...

Visada tas pats rezultatas. 

Avalanche effect:
* Jei pakeisi net viena bita, hash pasikeis visiskai

PVZ:

hello
hella

hash bus visiskai skirtingi.

Tai labai svarbus integrity checking.

* One way funckion

Hash funkcija yra negrazinama

Password -> Hash 

Bet:

hash -> Password

Atgauti neimanoma matematiskai.
hash → original input atgauti matematiškai neįmanoma.

Tačiau password galima rasti bandant spėti kandidatą:
candidate password → hash → compare
Password cracking vyksta ne atsukant hash o spejant password ir hashinant ji. 


3. Hash encoding 

Hash rezultatas daznaiausiai yra raw bytes. 

