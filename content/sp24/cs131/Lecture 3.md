---
title: Lecture 3
draft: 
tags: 
date: 2024-04-09
---
## OCaml
### Shorthands
In OCaml, more functions are anonymous than named. You can write function declarations more verbose, but most people will do it the quick and easy way.
```ocaml
let f = fun x -> x + 1;;
let n = (c + 1);;
let f x = x + 1;; 
```
The latter is the most common.
Some shorthands for **pattern matching** are as follows:
```ocaml
fun x -> match x with | 0 -> -1 | y -> y +1;;
function | 0 -> -1 | y -> y + 1;;
```
Shorthand for **currying** is
```ocaml
let p = fun x y -> x + y + 1;;
let p = fun x -> fun y -> (x + y + 1);;
let q = fun pr -> match pr with | (x,y) -> x + y + 1;;
let q = function (x,y) -> x + y + 1;;
let q (x,y) = x + y + 1;;
```
The function `q` is an example of not currying, and insists that there are two arguments in order to start any part of the function.
```ocaml
let f = p 27;; (*example of currying, you are creating a function that was returned from another function *)
let h x y = q(x,y);;
let g = h 27;; (*curried tuple program lol*)
```
Most OCaml programmers would prefer the curried style rather than a tuple style.
### Recursion
If you find the recursive function in OCaml you must tell the OCaml function that it is recursive.
```ocaml
let rec fact = function | 0 -> 1
						| n -> n * fact(n-1)
```
Eggert actually doesn't like this implementation, since it will cap out its stack and also you should just use a lookup table. Let's instead look at how to reverse a list.
```ocaml
let rec reverse l = match l with
	| [] -> []
	| h::t -> (reverse t)@h
```
This would get an error: h is not a list! TIP: when you are doing this at home, do not just gloss over the types, make sure that your types are correct.
You would just have to put square brackets around h, where `[h]` is consumed by the `@` operator.

**Eggert**: But this function is still very expensive, since you are concatenating the list which is an $O(N)$ function, making the function with a total $O(N^2)$.

>[!Question]
>Why do you have to specify `rec`?
>OCaml won't let you define a variable in terms of itself, for example `let n = n*n - 4`. 

Instead, lets define a new function `rev`.
```ocaml
let rec rev l a = match l with
	| [] -> a
	| h::t -> rev t (h::a)
let reverse l = rev l []
```
The difference is that the function `::` is very inexpensive, specifically $O(1)$ meaning that our algorithm will be $O(N)$.
Say that someone asks you to make your function look beautiful, what would you do?
```ocaml
let reverse = let rec rev a = function
	| [] -> a
	| h::t -> rev (h::a) t in rev []
```
There is a name for this technology or style, an **accumulator**:
An extra argument passed into a function to the next iteration of the recursive function in order to *accumulate* for the result of the function.
### Higher order Functions
`max` takes a list and gives us the biggest integer in that list.
```ocaml
let rec max = function
	| h::t -> let m = max t in if h < m then m else h
	| [] -> -9999999999999 (*ocaml does not have a way of doing this*)
```
This max function only works on lists of integers, since the `<` operator only works on integers.
```ocaml
let max i lt = function
	| [] -> i
	| h::t -> let m = max i t in if lt h m then m else h
```
This max function is more general. It takes in 3 argument, and is curried.
```ocaml
let imax = max -999999999 (<)
```
This is a function in OCaml that is specifically for finding the max of the integers.
```ocaml
max: 'a -> ('a -> 'a -> bool) -> 'a list -> 'a
```
**Eggert Tip**: you should think and guess of the type of a function before you actually write it in the interpreter, it'll help you understand it better.
### Fun vs. Function
Both of them will let you do
```ocaml
fun x -> x + 1;;
function x -> x + 1;;
```
They generalize in certain ways - `fun` is in currying and `function` is in pattern matching.
```ocaml
fun x y -> x + y
fun x -> fun y -> x + y

function
	| 0 -> -3
	| x -> x + 1
```
### Defining your own types
Defining discriminant unions, you have the **type tag** and the **value**
```ocaml
type 'a option = 
	| None
	| Some of 'a (*constructors*)
```
The `Some` operator can either construct or pattern match
```ocaml
Some ["x"; "y"] (*this is of type string list option*)
None (*this is of type 'a option*)
```
What if we tried to make our own list type?
```ocaml
type 'a mylist =
	| Empty
	| Nonempty of 'a * 'a mylist
let consmylist a l = Nonempty (a,l)
```
compared to C, which a union has some overlap of an int and a float, you can't really take a discriminant union apart since you must declare the type in the type tag.
- OCaml does not have the major problem as in cpp because cpp crashes at a null pointer. OCaml that cannot happen because you must take apart the value of a type.
- OCaml avoids this whole class of runtime errors.

**Eggert:** OCaml, types are inferred. Never written down, it's just never needed. Some OCaml programmers don't like that. You can declare type if you'd like.
```ocaml
fun (x:int) -> x + 1;;
```
If you always write the types down, you won't learn as much about your code. If you let OCaml figure it out for you, you'll figure more about how your code works.
## Syntax
**Form** independent of meaning - easy stuff.
Programming language syntax has a lot in common with natural language syntax - we stole a bunch of theory from the lateral language syntax.
- This is to prove that people know what they are doing.
"Colorless green ideas sleep furiously." - N. Chamsky
- This is an example of a sentence that is syntactically perfect in English but it does not really mean anything.
"Ireland has leprechauns galore." - P. Eggert
- This is an example of a sentence that can be interpreted by most if not all Americans but is syntactically odd, since galore is an adjective.
- This a strange corner of English, where the syntax is maybe wrong but semantically coherent.
- When the English conquered Ireland, they took Ireland's word "galore" in Irish Gaelic where you put the adjective after the word.
"Time flies."
- Syntactically and semantically correct, but it is **too** right.
- There are two meanings to this, time flies as in time how fast the flies are flying or time fies, time is going by so fast.
- Ambiguous, so you need context.
Ambiguous sentences have uses in natural language, however programmers don't like ambiguity.
- We don't want the compiler to misinterpret our sentences.
Most programming languages have multiple levels of syntax:
### Lexical Analysis
Take the original characters or bytes and figure out where the boundary are between the words.
```cpp
int main(void) { return !getChar();}
```
- Instead of looking at individual characters, use the individual tokens.
```
**THE LEXEM**
INT ID ( VOID ) { RETURN ! ID ( ); }
```
This loses function and is too general, you must specify `ID` where you have extra information associated with them to know what the program meant, but otherwise, the lexical interpretation is all you need to know.
#### Issues of Tokenization
Where are the token boundaries?
- Sometimes spaces don't matter, but sometimes they do. 
- Greedy Tokenization - Looks at the following sequence of bytes, finds the longest token that could be a token and says that is your token.
```cpp
return a+++++b;
```
```
RETURN ID ++ ++ + ID ;
```
- This particular C Program is invalid, since you cannot apply the plus plus suffix operator that is not a variable. This is an invalid expression even though it could have been valid if the tokenizer had been lazier.
```
RETURN ID ++ + ++ ID;
```
In current C, this is a valid program
```
return <AMBIGUOUS CHARACTER>;
```
is this a good idea? is it okay to have international characters and identifiers?
```cpp
int o = 27; // ASCII o
return o + 1; // Cyrillic o
```
- Could be hard to read, example above lol.
- This feature is often disabled. It can be a security concern.
This could be the problem with domain names as well, where `microsoft.com` could contain a cyrillic o instead of an ascii o.

For keywords, `if` is a reserved word. therefore the following would be illegal.
```cpp
int if = 12;
```
Having reserved words make languages harder to extend, since if you add new keywords you are going to break preexisting working code.
j