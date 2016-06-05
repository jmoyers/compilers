# Finite Automata

Finite automata = can be implementation of regex

They both can specify the regular languages. Simply think about it as a character by character analysis of a string. This can also be thought of/implemented as a set of nested switch statements and state variables. Implementation can be boiled down to a lookup table.

|Finite automata has..|Symbol|
|--|--|
|An input alphabet|∑|
|A set of states|S|
|A start state|n|
|A set of accepting states|F⊆S|
|A set of transitions from state to state|state →<sup>input</sup> state

**Transitions**
* Defined as: S<sub>1</sub> →<sup>a</sup> S<sub>2</sub>
* Read as: In state S<sub>1</sub> on input a go to state S<sub>2</sub>

**Accept/Reject**
* If we at the **end of input** and in **accepting state** => accept
* Otherwise => reject -- this means if there is no transition for the input, it fails as well
* Otherwise can be said that `S ⊈ F` // not totally sure this is the same symbol

**Finite automata diagrams**

|Name|Symbol|
|--|--|
|A state|![state](state.png)|
|The start state|![start state](start.png)|
|An accepting state|![accept state](accept.png)|
|A transition|![transition](transition.png)|


Okay, so, skipping the dumb ones.

Example (deterministic): any number of 1s, followed by a single 0

![fig 1](fig1.png)


Note I forgot the start arrow, but it starts at A. Follow it in your head and you can pretty easily convince yourself that this which match the description. It also handles rejection, which you can also convince yourself of -- just have to remember that if you don't move, you reject.

This can be simplified to a table of states and transitions for each input.

|State|0 Input|1 Input|
|--|--|--|
|A|B  |A|
|B| | | |

This can be a lookup table, in a programming language, such as shown here: http://www.geeksforgeeks.org/searching-for-patterns-set-5-finite-automata/

Example 2 (deterministic): I had some trouble working through these, so I thought I'd put them here.

![Fig 2](fig2.png)

Options (choose 1 regular language to match above):
* (0 + 1)*
* (1* + 0)(1 + 0)
* 1* + (01)* + (001)* + (000*1)*
* (0 + 1)*00

I like to work through the obvious paths that look like state transition chains which have no options.  From the above, you can work out that there are at least two zeroes in a row before hitting the last node. Due to repeating non-optional zeroes, the last one is correct.

### Deterministic vs Nondeterministic

* **Epsilson (ε) move**: there is a choice whether you take the move
* **Deterministic**: For each input, you can only take one path (no ε moves)
* **Nondeterministic**: For each input, you can take many paths (can hae ε moves)


This looks promising: http://ivanzuzak.info/noam/webapps/fsm_simulator/