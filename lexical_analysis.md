# Lexical Analysis

* **Lexer** divides input stream/string into tokens, which have classes
* **Tokens** look like a tuple (`class`, `original string`), so like (`operator`, `+`)
* The original string is called a **lexeme**
* **Token Class** examples...
  * Identifier - variable names - letters or digits, start with letter
  * Integer - non empty string of digits
  * Keyword - `else`, `if`, `begin`, etc
  * Whitespace - `/t`, `/n`, ` `
  * Operator - `=`, `+`, `*`, etc
  * Things like `)`, `(`, `;` sound like they're their own class
  * etc

### Example of Tokenizing
```
if (i==j)
  z = 0;
else
  z = 1;
```

**Yields:**
* **(CLASS, LEXEME)**
* (Keyword, "if")
* (Whitespace, " ")
* (OpenParen, "(")
* (Identifier, "i")
* (Operator, "==")
* (Identifier, "j")
* (CloseParen, ")")
* (Whitespace, "\n\t")
* (Identifier, "z")
* and so on...

### Importance of Whitespace, Looking Ahead
* FORTRAN. `VAR1` is the same as `VA R1`. Whitespace is collapsed.
* Goes on to explain that it can sometimes be important for meaning.

```
DO 5 I = 1,25 // Loop i = 1 to 25, end of loop denoted by label 5
```
```
DO 5 I = 1.25 // Variable DO5I = 1.25, no loop at all, only difference is , -> .
```
**Moral:** whitespace can make a difference and you also need to look ahead to understand how to tokenize (the `,` was the only difference here, wildly different meanings).

* Some languages from the past even had keywords overlap with identifiers. PL/1 (IBM) had this:
```
IF ELSE THEN THEN = ELSE; ELSE ELSE = THEN 
// if (variable else) { variable then = variable else; } { variable else = variable then }
```
Lexical analysis was hard in this language.

C++ Example

```
Foo<Bar>      // template declaration
cin >> var;   // stream operator
Foo<Bar<Baz>> // nested template, previously had to be Foo<Bar<Baz> > <--extra space
```
So its hard sometimes and stuff.

### Regular Expressions

* **regular expressions** specify **regular languages**
* a list of them can formally specify what is in the language
* a lexeme can sort of be described by a regular expression
* Looking ahead: Finite automata can be used to repesent a regular expression
* Really dense formal set notation (I won't spend time on it, frankly)
* 5 things and stuff
  * Base case: 1 character string
  * Base case: empty string (epsilon or ε)
  * Three compound expressions
    * Union - `∪` or `+` which is the same as logical or, like `a+b` is a or b
    * Concatenation - add two things together, no symbol, just sitting together `ab`
    * Iteration - `*` repeat 0 to some number of times

Special symbols:
* { } = a set
* ε = epsilon = set with one {""}, an empty string

**Compound Expressions**

| Non-shorthand | Formal name | Regex/Shorthand | Plain english |
| -- | -- | -- | -- |
| `'a' + 'b'` | Union (∪) | `a|b` or `[ab]` | a or b |
| `('a' + 'b')*` | Iteration (*) | `[ab]*` | its a or b, repeating some number of times (including zero times) |
| `ab` | Concatentation | `ab` | you see the real string `ab` in pattern |

**Shorthand...**

| Non-shorthand | Derivation | Regex/Shorthand | Plain english |
|--|--|--|--|
| `('a' + 'b')+` | `ab[ab]*` | `[ab]+` | same as *, but need at least one char |
| `('a' + 'b') + ε` |  | `[ab]?` | whole thing is optional |
| `('a' + 'c'`+ .. 'z') | | `[a-z]` | the range of characters a through z |
| | |`[^a-z]`| excloding the range of characters a-z |

Note that it matches the SMALLEST section that matches the pattern.

### Meaning functions

* Take a given regular expression like `A+B`
* Where A and B are the name of a set of characters, lets say (not the actual uppercase `A` and `B`)
* This is equivalent to `A ∪ B`, or A Union B
* Meaning function is L where
  * L(A+B) = L(A) ∪ L(B)

**So what's the meaning function for?**
* Make clear what is syntax and what is semantic
* Allows us to consider notation as separate issue
* Expressions and meaning are not 1-to-1

**Notation?**
* Notion in general is important because it defines how we think.
* Arabic numeral system makes it much easier to reason about arithmetic than roman numeral system
* Therefore, its important to have appropriate notation

**Many expressions = one meaning?**

Consider the following expressions...
* `0*`
* `0 + 0*`
* `ε + 00*`
* `ε + 0 + 0*`


* All have the same Meaning.
* Many different Syntaxes can have the same Meaning. 
* The basis for **optimization**
* You replace one program with another that have the same meaning. 
* One is faster or smaller or whatever your optimization parameters are

Never want one meaning function mapping to two different syntaxes. Means its ambiguous.

### Lexical Specifications

I'm going to mostly write in the shorthand notation, since thats what every regex engine ever uses.

**Example: Keyword `if` or `else` or `then` or...**
* Shorthand: `'i''f' + 'e''l''s''e'` becomes `'if' + 'else'`
* Said as: if union else

**Example: Integer (non-empty string of digits)**
* Naming...
  * `digit = '0'+'1'+'2'...+'9'`
* Multiple digits...
  * `digit digit*`
  * This pattern is shorthanded to `digit+` (at least one digit)

**Example: Identifier: string of letters or digits, starting with letter**
* Letters first...
  * `letter = 'a' + 'b' ... + 'Z'`
  * Shorthand: `letter = [a-zA-Z]`
* Now add digits...
  * `letter(letter+digit)*`
  * Starts with a letter, followed by union of leter and digit repeated however many times

**Example: Whitespace**

* Escape sequences, oh my
  * \n = newline
  * \r = carriage return
  * \t = tab
  *    = space
* So therefore...
  * Definition: `(' ' + '\n' + '\t')+` which by the way is just `[ \n\t]+` in any regex engine

Skipping email shit. Not worth it, regex is old hat baby.

**Example: Pascal** - this is a good one. 


> Epsilon means optional (read as Union epsilon, but really they mean OR NOT HERE AT ALL BRO).

```
digit          = '0' + '1' + '2' + '3' + '4' + '5' + '6' + '7' + '8' + '9'
digits         = digit+
opt_fraction   = ('.'digits) + ε                   // also known as [\.digits]?
opt_exponent   = ('E' ('+' + '-' + ε) digits) + ε
num            = digits opt_fraction opt_exponent
```

So, things like...
* digits: `12`
* add optional fraction: `12.23` 
* add optional exponent: `12.23E+2`

