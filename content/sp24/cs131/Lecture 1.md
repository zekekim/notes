---
title: Lecture 1
draft: 
tags: 
date: 2024-04-02
---
[TIOBE Index](https://www.tiobe.com/tiobe-index/)

>[!Question]
> Why are there multiple Programming Languages?
> Rather than Programming Languages, Eggert would rephrase the course to Programming Notations since ther is a distinct way that each programming language works.

## Eggert Bedtime Story
D.E. Knuth's *The Art of Computer Programming* (TAoCP) 1969 Vol.1
- First book to really utilize `TeX`
Addison Wesley Created `TeX` was built on the foundation of writing text anywhere and making it look good.
- `TeX` was written in the programming language `Pascal`.
- D.E Knuth set out to write a book about `TeX` to show the beauty of it. 
Dream of code and documentation together, for the `TeX` book:
```tex
// tex.texpas?
\begin{section}
Some documentation
\end{section}
function func()
begin

end
```
Called it **literate programming**, writing a program and writing the literature of the program at the same time.
D.E. Knuth went to the **CACM** M.D. Mclroy
- Mclroy contributed the pipe (`|`) to the shell.
- D.E. Knuth published a paper with literate programming that solved the problem of sorting the popularity of the word using hash tries, the code and the literature in the same paper.
	- In the afterword by Mclroy he attempted to implement this in the shell.
```sh
tr -cs A-Za-z '[\n*]' | sort | uniq -e | sort -rn
```
- Sorted the words into a line each, then find the unique words with a count appended at the front, then sort it by reverse and by number.

>[!Question]
>Which program ran faster?
>Knuth's runs faster as it is a single program that is inherently sequential, while Mclroy's shell solution seems implicitly sequential without any parallelism due to the fact that each input depends on the output of each other program.

>[!Question]
>Which program is easier to maintain?
>Eggert argues that the bash script is easier to maintain and extend.

Based on what you need the program for, you may want to pick different solutions and different approaches in terms of programming language for each.
- **Sapir-Whorf Hypothesis** - There is no fundamental grammar, it is all learnt.
	- The structural diversity of languages is limitless.
	- There is a finite number of sentences that can be said of a language.
	- The language we use, to some extent, ~~determines~~ affects how we view the world and how we think.
Language is not only a tool for thought, but governs how you think.
Eggert argues that the Sapir-Whorf Hypothesis is *kind of* true in programming languages.
- Somewhat of a nice feature of a language but also a curse.

>[!Quote]
>Use languages as a tool but do not conform your mind to just one - Eggert

## Logistics 
Read Webber, Chapter 1-3, 5, 7, 9
- First programming assignment is in OCaml, based off of ML.
Don't Cheat!
- If the best you can do in this class is what ChatGPT can do, you will lose.
$3^{n-1}$ for $n$ days late.
## Structure of the Class
### Theory
- Syntax
- Semantics
- Functions
- Names
- Types
- Control
- Objects
- Exceptions
- Concurrency
- Scripting
- Memory
### Practice
- OCaml - Functions, Types, Syntax (HW 1,2)
- Scheme - Semantics, Control 
- Java - Exceptions, Concurrency, Names, Types, Memory
- Python - Concurrency, Scripting
- Rust - Memory
- Prolog - Semantics, Control
## Judging Languages
Eggert wants to stay away from any of the monopolistic/ethical concerns with languages.

>[!Question]
>How do we pick a language?
>Measure the costs and benefits of the language: we want the benefits to outweigh the costs.

### Costs 
- Training
	- If everyone knows `cpp`, why use `prolog`?
- Reliability
	- Encourage reliable code, we should get what we asked for in terms of a program.
- Program Development
	- Elegance in code and documentation.
- Runtime Expense
	- How much CPU time? How much RAM? How many resources?
- Compile/Build Time
	- Ex. Go was engineered to minimize the compile time compared to gcc or clang.
- Maintenance
	- Being able to fix bugs and stuff
- Licensing Fees
	- Should be zero by FreeBSD, why does MATLAB even exist.
- Porting Expense
	- Ability to port on different machines. Ex. React Native vs Swift

## Language Design Issues
- Orthogonality - Different aspects of your system should be independent.
	- If you pick one feature in one axis you can pick any feature in any of the other axes.

