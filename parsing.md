# Parsing

Arbitrarily nested things can't be represented as a regular language. We need a parser for our lexer! You can reason this out by looking at a finite state machine with a few states. It can only count up to modulus k, where k is the number of states.

* Lexers output tokens.
* Parsers output parse trees.
* Sometimes these things are combined and sometimes you don't actually keep a tree.

## Context Free Grammar (CFG)

* Programming languages are natrually a recursive structure. Like `if { if { if { } } }`
* Context free grammar is a way to describe these structures.

They contain...
* A set of terminals (T)
* A set of non-terminals (N)
* A start symbol (S) (also, S is a part of N)
* A set of productions, which is just a tree representation. X -> Y<sub>1</sub> ... Y<sub>n</sub>
