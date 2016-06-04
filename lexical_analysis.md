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

| Non-set formal notation | Formal name | Regex | Plain english |
| -- | -- | -- | -- |
| `'a' + 'b'` | Union (∪) | `[abc]` | a or b |
| `('a' + 'b')*` | Iteration (*) | `[ab]*` | its a or b, repeating some number of times (including zero times) |
| `ab` | Concatentation | `ab` | you see the real string `ab` in pattern |

**Other Stuff**

| Non-set formal notation | Formal name | Regex | Plain english |
|--|--|--|--|
| `('a' + 'b')+` |  | `[ab]+` | same as *, but need at least one char |
| `('a' + 'b')?` |  | `[ab]?` | whole thing is optional |

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

Never want one meaning function mapping to two different syntaxes.