# THM – Linux Fundamentals Part 1–3

## 1. Room esmė
Šitie trys roomai yra Linux pagrindų stuburas.
Pagrindinė mintis:
> Linux nėra “hacker terminal magic”. Tai OS, kurią reikia mokėti valdyti tekstu: judėti per failus, skaityti output, ieškoti, kopijuoti, keisti permissions, jungtis per SSH, valdyti procesus, diegti packages ir skaityti logs.
Šitas notes apjungia:
- Linux background
- terminalo pagrindus
- filesystem navigation
- file search
- shell operators
- SSH
- flags ir arguments
- copy/move/delete
- permissions
- common directories
- scripts/executables
- text editors
- useful utilities
- processes
- automation
- package management
- logs
Cybersecurity kontekste Linux yra būtinas, nes:
- dauguma tools veikia Linux’e
- dauguma serverių yra Linux/Unix tipo
- THM/CTF/lab’ai remiasi terminalu
- enumeration, exploitation ir post-exploitation dažnai vyksta per shell
- logs, permissions, processes ir services yra kasdienė security realybė
---

## 2. Kas yra Linux?
Linux yra operacinė sistema.
Paprastai:
> Linux = OS, kuri valdo hardware, failus, procesus, users, networking ir programas.
Linux naudojamas:
- serveriuose
- cloud infrastructure
- Android
- embedded devices
- routers
- supercomputers
- cybersecurity labs
- pentesting distros
- IoT
- development systems
Svarbu:
> Linux nėra viena konkreti OS kaip Windows. Linux turi daug distribucijų.
Distribucijos:
- Ubuntu
- Debian
- Kali Linux
- Parrot OS
- Fedora
- Arch
- Mint
- CentOS/RHEL
Distribucija = Linux kernel + tools + package manager + default software + configuration.
---

## 3. Terminalas
Terminalas leidžia valdyti sistemą komandomis.
Paprastai:
> Terminal = tekstinė sąsaja su sistema.
Shell yra programa, kuri interpretuoja tavo komandas.
Dažnas shell:
```bash
bash
```
Prompt dažnai atrodo taip:
```bash
user@machine:~$
```
Reiškia:
- `user` = dabartinis useris
- `machine` = hostname
- `~` = dabartinė lokacija, dažnai home directory
- `$` = normal user
- `#` = root user
Svarbu:
> Terminalas nėra baisus. Jis tiesiog griežtas. Jei kelias arba komanda netiksli, jis tavęs negelbės.
---

## 4. Pirmos komandos
Pažiūrėti kas esi:
```bash
whoami
```
Pažiūrėti hostname:
```bash
hostname
```
Pažiūrėti dabartinę direktoriją:
```bash
pwd
```
Išvalyti terminalą:
```bash
clear
```
Arba:
```bash
Ctrl + L
```
Pažiūrėti komandų istoriją:
```bash
history
```
Paleisti komandą iš history pagal numerį:
```bash
!123
```
Išeiti iš terminal session:
```bash
exit
```
---

## 5. Filesystem navigation
Linux filesystem yra hierarchinis medis.
Root direktorija:
```bash
/
```
Home direktorija:
```bash
~
```
Dabartinė direktorija:
```bash
.
```
Viena direktorija aukštyn:
```bash
..
```
Judėti į direktoriją:
```bash
cd /path/to/folder
```
Į home:
```bash
cd ~
```
Arba:
```bash
cd
```
Viena direktorija atgal:
```bash
cd ..
```
Į root:
```bash
cd /
```
Grįžti į ankstesnę direktoriją:
```bash
cd -
```
Svarbi mintis:
> `pwd` pasako kur esi, `ls` parodo kas ten yra, `cd` pakeičia vietą.
---

## 6. Failų ir folderių listinimas
Paprastas listinimas:
```bash
ls
```
Rodyti daugiau info:
```bash
ls -l
```
Rodyti hidden failus:
```bash
ls -a
```
Rodyti viską detaliai:
```bash
ls -la
```
Human readable sizes:
```bash
ls -lh
```
Detaliai + hidden + human readable:
```bash
ls -lah
```
Pavyzdys:
```bash
drwxr-xr-x 2 user user 4096 May 27 10:00 Documents
-rw-r--r-- 1 user user  120 May 27 10:01 notes.md
```
Pirmas simbolis:
- `d` = directory
- `-` = file
- `l` = symbolic link
---

## 7. Failų skaitymas
Parodyti visą failą:
```bash
cat file.txt
```
Skaityti puslapiais:
```bash
less file.txt
```
Išeiti iš `less`:
```bash
q
```
Pirmas 10 eilučių:
```bash
head file.txt
```
Paskutinės 10 eilučių:
```bash
tail file.txt
```
Stebėti failą live:
```bash
tail -f file.log
```
Su line numbers:
```bash
cat -n file.txt
```
Gražesnis `cat` variantas, jei įdiegtas:
```bash
batcat file.md
```
Be wrap:
```bash
batcat --wrap=never file.md
```
Svarbu:
> `cat` geras mažiems failams. Dideliems failams naudok `less`, nes kitaip terminalas apsivems tekstu.
---

## 8. Failų ir folderių kūrimas
Sukurti tuščią failą:
```bash
touch notes.txt
```
Sukurti folderį:
```bash
mkdir Cyber
```
Sukurti nested folderius:
```bash
mkdir -p Cyber/Notes/Linux
```
`-p` reiškia:
> sukurk ir parent directories, jei jų dar nėra.
Pavyzdys:
```bash
mkdir -p ~/Documents/Cyber-Notes/Pre-Security/Linux-Fundamentals
```
---

## 9. Copy, move, rename, delete
Kopijuoti failą:
```bash
cp source.txt destination.txt
```
Kopijuoti failą į folderį:
```bash
cp notes.md ~/Documents/
```
Kopijuoti folderį:
```bash
cp -r folder/ backup-folder/
```
Perkelti failą:
```bash
mv source.txt ~/Documents/
```
Pervadinti failą:
```bash
mv oldname.txt newname.txt
```
Perkelti ir pervadinti:
```bash
mv old.txt ~/Documents/new.txt
```
Ištrinti failą:
```bash
rm file.txt
```
Ištrinti folderį su turiniu:
```bash
rm -r folder/
```
Force delete:
```bash
rm -rf folder/
```
Svarbu:
> `mv` ir perkelia, ir pervadina. Jeigu destination folderio nėra, `mv` gali pervadinti failą į tą pavadinimą. Čia klasikinė vieta pamesti failą.
Saugiau:
```bash
ls -la source
ls -la destination-folder/
mv source destination-folder/
ls -la destination-folder/
```
---

## 10. Absolute ir relative paths
Absolute path prasideda nuo `/`.
Pvz:
```bash
/home/ignelis/Documents/Cyber-Notes
```
Relative path priklauso nuo to, kur dabar esi.
Pvz:
```bash
Documents/Cyber-Notes
```
Home shortcut:
```bash
~
```
Pvz:
```bash
~/Documents/Cyber-Notes
```
Svarbi mintis:
> Jei nesi tikras kur esi, naudok `pwd`. Jei nenori spėlioti, naudok absolute path arba `~/`.
---

## 11. Failų paieška su find
Ieškoti nuo dabartinės direktorijos:
```bash
find . -name "file.txt"
```
Case-insensitive:
```bash
find . -iname "file.txt"
```
Ieškoti home:
```bash
find ~ -iname "osi.md"
```
Ieškoti pagal pattern:
```bash
find ~ -iname "*osi*"
```
Ieškoti tik failų:
```bash
find ~ -type f -iname "*osi*.md"
```
Ieškoti tik folderių:
```bash
find ~ -type d -iname "*cyber*"
```
Ieškoti pagal size:
```bash
find . -type f -size 33c
```
Ieškoti pagal user:
```bash
find . -type f -user bandit7
```
Ieškoti pagal group:
```bash
find . -type f -group bandit6
```
Kombinuotas pavyzdys:
```bash
find . -type f -size 33c -user bandit7 -group bandit6
```
Ieškoti failų modifikuotų per paskutines 2 val:
```bash
find ~ -type f -mmin -120
```
Svarbi mintis:
> `find` eina per filesystem ir ieško pagal sąlygas. Sąlygas galima kombinuoti kaip filtrus.
---

## 12. Paieška failo viduje su grep
Ieškoti teksto faile:
```bash
grep "word" file.txt
```
Case-insensitive:
```bash
grep -i "word" file.txt
```
Rodyti line numbers:
```bash
grep -n "word" file.txt
```
Recursive search:
```bash
grep -R "word" .
```
Recursive + case-insensitive + line numbers:
```bash
grep -Rin "password" .
```
Rasti failus, kuriuose yra tekstas:
```bash
grep -Ril "Open Systems Interconnection" ~
```
Ignoruoti klaidas:
```bash
grep -Ril "text" ~ 2>/dev/null
```
Ieškoti kelių žodžių:
```bash
grep -Rin "password\|admin\|token\|secret" .
```
Svarbu:
> `find` ieško failų pagal properties/pavadinimą. `grep` ieško teksto failų viduje.
---

## 13. Shell operators
Output redirect į failą:
```bash
echo "hello" > file.txt
```
`>` perrašo failą.
Append į failą:
```bash
echo "new line" >> file.txt
```
`>>` prideda prie galo.
Pipe:
```bash
cat file.txt | grep "admin"
```
Pipe siunčia vienos komandos output į kitą komandą.
Command chaining su `&&`:
```bash
mkdir test && cd test
```
Antra komanda vykdoma tik jei pirma pavyko.
Command chaining su `;`:
```bash
pwd; ls
```
Abi komandos vykdomos iš eilės.
Background process:
```bash
sleep 60 &
```
Svarbi mintis:
> Shell operators leidžia jungti komandas į mažus workflow.
---

## 14. Flags, switches ir arguments
Komanda dažnai turi:
```bash
command -flag argument
```
Pvz:
```bash
ls -la /home
```
Čia:
- `ls` = command
- `-la` = flags
- `/home` = argument/path
Short flag:
```bash
ls -a
```
Long flag:
```bash
ls --all
```
Help:
```bash
command --help
```
Manual:
```bash
man command
```
Pvz:
```bash
man ls
man find
man grep
```
Ieškoti man puslapyje:
```txt
/keyword
```
Išeiti:
```txt
q
```
Svarbi mintis:
> Flags keičia kaip komanda veikia. Arguments pasako su kuo komanda dirba.
---

## 15. SSH
SSH reiškia:
> Secure Shell
Paprastai:
> SSH = saugus būdas prisijungti prie remote Linux terminalo.
Sintaksė:
```bash
ssh user@ip
```
Pvz:
```bash
ssh tryhackme@10.10.10.10
```
Su portu:
```bash
ssh user@ip -p 2222
```
Pirmą kartą jungiantis gali klausti dėl host key:
```txt
Are you sure you want to continue connecting?
```
Jei tai tavo legalus lab targetas, rašai:
```txt
yes
```
Kopijuoti failą per SSH su `scp`:
```bash
scp file.txt user@ip:/home/user/
```
Parsisiųsti failą:
```bash
scp user@ip:/home/user/file.txt .
```
Svarbu:
> SSH leidžia valdyti remote machine kaip terminalą. THM Part 2 nuo čia pradeda normalų remote Linux darbą.
---

## 16. Permissions 101
Linux permissions rodo, kas gali skaityti, rašyti ir vykdyti failą.
Pvz:
```bash
-rwxr-xr--
```
Skirstoma į tris grupes:
```txt
owner | group | others
rwx     r-x     r--
```
Reikšmės:
- `r` = read
- `w` = write
- `x` = execute
- `-` = permission nėra
Pavyzdys:
```bash
-rw-r--r-- 1 user user file.txt
```
Reiškia:
- owner gali read/write
- group gali read
- others gali read
Folderiui `x` reiškia:
> gali įeiti/traverse’inti į direktoriją.
Failui `x` reiškia:
> gali vykdyti kaip programą/scriptą.
---

## 17. chmod
Pridėti execute permission:
```bash
chmod +x script.sh
```
Nuimti execute:
```bash
chmod -x script.sh
```
Owner read/write/execute, group read/execute, others read/execute:
```bash
chmod 755 script.sh
```
Owner read/write, group read, others read:
```bash
chmod 644 file.txt
```
Skaičiai:
```txt
r = 4
w = 2
x = 1
```
Todėl:
```txt
7 = rwx
6 = rw-
5 = r-x
4 = r--
```
Dažni:
```bash
chmod 755 script.sh
chmod 644 notes.txt
```
Svarbu:
> Neduok `777` iš įpročio. Tai “visiems viską” ir dažnai yra bloga praktika.
---

## 18. chown
Keisti owner:
```bash
sudo chown user file.txt
```
Keisti owner ir group:
```bash
sudo chown user:group file.txt
```
Recursive:
```bash
sudo chown -R user:group folder/
```
Pažiūrėti owner/group:
```bash
ls -l
```
Svarbu:
> `chmod` keičia permissions, `chown` keičia kam failas priklauso.
---

## 19. Common directories
Root:
```bash
/
```
Viso filesystem pradžia.
Home users:
```bash
/home
```
Tavo home:
```bash
/home/username
```
Config files:
```bash
/etc
```
Logs:
```bash
/var/log
```
Temporary files:
```bash
/tmp
```
Binaries:
```bash
/bin
/usr/bin
```
System/admin binaries:
```bash
/sbin
/usr/sbin
```
Devices:
```bash
/dev
```
Processes/system info:
```bash
/proc
```
Boot files:
```bash
/boot
```
Root user home:
```bash
/root
```
Mounted devices:
```bash
/mnt
/media
```
Svarbu:
> `/etc` = configs, `/var/log` = logs, `/home` = users, `/tmp` = temporary, `/bin` ir `/usr/bin` = commands/programs.
---

## 20. Scripts ir executables
Script failas gali atrodyti taip:
```bash
#!/bin/bash
echo "Hello"
```
Išsaugai kaip:
```bash
hello.sh
```
Padarai executable:
```bash
chmod +x hello.sh
```
Paleidi:
```bash
./hello.sh
```
Kodėl `./`?
> Nes dabartinė direktorija pagal nutylėjimą dažnai nėra PATH.
Paleisti su bash:
```bash
bash hello.sh
```
Pažiūrėti PATH:
```bash
echo $PATH
```
Svarbi mintis:
> Scriptas turi turėti execute permission arba būti paleistas per interpreterį, pvz `bash script.sh`.
---

## 21. Text editors
Terminaliniai editoriai:
- nano
- vim
- vi
Nano paprastesnis:
```bash
nano notes.md
```
Išsaugoti nano:
```txt
Ctrl + O
Enter
```
Išeiti:
```txt
Ctrl + X
```
Vim atidaryti:
```bash
vim file.txt
```
Vim insert mode:
```txt
i
```
Išeiti iš insert:
```txt
Esc
```
Išsaugoti ir išeiti:
```txt
:wq
```
Išeiti be saugojimo:
```txt
:q!
```
Svarbu:
> Nano geras greitam darbui. Vim galingas, bet pradžioj gali atrodyti kaip užrakintas kambarys be durų.
---

## 22. Useful utilities
Parsisiųsti failą:
```bash
wget http://example.com/file.txt
```
Curl HTTP requestams:
```bash
curl http://example.com
```
Rodyti headers:
```bash
curl -I http://example.com
```
Failų checksum:
```bash
md5sum file.txt
sha256sum file.txt
```
Pažiūrėti failo tipą:
```bash
file file.txt
```
Skaičiuoti eilutes/žodžius/bytes:
```bash
wc file.txt
```
Tik eilutės:
```bash
wc -l file.txt
```
Sort:
```bash
sort file.txt
```
Unikalioms eilutėms:
```bash
uniq file.txt
```
Dažnas pattern:
```bash
sort file.txt | uniq
```
Skaičiuoti pasikartojimus:
```bash
sort file.txt | uniq -c
```
Rasti kur yra komanda:
```bash
which nmap
```
Rasti command info:
```bash
whereis nmap
```
Rodyti calendar:
```bash
cal
```
Rodyti date:
```bash
date
```
---

## 23. Python simple HTTP server
Paleisti HTTP serverį dabartinėje direktorijoje:
```bash
python3 -m http.server
```
Default port:
```txt
8000
```
Su konkrečiu portu:
```bash
python3 -m http.server 9000
```
Tada failai pasiekiami:
```txt
http://IP:8000/
```
Svarbu:
> Serverį reikia paleisti tame machine/folderyje, iš kurio nori serve’inti failus.
Jeigu paleidi serverį AttackBox’e, o bandai traukti iš target IP, neveiks. Klasikinis THM galvos laužymas.
---

## 24. Processes 101
Procesas = veikianti programa.
Pažiūrėti procesus:
```bash
ps
```
Daugiau info:
```bash
ps aux
```
Live process viewer:
```bash
top
```
Jei įdiegtas:
```bash
htop
```
Rasti procesą:
```bash
ps aux | grep firefox
```
Process ID:
```txt
PID
```
Nužudyti procesą:
```bash
kill PID
```
Force kill:
```bash
kill -9 PID
```
Nužudyti pagal pavadinimą:
```bash
pkill processname
```
Background:
```bash
command &
```
Jobs:
```bash
jobs
```
Grąžinti į foreground:
```bash
fg
```
Suspend:
```txt
Ctrl + Z
```
Nutraukti:
```txt
Ctrl + C
```
Svarbu:
> `Ctrl+C` stabdo procesą. `Ctrl+Z` tik suspendina. Tai ne tas pats.
---

## 25. Services
Service = background procesas, dažnai valdantis kokią nors sistemos funkciją.
Pvz:
- ssh
- apache2
- nginx
- cron
- networking
Systemd sistemose valdoma su `systemctl`.
Patikrinti statusą:
```bash
systemctl status ssh
```
Start:
```bash
sudo systemctl start ssh
```
Stop:
```bash
sudo systemctl stop ssh
```
Restart:
```bash
sudo systemctl restart ssh
```
Enable on boot:
```bash
sudo systemctl enable ssh
```
Disable on boot:
```bash
sudo systemctl disable ssh
```
Svarbu:
> Process yra veikianti programa. Service dažnai yra managed background procesas.
---

## 26. Automation su cron
Cron naudojamas planuoti automatinį komandų/scriptų vykdymą.
Paprastai:
> cron = Linux scheduler.
Redaguoti user crontab:
```bash
crontab -e
```
Pažiūrėti crontab:
```bash
crontab -l
```
Cron format:
```txt
minute hour day-of-month month day-of-week command
```
Pvz kasdien 09:00:
```cron
0 9 * * * /home/user/script.sh
```
Kas minutę:
```cron
* * * * * /home/user/script.sh
```
Kas valandą:
```cron
0 * * * * /home/user/script.sh
```
Kas sekmadienį 02:30:
```cron
30 2 * * 0 /home/user/backup.sh
```
Svarbu:
> Cron naudoja ribotą environment. Scriptuose geriau naudoti absolute paths.
Security mintis:
> Blogai sukonfigūruoti cron jobs gali būti privilege escalation vektorius.
---

## 27. Package management
Package manager diegia, atnaujina ir šalina software.
Debian/Ubuntu naudoja:
```bash
apt
```
Update package list:
```bash
sudo apt update
```
Upgrade installed packages:
```bash
sudo apt upgrade
```
Install package:
```bash
sudo apt install package-name
```
Remove package:
```bash
sudo apt remove package-name
```
Search package:
```bash
apt search package-name
```
Show package info:
```bash
apt show package-name
```
Svarbu:
> `apt update` neatnaujina programų. Jis atnaujina package list. `apt upgrade` realiai atnaujina installed packages.
---

## 28. Repositories ir sources
Linux packages dažniausiai ateina iš repositories.
Repository = software šaltinis.
APT sources dažnai yra:
```bash
/etc/apt/sources.list
/etc/apt/sources.list.d/
```
GPG keys naudojami patikrinti package/source trusted status.
Svarbu:
> Nedėk random repositories aklai. Supply chain rizika reali.
Praktinė taisyklė:
```txt
Jei nesuprantu iš kur package ateina, nestatau jo ant savo pagrindinės sistemos.
```
---

## 29. Logs
Logs saugo įvykius sistemoje.
Dažna vieta:
```bash
/var/log
```
Pažiūrėti:
```bash
ls -la /var/log
```
Svarbūs logai:
```txt
/var/log/auth.log
/var/log/syslog
/var/log/kern.log
/var/log/apache2/
/var/log/nginx/
```
Skaityti:
```bash
less /var/log/syslog
```
Stebėti live:
```bash
tail -f /var/log/syslog
```
Ieškoti auth įvykių:
```bash
grep -i "failed" /var/log/auth.log
```
Systemd logs:
```bash
journalctl
```
Follow live:
```bash
journalctl -f
```
Konkrečiam service:
```bash
journalctl -u ssh
```
Nuo šiandien:
```bash
journalctl --since today
```
Svarbu:
> Logs yra sistema kalbanti atgal. Kai kažkas neveikia, logs dažnai pasako kodėl.
---

## 30. File permissions ir security
Permissions tiesiogiai susijusios su security.
Blogi pavyzdžiai:
```bash
chmod 777 sensitive.txt
```
```bash
passwords.txt world-readable
```
```bash
script.sh writable by everyone
```
Kodėl blogai?
- kiti useriai gali skaityti sensitive data
- kiti useriai gali pakeisti scriptą
- jei scriptą paleidžia root, gali būti privilege escalation
Gera praktika:
- sensitive files tik owneriui
- scripts neturi būti world-writable
- naudoti least privilege
- tikrinti owner/group
Svarbi mintis:
> Linux security labai daug remiasi permissions ir ownership.
---

## 31. Wildcards
Wildcard `*` reiškia bet kokį tekstą.
Pvz visi `.txt` failai:
```bash
ls *.txt
```
Visi failai su `log` pavadinime:
```bash
ls *log*
```
Kopijuoti visus markdown:
```bash
cp *.md ~/Documents/
```
`?` reiškia vieną simbolį.
Pvz:
```bash
ls file?.txt
```
Svarbu:
> Wildcards išplečia shell prieš komandai pradedant veikti. Todėl visada pagalvok, ką realiai matchina.
---

## 32. Environment variables
Rodyti environment:
```bash
env
```
Rodyti konkretų variable:
```bash
echo $PATH
```
PATH pasako, kur shell ieško komandų.
Pvz:
```bash
/usr/local/bin:/usr/bin:/bin
```
Sukurti temporary variable:
```bash
export NAME=Ignelis
```
Panaudoti:
```bash
echo $NAME
```
Svarbu:
> Jei komanda neveikia be `./`, greičiausiai jos nėra PATH.
---

## 33. Sudo ir root
`sudo` leidžia vykdyti komandą su elevated privileges.
Pvz:
```bash
sudo apt update
```
Root yra admin useris.
Root prompt dažnai:
```bash
#
```
Normal user:
```bash
$
```
Svarbu:
> Su `sudo` galima sutvarkyti sistemą, bet galima ir sudeginti tvartą. Nenaudoti automatiškai, jei nesupranti komandos.
Niekada aklai:
```bash
sudo rm -rf /
```
Čia ne “hacker move”, čia self-destruct button.
---

## 34. Praktinė vertė white hat mokymuisi
Linux Fundamentals tiesiogiai reikalingi:
- THM rooms
- OverTheWire Bandit
- Nmap usage
- Metasploit usage
- SSH into target
- reading logs
- file hunting
- privilege escalation basics
- permissions analysis
- scripts
- package install
- troubleshooting VPN/AttackBox/Warhorse
- web testing tools
- writing notes/GitHub workflow
Praktinis cyber mąstymas:
```txt
Kur aš esu? -> pwd
Kas čia yra? -> ls -la
Kas aš esu? -> whoami
Kokias teises turiu? -> id / ls -l
Kas faile? -> cat / less
Kur failas? -> find
Kur tekstas? -> grep
Kas veikia? -> ps aux / systemctl
Kas nutiko? -> logs
Kaip pasijungti? -> ssh
Kaip atsisiųsti? -> wget/curl/scp
```
---

## 35. Mano paaiškinimas žmonių kalba
Linux terminalas yra kaip dirbtuvės.
`pwd`:
> Pažiūriu kur stoviu.
`ls`:
> Apsižvalgau kas ant lentynų.
`cd`:
> Nueinu į kitą kambarį.
`cat/less`:
> Perskaitau lapą.
`find`:
> Ieškau daikto pagal aprašymą.
`grep`:
> Ieškau konkretaus žodžio lapuose.
`cp`:
> Pasidarau kopiją.
`mv`:
> Perkeliu arba pervadinu.
`rm`:
> Išmetu. Ir šiukšliadėžės dažnai nėra.
`chmod/chown`:
> Nustatau kas gali liesti daiktą.
`ps/top`:
> Žiūriu kas dabar dirba.
`kill`:
> Išjungiu triukšmadarį.
`journalctl/logs`:
> Skaitau kas ką pridirbo.
Svarbiausia:
> Terminalas nėra protingas draugas. Jis tiksliai daro ką pasakai. Net jei pasakai nesąmonę.
---

## 36. Ką būtina atsiminti
- Linux yra OS, plačiai naudojama serveriuose ir cyber.
- Terminalas yra tekstinė sąsaja su sistema.
- `pwd`, `ls`, `cd` yra navigacijos pagrindas.
- `cat`, `less`, `head`, `tail` naudojami failams skaityti.
- `touch`, `mkdir`, `cp`, `mv`, `rm` valdo failus/folderius.
- `find` ieško failų pagal sąlygas.
- `grep` ieško teksto failų viduje.
- Shell operators leidžia jungti komandas.
- Flags keičia komandų veikimą.
- `man` ir `--help` yra tavo draugai.
- SSH leidžia prisijungti prie remote terminalo.
- Permissions susideda iš read/write/execute owner/group/others.
- `chmod` keičia permissions.
- `chown` keičia ownership.
- `/etc` configs, `/var/log` logs, `/home` user files.
- Scriptui reikia execute permission arba interpreterio.
- `nano` paprastas editorius, `vim` galingas bet pradžioje piktas.
- `wget` ir `curl` traukia/siunčia HTTP requests.
- Procesai valdomi su `ps`, `top`, `kill`, `jobs`.
- Services valdomi su `systemctl`.
- Cron automatizuoja komandas.
- APT valdo packages Debian/Ubuntu sistemose.
- Logs padeda troubleshootinti.
---

## 37. Ką galiu pamiršti ir nesukti galvos dabar
Šitam etape nereikia giliai kapstytis į:
- kernel internals
- systemd unit file kūrimą nuo nulio
- advanced bash scripting
- sed/awk gylį
- SELinux/AppArmor
- advanced cron abuse
- package signing internals
- Linux boot process gylį
- filesystem internals
- LVM
- containers
- advanced privilege escalation
Dabar svarbiausia:
> Mokėti judėti per sistemą, skaityti failus, ieškoti, suprasti permissions, jungtis per SSH, valdyti procesus ir skaityti logs.
---

## 38. Mini testas sau
Atsakyk nežiūrėdamas:
1. Ką daro `pwd`?
2. Kuo skiriasi `.` ir `..`?
3. Ką reiškia `~`?
4. Kuo skiriasi absolute ir relative path?
5. Kaip parodyti hidden failus?
6. Kuo skiriasi `cat` ir `less`?
7. Kaip sukurti folderį?
8. Kaip sukurti nested folderius?
9. Kuo `mv` gali būti pavojingas?
10. Kaip ieškoti failo pagal pavadinimą?
11. Kaip ieškoti teksto failų viduje?
12. Ką daro `>`?
13. Ką daro `>>`?
14. Ką daro pipe `|`?
15. Kuo flag skiriasi nuo argument?
16. Kaip prisijungti per SSH?
17. Ką reiškia `rwx`?
18. Kuo failui skiriasi `x` permission nuo folderio `x`?
19. Ką daro `chmod +x`?
20. Ką daro `chown`?
21. Kam skirtas `/etc`?
22. Kam skirtas `/var/log`?
23. Kaip paleisti scriptą?
24. Kaip išeiti iš nano?
25. Kaip išeiti iš vim be saugojimo?
26. Ką daro `wget`?
27. Ką daro `curl -I`?
28. Kaip pažiūrėti procesus?
29. Kuo skiriasi `Ctrl+C` ir `Ctrl+Z`?
30. Kaip patikrinti service statusą?
31. Kam naudojamas cron?
32. Kuo skiriasi `apt update` ir `apt upgrade`?
33. Kur dažniausiai laikomi logs?
34. Kaip skaityti live logą?
35. Kodėl `chmod 777` dažnai bloga idėja?
---

## 39. Mano verdiktas
Linux Fundamentals Part 1–3 yra ne glamūrinė cyber dalis, bet ji yra būtina.
Statusas:
```txt
Status: Foundation
Depth: Low-Medium
Practical value: Very High
Revisit: verta visada, kai stringi THM/Bandit/terminal workflow
```
Svarbiausia mintis:
> Linux terminalas yra darbo aplinka. Jei nemoki judėti, skaityti, ieškoti, keisti permissions ir suprasti output, cyber tools taps tik mygtukų spaudymu.
Core rule:
> Ne mokytis komandas kaip burtažodžius, o suprasti: ką komanda daro, su kuo dirba, ir kokį output grąžina.
Minimalus Linux mental model:
```txt
Kur esu?       pwd
Kas čia yra?   ls -la
Kur eiti?      cd
Kas faile?     cat/less
Kur failas?    find
Kur tekstas?   grep
Kas leidžiama? ls -l
Kas veikia?    ps/top/systemctl
Kas nutiko?    logs/journalctl
```
Jeigu šitą valdai be panikos, Linux pagrindas jau gyvas.
