# HASHING – FULL CONCEPT (THM ROOM) # 
1. Kas yra Hashing

Hashing yra procesas, kuris paima bet kokio dydžio duomenis ir paverčia juos į fiksuoto dydžio reikšmę, vadinamą hash value arba digest.

Schema:

Input data → Hash function → Fixed-length hash

Pvz:

password → MD5 → 
5f4dcc3b5aa765d61d8327deb882cf99
Svarbu suprasti:

input gali būti bet kokio dydžio
output visada yra fiksuoto dydžio
2. Hash Function

Hash funkcija yra matematinė funkcija kuri:

paima input duomenis
sugeneruoja fixed-length hash

Svarbios savybės:

Deterministic
tas pats input → tas pats hash

Pvz:

hello → 2cf24dba5...
hello → 2cf24dba5...

visada tas pats rezultatas.

Avalanche effect

Jei pakeisi net vieną bitą, hash pasikeis visiškai.

Pvz:

hello
hella

hash bus visiškai skirtingi.

Tai labai svarbu integrity checking.

One-way function

Hash funkcija yra negrįžtama.

password → hash

Bet:

hash → password

atgauti neįmanoma matematiškai.

Password cracking vyksta ne „atsukant hash“, o spėjant password ir hashinant jį.

3. Hash Encoding

Hash rezultatas dažniausiai yra raw bytes, todėl jis pateikiamas tekstiniu formatu.

Dažniausi formatai:

hexadecimal
base64

Pvz Linux komandos:

md5sum
sha1sum
sha256sum
sha512sum

Šios komandos rodo hash hexadecimal formatu.

Svarbu:

1 byte = 2 hex simboliai

Pvz:

MD5 = 128 bits = 16 bytes = 32 hex simboliai
4. Hash Collisions

Collision įvyksta kai:

du skirtingi input → tas pats hash

Pvz:

data1 → hash
data2 → hash

Hash funkcijos kuriamos taip, kad:

collision būtų labai sunku sukurti
5. Pigeonhole Principle

Jei:

input galimų reikšmių daugiau
nei hash reikšmių

tai collision matematiškai neišvengiama.

Pvz:

4-bit hash

galimi tik:

2^4 = 16 hash reikšmių

Bet input gali būti begalinis.

Todėl collisions teoriškai visada egzistuoja.

6. Insecure Hash Algorithms

Kai kurie hash algoritmai šiandien laikomi nesaugiais.

Pvz:

MD5
SHA1

Priežastys:

galima generuoti collisions
labai greiti → lengva brute-force

Todėl password saugojimui naudojami lėti hash algoritmai.

Pvz:

bcrypt
scrypt
yescrypt
7. Password Storage

Passwordai neturėtų būti saugomi plaintext.

Blogas pavyzdys:

username | password
john     | 123456

Teisingas metodas:

username | password_hash
john     | 5f4dcc3b5aa765...

Kai vartotojas loginasi:

input password → hash

ir lyginama su saugomu hash.

8. Password Salting

Salt yra atsitiktinė reikšmė kuri pridedama prie password prieš hashinant.

hash(password + salt)

Salt naudojamas tam, kad:

tas pats password → skirtingi hash

Pvz:

password + salt1 → hash1
password + salt2 → hash2

Tai apsaugo nuo:

rainbow tables
precomputed hashes
9. Password Hash Storage Linux

Linux password hash saugomi:

/etc/shadow

Šis failas yra:

readable tik root

Shadow file struktūra:

username:hash:...

Hash dalis gali atrodyti taip:

$prefix$options$salt$hash

Pvz:

$y$j9T$76UzfgEM5PnymhQ7TlJey1$/OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4
Prefix nurodo algoritmą

Pvz:

$y$ → yescrypt
$6$ → SHA512
$2b$ → bcrypt
$1$ → MD5

Tai leidžia greitai atpažinti hash tipą.

10. Windows Password Hashes

Windows naudoja:

NTLM hashes

Jie saugomi:

SAM database

Hash galima išgauti naudojant tam tikrus įrankius.

11. Password Cracking

Password cracking vyksta taip:

candidate password → hash → compare

Dažniausi metodai:

wordlist attack
brute-force attack
rule-based attack
12. Wordlists

Wordlist yra failas su galimais password.

Pvz:

rockyou.txt

Pvz:

123456
password
qwerty
dragon

Hashcat kiekvieną žodį:

hashina
lygina su target hash
13. Hashcat

Hashcat yra vienas greičiausių password cracking įrankių.

Basic syntax:

hashcat -m <hash_type> -a <attack_mode> hashfile wordlist

Pvz:

hashcat -m 3200 -a 0 hash.txt rockyou.txt
Parametrai
-m → hash algorithm
-a → attack type

Pvz:

-m 0 → MD5
-m 1000 → NTLM
-m 3200 → bcrypt
14. GPU vs CPU Cracking

Password cracking gali būti daromas:

CPU
GPU

GPU dažnai yra daug greitesnis, nes gali atlikti daug operacijų vienu metu.

Tačiau algoritmai kaip:

bcrypt
scrypt
yescrypt

specialiai sukurti būti lėti ir memory intensive.

15. Cracking on Virtual Machines

VM dažnai neturi prieigos prie GPU.

Todėl:

hashcat VM aplinkoje gali būti lėtesnis

Dažnai rekomenduojama:

run hashcat on host OS
16. Hashing for Integrity Checking

Hash gali būti naudojamas patikrinti:

ar failas buvo pakeistas

Pvz:

sha256sum filename

Jei hash sutampa su originalu:

failas nebuvo modifikuotas
17. Duplicate File Detection

Hash taip pat leidžia rasti:

duplicate files

Jei du failai turi tą patį hash:

jie tikėtina identiški
18. HMAC

HMAC reiškia:

Hash-based Message Authentication Code

Jis naudojamas užtikrinti:

authenticity
integrity

HMAC naudoja:

hash function + secret key

Tai leidžia patvirtinti:

kas sukūrė žinutę
ir kad ji nebuvo pakeista
19. Encoding vs Hashing

Svarbu nepainioti:

Hashing
one-way process
Encryption
reversible su key
Encoding
data format conversion
20. Base64 Encoding

Base64 naudojamas duomenims pateikti tekstiniu formatu.

Pvz:

RU5jb2RlREVjb2RlCg==

Dekodavimas:

base64 -d

Svarbu:

Base64 nėra encryption
Base64 nėra hashing

Tai tik encoding.

FINAL SUMMARY

Hashing naudojamas:

password storage
file integrity verification
authentication systems
duplicate detection

Svarbiausios savybės:

fixed-length output
deterministic
avalanche effect
one-way function
