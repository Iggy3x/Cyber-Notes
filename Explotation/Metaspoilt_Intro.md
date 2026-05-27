### Metaspoilt ###

Metaspoilt leidzia isnaudoti pazeidziamumus target sistemose. Jis gali buti naudojamas *exploit,scanning* ir kai kurioms kitoms uzduotims,pvz brute-force atakoms, priklausomao nu naudojamo modulio. 

# Pagrindinis konceptas #

*Exploit*
Tai yra kodo gabalas arba modulis, kuris isnaudoja konnkrecia target sistemos vuln. 

*Vulnerability*
Tai yra silpna vieta target sistemoje.Tai gali buti dizaino klaida, programavimo klaida arba logine klaida. Isnaudojus vuln galima:
* atskleisti konfidenciala informacija
* vykdyti koda target sistemoje
* gauti neautorizuota prieiga
* Leisti attackeriui vykdyti koda target sistemoje

*Payload*
Exploit isnaudoja vuln, bet vien to neuztenka, jei norime konkretaus rezultato. Payload yra tai, kas bus paleista target sistemoje po sekmingo exploitinimo. Payload leidzia pasiekti norima efekta,pvz:
* gauti priega prie sistemos
* skaityti konfidencialius duomenis
* vykdyti komandas targete

# Trumpa logika # 
* Vulnerability = silpna vieta
* Exploit = budas ta silpna vieta isnaudoti
* Payload = rezultatas arba veiksmas po sekmingo isnaudojimo

____________________________________________________________________________

### Main Components of Metasploit ###

Naudoajant Metaspoilt Framework, pagrindinis irankis bus console. Ja galima paleisti AttackBox su komanda:
<msfconsole> 
Console yra pagrindine sasaja, per kuria bendrauti su skirtingais Metaspoilt moduliais.

*Modules* tai metasploit framework komponentai, skirti konkreciai uzduociai atlikti. Pvz:
* Isnaudoti vulnerability
* scaninti target
* atlikti kai kurias brute-force uzduotis

Pries einant prie moduliu, svarbu suprasti tris pagrindinis terminus:

*Exploit*:
Kodo gabala, kuris isnaudoja target sistemoje esancius vulnerability.

*Vulnerability*:
Dizaino, programavimo arba logine klaida, veikianti target sistema. Isnaudojus vulnerability galima atskleisti konfidencialia informacija arba leisti vykdyti koda target sistemoje.

*Payload*:
Eploit isnaudoja vulnarability, o payload yra kodas,kuris bus paleistas target sistemoje po sekmingo exploitinimo

# Pagrindines moduliu kategorijos #

*Auxiliary* 
Cia randami pagalbiniai moduliai tokie kaip:
* scaneriai 
* crawleriai 
* fuzzeriai

Trumpai: Auxiliary moduliai naudojami pagalbinems uzduotisms, ne tiesioginiam exploitinimui

*Encoders*
Encoders leidzia uzkoduiti exploit arba payload, tikintis, kad singature-based antivirus spendimai jo neatpazins.
Taciau jus sekme gali buti ribota. 

*Evasion*
Sie moduliai labiau skirti bandyt apeiti antivirus ar kitas apsaugos priemones.

*Exploits* 
Sioje kategorijoje yra explot moduliai, suskirstyti pagal target sistema pvz:
* Linux 
* Windows
* Android
* Unix

Trumpai: *exploit moduliai skirti isnaudoti target sistemos vulnerabilities

*NOPs* 
Reiskia *no operation* instrukcija kuri nieko nedaro. 
Daznai naudojami kaip pagalbinis komponentas exploit kurime.

*Payloads*
Kodas, kuris bus paleistas target sistemoje

Exploit isnaudoja vulnerability, bet payload nusako koks bus rezultatas.
Pvz:
* Gauti shell
* paleisto komanda
* paleisti <calc.exe> kaip proof of concept

# Payload tipai #
Metaspoilt payload kataloge matysi keturias pagrindines grupes:

*Adapters* 
Apgaubia single payload ir pavercia ji kitu formatu

*Singles* 
Tai savarankiski payload kuriesm nereikia papildomai atsiusti kito komponento

*Stagers*
Tai savarankiski payloadai kuriesm nereikia papildomai atsiusti kito komponento

*Stages* 
Tai yra antra payload dalis kuri atsiunciama po stagerio. Tai leidzia naudoti didesnius payloadus

# Single ir staged payload skirtumas #

*Single/inline payload*:
<generic/shell_reverse_tcp>

*stage payload*
<winodws/x64/shell/reverse_tcp>

Metaspoilt pavadinimuose:
* single payload daznai atpazistamas is < _ > 
* staged payload daznai atpazystamas is < / >

Trumpai:
* Single=viskas viename
* staged=pirma maza dalis, po to likes payload

# Post # 
Post moduliai naudojami jau po sekmingo exploitation etapo t.y. post-explotation stadijoje.
Jie naudingi tolimesniems veiksmams po pradines priegos gavimo.

# Trupa logika # 
* Auxiliary = pagalbiniai moduliai
* Encoders = koduoja exploit arba payload
* Evasion = bando apeiti apsaugas
* Exploits = išnaudoja vulnerabilities
* NOPs = pagalbinės „nieko nedarančios“ instrukcijos
* Payloads = kodas, kuris vykdomas targete
* Post = moduliai po sėkmingo exploitinimo

_____________________________________________________________________________

### MSFCONSOLE ###

# Using msfconsole # 
Console bus pagrindine sasaja su darb su *Metaspoilt Framework* Ja galima paleisti naudojant komanda:
<msfconsole>

Tai galima darytis tiek AttackBox terminale, tiek bet kurioje sistemoje, kur idiegtas Metaspoilt Framework
Paleidus komanda, komandine eilute pasikeis i <msf6> <msf5> priklauso nuo versijos.

<msfconsole> gali buti naudojama panasiai kaip iprastas command-line shell. Pvz, joje galima vygdyti tokias komandas:
* ls
* ping
* clear

Bet nepalaiko visu iprasto shell funkciju. Pvz: output redirection neveikia

# Help ir history #
Komanda <help> gali buti naudjama viena pati arba su konkrecia komanda.
Pvz:
* Help
* help set
Ji parodo bendra pagalaba arba konkrecios komandos naudojima.
Komanda <history> leidzia perziureti anksciau ivestas komandas.
Taip pat <msfconsole> palaiko *tab completion*, todel pradejus rasyti komanda ir paspaudus *tab*, ji gali buti automatiskai uzbaigta. 

# Context # 

Vienas svarbiausiu <msfconsole> bruozu yra *context*

Tai reiskia, kad pasirinkus konkretu moduli su komanda <use> pradedi dirbti to modulio kontekste. Visi tame modulyje nustatyti parametrai lieka tik tam konktekste, nebent jie butu nustatyti globaliai.

Jei pereisi i kita moduli, parametrus, tokius kaip:
* `RHOSTS`
* `RPORT`
* `LHOST`
* `LPORT`
Dazniausiai reikes nustatyti is naujo.
Svarbu:Tai nera tas pats kas eiti i kataloga. Propt pasiketimus tiesio rodo, kad dabar dirbi konkretus modulio konktekste.

# Use # 
Komanda <use> naudojama pasirinkti moduli
Pvz:
< use exploit/windows/smb/ms17_010_eternalblue >

Pasirinkus moduli, prompt pasikeis is:
< msf6 > >
I:
< msf6 exploit(windows/smb/ms17_010_eternalblue) > >
Tai rodo, kad dabar dirbi to modulio kontekste.
Moduli taip pat galima pasirinkti naudojant jo numeri is <search> rezultatu.

# Show options #

Komanda <show options> parodo pasirinkti maudlio paramaterus.
Ji leidzia matyti:
* Kokie parametrai reikalingi
* kokie jau turi reiksmes
* kurie yra provalomi

Skirtingi moduliai tures skirtingus parametrus.
Pvz exploit modulis gali reikalautiL
* <RHOSTS>
* <RPORT>
o post modulis gali reikalauti:
* session
Tai priklausi nuo modulio tipo ir jo paskirties.

# Show payloads # 
<show payloads> parodo payloadus kurie yra suderinami su pasirinktu exploit modulio.
Jei si komanda naudojama modulio kontekste, parodys tik tam moduliui tinkamus payloadus

# Back # 
<back> leidzia iseiti is dabartinioo modulio konkteksto ir grizti i pagrindini <msfconsole> promt

# Info #
< Info > pateikia detalia info apie moduli:
ji gali parodyti:
* modulio pavadinima
* aprasyma 
* autorius
* nuorodas
* palaikomus targetus
* reikalingus parametrus

Svarbu:
* <help> paaiskina komanda
* <info> paaiskina moduli
<info> galima naudoti tiek modulio kontekste, tiek is pagrindinio promt nurodant pilna modulio kelia. 

# search # 
 Viena naudngiausiu komandu yra <search> 
Ji leidzia ieskoti moduliu pagal:
* CVE numeri
* exploit pavadinima
* target sistema
* kitus raktazodzius
PVZ:
<search ms17-010>
<search> rezultatai pateikia info apie:
* modulio tipa
* modulio pavadinima
* data
* rank
* trumpa aprasyma
Moduli galima pasirinkti:
* pilnu pavadinimu
* arba jo numeriu is paieskos rezultatu
<search> taip pat galima tikslinti naudonant filtrus pvz:
* <type:auxiliary>
* <Platform:windows>
PVZ:
<search type:auxiliary telnet> 

# Rank #

<search> rezultatuose matomas *rank* stulpelis nurodo exploito modulio patikimuma

Taciau svarbu suprasti:
* Zemo rango exploit gali veikti puikiai
* auktos rank exploit gali nesuveikti
* kai kurie exploit gali elgtis netiketai ar net crashinti target sistema

Todel rank yra naudinga nurododa, bet ne garantija

# Trumpa logika # 
* msfconsole = pagrindinė Metasploit sąsaja
* help = komandos pagalba
* history = anksčiau įvestos komandos
* use = pasirinkti modulį
* show options = parodyti modulio parametrus
* show payloads = parodyti suderinamus payload’us
* info = detali informacija apie modulį
* search = ieškoti modulių
* back = grįžti iš modulio konteksto

________________________________________________________________________

### Working with modules ###

# Setting parameters and using modules # 
Kai pasirenki modui su komanda <use> , pereni i to modulio *context* . tada reikia nustatyti parametrus, kuriu tam moduliui reikia. 
Dazniausiai naudojami parametrai yra:
* RHOSTS
* RPORT
* PAYLOAD
* LHOST
* LPORT
* SESSION

Taciau priklausomai nuo modulio gali reiketi ir kitu parametru.
Todel gera praktika visada naudoti:
< show options > 
Si komanda parodys, kokie parametrai yra galimi kurie yra privalomi ir kokios ju dabartines reiksmes

# Parametru nustatymas # 

Visi parametrai nustatomi ta pacia sintakse:
<set PARAMETER_NAME VALUE>
Pvz:
<set RHOSTS 10.10.165.39>

Nustacius parametra, verta dar karta paleisti <show options>, kad patikrintum, ar reiksme tikrai buvo nustatyta teisingai.

# Svarbu tikrinti promt # 
Pries nustatant parametrus ar paleidziant moduli, visada verta pasiziureti i <msfconsole> promt, kad isitikintum, jog esi teisingame context

Dirbant su Metasploit galima matyti kelis skirtingus promt tipus:
1. Iprastas command prompt:
<root@ip-10-10-XX-XX:~#>
Cia Metaspoilt komandu naudoti negali, nes tai paprastas sistemos terminalas.

2. msfconsole promt
Pvz:
<msf6> 
Cia esi <msfconsole>, bet dar nesi pasirinkes konkretau modulio, todel context-specific komandos da nera naudojamos modulio lygyje.

3. Modulio contect promt
<msf6 exploit(windows/smb/ms17_010_eternalblue) >>
CIa jau dirbi konretaus modulio kontekste ir gali naudoti tokias komandas kaip:
<set RHOSTS 10.10.x.x> 

4. Meterpreter Promt
<meterpreter >>
Tai reiskia kad Meterpepreter payload buvo sekmingai paleistas sistemoje ir prisijunge atgal. Cia galima naudoti Meterpreter komandas. 

5. Shell promt target sistemoje
<C:\Windows\system32>>
tai paprastas comman shell target sistemoje. Viska ka cia rasai, vygdoma target kompiuteryje

# Dazniausiai naudojami parametrai # 

*RHOSTS*
"Remote hosts" - target sistemos IP adresas arba adresai.

Galima naudoti:
* Viena IP 
* tinklo range
* CIDR notaija, pvz </24>
* faila su target sarasu

*RPORT* 
"Remote port" - portas target sistemoje, kuriame veikia pazeidziama aplikacija ar servisas.

*PAYLOAD* 
Kuris bus naudojamas kartu su exploit moduliu. 

*LHOST* 
"Local Host" - tavo atakuojancios masinos IP adresas
* AttackBox IP
* Kali LInux IP

*LPORT*
"Local Port"  - portas tavo masinoje, i kuri jungsis reverse shell ar payload.
Jis turi buti laisbas t..y. nenaudojamas kitos programos.

# Session # 

*set, unset, unset all* 
Jei nori pakeisti parametro reiksme, tiesiog dar karta naudok < set > su nauja reiksme. 
Jei nori isvalyti parametra:
<unset PARAMETER_NAME>
Jei nori isvalyti visus parametrus tam moduli:
<unset all>

*setg ir unsetg*
Komanda <setg> naudojama nustatyti *globalia reiksme*, kuri galios skirtingiems moduliamds.
Pvz:Jei <RHOSTS> nustatysi su <setg>, tada pereus i kita moduli si reiksme gali jau buti automatiskai uzpildyta.
Tai patogu, kai keli moduliai dirba pries ta pati targeta.
Globalias reiksmes isvalyti:
<unsetg PARAMETER_NAME>

Trumpai:
* <set> = reiksme tik dabartiniam moduliui
* <setg> = reiksme globaliai per kelis modulius 

# Using modules# 

Kai visi reikalingi parametrai nustatyt, moduli galima paleisti su komanda:
<exploit>
Metaspoilt palaiko komanda:
<run> 

<run> yra alias <exploit> komandai. Tai naudinga todel:kad zodis <exploit> ne visada tinka moduliams, kurie nera exploitai pvz scanneriai

*exploit -z*
CMD:
<exploit -z>
paleidzia exploita ir jei sesija sekmingai atsidaro is karto ja *backgroundina*
Tai reiskia kad tu gristi i modulio context promt, o seija liks vykti fone.

*check* 
Kai kurie moduliai palaiko komanda:
<check>
Ji leidzia patikrinti, ar target sistema yra pazeidziama. neatlikti pilno *exploitinimo*
Tai naudnga, kai nori pirma patikrinti targeta pries bandydamas exploit.

*session*
Kai exploit vyksta, sukuriama session.
Tai komunikacijos kanlas tarp target sistemos ir Metaspoilt.

*background* 
Jei esi sesijoje, gali ja perkelti i fona su cmd:
<background> 
arba
<ctrl+z>
Tai grazina i <msfconsole>

*sessions*
CMD:
<sessions>
parodo visas siuo metu aktyvias sesijas.
Ja galima naudoti tiek <msfconsole> promte, tiek modulio kontekste

*session -i*
Prisijungti prie konkrecios sesijos:
<sessions -i SESSION_ID>
Pvz:
<sessions -i 2>
Tai prades darba su pasirinkta sesija

*Trumpa logika*
* use = pasirenki modulį
* show options = pasižiūri, kokių parametrų reikia
* set = nustatai parametrus moduliui
* setg = nustatai parametrus globaliai
* exploit / run = paleidi modulį
* check = patikrini pažeidžiamumą, jei modulis tai palaiko
* sessions = matai aktyvias sesijas
* sessions -i = prisijungi prie konkrečios sesijos
* background = nusiunti sesiją į foną

_________________________________________________________________________________
