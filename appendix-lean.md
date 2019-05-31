# How to read and write in Lean #

## Understanding Lean ##

### Functions ###

You know functions in a different context: continuous mathematics.

_f(x) = x + 1_

That should look familiar. Here's the Lean equivalent (except only over the natural numbers):

	def f (x : nat) := x + 1

If your input is _x = 1_, what's your output?

_f(1) = 1 + 1 = 2_

	#eval f(1) -- result: 2

You can use parentheses if it makes things clearer, but a lot of Lean function calls that you will see in this class will look more like this (without the #eval):

	#eval f 1

Here is what is happening:

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `f` and knows that the thing it needs to evaluate is the function `f`
3. Lean reads `1` and interprets this as an argument to `f`
4. Lean evaluates `f` with an argument of `1` (result: 2)

Here's a more complicated example with function composition. You will likely be dealing with a lot of this!

Let _g(x) = x + 1_ and _h(x) = 2 * x_.

	def g (x : nat) := x + 1
	def h (x : nat) := 2 * x

What if you want to evaluate _h(g(x))_? If _x = 6_, what is the output of _h(g(x))_?

_g(6) = 6 + 1 = 7_, so _h(g(6)) = h(7) = 2 * 7 = 14_. (Alternative method: _h(g(6)) = 2 * g(6) = 2 * (6 + 1) = 2 * 7 = 14_)

	#eval h (g 6) -- result: 14

Here is what is happening:

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `h` and knows that the thing it needs to evaluate is the function `h`
3. Lean reads `(g 6)` as one unit due to the parentheses, and Lean interprets this unit as an argument to `h`; on the inside:
	1. Lean reads `g` and knows that the thing it needs to evaluate is the function `g`
	2. Lean reads `6` and interprets this as an argument to `g`
	3. Lean evaluates `g` with an argument of `6` (result: 7)
4. Lean evaluates `h` with an argument of `(g 6)` a.k.a. 7 (result: 14)

The parentheses are important! Lean will interpret `h g 6` (no parentheses) as calling the function `h` with two arguments: the function `g`, and the number 6. That's different from how it evaluates `h (g 6)` as calling the function `h` with just the one argument of the result of `g 6`.

Here is an example of a function that takes in more than one argument. You will be dealing with a lot of these!

_z = x + y_ (this could also be written as _z(x,y) = x + y_)

	def z (x : nat) (y : nat) := x + y

If your inputs are _x = 3_ and _y = 7_:

_z = 3 + 7 = 10_

	#eval z 3 7 -- result: 10

1. Lean reads `#eval` and knows the next argument should indicate what to evaluate
2. Lean reads `z` and knows that the thing it needs to evaluate is the function `z`
3. Lean reads `3` and interprets this as the first argument to `z`; now Lean will expect the second argument to follow
4. Lean reads `7` and interprets this as the second argument to `z`
5. Lean evaluates `z` with arguments of `3` and `7` in that order (result: 10)


old draft/scratch explanation based on exam 2:

Let's say you see this:

	axiom Person : Type
	axiom Likes : Person → Person → Prop

Think of Likes as a function that takes two arguments/parameters and returns a Prop. In a different programming language, you might have something like:

	Prop Likes(Person p1, Person p2)
	{
		return p1.toString() + " likes " + p2.toString();
	}

or maybe you have this:
	def Likes(p1, p2):
		return (string)p1 + " likes " + (string)p2

and when you call it in those programming languages, you might type `Likes(person1, person2);` or `Likes(person1, person2)`

but since we're in Lean, the way you call it is `Likes person1 person2`

(This particular example needs to be polished; e.g. `Likes person1 person2` is actually a Prop object/returns a Prop object; also, I'm still not as familiar with functional programming and am definitely approaching Lean from an OOP mindset)

### Tactics ###

the |- symbol thing for goals

work on one goal at a time

order matters (link to last section)


### Reading Lean description and error messages ###


### Associativity ###

for the stuff we use in the course, generally right associative


## Small things ##

### Argument order ###

it matters


### Whitespacing ###

A whitespace is a character that is visually blank, hence the name. These include not only spaces and tabs, but also newlines, also known as line breaks. Think of the what you see when you show [nonprinting characters](https://en.wikipedia.org/wiki/Non-printing_character_in_word_processors) in a word processor.

One whitespace is required between every term. More than one whitespace is not required. Lean does not treat terms differently if they have different whitespacing.

The following three definitions are completely identical in all ways except for how they appear to humans:

	def construct_a_proof (P Q : Prop) (p : P) (q : Q) : P ∧ Q := and.intro p q

	def construct_a_proof'
	(P Q : Prop)
	(p : P)
	(q : Q)
	: P ∧ Q
	:= and.intro p q
	
	def construct_a_proof''
		(P Q : Prop) (p : P) (q : Q)
	: P ∧ Q
		:= and.intro p q

Note that the following not-very-human-readable code is valid in Lean and behaves as you would expect if the two definitions were separated by a line break:

	def zero_eq_zero: 0 = 0 := rfl def one_eq_one: 1 = 1 := rfl

Lean really does not care what whitespacing you use, as long as you use at least one whenever necessary.


### End of definitions, lines, etc. ###

text

whitespace, doesn't matter what kind (see above)

keywords

lean might be expecting another argument to a function--remember, the type of whitespacing doesn't matter to Lean

so some keywords might be unexpected and make Lean go WHAT ARE YOU DOING THIS ISN'T AN ARGUMENT

if in doubt just put #check 0 after a red squiggly to see if your later problems are fixed

if in tactic state, commas instead of whitespacing


### Comments ###

You can comment with `--` or `/- -/`, analogous to the `//` and `/* */` that you may be familiar with. Skip this section if you're familiar with comment syntax.

The following code block demonstrates how you can comment:

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

When Lean decides if your code is valid, it completely ignores your comments. As long as your comments have the correct syntax (e.g. no missing `-/`), you do not need to worry about your comments breaking your Lean code.