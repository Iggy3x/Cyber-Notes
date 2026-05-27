# THM – Windows Fundamentals 1–3 + Active Directory Basics

## 1. Room esmė
Šitie roomai yra Windows pagrindų blokas.
Pagrindinė mintis:
> Jeigu Linux yra terminalo/servų stuburas, tai Windows yra corporate pasaulio kasdienybė: users, permissions, services, logs, security tools, domainai ir Active Directory.
Šitas notes apjungia:
- Windows editions
- desktop / GUI
- NTFS file system
- System32
- users / groups / profiles
- UAC
- Settings / Control Panel
- Task Manager
- System Configuration
- Computer Management
- Event Viewer
- Resource Monitor
- Command Prompt
- Registry
- Windows Security
- Defender / Firewall
- BitLocker
- Volume Shadow Copy
- Active Directory basics
- Domains
- Domain Controllers
- OUs
- Users / Groups / Computers
- Group Policy
- Forests / Trees / Trusts
Cybersecurity kontekste Windows būtinas, nes:
- daug įmonių naudoja Windows endpoints
- daug corporate identity sukasi aplink Active Directory
- daug atakų taikosi į Windows credentials, permissions, services ir AD
- blue team/SOC dažnai analizuoja Windows logs
- IT Support realiai beveik kasdien dirba su Windows
---

## 2. Windows kaip corporate OS
Windows yra dominuojanti OS home ir corporate aplinkose.
Paprastai:
> Windows = pagrindinė userių darbo aplinka daugumoje įmonių.
Windows naudojamas:
- user laptops/desktops
- domain joined machines
- file servers
- print servers
- application servers
- Active Directory Domain Controllers
- RDP administration
- enterprise environments
Kodėl security mokymuisi svarbu:
- user accounts
- permissions
- malware target
- logs
- endpoint security
- registry
- services
- domain authentication
- lateral movement
- privilege escalation
Svarbi mintis:
> Jeigu nori dirbti IT Support, SOC ar vėliau pentesting, Windows negali būti “ai ten GUI, px”. Corporate realybėje Windows yra didelė kovos lauko dalis.
---

## 3. Windows Editions
Windows turi skirtingas editions.
Pavyzdžiai:
- Home
- Pro
- Enterprise
- Education
- Server editions
Skirtumas:
- Home labiau namų useriams
- Pro turi daugiau business/security features
- Enterprise skirta didelėms organizacijoms
- Server skirta server roles
Svarbus Pro/Enterprise feature:
> BitLocker.
BitLocker leidžia šifruoti diską.
Security mintis:
> Windows edition lemia, kokias security ir admin funkcijas turi sistema.
Pvz IT Support situacijoje svarbu žinoti, ar userio machine yra Home ar Pro, nes kai kurios funkcijos gali neegzistuoti.
---

## 4. Windows Desktop / GUI
Windows Desktop yra pagrindinė grafinė aplinka po login.
GUI reiškia:
> Graphical User Interface
Desktop komponentai:
- Desktop icons
- Start Menu
- Search box
- Task View
- Taskbar
- Notification Area
- System tray
- Settings shortcuts
Start Menu:
> greita prieiga prie apps, settings, power options, user account actions.
Taskbar:
> rodo atidarytas programas ir pinned apps.
Notification Area:
> rodo laiką, network, volume, security icons ir kitus background statusus.
Svarbi IT mintis:
> GUI yra patogu, bet daug Windows admin/security darbo vyksta per tools, MMC consoles, PowerShell, cmd ir logs.
---

## 5. File Explorer
File Explorer naudojamas naršyti failus ir folderius.
Dažni keliai:
```txt
C:\
C:\Users
C:\Windows
C:\Windows\System32
C:\Program Files
C:\Program Files (x86)
```
User profile:
```txt
C:\Users\Username
```
Dažni user folderiai:
- Desktop
- Documents
- Downloads
- Pictures
- Music
- Videos
Svarbu:
> `C:\Users\Username` yra panašu į Linux `/home/username`.
Security/forensics mintis:
> Downloads, Desktop, AppData ir Documents dažnai turi user activity pėdsakų, downloaded files, scripts, configs ar malware artifacts.
---

## 6. NTFS
NTFS reiškia:
> New Technology File System
Tai modernus Windows filesystem.
NTFS palaiko:
- didelius failus
- file/folder permissions
- journaling
- compression
- encryption per EFS
- Alternate Data Streams
Paprastai:
> NTFS = Windows filesystem su permissions ir papildomomis security funkcijomis.
NTFS permissions:
- Full control
- Modify
- Read & Execute
- List folder contents
- Read
- Write
Security mintis:
> Windows file access labai daug remiasi NTFS permissions.
Jeigu permissions blogai sukonfigūruotos:
- user gali skaityti sensitive data
- user gali modifikuoti files
- service gali būti hijackintas
- gali atsirasti privilege escalation keliai
---

## 7. NTFS permissions
Permissions nustato, kas gali ką daryti su failu/folderiu.
Pagrindinės reikšmės:
- Read
- Write
- Modify
- Read & Execute
- Full Control
Failo/folderio permissions žiūrimos:
```txt
Right click -> Properties -> Security tab
```
Svarbi mintis:
> Windows permissions dažnai paveldimos iš parent folderio.
Inheritance reiškia:
> child files/folders paveldi permissions iš parent folderio.
Security požiūriu reikia tikrinti:
- ar user turi daugiau teisių nei reikia
- ar Everyone/Users turi Write ten, kur neturėtų
- ar service folderiai world-writable
- ar sensitive data readable by normal users
---

## 8. Alternate Data Streams
ADS reiškia:
> Alternate Data Streams
ADS yra NTFS funkcija, leidžianti failui turėti papildomus data streams.
Paprastai:
> ADS = papildoma paslėpta data prie failo, kurios File Explorer normaliai nerodo.
Security mintis:
> Malware kartais naudojo ADS slėpti data.
Ne visi ADS yra malicious.
Pvz Windows gali naudoti ADS pažymėti, kad failas atsisiųstas iš interneto.
PowerShell pavyzdys:
```powershell
Get-Item -Path .\file.txt -Stream *
```
Svarbu:
> ADS nėra kasdienis beginner dalykas, bet žinoti verta, nes tai Windows-specific filesystem feature.
---

## 9. C:\Windows ir System32
Windows folderis:
```txt
C:\Windows
```
System32:
```txt
C:\Windows\System32
```
System32 turi daug svarbių OS files ir admin utilities.
Svarbi mintis:
> System32 nėra joke folderis. Ten gyvena daug kritinių Windows komponentų.
Neliesti aklai:
```txt
C:\Windows\System32
```
Environment variable:
```txt
%windir%
```
Dažnai rodo į:
```txt
C:\Windows
```
Security/IT mintis:
> Daug Windows tools gyvena System32. Jei komanda veikia iš Run/cmd, labai dažnai ji yra System32 arba PATH vietoje.
---

## 10. Environment Variables
Environment variables saugo OS/aplinkos informaciją.
Pavyzdžiai:
```txt
%windir%
%USERPROFILE%
%PATH%
%TEMP%
```
Patikrinti cmd:
```cmd
echo %windir%
echo %USERPROFILE%
echo %PATH%
```
PowerShell:
```powershell
$env:windir
$env:USERPROFILE
$env:PATH
```
Svarbu:
> PATH nurodo, kur sistema ieško executable komandų.
Security mintis:
> PATH hijacking arba blogos environment configs gali sukelti command execution/security problemų.
---

## 11. User accounts
Windows useriai gali būti local arba domain.
Local user:
> egzistuoja tik konkrečiame kompiuteryje.
Domain user:
> egzistuoja Active Directory domain’e ir gali būti naudojamas domain-joined machines.
Local account types:
- Administrator
- Standard User
Administrator:
- gali keisti system settings
- install software
- manage users/groups
- keisti security settings
Standard User:
- ribotos teisės
- negali normaliai keisti system-level settings
Svarbi mintis:
> Least privilege reiškia, kad useriui duodama tik tiek teisių, kiek būtina.
---

## 12. User profiles
Kai useris pirmą kartą prisijungia, Windows sukuria profile.
Kelias:
```txt
C:\Users\Username
```
Profile turi:
- Desktop
- Documents
- Downloads
- AppData
- Pictures
- Music
- Videos
Svarbus folderis:
```txt
C:\Users\Username\AppData
```
AppData dažnai turi:
- app configs
- browser data
- temp files
- tokens/cache
- user-specific program data
Security/forensics mintis:
> AppData yra labai svarbi vieta incident response ir malware analizėje.
---

## 13. Local Users and Groups
Atidaryti:
```txt
Run -> lusrmgr.msc
```
Arba:
```cmd
lusrmgr.msc
```
Čia galima valdyti:
- local users
- local groups
Svarbios local groups:
- Administrators
- Users
- Remote Desktop Users
- Backup Operators
- Event Log Readers
- Power Users
Svarbu:
> Useris paveldi teises iš grupių.
Security mintis:
> Tikrinti group membership yra būtina. Dažnai privilege klausimas yra ne useris pats, o kokiai grupei jis priklauso.
---

## 14. UAC
UAC reiškia:
> User Account Control
Paprastai:
> UAC = Windows mechanizmas, kuris prašo patvirtinimo prieš elevated/admin veiksmus.
UAC padeda:
- sumažinti accidental admin changes
- priversti patvirtinti privilege elevation
- atskirti normal user veiksmus nuo admin veiksmų
Svarbu:
> UAC nėra pilna security boundary kaip firewall. Tai apsauginis mechanizmas nuo neapgalvotų/elevated veiksmų.
Security mintis:
> Malware dažnai bando apeiti UAC arba priversti userį patvirtinti elevation.
---

## 15. Settings ir Control Panel
Windows turi dvi pagrindines nustatymų vietas:
- Settings
- Control Panel
Settings:
> modernesnė Windows nustatymų sąsaja.
Control Panel:
> senesnė, bet vis dar svarbi administravimo sąsaja.
Dažni nustatymai:
- Network
- Users
- Updates
- Apps
- Firewall
- Devices
- Power options
- System info
Svarbu:
> Windows turi daug dubliuotų kelių į tą patį nustatymą. Nes Microsoft mėgsta palikti senus rūsius po nauju fasadu.
---

## 16. Task Manager
Atidaryti:
```txt
Ctrl + Shift + Esc
```
Arba:
```txt
Ctrl + Alt + Delete -> Task Manager
```
Task Manager rodo:
- running processes
- CPU/RAM/Disk/Network usage
- startup apps
- users
- services
- performance
Naudinga:
- rasti kas ryja CPU/RAM
- killinti frozen process
- pažiūrėti startup apps
- basic troubleshooting
Security mintis:
> Task Manager gali parodyti įtartinus procesus, bet advanced analizė reikalauja gilesnių tools kaip Process Explorer, autoruns, logs.
---

## 17. System Configuration
Atidaryti:
```cmd
msconfig
```
System Configuration leidžia matyti/keisti:
- boot options
- services
- startup-related settings
- tools shortcuts
Svarbu:
> `msconfig` labiau troubleshooting įrankis, ne kasdienė admin console.
Security/IT mintis:
> Galima tikrinti boot mode, services ir troubleshooting režimus.
---

## 18. Computer Management
Atidaryti:
```cmd
compmgmt.msc
```
Computer Management turi:
- System Tools
- Storage
- Services and Applications
System Tools:
- Task Scheduler
- Event Viewer
- Shared Folders
- Local Users and Groups
- Performance
- Device Manager
Storage:
- Disk Management
Services and Applications:
- Services
- WMI Control
Svarbu:
> Computer Management yra kaip Windows admin dashboard’as vienoje vietoje.
---

## 19. Task Scheduler
Task Scheduler leidžia automatiškai vykdyti tasks.
Atidaryti:
```cmd
taskschd.msc
```
Task gali veikti:
- pagal laiką
- startup metu
- login metu
- pagal event
- kas X minučių/valandų
Task gali paleisti:
- programą
- scriptą
- command
Security mintis:
> Scheduled Tasks yra labai svarbūs persistence mechanizmai. Malware ir adminai abu mėgsta automation.
Tikrinti:
```powershell
Get-ScheduledTask
```
---

## 20. Event Viewer
Atidaryti:
```cmd
eventvwr.msc
```
Event Viewer rodo Windows logs.
Svarbūs logs:
- Application
- Security
- System
- Setup
- Forwarded Events
Security log:
> login/logout, auth, privilege events, audit events.
System log:
> services, drivers, OS events.
Application log:
> apps klaidos/events.
Svarbūs event ID pavyzdžiai:
```txt
4624 = successful logon
4625 = failed logon
4634 = logoff
4688 = process creation, jei auditing įjungtas
7045 = service installed
```
Svarbi mintis:
> Blue team/SOC Windows pasaulyje gyvena Event Viewer/SIEM loguose.
---

## 21. Disk Management
Atidaryti:
```cmd
diskmgmt.msc
```
Naudojama:
- disks
- partitions
- volumes
- drive letters
- formatting
- resizing
Svarbu:
> Disk Management yra storage troubleshooting įrankis.
Security/forensics mintis:
> Disk layout, attached drives ir volumes gali būti svarbūs incident response metu.
---

## 22. Device Manager
Atidaryti:
```cmd
devmgmt.msc
```
Naudojama:
- devices
- drivers
- hardware status
- disable/enable devices
- update drivers
IT Support mintis:
> Device Manager labai svarbus troubleshooting: Wi-Fi, GPU, audio, USB, Bluetooth, unknown devices.
Security mintis:
> Driver issues gali būti stability problema, o malicious drivers/kernel components jau advanced security tema.
---

## 23. Services
Atidaryti:
```cmd
services.msc
```
Service = background component/programa.
Services turi:
- name
- status
- startup type
- logon account
Startup types:
- Automatic
- Automatic Delayed
- Manual
- Disabled
Svarbu:
> Services gali veikti kaip LocalSystem, NetworkService, LocalService arba custom user.
Security mintis:
> Blogos service permissions, writable service paths ar insecure service accounts gali virsti privilege escalation.
PowerShell:
```powershell
Get-Service
Start-Service Name
Stop-Service Name
Restart-Service Name
```
Cmd:
```cmd
sc query
sc qc ServiceName
```
---

## 24. Resource Monitor
Atidaryti:
```cmd
resmon
```
Resource Monitor rodo:
- CPU
- Memory
- Disk
- Network
Naudinga:
- matyti kurie procesai naudoja network
- disk activity
- memory usage
- CPU bottlenecks
IT Support mintis:
> Kai useris sako “kompas lėtas”, Resource Monitor gali parodyti, ar kalta RAM, diskas, CPU ar network.
---

## 25. System Information
Atidaryti:
```cmd
msinfo32
```
Rodo:
- OS info
- hardware info
- system summary
- components
- software environment
Naudinga:
- hardware specs
- BIOS/UEFI info
- OS version
- installed components
- network connections
Svarbu:
> `msinfo32` yra greitas būdas surinkti machine info troubleshootingui.
---

## 26. Command Prompt basics
Command Prompt:
```cmd
cmd
```
Dažnos komandos:
```cmd
hostname
whoami
ipconfig
ipconfig /all
ping 8.8.8.8
tracert google.com
netstat -ano
systeminfo
tasklist
taskkill /PID 1234 /F
dir
cd
type file.txt
copy file.txt C:\Temp\
move file.txt C:\Temp\
del file.txt
```
Panašumai su Linux:
- `dir` panašu į `ls`
- `cd` tas pats konceptas
- `type` panašu į `cat`
- `copy` panašu į `cp`
- `move` panašu į `mv`
Svarbu:
> Windows cmd nėra Linux bash. Komandos ir path sintaksė skiriasi.
---

## 27. PowerShell basics
PowerShell yra galingesnė Windows shell ir scripting aplinka.
Atidaryti:
```powershell
powershell
```
Cmdlets pavyzdžiai:
```powershell
Get-Process
Get-Service
Get-ChildItem
Get-Location
Set-Location C:\
Get-Content file.txt
Copy-Item file.txt C:\Temp\
Move-Item file.txt C:\Temp\
Remove-Item file.txt
```
Aliases:
```powershell
ls
cd
cat
pwd
```
Svarbu:
> PowerShell dirba su objects, ne tik plain text. Dėl to jis labai stiprus administravimui.
Security mintis:
> PowerShell dažnai naudojamas tiek adminų, tiek attacker’ių. Todėl PowerShell logs yra svarbūs detection.
---

## 28. Registry
Registry yra Windows configuration database.
Atidaryti:
```cmd
regedit
```
Pagrindiniai hives:
```txt
HKEY_CLASSES_ROOT
HKEY_CURRENT_USER
HKEY_LOCAL_MACHINE
HKEY_USERS
HKEY_CURRENT_CONFIG
```
HKLM:
> machine-wide settings.
HKCU:
> current user settings.
Security mintis:
> Registry dažnai naudojamas persistence, config, malware artifacts ir Windows internals analizei.
Pvz Run keys:
```txt
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```
Atsargiai:
> Registry redagavimas aklai gali sugadinti sistemą.
---

## 29. Windows Security
Windows Security yra centrinė vieta security funkcijoms.
Apsaugos sritys:
- Virus & threat protection
- Firewall & network protection
- App & browser control
- Device security
Status icons:
- green = protected / no action needed
- yellow = recommendation
- red = immediate attention needed
Svarbu:
> Windows Security yra user-facing dashboard’as, bet po juo yra Defender, firewall, policies ir OS security features.
---

## 30. Microsoft Defender
Microsoft Defender yra built-in antivirus/endpoint protection.
Funkcijos:
- real-time protection
- cloud-delivered protection
- sample submission
- ransomware protection
- scan options
Scan types:
- Quick scan
- Full scan
- Custom scan
- Offline scan
Security mintis:
> Defender nėra “žaislas”. Moderniose Windows sistemose jis yra realus EDR/AV komponentas, ypač su enterprise integracijomis.
PowerShell pavyzdžiai:
```powershell
Get-MpComputerStatus
Start-MpScan -ScanType QuickScan
```
---

## 31. Windows Firewall
Windows Defender Firewall filtruoja inbound/outbound network traffic.
Profiliai:
- Domain
- Private
- Public
Domain:
> kai machine joined prie domain ir prisijungęs prie domain network.
Private:
> trusted private network.
Public:
> untrusted network, pvz café Wi-Fi.
Svarbu:
> Public profile turi būti griežtesnis.
Firewall gali turėti rules:
- allow
- block
- inbound
- outbound
- port-based
- program-based
PowerShell:
```powershell
Get-NetFirewallProfile
Get-NetFirewallRule
```
Security mintis:
> Firewall profile misconfiguration gali atverti services netinkamame networke.
---

## 32. App & Browser Control
App & Browser Control susijęs su:
- SmartScreen
- reputation-based protection
- exploit protection
SmartScreen padeda blokuoti:
- malicious websites
- suspicious downloads
- unknown/untrusted apps
Security mintis:
> SmartScreen nėra galutinis saugumas, bet padeda prieš common phishing/malware download flow.
---

## 33. Device Security
Device Security rodo hardware-backed security features.
Gali apimti:
- Core isolation
- Memory integrity
- TPM
- Secure Boot
TPM:
> Trusted Platform Module, naudojamas key storage, BitLocker ir kitoms security funkcijoms.
Secure Boot:
> padeda užtikrinti, kad boot metu nebūtų paleistas nepatikimas bootloader/malware.
Security mintis:
> Modernus endpoint security remiasi ne tik antivirusu, bet ir hardware/boot-level protections.
---

## 34. BitLocker
BitLocker šifruoja diską.
Paprastai:
> BitLocker = full disk encryption Windows sistemose.
Kam naudinga:
- jei laptopas pavogtas
- jei diskas išimtas
- apsaugo data at rest
Reikalauja:
- supported Windows edition
- dažnai TPM
Svarbu:
> BitLocker saugo duomenis kai device išjungtas/užrakintas, bet neapsaugo nuo attackerio, kuris turi unlocked session.
Recovery key:
> būtinas diskui atkurti, jei TPM/boot chain pasikeičia arba kyla problemos.
---

## 35. Volume Shadow Copy
Volume Shadow Copy leidžia kurti snapshots/previous versions.
Paprastai:
> Shadow Copies = ankstesnės failų/sistemos būsenos kopijos.
Naudinga:
- restore previous file versions
- recovery
- backup-like functionality
Security mintis:
> Ransomware dažnai bando trinti shadow copies, kad useris negalėtų atkurti failų.
Komanda, kurią attackeriai gali naudoti blogiems tikslams:
```cmd
vssadmin delete shadows /all
```
Žinoti verta detection/defense kontekste.
---

## 36. Remote Desktop / RDP
RDP leidžia jungtis prie Windows GUI nuotoliniu būdu.
Default port:
```txt
3389
```
Prisijungimui Linux’e galima naudoti:
```bash
xfreerdp /u:USER /p:PASSWORD /v:IP
```
arba GUI:
```txt
Remmina
```
Security mintis:
> RDP atviras į internetą yra didelė rizika. Brute force, credential stuffing ir exposed RDP yra klasikinė problema.
Gera praktika:
- VPN
- MFA
- strong passwords
- firewall restrictions
- account lockout
- monitoring failed logons
---

## 37. Active Directory
Active Directory reiškia:
> Microsoft directory service corporate networks valdymui.
Paprastai:
> AD = centralizuota users, computers, groups, permissions ir policies valdymo sistema.
Kodėl AD svarbus:
- centralised identity management
- user authentication
- computer management
- group policies
- access control
- corporate login
- domain resources
Realus pavyzdys:
> Darbe turi vieną username/password ir gali prisijungti prie skirtingų domain computers, nes auth tikrinamas per AD.
Security mintis:
> AD yra corporate kingdom raktų dėžė. Jeigu AD krenta, dažnai krenta visa įmonė.
---

## 38. Windows Domain
Domain yra centralizuotai valdomas Windows network.
Paprastai:
> Domain = grupė users/computers/resources, valdoma per Active Directory.
Domain privalumai:
- centralizuotas user management
- centralizuotas computer management
- centralizuotos security policies
- easier password/account control
- group-based permissions
- login iš skirtingų machines
Svarbu:
> Be domain reikėtų kiekvieną PC valdyti atskirai. Su domain daug kas valdoma centralizuotai.
---

## 39. Domain Controller
Domain Controller:
> serveris, kuriame veikia Active Directory Domain Services ir kuris autentifikuoja domain users/computers.
DC daro:
- authentication
- authorization support
- directory queries
- Group Policy distribution
- domain object management
- Kerberos/NTLM auth flow
Svarbu:
> DC yra critical asset.
Security mintis:
> Domain Admin arba DC compromise dažnai reiškia viso domain compromise.
---

## 40. AD Objects
AD turi objects.
Dažni object types:
- Users
- Computers
- Groups
- Organizational Units
- Printers
- Shared folders
User object:
> žmogaus account.
Computer object:
> domain joined machine.
Group object:
> access/policy valdymui.
OU:
> container organizuoti objects ir taikyti policies.
Svarbi mintis:
> AD yra ne “sąrašas userių”, o struktūrizuota identity/resource valdymo sistema.
---

## 41. Machine accounts
Kai computer prisijungia prie domain, sukuriamas computer/machine account.
Machine account formatas:
```txt
COMPUTERNAME$
```
Pvz:
```txt
DC01$
LAPTOP123$
```
Svarbu:
> Machine accounts irgi yra security principals.
Machine account password:
- labai ilgas/random
- automatiškai keičiamas
- naudojamas computer-domain pasitikėjimui
Security mintis:
> Computer accounts gali turėti teises AD aplinkoje, todėl jų negalima ignoruoti.
---

## 42. AD Groups
Groups leidžia valdyti permissions daugeliui userių vienu metu.
Pavyzdžiai:
- Domain Users
- Domain Computers
- Domain Controllers
- Domain Admins
- Enterprise Admins
- Account Operators
- Server Operators
- Backup Operators
Svarbu:
> Access control dažnai daromas per groups, ne individualius userius.
Security mintis:
> Group membership review yra vienas svarbiausių AD audit dalykų.
Ypač tikrinti:
- Domain Admins
- Enterprise Admins
- local Administrators
- privileged operators
- nested groups
---

## 43. Organizational Units
OU reiškia:
> Organizational Unit
Paprastai:
> OU = container AD objektams organizuoti.
OUs gali laikyti:
- users
- computers
- groups
- other OUs
Pavyzdžiai:
```txt
Sales
IT
HR
Workstations
Servers
Domain Controllers
```
Kam naudojama:
- organizacija
- delegated administration
- Group Policy taikymas
Svarbu:
> OU nėra tas pats kas security group.
OU organizuoja ir leidžia taikyti GPO.
Group suteikia permissions/access.
---

## 44. Active Directory Users and Computers
Atidaryti DC:
```txt
Start -> Active Directory Users and Computers
```
Arba:
```cmd
dsa.msc
```
Naudojama valdyti:
- users
- computers
- groups
- OUs
- domain objects
Svarbu:
> ADUC yra viena pagrindinių GUI konsolių AD administravimui.
IT Support realybėje:
- reset password
- unlock account
- move user/computer to OU
- check group membership
- disable leaver account
- create user
---

## 45. Group Policy
Group Policy leidžia centralizuotai taikyti settings users/computers.
GPO reiškia:
> Group Policy Object
GPO gali kontroliuoti:
- password policy
- lock screen
- firewall settings
- mapped drives
- software restrictions
- scripts
- desktop settings
- Windows Update settings
- security policies
GPO taikoma:
- domain
- site
- OU
Svarbu:
> GPO yra vienas pagrindinių būdų centralizuotai valdyti Windows machines.
Security mintis:
> Blogai sukonfigūruotas GPO gali sukelti didelę riziką arba duoti attacker’iui kelią į platesnį compromise.
---

## 46. Group Policy Management
Atidaryti:
```cmd
gpmc.msc
```
Naudojama:
- kurti GPO
- linkinti GPO prie OU/domain
- keisti policy settings
- žiūrėti inheritance
- valdyti policy scope
Client pusėje atnaujinti policies:
```cmd
gpupdate /force
```
Pažiūrėti applied policies:
```cmd
gpresult /r
```
HTML report:
```cmd
gpresult /h report.html
```
Svarbu:
> Kai pakeiti GPO, clientams gali reikėti laiko arba `gpupdate`.
---

## 47. Authentication AD aplinkoje
AD auth dažniausiai remiasi:
- Kerberos
- NTLM
Kerberos:
> modernus domain authentication protokolas su tickets.
NTLM:
> senesnis challenge-response auth mechanizmas, vis dar sutinkamas.
Labai supaprastintai:
```txt
User login -> DC patikrina credentials -> user gauna access prie domain resources pagal permissions/groups
```
Security mintis:
> Kerberos/NTLM yra didžiulė AD security tema. Beginner lygyje svarbu žinoti, kad domain auth nėra lokali password patikra kiekviename PC.
---

## 48. Trees, Forests ir Trusts
Domain:
> viena AD administracinė zona.
Tree:
> keli domainai toje pačioje namespace struktūroje.
Forest:
> didžiausias AD boundary, gali turėti kelis trees/domainus.
Trust:
> pasitikėjimo ryšys tarp domainų/forests.
Pavyzdys:
```txt
corp.example.com
dev.example.com
```
gali būti tame pačiame tree.
Forest:
```txt
example.local forest
```
Trust leidžia:
> vieno domain useriams pasiekti kito domain resources, jei leidžia permissions.
Security mintis:
> Trust relationships plečia attack surface. Jei pasitiki kitu domain, jo compromise gali turėti pasekmių.
---

## 49. Local vs Domain accounts
Local account:
```txt
COMPUTER\username
```
Domain account:
```txt
DOMAIN\username
```
Arba:
```txt
username@domain.local
```
Local admin:
> admin tik tame machine.
Domain Admin:
> privileged account domain mastu.
Svarbu:
> Local admin ant vienos mašinos nėra tas pats kas Domain Admin. Bet daug local admin teisių per daug machines vis tiek yra didelė rizika.
---

## 50. Windows + AD praktinė IT Support vertė
IT Support realybėje šitie dalykai labai dažni:
- user password reset
- account unlock
- check group membership
- add/remove user from group
- join machine to domain
- troubleshoot login
- check IP/DNS
- check mapped drives
- check printer
- read Event Viewer
- restart service
- check Task Manager
- install/update apps
- verify Windows Defender/Firewall
- check BitLocker recovery
- RDP into machine/server
Svarbu:
> Entry IT Support yra daug Windows + AD + users + tickets + troubleshooting.
Čia tavo perėjimui labai aktualu.
---

## 51. Windows security praktinė vertė
Security/SOC požiūriu:
- Event Viewer = raw Windows logs
- Security log = auth events
- Defender = endpoint protection
- Firewall = local traffic control
- Services = persistence/privilege escalation surface
- Scheduled Tasks = persistence
- Registry = persistence/config/artifacts
- Users/Groups = permissions/privilege
- AD = identity backbone
- GPO = central policies
- RDP = remote access risk
Core security klausimai:
```txt
Kas prisijungė?
Iš kur prisijungė?
Kokiam useriui?
Ar login pavyko?
Kokios grupės?
Kokie procesai paleisti?
Kokie services įdiegti?
Kokie scheduled tasks?
Ar Defender veikia?
Ar Firewall įjungtas?
Ar machine domain joined?
```
---

## 52. Komandos / Run shortcuts
Windows tools:
```cmd
control
msconfig
compmgmt.msc
taskmgr
eventvwr.msc
services.msc
taskschd.msc
devmgmt.msc
diskmgmt.msc
lusrmgr.msc
regedit
msinfo32
cmd
powershell
```
Networking:
```cmd
ipconfig
ipconfig /all
ping 8.8.8.8
tracert google.com
nslookup example.com
netstat -ano
```
System:
```cmd
hostname
whoami
whoami /priv
whoami /groups
systeminfo
tasklist
taskkill /PID 1234 /F
```
Files:
```cmd
dir
cd
type file.txt
copy file.txt C:\Temp\
move file.txt C:\Temp\
del file.txt
```
Users/groups cmd:
```cmd
net user
net localgroup
net localgroup administrators
```
PowerShell:
```powershell
Get-Process
Get-Service
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember Administrators
Get-EventLog -LogName Security -Newest 10
Get-WinEvent -LogName Security -MaxEvents 10
Get-ScheduledTask
Get-NetFirewallProfile
```
AD tools:
```cmd
dsa.msc
gpmc.msc
gpupdate /force
gpresult /r
```
---

## 53. Mano paaiškinimas žmonių kalba
Windows standalone machine yra kaip atskiras garažas.
Local users:
> kas turi raktus į tą vieną garažą.
Local groups:
> kokias teises tie žmonės turi tame garaže.
NTFS permissions:
> kurią spintelę gali atidaryti, ką gali keisti, ką gali tik skaityti.
UAC:
> “ar tikrai nori daryti admin veiksmą?”
Task Manager:
> kas dabar dirba ir kas ryja resursus.
Services:
> background mechanizmai, kurie sukasi net jei niekas neatidarė lango.
Event Viewer:
> garažo apsaugos kamera/logbook.
Active Directory:
> centrinė visų garažų administracija.
Domain Controller:
> pagrindinė kontora, kuri tikrina tapatybes.
GPO:
> taisyklės visiems garažams iš centro.
Security groups:
> kas gali į kurią zoną.
Svarbiausia:
> Windows nėra tik GUI. Po juo yra users, permissions, services, logs, registry ir domain identity sistema.
---

## 54. Ką būtina atsiminti
- Windows yra pagrindinė corporate endpoint OS.
- NTFS yra modernus Windows filesystem.
- NTFS permissions labai svarbios security.
- System32 turi kritinius OS failus ir tools.
- `%windir%` dažnai rodo į `C:\Windows`.
- Local user egzistuoja viename machine.
- Domain user egzistuoja AD domain’e.
- User profile yra `C:\Users\Username`.
- UAC prašo elevation admin veiksmams.
- Task Manager rodo procesus/performance/startup/services.
- Computer Management sujungia daug admin įrankių.
- Event Viewer rodo Windows logs.
- Services yra background komponentai.
- Scheduled Tasks naudojami automation ir gali būti persistence.
- Registry yra Windows config database.
- Defender yra built-in endpoint protection.
- Windows Firewall turi Domain/Private/Public profiles.
- BitLocker šifruoja diską.
- AD centralizuoja users, computers, groups ir policies.
- Domain Controller autentifikuoja domain users/computers.
- OU organizuoja AD objects ir leidžia taikyti GPO.
- Group suteikia permissions/access.
- GPO centralizuotai valdo settings.
- Forest yra didžiausias AD boundary.
- Trust leidžia ryšį tarp domainų/forests.
---

## 55. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- Kerberos ticket abuse
- Golden/Silver tickets
- advanced AD attacks
- BloodHound graph theory
- Windows internals/kernel
- advanced registry forensics
- Sysmon tuning
- EDR bypass
- GPO abuse chains
- constrained delegation
- ADCS attacks
- domain persistence
- advanced PowerShell tradecraft
Dabar svarbiausia:
> Suprasti Windows kaip OS, mokėti rasti tools/settings/logs, suprasti users/groups/permissions, ir suvokti Active Directory kaip centralizuotą corporate identity sistemą.
---

## 56. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kuo Windows Home skiriasi nuo Pro security/business funkcijų prasme?
2. Ką reiškia NTFS?
3. Kokias funkcijas turi NTFS?
4. Kur žiūrėti file/folder permissions?
5. Kas yra ADS?
6. Kam skirtas `C:\Windows\System32`?
7. Ką reiškia `%windir%`?
8. Kuo skiriasi Administrator ir Standard User?
9. Kur saugomas user profile?
10. Kam skirtas `lusrmgr.msc`?
11. Kas yra UAC?
12. Kam skirtas Task Manager?
13. Kam skirtas `msconfig`?
14. Kam skirtas `compmgmt.msc`?
15. Kam skirtas Event Viewer?
16. Koks event ID rodo successful logon?
17. Koks event ID rodo failed logon?
18. Kam skirtas Services console?
19. Kam skirtas Task Scheduler?
20. Kodėl Scheduled Tasks svarbūs security?
21. Kas yra Registry?
22. Kam skirtas Defender?
23. Kuo skiriasi Firewall Domain, Private ir Public profiles?
24. Kas yra BitLocker?
25. Kas yra Volume Shadow Copy?
26. Kas yra RDP ir koks default port?
27. Kas yra Active Directory?
28. Kas yra Windows Domain?
29. Kas yra Domain Controller?
30. Kuo local user skiriasi nuo domain user?
31. Kas yra AD object?
32. Kaip atrodo machine account pavadinimas?
33. Kam naudojamos AD groups?
34. Kuo OU skiriasi nuo group?
35. Kas yra GPO?
36. Kam skirtas `dsa.msc`?
37. Kam skirtas `gpmc.msc`?
38. Ką daro `gpupdate /force`?
39. Ką rodo `gpresult /r`?
40. Kas yra forest?
41. Kas yra trust relationship?
42. Kodėl AD yra toks svarbus security pasaulyje?
---

## 57. Mano verdiktas
Windows Fundamentals 1–3 ir AD Basics yra labai praktiškas blokas tavo karjeros perėjimui.
Statusas:
```txt
Status: Foundation / Career-relevant
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš IT Support CV, SOC basics, AD rooms ir Windows privilege escalation
```
Svarbiausia mintis:
> Windows yra ne tik “desktopas”. Tai users, permissions, services, logs, registry, Defender, firewall ir Active Directory identity sistema.
Entry IT Support požiūriu šitas blokas yra labai naudingas, nes realūs darbai dažnai bus:
- useriai
- passwordai
- groups
- Windows troubleshooting
- Event Viewer
- services
- RDP
- domain login
- basic networking
- Defender/Firewall
Cybersecurity požiūriu:
> Kas valdo identity, tas valdo įmonę. Windows + AD yra viena svarbiausių corporate security sričių.
Core mental model:
```txt
Local machine:
Users -> Groups -> Permissions -> Services -> Logs

Domain:
AD -> Domain Controller -> Users/Computers/Groups/OUs -> GPO -> Access Control
```
Jeigu šitą supranti, Windows/AD nebeatrodo kaip random GUI meniu miškas. Tai tampa sistema.
