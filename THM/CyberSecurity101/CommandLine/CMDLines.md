# THM – Windows Command Line + Windows PowerShell + Linux Shells

## 1. Room esmė
Šitie roomai yra command-line blokas.
Pagrindinė mintis:
> GUI yra patogu, bet CLI leidžia greičiau, tiksliau ir giliau valdyti sistemą.
Šitas notes apjungia:
- Windows Command Prompt
- Windows PowerShell
- Linux shells
- basic navigation
- file/folder management
- system information
- network troubleshooting
- process/service analysis
- filtering/sorting
- pipeline logic
- scripting basics
- shell skirtumus
Cybersecurity kontekste command line būtinas, nes:
- daug tools veikia per terminalą
- remote access dažnai duoda shell
- logs, processes, services ir network info greičiau tikrinami CLI
- scripting leidžia automatizuoti kartojamus veiksmus
- Windows admin/security daug remiasi PowerShell
---

## 2. Kas yra CLI?
CLI reiškia:
> Command Line Interface
Paprastai:
> CLI = tekstinė sąsaja, kurioje sistemai duodi komandas.
GUI:
> spaudai mygtukus, langus, menus.
CLI:
> rašai komandas.
Kodėl CLI naudinga:
- greita
- tikslu
- lengva kartoti
- galima automatizuoti
- patogu remote administravimui
- gerai troubleshootinimui
- galima jungti komandas į workflow
Svarbi mintis:
> CLI pradžioje atrodo sunkiau nei GUI, bet kai pagauni struktūrą, daug dalykų padarai greičiau.
---

## 3. Shell vs Terminal
Terminalas:
> programa/langas, kuriame rašai komandas.
Shell:
> programa, kuri interpretuoja tavo komandas ir perduoda jas OS.
Pavyzdžiai:
- `cmd.exe` = Windows Command Prompt shell
- `powershell.exe` = Windows PowerShell shell
- `bash` = dažnas Linux shell
- `zsh` = kitas Linux/Unix shell
- `fish` = user-friendly shell
Svarbu:
> Terminalas yra langas. Shell yra komandų interpretatorius.
Kaip tattoo machine ir ranka: vienas yra įrankis, kitas daro veiksmą. Jei abu kreivi — bus tribal 2003.
---

## 4. Windows Command Prompt
Command Prompt dažnai vadinamas:
```cmd
cmd
```
Atidaryti:
```cmd
cmd
```
Arba Run:
```cmd
cmd.exe
```
Paprastai:
> cmd = senesnė Windows command-line aplinka.
Naudojama:
- basic file navigation
- system info
- network troubleshooting
- process list
- basic admin commands
- scripts su `.bat` / `.cmd`
Svarbu:
> cmd vis dar naudingas, bet PowerShell yra galingesnis moderniam administravimui.
---

## 5. PowerShell
PowerShell yra moderni Windows shell ir scripting kalba.
Paprastai:
> PowerShell = command shell + scripting language + automation framework.
PowerShell stiprybė:
- dirba su objects, ne tik text
- turi cmdlets
- labai stiprus Windows administravimui
- tinka AD, Microsoft 365, Azure, services, logs, automation
- naudingas blue team ir red team
Svarbu:
> PowerShell nėra tik “gražesnis cmd”. Tai daug galingesnis įrankis.
PowerShell command struktūra dažnai:
```powershell
Verb-Noun
```
Pvz:
```powershell
Get-Process
Get-Service
Get-ChildItem
Set-Location
```
---

## 6. Linux Shells
Linux shell yra CLI aplinka Linux sistemose.
Dažniausi shell’ai:
- Bash
- Sh
- Zsh
- Fish
- Dash
Bash:
> dažniausias ir svarbiausias mokymuisi.
Zsh:
> daug quality-of-life features, populiarus Kali/macOS pasaulyje.
Fish:
> user-friendly, gera auto-suggestions/syntax highlighting.
Dash:
> lengvas POSIX shell, dažnai naudojamas scriptams.
Svarbi mintis:
> Linux komandos ir shell nėra tas pats. Komandos yra įrankiai, shell jas interpretuoja ir sujungia.
Pvz `ls`, `grep`, `find` yra komandos/tools.
Bash yra shell, kuris jas paleidžia.
---

## 7. Prompt supratimas
Windows cmd prompt:
```cmd
C:\Users\Ignelis>
```
PowerShell prompt:
```powershell
PS C:\Users\Ignelis>
```
Linux Bash prompt:
```bash
user@machine:~$
```
Reikšmė:
- dabartinis useris
- dabartinė lokacija
- shell tipas
- privilege lygis
Linux:
```bash
$
```
normal user.
```bash
#
```
root.
PowerShell:
```powershell
PS
```
rodo, kad esi PowerShell aplinkoje.
Svarbu:
> Visada žinok kur esi prieš rašydamas komandą.
---

## 8. Dabartinė lokacija
Windows cmd:
```cmd
cd
```
PowerShell:
```powershell
Get-Location
```
Alias:
```powershell
pwd
```
Linux:
```bash
pwd
```
Paprastai:
> Prieš judindamas failus, žinok dabartinį folderį.
---

## 9. Navigacija per folders
Windows cmd:
```cmd
cd C:\Users\Ignelis\Documents
```
Vienu lygiu aukštyn:
```cmd
cd ..
```
Į root drive:
```cmd
cd \
```
Keisti drive:
```cmd
D:
```
PowerShell:
```powershell
Set-Location C:\Users\Ignelis\Documents
```
Alias:
```powershell
cd C:\Users\Ignelis\Documents
```
Linux:
```bash
cd /home/ignelis/Documents
cd ..
cd ~
cd /
```
Svarbi mintis:
> Windows naudoja drive letters kaip `C:\`, Linux naudoja vieną root medį `/`.
---

## 10. Listinti failus/folderius
Windows cmd:
```cmd
dir
```
PowerShell:
```powershell
Get-ChildItem
```
Aliases:
```powershell
ls
dir
gci
```
Linux:
```bash
ls
ls -la
```
PowerShell su hidden:
```powershell
Get-ChildItem -Force
```
Cmd su hidden:
```cmd
dir /a
```
Linux hidden:
```bash
ls -a
```
Svarbi mintis:
> Windows hidden failai yra attribute. Linux hidden failai dažnai prasideda tašku.
---

## 11. Failų skaitymas
Windows cmd:
```cmd
type file.txt
```
PowerShell:
```powershell
Get-Content file.txt
```
Alias:
```powershell
cat file.txt
```
Linux:
```bash
cat file.txt
less file.txt
head file.txt
tail file.txt
```
PowerShell pirmos eilutės:
```powershell
Get-Content file.txt -TotalCount 10
```
PowerShell paskutinės eilutės:
```powershell
Get-Content file.txt -Tail 10
```
PowerShell live log:
```powershell
Get-Content file.log -Wait
```
Linux live log:
```bash
tail -f file.log
```
---

## 12. Kurti failus/folderius
Windows cmd sukurti folderį:
```cmd
mkdir Notes
```
Cmd sukurti failą:
```cmd
echo hello > file.txt
```
PowerShell folder:
```powershell
New-Item -ItemType Directory -Name Notes
```
PowerShell file:
```powershell
New-Item -ItemType File -Name notes.txt
```
Linux:
```bash
mkdir Notes
touch notes.txt
```
Nested folderiai Linux:
```bash
mkdir -p Cyber/Notes/Linux
```
PowerShell nested:
```powershell
New-Item -ItemType Directory -Path Cyber\Notes\Windows -Force
```
---

## 13. Copy / Move / Rename / Delete
Windows cmd copy:
```cmd
copy file.txt C:\Temp\
```
Cmd move:
```cmd
move file.txt C:\Temp\
```
Cmd rename:
```cmd
ren old.txt new.txt
```
Cmd delete:
```cmd
del file.txt
```
Cmd remove folder:
```cmd
rmdir folder
```
Folder su turiniu:
```cmd
rmdir /s folder
```
PowerShell copy:
```powershell
Copy-Item file.txt C:\Temp\
```
PowerShell move:
```powershell
Move-Item file.txt C:\Temp\
```
PowerShell rename:
```powershell
Rename-Item old.txt new.txt
```
PowerShell delete:
```powershell
Remove-Item file.txt
```
PowerShell folder su turiniu:
```powershell
Remove-Item folder -Recurse
```
Linux:
```bash
cp file.txt ~/Documents/
mv file.txt ~/Documents/
mv old.txt new.txt
rm file.txt
rm -r folder/
```
Svarbu:
> `move` / `mv` gali ir perkelti, ir pervadinti. Jei destination kelias neteisingas, failas gali “dingti” tik todėl, kad buvo pervadintas.
---

## 14. Absolute ir relative paths
Windows absolute path:
```cmd
C:\Users\Ignelis\Documents\file.txt
```
Linux absolute path:
```bash
/home/ignelis/Documents/file.txt
```
Relative path:
```txt
Documents\file.txt
Documents/file.txt
```
Windows current folder:
```cmd
.
```
Parent:
```cmd
..
```
Linux taip pat:
```bash
.
..
```
PowerShell palaiko:
```powershell
.
..
```
Svarbu:
> Jei stringi, naudok pilną path. Terminalas neskaito minčių, jis tik laukia, kol suklysi.
---

## 15. Help sistemos
Windows cmd help:
```cmd
help
help dir
```
Komandos help:
```cmd
ipconfig /?
netstat /?
```
PowerShell help:
```powershell
Get-Help Get-Process
Get-Help Get-Process -Examples
Get-Help Get-Process -Full
```
Atnaujinti PowerShell help:
```powershell
Update-Help
```
Linux:
```bash
man ls
ls --help
```
Svarbi mintis:
> Išmokti naudoti help yra svarbiau nei mintinai žinoti visas komandas.
---

## 16. System information
Windows cmd:
```cmd
hostname
whoami
systeminfo
ver
```
PowerShell:
```powershell
hostname
whoami
Get-ComputerInfo
$PSVersionTable
```
Linux:
```bash
hostname
whoami
uname -a
lsb_release -a
```
Windows current user privileges:
```cmd
whoami /priv
```
Windows groups:
```cmd
whoami /groups
```
PowerShell local users:
```powershell
Get-LocalUser
```
PowerShell local groups:
```powershell
Get-LocalGroup
```
Group members:
```powershell
Get-LocalGroupMember Administrators
```
Security mintis:
> `whoami`, groups ir privileges yra pirmi dalykai po shell gavimo arba troubleshootinant permissions.
---

## 17. Network information
Windows cmd IP config:
```cmd
ipconfig
ipconfig /all
```
Ping:
```cmd
ping 8.8.8.8
```
Trace route:
```cmd
tracert google.com
```
DNS lookup:
```cmd
nslookup example.com
```
Connections:
```cmd
netstat -ano
```
PowerShell IP:
```powershell
Get-NetIPConfiguration
Get-NetIPAddress
```
DNS:
```powershell
Resolve-DnsName example.com
```
TCP connections:
```powershell
Get-NetTCPConnection
```
Adapters:
```powershell
Get-NetAdapter
```
Linux:
```bash
ip a
ip route
ping -c 4 8.8.8.8
traceroute google.com
dig example.com
ss -tulpn
```
Svarbu:
> Windows `ipconfig /all` yra labai panašu į Linux `ip a` + DNS/gateway info vienoje vietoje.
---

## 18. Processes
Windows cmd:
```cmd
tasklist
```
Kill process:
```cmd
taskkill /PID 1234 /F
```
Pagal image name:
```cmd
taskkill /IM notepad.exe /F
```
PowerShell:
```powershell
Get-Process
Stop-Process -Id 1234
Stop-Process -Name notepad
```
Linux:
```bash
ps aux
top
htop
kill PID
pkill processname
```
PowerShell filter:
```powershell
Get-Process | Where-Object {$_.CPU -gt 10}
```
Svarbu:
> Process = running program. PID = process ID.
---

## 19. Services
Windows cmd:
```cmd
sc query
sc query sshd
sc qc ServiceName
```
PowerShell:
```powershell
Get-Service
Get-Service sshd
Start-Service sshd
Stop-Service sshd
Restart-Service sshd
```
Linux:
```bash
systemctl status ssh
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
```
Svarbu:
> Service yra background component. Procesas gali būti trumpalaikis, service dažnai managed/long-running.
Security mintis:
> Services yra svarbūs persistence, privilege escalation ir troubleshooting vietose.
---

## 20. Environment variables
Windows cmd:
```cmd
set
echo %PATH%
echo %USERPROFILE%
echo %TEMP%
```
PowerShell:
```powershell
Get-ChildItem Env:
$env:PATH
$env:USERPROFILE
$env:TEMP
```
Linux:
```bash
env
echo $PATH
echo $HOME
echo $USER
```
PATH:
> vietos, kur shell ieško executable komandų.
Security mintis:
> PATH misconfig arba writable PATH location gali būti rizika.
---

## 21. Filtering cmd output
Windows cmd su `findstr`:
```cmd
tasklist | findstr chrome
```
Ieškoti teksto faile:
```cmd
findstr "password" file.txt
```
Case-insensitive:
```cmd
findstr /i "password" file.txt
```
PowerShell:
```powershell
Select-String "password" file.txt
```
Arba:
```powershell
Get-Content file.txt | Select-String "password"
```
Linux:
```bash
grep "password" file.txt
grep -i "password" file.txt
grep -Rin "password" .
```
Svarbu:
> `findstr` yra Windows cmd grep pusbrolis. Ne toks gražus, bet dirba.
---

## 22. Pipeline logika
Pipeline reiškia:
> Vienos komandos output perduodamas kitai komandai.
Windows cmd:
```cmd
tasklist | findstr chrome
```
Linux:
```bash
ps aux | grep chrome
```
PowerShell:
```powershell
Get-Process | Where-Object {$_.ProcessName -like "*chrome*"}
```
Svarbus skirtumas:
> Bash/cmd pipeline dažniausiai perduoda text. PowerShell pipeline perduoda objects.
Tai reiškia, kad PowerShell gali filtruoti pagal properties:
```powershell
Get-Process | Where-Object {$_.CPU -gt 100}
```
Ne reikia grepinti tekstą kaip urviniam žmogui su akmeniu.
---

## 23. Sorting PowerShell
PowerShell sort:
```powershell
Get-Process | Sort-Object CPU
```
Descending:
```powershell
Get-Process | Sort-Object CPU -Descending
```
Top 5 pagal CPU:
```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```
Pasirinkti properties:
```powershell
Get-Process | Select-Object Name, Id, CPU
```
Format table:
```powershell
Get-Process | Select-Object Name, Id, CPU | Format-Table
```
Svarbu:
> PowerShell labai stiprus, kai reikia filtruoti/sortinti structured system data.
---

## 24. Real-time analysis
Windows Task Manager:
```txt
Ctrl + Shift + Esc
```
Cmd:
```cmd
tasklist
netstat -ano
```
PowerShell live process refresh:
```powershell
while ($true) { Get-Process | Sort-Object CPU -Descending | Select-Object -First 10; Start-Sleep 5; Clear-Host }
```
Network connections:
```powershell
Get-NetTCPConnection | Sort-Object State
```
Linux:
```bash
top
htop
watch "ps aux | head"
watch "ss -tulpn"
```
Svarbu:
> Real-time analysis padeda matyti kas vyksta dabar, ne tik kas buvo prieš 10 minučių.
---

## 25. File search
Windows cmd:
```cmd
dir C:\ /s /b file.txt
```
Ieškoti pagal pattern:
```cmd
dir C:\ /s /b *password*
```
PowerShell:
```powershell
Get-ChildItem C:\ -Recurse -Filter "*password*" -ErrorAction SilentlyContinue
```
Linux:
```bash
find / -iname "*password*" 2>/dev/null
```
PowerShell ieškoti turinio:
```powershell
Select-String -Path *.txt -Pattern "password"
```
Recursive turinio paieška:
```powershell
Get-ChildItem . -Recurse -File | Select-String "password"
```
Linux:
```bash
grep -Rin "password" .
```
Svarbu:
> Failo pavadinimo paieška ir teksto paieška faile yra du skirtingi dalykai.
---

## 26. Redirection
Windows cmd:
```cmd
echo hello > file.txt
echo another line >> file.txt
```
PowerShell:
```powershell
"hello" > file.txt
"another line" >> file.txt
```
Arba:
```powershell
Set-Content file.txt "hello"
Add-Content file.txt "another line"
```
Linux:
```bash
echo "hello" > file.txt
echo "another line" >> file.txt
```
Reikšmė:
- `>` perrašo
- `>>` prideda
Svarbu:
> `>` yra pavojingas, jei nenori perrašyti failo.
---

## 27. Windows Command Prompt batch scripts
Batch failai:
```txt
.bat
.cmd
```
Pavyzdys:
```bat
@echo off
echo Hello
whoami
ipconfig
pause
```
Paleisti:
```cmd
script.bat
```
Kintamieji cmd:
```bat
set NAME=Ignelis
echo %NAME%
```
Argumentai:
```bat
echo %1
echo %2
```
Svarbu:
> Batch scripting vis dar sutinkamas, bet PowerShell dažniausiai geresnis moderniam Windows automation.
---

## 28. PowerShell scripting
PowerShell scripts:
```txt
.ps1
```
Pavyzdys:
```powershell
Write-Output "Hello"
Get-Process | Sort-Object CPU -Descending | Select-Object -First 5
```
Paleisti:
```powershell
.\script.ps1
```
Execution policy gali blokuoti scripts.
Patikrinti:
```powershell
Get-ExecutionPolicy
```
Laikinai current session:
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```
Svarbu:
> Execution Policy nėra pilna security boundary. Ji labiau safety mechanism prieš netyčinį scriptų paleidimą.
---

## 29. Bash scripting
Bash script failas:
```bash
script.sh
```
Shebang:
```bash
#!/bin/bash
```
Pavyzdys:
```bash
#!/bin/bash
echo "Hello"
whoami
ip a
```
Padaryti executable:
```bash
chmod +x script.sh
```
Paleisti:
```bash
./script.sh
```
Arba:
```bash
bash script.sh
```
Svarbu:
> Shebang pasako, kokiu interpreteriu vykdyti scriptą.
---

## 30. Bash variables
Kintamasis:
```bash
name="Ignelis"
```
Naudoti:
```bash
echo "$name"
```
Svarbu:
> Bash kintamuosiuose neturi būti tarpų aplink `=`.
Blogai:
```bash
name = "Ignelis"
```
Gerai:
```bash
name="Ignelis"
```
Arguments:
```bash
echo "$1"
echo "$2"
```
Paleisti:
```bash
./script.sh first second
```
`$1` = first argument.
`$2` = second argument.
Visi argumentai:
```bash
echo "$@"
```
---

## 31. Bash read input
Interaktyvus input:
```bash
read name
echo "Hello $name"
```
Su prompt:
```bash
read -p "Name: " name
echo "Hello $name"
```
Svarbu:
> `read` leidžia scriptui klausti userio input.
---

## 32. Bash arrays
Array:
```bash
tools=("nmap" "curl" "grep")
```
Pirmas elementas:
```bash
echo "${tools[0]}"
```
Visi:
```bash
echo "${tools[@]}"
```
Loop per array:
```bash
for tool in "${tools[@]}"; do
  echo "$tool"
done
```
Svarbu:
> Bash arrays prasideda nuo index 0.
---

## 33. Bash if statements
Basic if:
```bash
if [ "$name" = "Ignelis" ]; then
  echo "Labas"
fi
```
If else:
```bash
if [ -f "notes.md" ]; then
  echo "File exists"
else
  echo "File missing"
fi
```
Svarbūs testai:
```bash
-f file     # file exists
-d folder   # directory exists
-r file     # readable
-w file     # writable
-x file     # executable
```
Svarbu:
> Bash `[` ir `]` viduje tarpai būtini.
Blogai:
```bash
if ["$name" = "Ignelis"]; then
```
Gerai:
```bash
if [ "$name" = "Ignelis" ]; then
```
---

## 34. Bash loops
For loop:
```bash
for item in one two three; do
  echo "$item"
done
```
Loop per files:
```bash
for file in *.txt; do
  echo "$file"
done
```
While loop:
```bash
count=1
while [ "$count" -le 5 ]; do
  echo "$count"
  count=$((count + 1))
done
```
Svarbu:
> Loops leidžia kartoti tą patį veiksmą daug kartų.
Cyber praktikoje:
- tikrinti daug failų
- iteruoti per host list
- automatizuoti curl requests
- parse’inti output
---

## 35. Common shell comparison
| Dalykas | Windows cmd | PowerShell | Linux Bash |
|---|---|---|---|
| List files | `dir` | `Get-ChildItem` / `ls` | `ls` |
| Current location | `cd` | `Get-Location` / `pwd` | `pwd` |
| Change dir | `cd` | `Set-Location` / `cd` | `cd` |
| Read file | `type` | `Get-Content` / `cat` | `cat`, `less` |
| Copy | `copy` | `Copy-Item` | `cp` |
| Move | `move` | `Move-Item` | `mv` |
| Delete | `del` | `Remove-Item` | `rm` |
| Processes | `tasklist` | `Get-Process` | `ps aux` |
| Kill process | `taskkill` | `Stop-Process` | `kill` |
| IP info | `ipconfig` | `Get-NetIPConfiguration` | `ip a` |
| DNS lookup | `nslookup` | `Resolve-DnsName` | `dig`, `nslookup` |
| Search text | `findstr` | `Select-String` | `grep` |
---

## 36. Shells cyber kontekste
Shell gali reikšti dvi skirtingas, bet susijusias idėjas:
1. Normalus shell:
> CLI aplinka, kurioje useris valdo sistemą.
2. Remote shell:
> command-line prieiga prie remote targeto.
Cybersecurity dažnai kalba apie:
- interactive shell
- non-interactive shell
- reverse shell
- bind shell
- web shell
Interactive shell:
> gali normaliai rašyti komandas ir gauti output.
Non-interactive shell:
> komanda įvykdoma, bet nėra patogaus terminalo.
Reverse shell:
> targetas prisijungia atgal prie attacker/listener.
Bind shell:
> targetas atidaro portą, attacker jungiasi prie jo.
Web shell:
> komandų vykdymas per web aplikaciją/server-side failą.
Svarbu:
> Šitas notes yra command-line pagrindams. Reverse/bind/web shells giliau yra exploitation tema ir turi būti praktikuojama tik legaliose lab aplinkose.
---

## 37. Legalumo ribos
Command line įrankiai yra neutralūs.
Tie patys dalykai naudojami:
- admin darbui
- IT support
- troubleshooting
- blue team
- pentesting
- incident response
- attacker veiksmams
Svarbi taisyklė:
> Praktikuok tik savo sistemose, THM/CTF/lab’uose arba su aiškiu leidimu.
Nėra “tik pabandžiau”.
Yra:
```txt
leidimas yra / leidimo nėra
```
---

## 38. Praktinė vertė IT Support
Entry IT Support realybėje šitos žinios naudingos tiesiogiai:
- patikrinti IP/DNS/gateway
- rasti hostname/user
- pažiūrėti procesus
- killinti užstrigusią programą
- skaityti basic logs
- restartinti service
- kopijuoti/move’inti failus
- patikrinti folder permissions
- paleisti diagnostic commands
- rinkti informaciją ticketui
- automatizuoti mažus veiksmus
IT Support mini workflow:
```txt
Kas useris? -> whoami
Koks PC? -> hostname/systeminfo
Koks IP? -> ipconfig /all
Ar DNS veikia? -> nslookup
Kas ryja resursus? -> tasklist / Get-Process
Ar service veikia? -> Get-Service / services.msc
Kas loguose? -> Event Viewer / Get-WinEvent
```
---

## 39. Praktinė vertė cyber
Cyber/SOC/pentest požiūriu:
- command line leidžia greitai rinkti info
- PowerShell svarbus Windows auditui ir detection
- Bash svarbus Linux, tools ir automation
- process/network info padeda incident response
- scripts leidžia kartoti veiksmus
- filtering/sorting padeda analizuoti output
- shell access yra post-exploitation pagrindas
Security klausimai:
```txt
Kas aš esu?
Kokias grupes/privileges turiu?
Koks hostname?
Koks IP/gateway/DNS?
Kokie procesai veikia?
Kokie services veikia?
Kokie network connections atviri?
Kokie failai domina?
Kokie logs rodo įvykius?
```
---

## 40. Praktinis troubleshooting modelis
Kai esi Windows mašinoje:
```cmd
whoami
hostname
ipconfig /all
ping 8.8.8.8
nslookup example.com
tasklist
netstat -ano
systeminfo
```
Kai esi PowerShell:
```powershell
whoami
hostname
Get-NetIPConfiguration
Resolve-DnsName example.com
Get-Process
Get-Service
Get-NetTCPConnection
Get-ComputerInfo
```
Kai esi Linux:
```bash
whoami
hostname
ip a
ip route
ping -c 4 8.8.8.8
ps aux
ss -tulpn
uname -a
```
Svarbu:
> Pirma rink informaciją. Tik tada taisyk. Kitaip esi žmogus su plaktuku serverių kambaryje.
---

## 41. Mini command muscle memory
Windows cmd:
```cmd
dir
cd
type
copy
move
del
ipconfig /all
ping
tracert
nslookup
tasklist
taskkill
netstat -ano
systeminfo
whoami /groups
```
PowerShell:
```powershell
Get-ChildItem
Set-Location
Get-Content
Copy-Item
Move-Item
Remove-Item
Get-Process
Stop-Process
Get-Service
Get-NetIPConfiguration
Resolve-DnsName
Get-NetTCPConnection
Select-String
Where-Object
Sort-Object
Select-Object
```
Linux Bash:
```bash
pwd
ls -la
cd
cat
less
cp
mv
rm
ip a
ip route
ping
grep
find
ps aux
kill
ss -tulpn
chmod
chown
```
---

## 42. Mano paaiškinimas žmonių kalba
GUI yra kaip važiuoti automatu mieste.
CLI yra kaip mokėti mechaniką ir pavaras.
Cmd:
> senas, paprastas Windows įrankių rinkinys.
PowerShell:
> modernus Windows admin peilis su daug ašmenų.
Bash:
> Linux darbo arklys.
Pipeline:
> vienas įrankis perduoda rezultatą kitam įrankiui.
Script:
> užrašyta komandų seka, kad nereikėtų kiekvieną kartą ranka kartoti.
Process:
> programa, kuri dabar veikia.
Service:
> background mechanizmas.
Svarbiausia:
> Komandos nėra burtažodžiai. Jos yra klausimai sistemai arba nurodymai sistemai.
---

## 43. Ką būtina atsiminti
- CLI = tekstinė sąsaja su sistema.
- Terminalas yra langas, shell interpretuoja komandas.
- cmd yra senesnis Windows shell.
- PowerShell yra moderni shell + scripting kalba.
- Bash yra dažniausias Linux shell mokymuisi.
- PowerShell komandos dažnai naudoja `Verb-Noun`.
- Bash/cmd pipeline dažniausiai perduoda text.
- PowerShell pipeline perduoda objects.
- `dir`, `ls`, `Get-ChildItem` visi listina turinį pagal savo aplinką.
- `type`, `Get-Content`, `cat` skaito failą.
- `copy/Copy-Item/cp` kopijuoja.
- `move/Move-Item/mv` perkelia/pervadina.
- `del/Remove-Item/rm` trina.
- `ipconfig /all`, `Get-NetIPConfiguration`, `ip a` rodo network info.
- `tasklist`, `Get-Process`, `ps aux` rodo procesus.
- `findstr`, `Select-String`, `grep` ieško teksto.
- `>` perrašo, `>>` prideda.
- Scripts leidžia automatizuoti veiksmus.
- Execution Policy PowerShell nėra pilna security boundary.
- Bash kintamiesiems negalima dėti tarpų aplink `=`.
- Shell access cyber kontekste turi būti praktikuojamas tik legaliose lab aplinkose.
---

## 44. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- advanced PowerShell remoting
- WinRM exploitation
- PowerShell obfuscation
- AMSI bypass
- advanced Bash traps/signals
- zsh/fish customization
- advanced regex
- sed/awk gylį
- PowerShell classes/modules
- Windows internals
- advanced shell stabilisation
- offensive payload crafting
Dabar svarbiausia:
> Mokėti naudoti cmd, PowerShell ir Bash basic komandų lygmeniu, suprasti pipeline, filtruoti output, valdyti failus/procesus/network info ir pradėti scripting pagrindus.
---

## 45. Mini testas sau
Atsakyk nežiūrėdamas:
1. Kuo terminalas skiriasi nuo shell?
2. Kuo cmd skiriasi nuo PowerShell?
3. Kodėl PowerShell pipeline yra stipresnis už paprastą text pipeline?
4. Kas yra Bash?
5. Kuo Linux shell skiriasi nuo Linux komandų?
6. Kaip Windows cmd parodyti folderio turinį?
7. Kaip PowerShell parodyti folderio turinį?
8. Kaip Linux parodyti folderio turinį?
9. Kaip cmd perskaityti failą?
10. Kaip PowerShell perskaityti failą?
11. Kaip Linux perskaityti failą?
12. Kaip Windows cmd pažiūrėti IP config?
13. Kaip PowerShell pažiūrėti IP config?
14. Kaip Linux pažiūrėti IP?
15. Kaip cmd rasti DNS info?
16. Kaip PowerShell rasti DNS info?
17. Kaip Linux rasti DNS info?
18. Kaip cmd parodyti procesus?
19. Kaip PowerShell parodyti procesus?
20. Kaip Linux parodyti procesus?
21. Ką daro `taskkill`?
22. Ką daro `Stop-Process`?
23. Ką daro `kill`?
24. Ką daro `findstr`?
25. Ką daro `Select-String`?
26. Ką daro `grep`?
27. Ką reiškia `>`?
28. Ką reiškia `>>`?
29. Kas yra `.bat` failas?
30. Kas yra `.ps1` failas?
31. Kas yra `.sh` failas?
32. Kam skirtas shebang?
33. Kodėl Bash variable negali turėti tarpų aplink `=`?
34. Kas yra `$1` Bash script’e?
35. Kas yra PowerShell Execution Policy?
36. Kuo reverse shell skiriasi nuo normal shell sąvokos?
37. Kodėl command-line žinios svarbios IT Support?
38. Kodėl command-line žinios svarbios cyber?
---

## 46. Mano verdiktas
Šitas blokas yra labai praktiškas tavo perėjimui į IT/cyber.
Statusas:
```txt
Status: Foundation / Practical CLI
Depth: Low-Medium
Practical value: Very High
Revisit: verta prieš IT Support CV, SOC basics, Windows admin, Linux labs ir scripting
```
Svarbiausia mintis:
> CLI yra ne “hacker estetika”. Tai darbo įrankis, leidžiantis greitai rinkti informaciją, valdyti sistemą, automatizuoti veiksmus ir troubleshootinti.
Karjeros prasme:
> IT Support žmogus, kuris moka cmd/PowerShell basics, jau atrodo rimčiau už žmogų, kuris moka tik spaudyti Settings langą.
Cyber prasme:
> Shell supratimas yra būtinas, nes beveik visi rimtesni veiksmai galiausiai atsiremia į komandų vykdymą, output analizę ir automatizaciją.
Core mental model:
```txt
cmd = Windows basic legacy CLI
PowerShell = Windows modern admin/scripting powerhouse
Bash = Linux shell pagrindas
Pipeline = komandų jungimas
Script = komandų automatizavimas
Shell = vieta, kur OS pradeda kalbėti tekstu
```
Jeigu šitą bloką supranti, terminalas tampa ne scary black box, o normalus darbo stalas.
