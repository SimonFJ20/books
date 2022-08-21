
# Hvordan man laver en Lommeregner i Typescript

[**Også på engelsk**](../calculator-in-ts.md)

## Intro

Denne guide er lavet, som en kort guide til at lave en lommeregner.

## Opsætning

### Installer Deno

For at køre din kode skal du bruge en 'runner'.
Denne guide vil bruge Deno, som du kan downloade [her](https://deno.land/#installation).

Du kan teste om Deno virker ved at skrive `deno -V` eller `deno --version` i en terminal/kommandoprompt,
og den så noget i retningen af: 
```
$ deno -V
deno 1.20.1
```

### Hav et tekst-redigerings-værktøj

Jeg bruger normalt VS Code til Typescript,
men du kan bruge hvad end du har lyst til.

Hvis du bruger VS Code, så installer
[Deno extension'et](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno), i 'extensions'-menuen.

## Print hello world

For at teste om alting virker,
skriv:
```ts
console.log("hello world");
```
i en fil, og gem den som `main.ts`.

Kør derefter filen fra en terminal med kommandoen `deno run main.ts`.

### Hvordan man gør det i VS Code

1. Lav en mappe, fx `my-typescript-project` på skrivebordet.
2. Åben VS Code
3. Klik `File` i venstre hjørne -> Klik `Open Folder` -> Find og marker mappen du lavede -> Klik `Open`.
4. Åben 'command pallet' ved at trykke `ctrl + shift + p` -> søg på `deno init`, tryk `enter` på `Deno: Initialize Workspace Configuration` og tryk `enter` til de 2 spørgsmål.
5. Klik `New File` i 'explorer'-fanen, og giv filen navnet `main.ts`
6. Åben filen ved at dobbeltklikke på den.
7. Skrive koden fra før i filen.
8. Gem filen ved at trykke `ctrl + s` på dit tastatur.
9. Åben en terminal (hvis ikke den er åben allerede) ved at trykke `ctrl + j` eller ved at klikke og trække op fra overkanten af bundlinjen i VS Code.
10. Skriv `deno run main.ts` og tryk `enter`.

### Forklaring af koden

`console.log(...args)` er det samme som `print(...args)` funktionen i Python. Den output'er hvad enten man giver den som argumenter til brugeren i terminal.

## Læg 2 tal sammen

Som det første, vil vi have et simpelt program som tager 2 tal, og lægger dem sammen.

```ts
function addToNumbers(a: number, b: number): number {
    let result = a + b;
    return result;
}

const inputA = prompt("a? ")!;
const inputB = parseInt(prompt("b? ")!);
const output = addToNumbers(parseInt(inputA), inputB);
console.log(output);
```

### Hvordan man gør det i VS Code

1. Slet (eller kommenter ud) alt det gamle kode og skriv koden ovenover.
2. Gem filen ved at trykke `ctrl + s` på dit tastatur.
3. Åben en terminal (hvis ikke den er åben allerede) ved at trykke `ctrl + j` eller ved at klikke og trække op fra overkanten af bundlinjen i VS Code.
4. Skriv `deno run main.ts` og tryk `enter`.


### Forklaring af koden

`addToNumbers` er en 'funktion', som tager 2 tal, lægger dem sammen og retunere resultatet. `(a: number, b: number)`,
betyder at når vi kalder funktionen, giver vi den 2 tal, fx  `(23, variableContainingNumber)`. `): number`,
betyder at funktionen vil retunere et tal.

`prompt` er en funktion, som får input fra brugeren, eksempel:

```ts
let usersBirthday = prompt("what is your birthday? ")!;
console.log(`Your birthday is on ${usersBirthday}!`);
```
would run like:
```
$ deno run main.ts
what is your birthday? well, you see, my birthday is acually on the fourth of July
well, you see, my birthday is acually on the fourth of July
Your birthday is on well, you see, my birthday is acually on the fourth of July!
```

`parseInt` er en funktion, som får en string tekst og konvertere det til et tal. Example:
```ts
const stringA: string = "8";
const stringB = "4";
const addedStrings = stringA + stringB;
console.log(addedStrings); // will print '84'

const numberA: string = parseInt(stringA);
const numberB = parseInt(stringB);
const addedNumbers = numberA + numberB;
console.log(addedNumbers); // will print '12'
```

Hvis du vil bruge decimaltal (kaldet floating point numbers, or 'float(s)') fx `3.14` og `0.23`, brug `parseFloat`.

Udråbstegnet `!` bag ved `prompt(_)` er fordi `prompt` returnerer `string | undefined`.
'Pipe'-symbolet `|` betyder 'eller', så `prompt` funktionen vil enten returnere en `string`-værdi eller en `undefined`-værdi.
For at skrive et `|`, tryk `AltGr` (tasten ved siden af mellemrumstasten) + tasten ved siden af tilbagetasten.
I "rigtig" kode ville tjekke `undefined`-værdier sådan:
```ts
const maybeUsername = prompt("what is your username? ");
if (maybeUsername) { // (maybeUsername) and (maybeUsername !== undefined) is the same, because (undefined === false)
    let username: string = maybeUsername;
    loginWithUsername(maybeUsername);
} else {
    console.log('nice one, you just fucked it up.');
    throw new Error('fucked');
}
```
Men fordi error-håndtering ikke er en prioritet lige nu,
vil vi bare bruge udråbstegnet `!`,
som også hedder 'Non-null assertion operator',
altså ikke-nul forsikringsoperator. Eksempel:
```ts
let maybeValue: number | undefined = maybeGetValueFromSomewhere();
functionThatHasToGetAValue(maybeValue); // Argument of type 'undefined' is not assignable to parameter of type 'number'.(2345)
functionThatHasToGetAValue(maybeValue!); // Ok
```

## Multi-funktion lommeregner

Lad os udvide vores lommeregner til at kunne lave forskellige operationer.
```ts
function doOperation(a: number, b: number, operation: string): number {
    if (operation === '+') {
        return a + b;
    } else if (operation === '-') {
        return a - b;
    } else if (operation === '*') {
        return a * b;
    } else {
        throw new Error('unknown operation');
    }
}

const a = parseInt(prompt("a = ")!);
const b = parseInt(prompt("b = ")!);
const op = prompt("operation? ")!;
const result = doOperation(a, b, op);
console.log(`${a} ${op} ${b} = ${result}`);
```

Prøv selv at tilføje division `/`.

### Test

```
$ deno run main.ts
a = 4
a = 6
operation? -
4 - 6 = -2
```

### Forklaring af koden

Backticks'ne `` ` ` `` laver en 'template string',
ligesom en normalt tekst string med double quotes `" "` eller single quotes `' '`,
men du kan nu bruge dollarsign curlybrace syntaksen `${...}` for at indsætte værdier.
For at skrive `` ` ` `` tryk `Shift` + tasten ved siden af tilbagetasten.
Example:
```ts
let a = 5;
let myString = `word ${a} word word ${3 + 5 * a} word word`;
```

`throw new Error("error message")` består af `throw` keyword'et/operatoren of en 'thrown' værdi,
lige her er det et nyt object af `Error`-klassen, som er brugt til dette formål.
'Throwing' af en værdi (of henvists til, som 'throwing an exception/error', eller bare 'throwing'),
betyder at vi stopper det programmet lavet lige nu, og sætter det til en slags 'exception-mode'.
Dette concept hedder 'exceptions'.

Instead of the program crashing,
when we throw an exception, we can also `try` and `catch` the exception,
which is something you can read [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch).

In Javascript and Typescript we use single equals `=` for assignment, eg: `let a = 5;`,
and we use triple equals 'strict equality operator' `===` for testing whether 2 values are the same.
The 'strict in-equality operator' `!==` tests whether 2 values are not the same.
In Typescript we use `===` instead of `==`, because of [annoying history you can read here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality).

## Smartere input

Vi gidder ikke at skrive det første, andet og derefter det tredje input, fx (`5` -> `4` -> `-` => `5 - 4 = 1`),
vi vil hellere bare skrive, fx `5-4` -> `1`.

```ts
type Equation = {
    a: number,
    b: number,
    operation: string
};

function doOperation(equation: Equation): number {
    const {a, b, operation} = equation;
    if (operation === '+') {
        return a + b;
    } else if (operation === '-') {
        return a - b;
    } else if (operation === '*') {
        return a * b;
    } else {
        throw new Error('unknown operation');
    }
}

function parseEquation(text: string): Equation {
    const a = parseInt(text[0]);
    const op = text[1];
    const b = parseInt(text[2]);
    const equation: Equation = {
        a,
        b, 
        operation: op,
    };
    return equation;
}

const input = prompt("> ")!;
const equation = parseEquation(input);
const result = doOperation(equation);
console.log(result);
```

### Explaning the code

`const {a, b, operator} = equation;` is called 'destructuring assignment',
which you can read more of [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

`test[2]` is the value at 2's place in the string, counting starting from zero. Example:
```ts
const s = "hello world";
const a = s[0];
console.log(a); // prints "h"
const b = s[2]; 
const c = s[5];
const d = s[9];
console.log(a + b + c); // prints 'lol' 
```

## More flexible parser

The 'parser' is a bit stupid right now, as it only knows how to parse a very specific 'grammar',
eg: `3+2` and `8-9`, but now `23-2` or `2 + 4`, lets fix that.

```ts
type Equation = /* from before */;
function doOperation(equation: Equation): number { /* from before */}

type Parser = {
    text: string,
    index: number,
};

function parseEquation(text: string): Equation {
    let parser: Parser = {
        text,
        index: 0,
    };
    const a = parseNumber(parser);

    skipWhiteSpace(parser);

    const operator = parser.text[parser.index];
    parser.index++; // same as `index += 1`, same as `index = index + 1`

    skipWhiteSpace(parser);

    const b = parseNumber(parser);
    return {a, b, operator};
}

function skipWhiteSpace(parser: Parser) {
    while (isInputRemaning(parser) && parser.text[parser.index] === ' ') {
        parser.index++;
    }
}

function parseNumber(p: Parser): number {
    let value = "";
    value += p.text[p.index];
    p.index++;
    while (isInputRemaning(p) && isCharANumber(p.text[p.index])) {
        value += p.text[p.index];
        p.index++;
    }
    return parseInt(value);
}

function isCharANumber(char: string): boolean {
    if ("1234567890".includes(char))
        return true;
    else
        return false;
}

function isInputRemaning(p: Parser): boolean {
    return p.index < p.text.length;
}

const input = prompt("> ")!;
const equation = parseEquation(input);
const result = doOperation(equation);
console.log(result);
```

## Vil du have mere

Spørg Simon

