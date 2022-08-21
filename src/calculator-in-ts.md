
# How to make a Calculator in Typescript

[**Also available in danish**](in-danish/calculator-in-ts-danish.md)

## Intro

This is meant to be as short as possible, hence 'Intro' instead of 'Introduction'.

By the end of this small guide, you will have a working calculator.

## Prep

### Install Deno

To run your code, you need a 'runner'. Download [Deno from here](https://deno.land/#installation).

Test by running `deno -V` or `deno --version`, which should print something like:
```
$ deno -V
deno 1.20.1
```

### Also have a text editor

I use VS Code for Typescript, you can use whatever, I don't care.

If using VS Code,
install the [Deno](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno) extension,
from the extensions menu.

## Print hello world

To test that everything works, write 
```ts
console.log("hello world");
```
in a file, and save it as `main.ts`.

Now run the file by typing `deno run main.ts` in a terminal/command line.

### Doing it in VS Code

1. Create a folder, eg. `my-typescript-project` on the desktop.
2. Open VS Code
3. Click `File` up in the left corner -> Click `Open Folder` -> Find and mark the folder you created -> Click `Open`.
4. Open command pallet by pressing `ctrl + shift + p` -> search for `deno init`, press `enter` on `Deno: Initialize Workspace Configuration` and press `enter` for the 2 questions.
5. Click `New File` in the explorer tab, and name it `main.ts`
6. Open it by double clicking it.
7. Write the code above.
8. Save by hitting `ctrl + s` on your keyboard.
9. Open a terminal (if not already open) by hitting `ctrl + j` or dragging up from the top of the bottombar in VS Code.
10. Type `deno run main.ts` and press `enter`.

### Explaning the code

`console.log(...args)` is the same as the `print(...args)` function in Python. It outputs whatever it gets as arguments to the terminal.

## Adding 2 numbers

To get started we want a simple program that takes 2 numbers, and adds them.

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

### Doing it in VS Code

1. Delete (or comment out) all previous code, and write the code above.
2. Save by hitting `ctrl + s` on your keyboard.
3. Open a terminal (if not already open) by hitting `ctrl + j` or dragging up from the top of the bottombar in VS Code.
4. Type `deno run main.ts`, and press enter.

### Explaning the code

`addToNumbers` is a 'function', that takes 2 numbers, adds them and returns the result. `(a: number, b: number)`,
means that when we call the function, we give it 2 numbers, eg. `(23, variableContainingNumber)`. `): number`,
means that the function will return a number.

`prompt` is a function that gets input from the user, example:

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

`parseInt` is a function that takes a string and converts it to a number. Example:
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

If you want to use decimal numbers (called floating point numbers, or 'float(s)') eg. `3.14` and `0.23`, use `parseFloat`.

The exclamation point `!` behind `prompt(_)` is because `prompt` returns `string | undefined`.
The pipe symbol `|` means 'or', so the `prompt` function will either return a `string`-value or an `undefined`-value.
To type `|`, press `AltGr` (key right of spacebar) + the key left of backspace.
In "real" code we would check it like this:
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
But because we don't give a shit about error handling right now, we just use the exclamation point `!`, or 'Non-null assertion operator' as it is called in Typescript. Example:
```ts
let maybeValue: number | undefined = maybeGetValueFromSomewhere();
functionThatHasToGetAValue(maybeValue); // Argument of type 'undefined' is not assignable to parameter of type 'number'.(2345)
functionThatHasToGetAValue(maybeValue!); // Ok
```

## Multi-function calculator

Let's expand our calculator to be a able to do different operations.
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

Try and add division `/` yourself.

### Test

```
$ deno run main.ts
a = 4
a = 6
operation? -
4 - 6 = -2
```

### Explaning the code

The backticks `` ` ` `` makes a template string,
exactly like a normal string using double quotes `" "` or single quotes `' '`,
but you're able to use the dollarsign curlybrace syntax `${...}` to insert values.
To type `` ` ` `` press `Shift` + the key left of backspace.
Example:
```ts
let a = 5;
let myString = `word ${a} word word ${3 + 5 * a} word word`;
```

`throw new Error("error message")` consists of a `throw` keyword/operator and a thrown value,
in this case a new object of the `Error`-class, which is used for this purpose.
Throwing a value (most often reffered to as 'throwing an exception/error', or just 'throwing'),
means stopping whatever the program is doing, and going into 'exception-mode'.
This is called 'exceptions'.

Instead of the program crashing,
when we throw an exception, we can also `try` and `catch` the exception,
which is something you can read [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch).

In Javascript and Typescript we use single equals `=` for assignment, eg: `let a = 5;`,
and we use triple equals 'strict equality operator' `===` for testing whether 2 values are the same.
The 'strict in-equality operator' `!==` tests whether 2 values are not the same.
In Typescript we use `===` instead of `==`, because of [annoying history you can read here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality).

## Smarter input

We dont want to type the first, then the second and then the third part of the input, eg
(`5` -> `4` -> `-` => `5 - 4 = 1`),
we just want to type and equation and get it solved, eg
`5-4` -> `1`.

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

## Want more

Ask Simon to do more

