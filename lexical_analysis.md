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
So its hard sometimes.

### Regular Expressions

Not getting into formal notation here. They devote way too much time to it imo. Its useful to understand how they write out regular expressions in the context of the course though.

Special symbols:
* { } = a set
* ε = epsilon = set with one {""}

| Formal notation | Formal language | Regex | Plain english |
| -- | -- | -- | -- |
| `'a' + 'b' + 'c'` | Union | `[abc]` | a or b or c |
| `('a' + 'b' + 'c')*` | Iteration | `[abc]*` | either its empty, or its a or b or c, repeating |
| `('a' + 'b' + 'c')+` |  | `[abc]+` | same as above, but need at least one char |
| `abc` | Concatentation | `abc` | you see the real string `abc` in pattern |
| 0:6 | 1:6 | 2:6 | 3:6 |
| 0:7 | 1:7 | 2:7 | 3:7 |
| 0:8 | 1:8 | 2:8 | 3:8 |

Note that it matches the SMALLEST section that matches the pattern.