# Cool Language

## Tools

* **Assembler/Emulator** - spim, MIPS based emulator, takes a `.s` file
* **Compiler** - coolc, takes a set of `.cl` files. 
* **stdlib** - Include std lib type stuff as source files in the list for your build
* **Build** - `coolc source1.cl source2.cl && spim source1.s`


### Entry point, return semantics
* The entry point of the code is in `Main` class' `main` function
* No explicit return statemtn. Last expression in statement block looks like it returns
* You don't omit semi-colon like in rust always, not sure why its omitted here

```
class Main {
  main(): Int { 1 }
}
```

### Inheritance/stdlib

* Sometimes use inheritance for standard library classes (such as IO), call members (`out_string`)
* Occasionally do things like `(new IO).out_string("1\n")`, so sometimes composition
* `self` is the `this` equivalent keyword, and looks like it can be omitted

```
class Main inherits IO { 
  main(): Object {              // return type associated with last expression
    out_string("Hello World\n") // out_string returning an object here, no semicolon needed here
  } 
}
```
**So far...**
* A2I - used to convert ascii to integer and back (`i2a`, `a2i)
* IO - used to print out strings to stdou, read strings in from stdin


### Control structures

* Not terminators for control statements
* `if <expr> then <expr> else <expr> fi`
* `while <expr> loop <expr> pool`

### `let`, blocks, assignment, comparison

* `=` is comparison, not assignment
* `<-` is assignment
* `let` defines a variable within a block of its own, terminated by `in <expr>`

> recursive member function
```
fact(i: Int): Int {
  if (i = 0) then 1 else i * fact(i - 1) fi
};
```

> procedural member function
```
fact(i: Int): Int {
  let fact: Int <- 1 in {     // opens statement block
    while (not (i = 0)) loop  // open while loop, note i = 0 is comparison
    {
      fact <- fact * i;       // assign
      i <- i - 1;             // assign
    }
    pool;                     // end loop with `pool`, needs ;
    fact;                     // return fact from whole statement block
  }
}
```






