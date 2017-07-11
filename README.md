# NoFail

Nofail is a novel programming language. It is designed with a few of principles in mind:

- Principle of least surprise: if the intent of the code is clear, do what is expected.
- Robustness: The program should not crash during execution.
- Simplicity: Nofail is a simple language, suitable for everyone, in particular
beginners. There is no complex language feature. There are no exceptions, no
classes, no explicit memory management. As Go showed it, these concepts are not
needed.
- Easy to write. This means faster development, and thus better productivity.

The language is influenced by some of the most successful languages. For this
reason, its syntax should look familiar to most programmers. For productivity
and ease of use, it uses dynamic typing.

## Syntax

The most innovative feature of NoFail is probably its syntax. There are lots of
computer languages with slightly different syntaxes. Users are often confused,
especially when they need to switch from a language to another. There's no
reason why a programmer should remember which comment syntax to use (`#`, `//`,
`/*`, etc.), just to please a compiler. To improve productivity, we allow
multiple syntaxes: as long as the intent is clear and obvious, there is no need
to raise a syntax error.

In the past, programmers have spent huge amount of time fixing typos. Compilers
should be smarter and try to understand what you meant. Think about it: when you
talk with someone, they will try to understand you, even if your grammar or
pronunciation is not perfect. Compilers and interpreters should be your friends.

Welcome to the 21st century.

## Errors

In case of error, execution will continue anyway. This behavior is inspired by
successful languages like Bash, PHP
([include](http://php.net/manual/en/function.include.php) shows a warning if the
file doesn’t exist), and C. An error message is printed on the standard error to
let the developer know about it. This is a useful feature, because good programs
should never crash. Too many times in the history, users have lots data because
a software crashed. NoFail fixes this long-standing issue. It doesn't crash. If
you want the code to stop, you should call the exit function explicitly.

If you want to know if something has failed, check the variable `errno` (also
called `last_error`).

## Hello World

Here are multiple implementations of the traditional Hello World program:

```
print "Hello world!"
```
```
echo 'Hello world!'
```
```
printf("Hello world!");
```
```
puts <<EOF
Hello world!
EOF
```

As you can see, there are many ways to implement the program, no matter which
language you have learned before, at least one of them should be obvious and
familiar.

## Comments

Choose your favorite syntax:

```
# comment till end of line (Perl-style)
```
```
// comment till end of line (C++-style)
```
```
/* C-style comment
block */
```
```
(* comment (Pascal-style)
block *)
```
```
rem Hello (Basic-style)
```

## Conditions

The following examples are equivalent:

```
if a == 3: print a
```
```
if a == 3 then print a
```
```
print a if a == 3
```
```
a == 3 or print a
```
```
either a == 3 or print a
```

In some languages, `=` is for assignment, and `==` is for equality testing. This
is a widely known source of bugs. We fix this issue by treating `=` as an
equality testing (like in OCaml, Pascal) when it is inside an
expression. Because it’s obviously the user intent.

```
if (a = 3) print a
```
Note that you can also use `is` instead of `==`.
Operators `!=` and `<>` are equivalent.
Assignment is done using `=`, `:=` or `<-`.

```
unless a <> 3: print a
```
```
unless (a != 3) print a
```
```
if a is 3, print a
```

Using a comma to separate the condition from the statement seems natural. It’s
surprising no popular language does that (although, the comma operator in C has
some similarities).

# Loops

Like for conditions, there are many ways to do a loop. You will easily find one
that fits your needs.

```
while condition do ..
```
```
until condition do ..
```
```
do .. until condition
```
```
do .. while condition
```
```
repeat 5 times ..
```
```
for i in [1, 2, 3] ...
```
```
for each i in [1, 2, 3] ...
```
```
repeat 5 times ..
```

## Function call

Like the majority of programming languages:

```
foo(arg1, arg2)
```
```
foo(arg)
```

When a function has a single argument, parentheses are optional. This follows
the traditional mathematical tradition:

```
cos x
```

## Function definition

As in Python and some other languages, we can use indentation to parse the
blocks. Some people prefer to be explicit and use braces or `begin`/`end`.

```
def foo(x):
  a = 3
  return x + a
```
```
function foo(x) {
  a = 3
  x + 3
}
```
```
defun foo(x)
begin
  a = 3
  x + a
end
```
```
procedure foo(x)
begin
  a = x + 3
  print(x)
end
```
```
let foo(x) = (
  a = 3;
  x + a
)
```

As you see, the keyword `return` is optional for the last statement in the
function (as in Ruby).

Having multiple ways to create block is very valuable and improves readability,
especially with nested blocks. This practice has been used for many years in
typography conventions (for nested quotations), and many people find it more
readable. It’s surprising popular languages haven’t adopted it. (Languages like
Python and Bash support different quotes. We believe nested blocks are more
common than nested strings, though.)

```
for i in arr do begin
  if x {
    ...
  }
end
```

## Type conversions

Imagine the following code:

```
size := 18
print "size = " + size + " cm"
```

Since `size` is a number, it has to be converted to a string (like in
Java). More generally, when the type of a value is not the one you expected, the
value is converted.

For example, a string may be converted to a function of the same name:

```
def foo(x): print x
"foo"(3)
```

This technique can be used in other languages, but they typically require a call
to `eval`. We believe this kind of boilerplate is not useful.

## Spaces

As Python and Haskell showed it, using indentation to parse the blocks is
great. But we we can go further and use spaces for precedence.

It’s not a completely new idea. Most of the Go code is formatted using
gofmt. gofmt uses spaces around operators to [indicate the
precedence](https://github.com/golang/go/issues/1206#issuecomment-66052999). Look
at the following code:

```
a = 2+3 * 4
```

As you can guess, `a` is obviously assigned 20.

More specifically, the parser first checks the spaces. If there is one or many
spaces on each side of the operator, we treat it as low priority. If there are
no spaces, it is high priority. The feature allows developer to write cleaner
and more readable code, without polluting with parentheses.

If multiple operators have the same space priority, we use the traditional
mathematical rules to disambiguate (e.g. multiplication is done before
addition).

## Types

As mentioned in the introduction, simplicity is a goal of the language. For this
reason, there are only 4 types:

- numbers
- strings
- dictionaries
- functions

To represent an object, use a dictionary.

```
personne = {
  nom = "foo"
  âge = 18  # indeed, we’re not limited to ascii
}
```

Like in Javascript, arrays are represented using dictionaries (where keys are numbers). 

```
arr = [1, 2, 3, 4, 5]
arr = 1..5  # equivalent
```

For convenience, the arrays syntax automatically creates a field `length`.

## Accessing object fields

To access an element in the dictionary, use:

```
print arr["length"]
```

Or simply:

```
print arr.length
```

Or use the arrow operator:
```
print tab->length
```

Or use the `of` operator:
```
print length of arr
```

## Booleans

As in Python, we define:

- True = 1
- False = 0

This is useful, because it allows users to write `True + True` and get 2 as expected.
Following Python, any value is considered true in a condition, except:
- 0
- the empty dictionary
- the empty string

Of course, a [date object representing midnight](https://bugs.python.org/issue13936)
would be considered true.

## Modifying functions return value

This is a novel feature which I haven’t seen yet in other languages. It is
possible to change the return value of a function, after the function has been
defined.

For example:

```
def foo(x) { return x - 1}
foo(0) = 0

foo(0)  # returns 0
foo(1)  # returns 0
foo(2)  # returns 1
```
In other words, the function `foo` is redefined when it’s input is 0.  You can
define factorial like this:

```
define fact(x): x * fact x-1
fact(0) = 1
```

The feature makes it very easy to cache results, like in dynamic programming:
```
def fibo(x) =
begin
  if (x < 2) x
  else
    result = fibo(x - 1) + fibo(x - 2)
    fibo(x) = result  # cache the result
    return result
end
```

## It value

Some languages implicitly give a name to the last result (it in OCaml,
[underscore in
Python](http://stackoverflow.com/questions/200020/get-last-answer), etc.). Perl
also has
[an implicit default variable](https://perlmaven.com/the-default-variable-of-perl).
Kotlin and Groovy
have an implicit `it` variable inside lambdas.

In Nofail, we call it `it`. It is implicitly assigned when an expression is
computed (or after an assignment).

Examples:

```
person.age = compute_age()
either it >= 18 or person.underage = True
```

```
def injured():
  remaining_lives--
  if it is 0 {
    print "You are dead."
    exit(1)
  }
  return it  # remaining_lives
```

## Null values

There’s no _null_ in the language. Many inferior languages still have _null_,
even though because Tony Hoare himself recognized it is [a billion dollar
mistake](https://en.wikipedia.org/wiki/Tony_Hoare#Apologies_and_retractions).

Functions with no meaningful return value (e.g. `print`) will implicitly return
`True` (like in Bash).

## Naming conventions

Each language, each library, each team has their own naming conventions. This
can be very confusing for a developer when they arrive in a new
environment. Should we name a function `to_string`, `toString` or `ToString`?

This is a frequent source of compiler errors, for no good reason. Like in Visual
Basic and SQL, we decide to ignore case in variable names. (Windows and Mac
generally view filenames as case insensitive too. It is undoubtedly a more
user-friendly behavior.) Some users like to use an uppercase letter at the
beginning of each line, but there is no consensus about it. Similarly,
underscores are ignored: They don’t change the meaning of a word, they are used
only clarity.

For this reason, all 3 identifiers in the following code refer to the same variable:

```
either MyName != "" or my_name = "foo"
print myname
```

Note that diacritics are also ignored (`résumé` is the same as `resume`). This
is a good thing: If you use accents in your library, you don’t want to force
your users to type the accents. Doing so could slow them down (depending on
their keyboard configuration).

Additionally, a leading `$` can be used, as in PHP, Perl, and Bash. It is
ignored by the interpreter though.

```
foo = 2
print $foo
```

## Spelling

Let’s be honest: most people make typos. Even though we type text everyday, we
still make typos in emails, chats, etc. It’s not a big deal: the receiver of the
message generally understands it perfectly. Why should a compiler be nitpicking?

Some compilers, such as Clang, provide a good error message in case of a typo.

Although it's an improvement, it doesn't seem to be the right thing. It feels
wrong when a computer tells you what to do - it should really be the other way
around. Aren't the computers supposed to do what _we_ tell them?

When a symbol is undefined, NoFail will continue the execution using the correct
name (it will first look for case differences, before checking other typos). If
the flag `--fix-my-code` is passed, the source file will be updated.

```
height = 30
print heigth
```

## Implementation

There is no implementation of the language yet. If you'd like to see one, please
comment on the issue tracker.

Any implementation should try as hard as possible to parse grammatically code
(like in HTML, one of the most used languages in the world). If a construct
cannot be parsed, it should be skipped.

As Python 3, new versions of the language might break compatibility in any way
you can imagine.
