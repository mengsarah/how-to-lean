Under construction. Please feel free to contribute!

Intended for anyone learning Lean with little to no programming background, such as some students taking CS 2102 Discrete Mathematics at UVA.

# How to read and write in Lean #

**Table of Contents**

[Understanding Lean](#understanding-lean)
* [Functions](#functions)
* [Tactics](#tactics)
* [Reading Lean descriptions](#reading-lean-descriptions)
* [Reading Lean error messages](#reading-lean-and-error-messages)
* [Associativity](#associativity)

[Small things](#small-things)
* [Argument order](#argument-order)
* [Whitespacing](#whitespacing)
* [End of definitions, lines, etc.](#end-of-definitions-lines-etc)
* [Comments](#comments)

## Understanding Lean ##

The information here is meant to be a quick overview. For further reading, see [the Lean 3.4 documentation](https://leanprover.github.io/theorem_proving_in_lean/).

### Functions ###

You know functions in a different context: continuous mathematics.

_f(x) = x + 1_

That should look familiar. Here's the Lean equivalent (except only over the natural numbers):

```lean
def f (x : nat) := x + 1
```

If your input is _x = 1_, what's your output?

_f(1) = 1 + 1 = 2_

```lean
#eval f(1) -- result: 2
```

You can use parentheses for one argument if it makes things clearer, and you will want to use parentheses for complex function calls, but a lot of the more simple Lean function calls that you will see in this class look more like this (without the #eval):

```lean
#eval f 1
```

Here is what is happening:

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `f` and knows that the thing it needs to evaluate is the function `f`
3. Lean reads `1` and interprets this as an argument to `f`
4. Lean evaluates `f` with an argument of `1` (result: 2)

Here's a more complicated example with function composition. You will likely be dealing with a lot of this!

Let _g(x) = x + 1_ and _h(x) = 2 * x_.

```lean
def g (x : nat) := x + 1
def h (x : nat) := 2 * x
```

What if you want to evaluate _h(g(x))_? If _x = 6_, what is the output of _h(g(x))_?

_g(6) = 6 + 1 = 7_, so _h(g(6)) = h(7) = 2 * 7 = 14_. (Alternative method: _h(g(6)) = 2 * g(6) = 2 * (6 + 1) = 2 * 7 = 14_)

```lean
#eval h (g 6) -- result: 14
```

Here is what is happening:

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `h` and knows that the thing it needs to evaluate is the function `h`
3. Lean reads `(g 6)` as one unit due to the parentheses, and Lean interprets this unit as an argument to `h`; on the inside:
	1. Lean reads `g` and knows that the thing it needs to evaluate is the function `g`
	2. Lean reads `6` and interprets this as an argument to `g`
	3. Lean evaluates `g` with an argument of `6` (result: 7)
4. Lean evaluates `h` with an argument of `(g 6)` a.k.a. 7 (result: 14)

The parentheses are important! Lean will interpret `h g 6` (no parentheses) as calling the function `h` with two arguments: the function `g`, and the number 6. That's different from how it evaluates `h (g 6)` as calling the function `h` with just the one argument of the result of `g 6`. (You would get an error in this case, because `h` was only defined to take in one argument.)

Here is an example of a function that takes in more than one argument. You will be dealing with a lot of these!

_z = x + y_ (this could also be written as _z(x,y) = x + y_)

```lean
def z (x : nat) (y : nat) := x + y
```

If your inputs are _x = 3_ and _y = 7_:

_z = 3 + 7 = 10_

```lean
#eval z 3 7 -- result: 10
```

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `z` and knows that the thing it needs to evaluate is the function `z`
3. Lean reads `3` and interprets this as the first argument to `z`; now Lean will expect the second argument to follow
4. Lean reads `7` and interprets this as the second argument to `z`
5. Lean evaluates `z` with arguments of `3` and `7` in that order (result: 10)


old draft/scratch explanation based on exam 2:

Let's say you see this:

```lean
axiom Person : Type
axiom Likes : Person → Person → Prop
```

Think of Likes as a function that takes two arguments/parameters and returns a Prop. In a different programming language, you might have something like:

```java
Prop Likes(Person p1, Person p2)
{
return p1.toString() + " likes " + p2.toString();
}
```

or maybe you have this:

```python
def Likes(p1, p2):
	return (string)p1 + " likes " + (string)p2
```

and when you call it in those programming languages, you might type `Likes(person1, person2);` or `Likes(person1, person2)`

but since we're in Lean, the way you call it is `Likes person1 person2`

(This particular example needs to be polished; e.g. `Likes person1 person2` is actually a Prop object/returns a Prop object; also, I'm still not as familiar with functional programming and am definitely approaching Lean from an OOP mindset)

### Tactics ###

the |- symbol thing for goals

work on one goal at a time

order matters (link to last section)


### Reading Lean descriptions ###


### Reading Lean error messages ###

`type mismatch` - usually an issue with your arguments, either in your method definition or in your later usage; the message should tell you what Lean is expecting ("term x has type Y but is expected to have type Z"); check that you have the right number of explicit arguments, that they are of the right type(s), and that they are in the right order (match your later usage with your method definition; you will encounter some version of this in all programming languages)

other kinds of `mismatch` - similar to above, double check your method definitions and that your later usage matches your method definitions

`unknown identifier` - you did not define a term before using it--i.e. you did not tell Lean what type a term is; you can define terms within method definitions, just make sure that they are in the form (x : X) because Lean needs to know that x is of type X

`declaration uses sorry` - your definition is incomplete

`type expected`, `function expected` - exactly what they sound like, but this could be as simple of a fix as making sure you have all of the various punctuation syntax included; you may need to also refer to the comments under `type mismatch` above


### Associativity ###

for the stuff we use in the course, generally right associative

so P -> Q -> R is the same as P -> (Q -> R) (they both take two consecutive arguments) but is NOT the same as (P -> Q) -> R (this takes only one argument, which itself took an argument)


## Small things ##
(should be renamed)

### Argument order ###

it matters; you will encounter some version of this in all programming languages


### Whitespacing ###

A whitespace is a character that is visually blank, hence the name. These include not only spaces and tabs, but also newlines, also known as line breaks. Think of what you see when you show [nonprinting characters](https://en.wikipedia.org/wiki/Non-printing_character_in_word_processors) in a word processor.

One whitespace is required between every term. More than one whitespace is not required. Lean does not treat terms differently if they have different whitespacing.

The following three definitions are completely identical in all ways except for how they appear to humans:

```lean
def construct_a_proof (P Q : Prop) (p : P) (q : Q) : P ∧ Q := and.intro p q
```

```lean
def construct_a_proof'
(P Q : Prop)
(p : P)
(q : Q)
: P ∧ Q
:= and.intro p q
```

```lean
def construct_a_proof''
	(P Q : Prop)
		(p : P)
		(q : Q)
	: P ∧ Q
:=
	and.intro
		p
		q
```

Note that the following not-very-human-readable code is valid in Lean and behaves as you would expect if the two definitions were separated by a line break:

```lean
def zero_eq_zero: 0 = 0 := rfl def one_eq_one: 1 = 1 := rfl
```

Lean really does not care what whitespacing you use, as long as you use at least one whenever necessary.


### End of definitions, lines, etc. ###

whitespace, **doesn't matter what kind** (see above)

keywords like `def` mark the start of a new "line"

keywords like `end` mark the end of a "line"

lean might be expecting another argument to a function--remember, the type of whitespacing doesn't matter to Lean

so some keywords might be unexpected and make Lean go WHAT ARE YOU DOING THIS ISN'T AN ARGUMENT (e.g. if you called a 2-argument function but only gave one argument, even if you put a bunch of blank lines and then a `def`, Lean will not like that you put `def` instead of the second argument that you never gave)

if in doubt just put #check 0 after a red squiggly to see if your later problems are fixed

if in tactic state, commas instead of whitespacing


### Comments ###

You can comment with `--` or `/- -/`, analogous to the `//` and `/* */` that you may be familiar with. Skip this section if you're familiar with comment syntax.

The following code block demonstrates how you can comment (paste into a .lean file so you can see syntax highlighting):

```lean
-- one way to comment

/- another way to comment
this can span multiple lines -/
/- or it can span just one line -/

/--/
CAUTION: This text is a comment! You need something other than "/-" immediately before the closing -/

def construct_a_proof'''
(P Q : Prop) /- you can even comment between terms (but please don't do this in any language; it makes code harder to read) -/ (p : P) (q : Q)
: P ∧ /- this is also syntactically correct but absolutely terrible practice -/ Q
:= and.intro p q -- this kind of comment lasts to the end of the line
```

When Lean decides if your code is valid, it completely ignores your comments. As long as your comments have the correct syntax (e.g. no missing `-/`), you do not need to worry about your comments breaking your Lean code. However, you may find that some comments, particularly multi-line comments, can make it tricky to track down [whitespacing](#whitespacing) errors. (Comments are still excellent and not discouraged.)