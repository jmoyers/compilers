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