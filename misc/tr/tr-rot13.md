# `tr` ir ROT13 trumpas paaiškinimas

## Kas yra `tr`?

`tr` reiškia **translate**.

Jis keičia simbolius pagal lentelę:

`tr 'ką_keisti' 'į_ką_keisti'`

Pvz.:

`echo abc | tr 'abc' 'xyz'`

Reikšmė:

- `a` pakeičia į `x`
- `b` pakeičia į `y`
- `c` pakeičia į `z`

Rezultatas būtų:

`xyz`

---

## Kas yra ROT13?

ROT13 reiškia, kad raidės yra pasuktos per **13 pozicijų** anglų alfabete.

Anglų alfabetas turi 26 raides:

`ABCDEFGHIJKLMNOPQRSTUVWXYZ`

Kadangi 26 / 2 = 13, ROT13 persuka pusę alfabeto:

`A → N`

`B → O`

`C → P`

...

`M → Z`

Tada apsisuka:

`N → A`

`O → B`

...

`Z → M`

Tas pats vyksta su mažosiomis raidėmis:

`a → n`

`b → o`

...

`z → m`

---

## Kodėl ROT13 prasideda nuo `N`?

Nes nuo `A` pajudėjus 13 raidžių į priekį gaunasi `N`.

Jeigu būtų ROT10, tada būtų:

`A → K`

Ir tada map’as būtų kitoks.

ROT13 atveju:

`A-Z` keičiama į `N-ZA-M`

Tai reiškia:

`ABCDEFGHIJKLMNOPQRSTUVWXYZ`

paverčiama į:

`NOPQRSTUVWXYZABCDEFGHIJKLM`

---

## Komanda ROT13 decode/encode

`tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt`

Kas čia vyksta:

- `'A-Za-z'` = visos didžiosios ir mažosios raidės
- `'N-ZA-Mn-za-m'` = tos pačios raidės, tik pasuktos per 13
- `< data.txt` = paduoda `data.txt` turinį į `tr`

---

## Išskaidymas

`A-Z`

reiškia:

`ABCDEFGHIJKLMNOPQRSTUVWXYZ`

`a-z`

reiškia:

`abcdefghijklmnopqrstuvwxyz`

Todėl:

`A-Za-z`

reiškia:

`ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`

---

`N-ZA-M`

reiškia:

`NOPQRSTUVWXYZABCDEFGHIJKLM`

`n-za-m`

reiškia:

`nopqrstuvwxyzabcdefghijklm`

Todėl:

`N-ZA-Mn-za-m`

reiškia:

`NOPQRSTUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm`

---

## Kaip `tr` suporuoja raides

`tr` ima pirmą raidę iš pirmo rinkinio ir pakeičia ją pirma raide iš antro rinkinio.

Pavyzdys:

`A-Za-z`

lygina su:

`N-ZA-Mn-za-m`

Tai gaunasi:

- `A → N`
- `B → O`
- `C → P`
- `D → Q`
- ...
- `M → Z`
- `N → A`
- `O → B`
- ...
- `Z → M`
- `a → n`
- `b → o`
- ...
- `z → m`

---

## Kodėl ta pati komanda ir decode, ir encode?

ROT13 yra simetriškas.

Nes:

`13 + 13 = 26`

O anglų alfabetas turi 26 raides.

Tai jeigu tekstas jau pasuktas per 13, dar kartą prasukus per 13 grįžta originalas.

Todėl ta pati komanda tinka ir užkoduoti, ir atkoduoti:

`tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt`

---

## Mental model

`data.txt`

↓  

tekstas, kur raidės pasuktos per 13

↓  

`tr` pakeičia kiekvieną raidę pagal ROT13 lentelę

↓  

gauni normalų readable tekstą / passwordą

---

## Svarbiausia prisiminti

`tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt`

reiškia:

> Paimk visas didžiąsias ir mažąsias raides ir pakeisk jas į tas pačias raides, tik pasuktas per 13 pozicijų.
