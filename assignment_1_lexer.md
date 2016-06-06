# Writing a Flex lexer for Cool

I just finished the first programming assignment for the Stanford Compilers course, which was to implement a lexer for the Cool ([classroom object oriented language](https://en.wikipedia.org/wiki/Cool_(programming_language))) using Flex.

Flex is a C-language based lexical analyzer *generator*. It generates a valid c source file for the lexical rules you put together. The `.flex` file format is basically a c file with some special syntax.

The bulk of this assignment was hunting for implementation details. I do not enjoy courses as much when they don't go through an assignment pre-amble in the actual lectures. Feels bad to be hunting through pdfs. Yes, yes, RTFM and all that -- I just feel that education should be optimized for speed of intake, not to "teach you how to hunt for stuff." That's a valuable lesson I already learned literally 20 years ago. We're in a new age here.

###Gotchas
* `make lexer`, not just `make` - I spent 20 minutes wondering if my changes to the base `cool.flex` were actually taking effect

* A nice [base case](https://en.wikipedia.org/wiki/Flex_(lexical_analyser_generator)) for the flex file

* Pre-defined return types in a magic header not in your assignment folder (`/usr/class/cs143/include/PA2/cool-parse.h`)

* You can nest rules via [`%x`](http://flex.sourceforge.net/manual/Start-Conditions.html) directive -- directly applicable to string literal and comment parsing

* The actual lexeme is in a global variable called `yytext` which is a null terminated c-string

* When a lexeme is semantically relevant, you are expected to provide it to the parser by copying it into `cool_yylval` (which is a union, relevant members being `error_msg` and `symbol`)

* They expect you to not just copy the string, but use 2 different global variables `inttable`, `stringtable` which implement a custom `StringTable` class. These are definied in `stringtable.cc` and the only relevant method is `char* add_string(char *)`.

### Testing

Right up front, I will tell you `test.cl` has some minor syntax errors in it. Your parser should be able to handle them. You don't have to do anything thats particularly special for it to do so.

The output of running `./lexer`, with no rules defined, is just the entire output file. This makes it overwhelming to try to parse their supplied `test.cl`. Start with an extremely simple file containing just `=>`, on a line by itself. This is the only rule they provide. 

The output should be:
```
#name "your_file.cl"
#1 DARROW
```

The anatomy is the `#name` preamble, which is just the file, followed by `#{line number} {lexeme class} {lexeme if applicable}`. The operators and keywords and things are not expected to have a lexeme -- only things like `INT_CONST` which will have its associated value (like `42` or `1` or whatever the number is).

Start adding individual things you know are valid Cool syntax to the file. Write rules for them. The output should be only the `#{line number} {lexeme class} {lexeme if applicable}`. If its printing characters that aren't parsed, you've missed something in your ruleset.

Eventually switch to `test.cl` when you feel you've got a handle. Do a pass with `./lexer test.cl | less`, scroll through the input and look for things that aren't parsed. Fix them.

Once everything is parsed, you can find a **REFERENCE LEXER** that I didn't see mentioned anywhere in `usr/class/cs143/bin/reflexer`. Use it to parse the same file and output it to a test file like so `usr/class/cs143/bin/reflexer test.cl > ref_test.out`. Now do the same thing with your own lexer like so `lexer test.cl > own_test.out`. Now diff them `diff own_test.out ref_test.out`. Fix, rinse, repeat.

Overall, I learned something and was pleased when finished. Could have gone smoother.


Resources

* [Detailed Lecture Notes as Gitbook](http://jmoyers.org/compilers/) -- Cool Language, Lexical Analysis, and Finite Automata are the relevant sections
* [Stanford Course](https://class.coursera.org/compilers-004/) - self paced
* [My Original Course Post](http://jmoyers.org/stanford-compilers-course/)

