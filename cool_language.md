# Cool Language

## Tools

* **Assembler/Emulator** - spim, MIPS based emulator, takes a `.s` file
* **Compiler** - coolc, takes a set of `.cl` files. 
* **stdlib** - Include std lib type stuff as source files in the list for your build
* **Build** - `coolc source1.cl source2.cl && spim source1.s`


* The entry point of the code is in `Main` class' `main` function
* Expression based return (last expression in a block returns) as shown below by 1
* Not like Rust in that you omit semi-colon to distinguish between a 'statement' and expression

```
class Main {
  main(): Int { 1 }
}
```

* Sometimes using pattern inheriting from standard library classes (such as IO), then calling member functions like `out_string`.
* Also occasionally doing things like `(new IO).out_string("1\n")`, so composition is used too
* `self` is the `this` equivalent keyword, and looks like it can be omitted

```
class Main inherits IO { 
  main(): Object {              // return type associated with last expression
    out_string("Hello World\n") // out_string returning an object here, no semicolon needed here
  } 
}
```

* `if` terminates with `fi`
* `loop` terminates with `pool`

Standard libaries shown so far:
* A2I - used to convert ascii to integer and back (`i2a`, `a2i)
* IO - used to print out strings to stdou, read strings in from stdin




