---
title: Lecture 4
draft: 
tags: 
date: 2024-04-11
---
SG Lang - new ML efficiency and safety
- Look at examples
- Look at complete descriptions
In this course, we will look at the **syntax** and **semantics** and come to a complete description of SG Lang.
- They are going to give me a **grammar**

>[!Definition]
>**Grammar** is a way that we will communicate a language to each other efficiently.

If you go and find a grammar of html it is complicated and a little too complicated. Instead, we will go through a `HTML--` or `XML`, which is a paradigm to design and deliver data.
Today Eggert will give us a grammar of `XML`, from a bottom up point of view, starting from the lowest level - closest to the tokens - and working our way up.
**CS201** talks are pretty good, they are all public.
### About Tokens
#### Edge Cases
```cpp
int i = -21747948;
```
Some want to delineate a difference in `-21747948` with two tokens instead of one. However, you would get an overflow error as the positive will be outside a valid `int`. Instead, tokenizers handle this in a special case.
- For this specific one, it is a single token.
- For most negative numbers, two tokens.
Tokens are weird.
#### White Space and Comments
```cpp
print("x");
// Why isn't this working !@!??/
print("y");
print("z");
// Gives xz
```
The compiler had just two calls to print, and it didn't have a character string `"y"` in the machine code.
What happened is Danish programmers were unhappy with the fact that `\` looked weird so they instead used `??/`, a trigraph, that stood for `\`.
You MUST know the whitespace rules.
A little known white space rule in HTML, is that you cannot put `--` in an HTML comment.
## XML
XML in some sense is outside the scope of the course. This is not a programming language, it is a data language. But there is still a syntax/grammar for it.
```ebnf
CharData ::= [^<&]*
```
"CharData can contain anything you'd like except for `<&`"
- EBNF (Extended BNF) - Want to use shorthand for strict BNF.
- BNF grammar rule - You have a LHS -> RHS, where the LHS must be a single nonterminal symbols and the RHS can be any sequence of terminal or nonterminal symbols.
Terminal Symbols are the exact same as the tokens.
"If you see the sequence of tokens, it is an instance of the left hand side".
**Nonterminals** and **terminals** must be disjoint sets.
Nonterminals - skeleton nodes in a parse tree.
In BNF, you are trying to specify something is in a language. Each node in the parse tree has to follow one of the BNF grammar.
### Example
```ebnf
S::= aS
S::= Sb
S::= cde
```
So the
$$
\begin{align}
NT &= \{S\} \\
T &= \{a,b,c,d,e\} \\
\end{align}
$$
where $NT$ stands for nonterminal and $T$ stands for terminal. Therefore, $abcde$ would be a valid sequence for the LHS $S$.

### EBNF
EBNF is useful since you are creating a **shorter** translation of the rules.
- Whatever EBNF you are looking at you should really be able to translate it into BNF easily.
Harder to write the tools for BNF than EBNF, so some may opt for BNF.
- Many will do a pre-pass from EBNF to BNF and then do the bulk of the core work with the BNF.
**NOTE**: The RHS of a BNF rule may be empty, which leads to a nonterminal, denoting as an empty string or an epsilon or an empty array.
```ebnf
CharData ::= [^<&]*
x ::= a
x ::= b
x ::= c
...
CharData ::= 
CharData ::= x CharData
```
The rest of the rules
```ebnf
CharRef ::= '&#'([0-9]+ | 'x'[0-9a-fA-f]+)';'
Reference ::= EntityRef | CharRef
EntityRef ::= '&'Name';'
Eq ::= S? '=' S?
S ::= (#x20 | #x9 | #xD | #xA)+
AttValue ::= '"'([^<&"]|Reference)*'"' | "'"([^<&']|Reference)* "'"
NameChar ::= NameStartChar | "-" | "." | [0-9]
NameStartChar ::= ":" | [A-Za-z] | "_"
Name ::= NameStartChar (NameChar)*
Attribute ::= Name Eq AttValue
Stag ::= '<' Name (S Attribute)* S? '>'
Etag ::= '</' Name S? '>'
EmptyElemTag ::= '<' Name (S Attribute)* S? '/>'
element ::= EmptyElemTag | STag content Etag
content ::= CharData ? ((element | Reference) CharData?)*
```
A valid character reference in XML is for example
```xml
&#x98f;
```
which stands for unicode digit 98f.
A valid entity reference in XML is
```xml
&amp;
```
S stands for the terminal symbols in our notation.
Something interesting about XML is that it is simply parsing. It is not having two levels with tokenization in lexical analysis.
- Most real world compilers do operate at two levels even if the grammar is at one level because it will be more efficient that way.

#### The metanotations we are using
`[...]` anything within this range
`*` 0 or more `+` 1 or more
`|` or
`?` something is optional, 0 or 1 of this symbol

### The `element` and `content` nonterminal
These definitions are mutually recursive,.
```ebnf
element ::= EmptyElemTag | STag content Etag
content ::= CharData ? ((element | Reference) CharData?)*
```
A **Regular language** is a language where grammar can be converted into a regular expression.