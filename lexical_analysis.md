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

### Tokenizing
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
So its hard sometimes.