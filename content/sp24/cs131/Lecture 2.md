---
title: Lecture 2
draft: 
tags: 
date: 2024-04-04
---
Today Eggert wants to finish talking about Design Principles, in specific **Orthogonality**,
- The categories of programming languages
- And Functional Programming + OCaml
## Design Principles
### Efficiency
- CPU time
	- Real time, where the cores of a CPU matter
- RAM time
- Caching
- Power and Energy
- Network access
	- If my program requires a lot of network access, that can be a downer if needed for use offline.
- I/O to local devices
	- You wouldn't want your application to use local storage, RAM storage would be a better usage.
### Maintainability
- Writability
	- How long does it take to write code?
	- One of the most writable languages, APL. APL looked kind of greek and had greek symbols in it.
		- However, after 6 months later, he wasn't able to read it. It took him longer to read it than to write it.
- Readability
	- Many people have to read more than they have to write code.

>[!Remark]
>Readability is far more important than writability in programming.

- Extensibility
	- Successful languages evolve and make changes. It is a live language not a dead one.
	- You need this ability in programming languages in order to get things to work.
- Stability
- Documentation + Training
- Safety
- Portability
- Debuggability

Many languages will find their niche, and that niche is in one of these abilities.

>[!Question]
>This is the space that we want, but now how do we get to where we want?

## Eggert's 3 Main Categories of Languages
### Imperative Languages
Imperative - That's the person who gives orders. You're the emperor, you're in charge.
- A series of commands to be executed in order.
- Imperative languages are inherently **sequential**. The `;` command is very natural to the imperative language, where `;` indicates the end of a command in a sequence.
- Ex. C/C++/Java
- This is probably the most popular way that programs are written, it is the most obvious way.

>[!Question]
>Consider `A;B;C;`, if this is such a fundamental and logical way to write programs and to how the computer works, why do we need more?
>We need some method of abstraction, it can get disorganized (Subroutines, OOP).
>How will you learn how to do something in parallel? It insists, you must finish running `A`, before you run `B`.

Another problem with imperative languages is that you may run into the von Neumann bottleneck
### Functional Languages
Written by declaring functions, functions can be called in "parallel", not perfectly parallel but still pretty parallel.
```
A(B(x,y), C(z))
```
- A would have to wait for B and C, but B and C are being run in parallel
Ex. ML, Lisp, F#, Ocaml, Scheme, Scala
In functional programming, there are no side effects.
- We don't want to change memory in RAM. There are no assignment statements.
### Logic Languages
Not only do we give up side effects (no assignment statements), we give up function calls.
You have **predicates** - a statement about that world that is either true or false. Ex. It's raining.
- The way you hook together predicates in a logic language is in `AND(&), OR(|), etc.`
The basic idea of logic language is you tell the computer your problem in logic statements and ask the computer a question and it gives you the answer. 
- This is the dream of logic languages, efficiency problems (highest level languages).
Logic languages are a great way to learn how to code in other languages e.g. imperative languages.
Logic languages have more in common with **DB query languages** .
We will be working mostly with `Prolog`.

### Read Eval Print Loop
All of the languages, OCaml, Scala, Prolog work in this way.
it will finish reading your logic, evaluate it, and print the answer.

>[!Question]
>What can you do in functional languages that you can't do imperatively?
>It just seems like you have your hands tied around your back. There is a functional subset of C++ that you don't do assignment statements and you can get your work done. If you do that, your code is harder to read.
>The functional programming is a subset of imperative language. It will just be easier to read for the functional problems.

## Functional Programming
### Motivations
- Parallelizability: we want to escape Von Neumman's bottleneck. We want our programs to be faster.
- Clarity: We want our programs to be easier to understand.
	- At first glance they are not obvious at all. It will take some time to see the clarity introduced.
	- Mathematicians have been thinking about functions for a long time. We are used to a mathematical style which is well tuned for how humans think.
		- If you give a mathematician `i = i + 1`  then they will be like this don't make sense. We reinvented the wheel for programming languages
		- The issue is that later down the road `i + j` might mean something else than what it initially was. In other words, we need **referential transparency**.
## OCaml
### ML Basic Properties
- Compile-time type checking
- Type inference
	- Blessing: shorter and easier to read code
	- Curse: harder to debug, weird error message
- No 'del' or 'free'
- Higher order functions
	- So common that you do it all the time, it happens its a part of the territory
	- Clearer and more reliable code once you get used to it.
### Basic Syntax/Type Inference
```ocaml
# let x=37*37;; 
x: int = 1369
```

```ocaml
# if x < 2000 then "a" else "b";;
-: string = "a"
```

A problem in this code snippet is that there are no bounds for the if else, so there may be problems in nested if else etc.

```ocaml
# if x < 2000 then "a" else 27;;
ERROR
```

 There is no consistent type returned and hence it errors.
### Tuples

```ocaml
# 1, "a";;
-: int * string = (1,"a")
```

tuple can contain multiple types indicated by the `*`.
```ocaml
# 1, "a", 3.5;;
-: int * string * float = (1, "a", 3.5)
```

This would also work.
### Lists

```ocaml
# [1; -3; 17]
-: int list=[1;-3;17]
# [1; "a"]
ERROR
# [];;
-: 'a list = []
```

A list only contains one type, however it has its advantages for functions that would want to take a specific typed list. For tuples, the types `int * int` is not the same as `int * int * int`.
- It depends on what you need, do you need the arbitrary `int list` or a more specific `int * int` tuple?
An empty array is of type a, where `int list = a list`

```ocaml
# let f x = x + 1;;
f: int -> int = <fun>
# f 3;;
```

You write down the function, you write down the argument and you are done. You are stripping the unnecessary fluff.
```ocaml
# 3::[4;-2;7] (*construct a new value*)
[3;4;-2;7]
```

This is how to append to a list.

```ocaml
# let cons (x,y) = x::y;;
cons: 'a * 'a list = <fun>

# let car (x::y) = x;; (*used as pattern matching*)
car : `a list -> `a = <fun>
WARNING
```

The only thing that doesn't work here is `[]`, the empty `a list`. This function is no good, because it is not exhaustive.
- Please fix your program if you get a warning. This is an example of a function that you should not write.
A common architecture in OCaml is using the same operator to construct and destruct as shown with the `::` operator.
### OCaml Patterns
- Underscore discards the element.
- `P::Q` Any nonempty list where P matches the head of the list, or the first element of the list, and the tail, or the whole of the list after it.
- `P,Q,R` matches a tuple.
- `P;Q;R` matches a list.

```ocaml
let f l=(match l with | [] -> 0.0 | x::_-> x + 1.0);;
f: float list -> float = <fun>
```

Better way to use an array to get a result. Here is another and more complicated way to write `cons` in OCaml

```ocaml
# let ccons x y = x::y;;
ccons: `a -> 'a list -> `a list = <fun>
```

The arrows are right associative.

```ocaml
# ccons 3 [3;-2];
-: int list = [3;3;-2]
```

`ccons` technically returns a function and then uses that function to prepend to its later passed array `y`.

```ocaml
# let f = ccons "a";
f: string list -> string list = <fun>
# f ["b", "c"]
-: string list = ["a":"b":"c"]
```

This is an example of **currying**. 


