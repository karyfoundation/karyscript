# Incoming Grammars and Features
## Idea

#### Table Literals `IMPLEMENTED IN GRAMMAR`
Tables are the most natural way for writing tables! so why not having them in KaryScript? It can be super awesome:

```
def contacts =
    | name    | surname       | phone    |
    |---------|---------------|----------|
    | "Mr.X"  | "Jay Simpson" |          |
    | "Bart"  | "Simpson"     | 666      |
```

And that will compile to:

```
var contacts = [
	{
		name: "Mr.X",
		surname: "Jay Simpson",
		phone: null
	},
	{
		name: "Bart",
		surname: "Simpson",
		phone: 666
	}
]
```

And it can be used for Objects as well:

```
def contacts =
    |  #    | name    | surname       |
    |-------|---------|---------------|
    | homer | "Mr.X"  | "Jay Simpson" |
    | bart  | "Bart"  | "Simpson"     |
```

that as well compiles to:

```
var contacts = {
	homer: {
		name: "Mr.X",
		surname: "Jay Simpson"
	},
	bart: {
		name: "Bart",
		surname: "Simpson"
	}
};
```

A more complete version can be:

```
def contacts =
    |  #    | name    | middle-name | surname       | phone-number               |
    |-------|---------|-------------|---------------|----------------------------|
    | homer | "Homer" | "Jay"       | "Simpson"     | (get-phone-number "homer") |
    | bart  | "Bart"  |             | "Simpson"     | 666                        |

con homer = contacts/homer
con homer-surname = contacts/homer/surname
```


#### Placeholders `IMPORTANT`
This:

```
x(y(), z(), f(1, 8, 10))
```

Can be written as:

```
def p1 = (y)
def p2 = (z)
def p3 = (f 1 8 10)
(x p1 p2 p3)
```

Problem here is you __have__ to use 3 vars to write this clean. And may devs use vars like this just to write cleaner code. So place holders are macro-like vars for the _preprocessor_ that helps writing clean code while keeping it fast by just replacing place holder values:

```
hold p1 = (x) ⟶ (y) ⟶ (z $ 5)
(f (g) @p1)
```

This will compile to this:

```
// without using any variables
f(g( ), z(y(x()), 5))
```

#### Class extensions

```
class x extends y:
```

#### Set & Map Literals

Set and map literals will what is there for arrays and objects in JS to sets and maps.

```
def my-set = {1 2 3}
def my-map = {something/something: 23, 4: 2}
```

And can be compiled to:

```
var my_set = new Set([1, 2, 3]);
var my_map = (new Map( )).set(something.something, 23).set(4, 2);
```

#### Identifier Interpolation

```
"hello {{hello}}!"
```

#### For loops

```
for (x):
	(y)
end

for (x) step 2:
	(y)
end

for index in (x):
	(console/log i)
end

for index of (x):
	(console/log i)
end

for (x) to (y):
	(z)
end

for (x) to (y) step 2:
	(z)
end
```

#### Imports
Simple imports can be in KaryScript as:

```
use fs
```

Which compiles to

```
// common.js
const fs = require('fs');

// es6
import * as fs from 'fs';
```

and also can exists multiple imports

```
use fs path
```

also can be partial loads:

```
use a b c from "module"

// compiles to:
// import {a, b, c} from 'module';
```

And in the end:

```
use "fs" as fileSystem

// compiles to:
// const fileSystem = require('fs')
// or:
// import * as fileSystem from 'fs';
```

## Needs Thinking
JavaScript Has this

```
let [a, b, c] = f( );
let {a, b, c} = g( );
```

No clean grammar is present for this one.

## Accepted 

Assignments

```
x = 4
```

Inline comments

```
// inline comments
```

Object loaders

```
x = [ y | "hello" ]
x = [ y | 2 ]
x = [ y | z ⟶ (> z 2) ]
```

Literal Addresses

```
(5/toString)
("Hello, World!".substring 5)
```

## To Do
Nested placeholders:

```
(x) ⟶ (y (z $))
```