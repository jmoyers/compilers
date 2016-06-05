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
|An accepting state|![](accept.png)|
|A transition|![](transition.png)|


This looks promising: http://ivanzuzak.info/noam/webapps/fsm_simulator/
Created state diagrams with: http://madebyevan.com/fsm/