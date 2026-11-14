# JavaScript Essentials

## Intro

JavaScript (JS) yra populiari scripting kalba, leidžianti web developeriam pridėti interaktyvius elementus į puslapius, sukurtus naudojant HTML ir CSS.

- **HTML** – puslapio struktūra.
- **CSS** – puslapio išvaizda.
- **JavaScript** – puslapio elgesys ir logika.

Su JavaScript galima pridėti:

- Formų validation.
- `onClick` veiksmus.
- Animacijas.
- HTML elementų pakeitimus.
- Duomenų siuntimą ir gavimą iš serverio.

---

## Essential Concepts

### Variables

Variables yra pavadinti konteineriai, kuriuose saugomos reikšmės.

    const username = "Ignelis";
    let score = 10;

JavaScript turi tris pagrindinius variable kūrimo būdus:

#### `var`

`var` yra senesnis būdas kurti variables. Jis yra **function-scoped**, todėl nėra apribotas paprasto `{ }` bloko.

    if (true) {
        var message = "Labas";
    }

    console.log(message); // "Labas"

Jeigu `var` sukurtas funkcijoje, jis egzistuoja tik toje funkcijoje:

    function test() {
        var secret = "password";
    }

    console.log(secret); // klaida

Šiuolaikiniame JS geriau naudoti `let` arba `const`, bet `var` reikia mokėti atpažinti sename kode.

#### `let`

`let` naudojamas, kai variable reikšmė vėliau gali pasikeisti.

    let score = 10;
    score = 20;

`let` yra **block-scoped** – variable egzistuoja tik tame `{ }` bloke, kuriame buvo sukurtas.

    if (true) {
        let message = "Labas";
        console.log(message); // veikia
    }

    console.log(message); // klaida

#### `const`

`const` naudojamas, kai variable nebus priskirta nauja reikšmė.

    const username = "Ignelis";
    username = "Bones"; // klaida

`const` taip pat yra **block-scoped**.

Jeigu `const` saugo array arba object, jų viduje esančias reikšmes galima keisti, tačiau negalima viso variable pakeisti kitu:

    const numbers = [101, 102];

    numbers.push(103);    // galima
    numbers[0] = 999;     // galima
    numbers = [500, 600]; // negalima

Praktinė taisyklė:

- Pirmiausia rinktis `const`.
- Jeigu reikšmė keisis – naudoti `let`.
- `var` dažniausiai nenaudoti.

---

### Data Types

Data type nustato, kokio tipo reikšmę laiko variable.

#### String

Tekstas, rašomas tarp kabučių:

    const username = "Ignelis";

#### Number

Sveiki arba dešimtainiai skaičiai:

    const age = 33;
    const price = 10.99;

#### Boolean

Turi tik dvi galimas reikšmes:

    const isAdmin = true;
    const isBlocked = false;

#### Null

Variable egzistuoja, bet jam specialiai priskirta „tuščia“ reikšmė:

    const selectedUser = null;

#### Undefined

Variable egzistuoja, bet jam dar nepriskirta reikšmė:

    let password;
    console.log(password); // undefined

`undefined` taip pat gaunamas bandant pasiekti neegzistuojančią array poziciją.

#### Array

Sąrašas, kuriame galima laikyti kelias reikšmes:

    const rollNumbers = [101, 102, 103];

Array pozicijos skaičiuojamos nuo `0`:

    rollNumbers[0]; // 101
    rollNumbers[1]; // 102
    rollNumbers[2]; // 103
    rollNumbers[3]; // undefined

#### Object

Viename variable leidžia laikyti kelias susijusias reikšmes:

    const user = {
        username: "Ignelis",
        age: 33,
        isAdmin: false
    };

Reikšmės pasiekiamos naudojant property vardą:

    console.log(user.username); // Ignelis

---

### Functions

Function yra kodo blokas, sukurtas atlikti konkrečią užduotį. Funkcija leidžia tą pačią logiką panaudoti kelis kartus nekopijuojant viso kodo.

    function PrintResult(rollNum) {
        alert("Student with roll number " + rollNum + " passed the exam");
    }

Šita dalis tik **sukuria** funkciją. Kol jos neiškviečiame, nieko neįvyksta.

Funkcijos iškvietimas:

    PrintResult(101);

Kas įvyksta:

1. `101` perduodamas funkcijai.
2. Funkcijos viduje `rollNum` tampa `101`.
3. `alert()` parodo pranešimą su studento numeriu.

`rollNum` yra **parameter** – laikinas vardas reikšmei, kurią funkcija priims.

`101` yra **argument** – konkreti reikšmė, kuri perduodama kviečiant funkciją.

    PrintResult(777);

Šį kartą `rollNum` reikšmė bus `777`.

---

### Loops

Loops leidžia kartoti kodo bloką tol, kol nustatyta condition yra `true`.

Dažniausi JavaScript loops:

- `for`
- `while`
- `do...while`
- `for...of`

#### `for` loop

    const rollNumbers = [101, 102, 103];

    for (let i = 0; i < rollNumbers.length; i++) {
        PrintResult(rollNumbers[i]);
    }

`for` struktūra:

    for (pradžia; sąlyga; pakeitimas)

Šiame pavyzdyje:

- `let i = 0` – pradėti nuo `0`.
- `i < rollNumbers.length` – kartoti, kol `i` mažesnis už array ilgį.
- `i++` – po kiekvieno rato prie `i` pridėti `1`.

Loop vykdymas:

1. `i = 0` → `rollNumbers[0]` → `101`.
2. `i = 1` → `rollNumbers[1]` → `102`.
3. `i = 2` → `rollNumbers[2]` → `103`.
4. `i = 3` → sąlyga `3 < 3` yra `false` → loop sustoja.

THM pateiktame pavyzdyje buvo:

    for (let i = 0; i < 100; i++) {
        PrintResult(rollNumbers[i]);
    }

Array turi tik tris reikšmes, todėl:

- Pirmi 3 iškvietimai gauna `101`, `102`, `103`.
- Likę 97 iškvietimai gauna `undefined`.

Tai parodo, kas nutinka bandant pasiekti neegzistuojančias array pozicijas, tačiau praktiškai geriau naudoti `rollNumbers.length`.

#### `for...of`

Paprastesnis būdas pereiti per visas array reikšmes:

    for (const rollNum of rollNumbers) {
        PrintResult(rollNum);
    }

Tai reiškia: kiekvienai `rollNumbers` reikšmei paleisti `PrintResult()`.

#### `while`

`while` pirmiausia patikrina condition ir tik tada vykdo kodą:

    let i = 0;

    while (i < 3) {
        console.log(i);
        i++;
    }

Rezultatas:

    0
    1
    2

Jeigu condition nuo pradžių yra `false`, kodas nepasileis nė karto.

#### `do...while`

`do...while` pirmiausia paleidžia kodą ir tik tada patikrina condition:

    let i = 10;

    do {
        console.log(i);
        i++;
    } while (i < 3);

Nors `10 < 3` yra `false`, kodas vis tiek paleidžiamas vieną kartą ir parodo `10`.

---

## Request-Response Cycle

Request-response cycle vyksta, kai userio browseris, veikiantis kaip **client**, siunčia requestą web serveriui, o serveris grąžina response.

Pagrindinis procesas:

1. Useris browseryje atidaro URL arba paspaudžia mygtuką.
2. Browseris siunčia HTTP requestą serveriui.
3. Serveris requestą gauna ir apdoroja.
4. Serveris grąžina HTTP response.
5. Browseris atvaizduoja HTML arba JavaScript apdoroja gautus duomenis.

Response gali turėti:

- HTML puslapį.
- CSS arba JavaScript failą.
- Nuotrauką ar kitą resource.
- JSON duomenis.
- Error message.

JavaScript gali veikti abiejose pusėse:

- **Client-side JavaScript** – veikia browseryje ir valdo puslapio interaktyvumą.
- **Server-side JavaScript** – veikia serveryje, pvz., naudojant Node.js, priima requestus ir grąžina responses.
