<div align="center">

# PROLOG Practicals

### Artificial Intelligence (DSC-09) · B.Sc. (Hons.) Computer Science · Semester III

A collection of 17 classical PROLOG programs — from a family-tree knowledge base to list manipulation, arithmetic recursion and Euclid's algorithm — written and tested for the AI practical examination at Ramanujan College, University of Delhi.

![Language](https://img.shields.io/badge/Language-PROLOG-1f4e79?style=for-the-badge)
![Interpreter](https://img.shields.io/badge/Interpreter-SWI--Prolog-EA9B30?style=for-the-badge)
![Practicals](https://img.shields.io/badge/Practicals-17-2E8B57?style=for-the-badge)
![Semester](https://img.shields.io/badge/Semester-III-6A5ACD?style=for-the-badge)

**Author:** Nishant Das · Examination Roll No. 25020570008
**Submitted to:** Ms. Bhavya Ahuja, Department of Computer Science
**Session:** 2026–27

</div>

---

## Table of Contents

| # | Practical | Concept | Code |
|:-:|-----------|---------|:----:|
| 1 | [Family Tree](#practical-1--family-tree) | Facts, rules, relations | `family_tree.pl` |
| 2 | [Concatenation of Two Lists](#practical-2--concatenation-of-two-lists) | Structural recursion on lists | `concatenation_of_lists.pl` |
| 3 | [Reversal of a List](#practical-3--reversal-of-a-list) | Recursion + accumulator list | `reverse_list.pl` |
| 4 | [Sum of Two Numbers](#practical-4--sum-of-two-numbers) | Arithmetic evaluation with `is` | `sum_of_2_numbers.pl` |
| 5 | [Maximum of Two Numbers](#practical-5--maximum-of-two-numbers) | Guarded comparison clauses | `max_min.pl` |
| 6 | [Factorial of a Number](#practical-6--factorial-of-a-number) | Linear arithmetic recursion | `factorial.pl` |
| 7 | [Power of a Number](#practical-7--power-of-a-number) | Linear arithmetic recursion | `power_of_number.pl` |
| 8 | [Multiplication of Two Numbers](#practical-8--multiplication-of-two-numbers) | Arithmetic evaluation with `is` | `multiply_2_numbers.pl` |
| 9 | [Membership in a List](#practical-9--membership-of-an-element-in-a-list) | Non-deterministic recursion | `searching_in_lists.pl` |
| 10 | [Sum of List Elements](#practical-10--sum-of-the-elements-of-a-list) | Head/tail decomposition | `sum_of_elements_in_list.pl` |
| 11 | [Even / Odd Length](#practical-11--even-and-odd-length-of-a-list) | Pattern matching two at a time | `evenlength_oddlength.pl` |
| 12 | [Maximum Element of a List](#practical-12--maximum-element-of-a-list) | List recursion + helper predicate | `max_in_list.pl` |
| 13 | [Insertion at Nth Position](#practical-13--insertion-of-an-item-at-the-nth-position) | Positional counter recursion | `permutation_generator.pl` |
| 14 | [Deletion at Nth Position](#practical-14--deletion-of-the-element-at-the-nth-position) | Positional counter recursion | `deleting_from_list.pl` |
| 15 | [GCD of Two Numbers](#practical-15--greatest-common-divisor-of-two-numbers) | Euclid's algorithm | `gcd.pl` |
| 16 | [Length of a List](#practical-16--length-of-a-list) | Counting by recursion | `length_of_list.pl` |
| 17 | [Nth Term of Fibonacci](#practical-17--nth-term-of-the-fibonacci-series) | Tree recursion | `fibonacci.pl` |

---

## About This Repository

PROLOG is a **declarative** language: instead of telling the computer *how* to compute, you state *what is true*. A program is a set of **facts** (unconditionally true statements) and **rules** (statements true if some conditions hold). The interpreter answers a **query** by searching for a proof — this is exactly the resolution-based inference studied in the AI theory paper.

Every program here follows the same shape:

```prolog
head :- body.        % a rule: head is true if body can be proved
head.                % a fact: head is unconditionally true
```

and every recursive predicate here has the same two-part skeleton:

```prolog
predicate(Base_Case, Base_Result).                  % termination
predicate(Input, Result) :-                         % recursion
    <reduce Input>,
    predicate(Smaller_Input, Sub_Result),
    <combine Sub_Result into Result>.
```

> **Course context:** these practicals correspond to the PROLOG component of DSC-09 (Artificial Intelligence), University of Delhi. They illustrate knowledge representation with facts and rules (Practical 1) and recursive problem solving in a logic programming language (Practicals 2–17).

---

## Repository Structure

```
prolog-practicals/
├── README.md
├── family_tree.pl                 # Practical 1  — family relationships
├── concatenation_of_lists.pl      # Practical 2  — conc/3
├── reverse_list.pl                # Practical 3  — reverse/2
├── sum_of_2_numbers.pl            # Practical 4  — sum/3
├── max_min.pl                     # Practical 5  — max/3, min/3
├── factorial.pl                   # Practical 6  — fact/2
├── power_of_number.pl             # Practical 7  — power/3
├── multiply_2_numbers.pl          # Practical 8  — multi/3
├── searching_in_lists.pl          # Practical 9  — mem/2
├── sum_of_elements_in_list.pl     # Practical 10 — sum_list/2
├── evenlength_oddlength.pl        # Practical 11 — evenlength/1, oddlength/1
├── max_in_list.pl                 # Practical 12 — maxlist/2
├── permutation_generator.pl       # Practical 13 — insert/4 (+ permutation/2)
├── deleting_from_list.pl          # Practical 14 — delete/3
├── gcd.pl                         # Practical 15 — gcd/3
├── length_of_list.pl              # Practical 16 — len/2
└── fibonacci.pl                   # Practical 17 — fibo/2
```

Each `.pl` file is self-contained: it holds the predicates for one practical, so it can be consulted on its own.

---

## Getting Started

**1. Install SWI-Prolog**

| Platform | Command |
|----------|---------|
| Windows | Installer from [swi-prolog.org/download/stable](https://www.swi-prolog.org/download/stable) |
| Ubuntu / Debian | `sudo apt install swi-prolog` |
| macOS | `brew install swi-prolog` |

**2. Load a program and run queries**

```bash
swipl                          # start the interpreter
?- consult('factorial.pl').    # load the program  (or: [factorial]. )
true.

?- fact(5, F).                 # ask a query
F = 120.
```

You can also start directly with a file: `swipl factorial.pl`.

**3. Useful keys while testing**

| Key | Action |
|:---:|--------|
| `;` | ask for the next solution |
| `Enter` | accept the current solution and stop |
| `halt.` | exit the interpreter |

---

## Practicals

### Practical 1 — Family Tree

**Question.** Write a PROLOG program to implement the family tree and demonstrate the family relationship.

**Program** — `family_tree.pl`

```prolog
% male and female members of the family
male(ramesh).
female(amrita).
male(suresh).
female(saumya).

% father(Father, Child)
father(daanish, raaju).
father(daanish, ramya).
father(rahul, aditya).
father(rahul, daisy).

% mother(Mother, Child)
mother(sangeeta, raaju).
mother(sangeeta, ramya).
mother(nandini, aditya).
mother(nandini, daisy).

% grandparents of the family
fatherkimother(amrita, daanish).   % amrita is daanish's father's mother
fatherkafather(ramesh, daanish).   % ramesh is daanish's father's father

mummykimummy(saumya, sangeeta).    % saumya is sangeeta's mother's mother
mummykepapa(suresh, sangeeta).     % suresh is sangeeta's mother's father

% -------- Sibling Rules --------
sister(X, Y) :-
    female(X),
    father(A, X),
    father(A, Y),
    mother(B, X),
    mother(B, Y).

brother(C, D) :-
    male(C),
    father(A, C),
    father(A, D),
    mother(B, C),
    mother(B, D).

% -------- Grandparent Rules --------
grandfather(F, G) :-
    male(F),
    fatherkafather(F, A),
    father(A, G).

grandmother(M, H) :-
    female(M),
    fatherkimother(M, A),
    father(A, H).

% -------- Generic Parent Rule --------
parent(L, P) :-
    father(L, P);
    mother(L, P);
    fatherkimother(L, P);
    fatherkafather(L, P).

grandparent(Q, R) :-
    parent(Q, S);
    parent(S, R).

% ===============================================
% NEW: Ancestor Rule
% Reaches back 4 generations: parent, grandparent,
% great-grandparent, and great-great-grandparent
% ===============================================

% 1 generation back -> parent
ancestor(X, Y) :-
    parent(X, Y).

% 2 generations back -> grandparent
ancestor(X, Y) :-
    parent(X, Z1),
    parent(Z1, Y).

% 3 generations back -> great-grandparent
ancestor(X, Y) :-
    parent(X, Z1),
    parent(Z1, Z2),
    parent(Z2, Y).

% 4 generations back -> great-great-grandparent
ancestor(X, Y) :-
    parent(X, Z1),
    parent(Z1, Z2),
    parent(Z2, Z3),
    parent(Z3, Y).
```

**The family as a tree**

```
      ramesh ─── amrita                    saumya ─── suresh
              │                                   │
              └──────── daanish ─── sangeeta ─────┘
                            │
                  ┌─────────┴─────────┐
                raaju               ramya

      rahul ─── nandini            (separate branch)
              │
      ┌───────┴───────┐
    aditya          daisy
```

**Sample queries**

```prolog
?- father(X, raaju).
?- mother(X, aditya).
?- grandfather(X, raaju).
?- grandmother(X, raaju).
?- parent(X, ramya).
?- ancestor(ramesh, raaju).
```

**Output**

```prolog
?- father(X, raaju).
X = daanish.

?- mother(X, aditya).
X = nandini.

?- grandfather(X, raaju).
X = ramesh .

?- grandmother(X, raaju).
X = amrita.

?- parent(X, ramya).
X = daanish ;
X = sangeeta.

?- ancestor(ramesh, raaju).
true
```

**How it works.** The database holds only the raw facts (`male`, `female`, `father`, `mother`, plus four grandparent facts). Every other relationship is *derived* by a rule. `sister/2` and `brother/2` define siblings as two people sharing **both** the same father and the same mother, which is why the program needs both `father(A, ...)` and `mother(B, ...)` to succeed before it answers. `ancestor/2` is written as four separate clauses, one per generation depth, so a proof succeeds only if the chain of `parent/2` goals reaches the required number of steps.

---

### Practical 2 — Concatenation of Two Lists

**Question.** Write a PROLOG program to implement `conc(L1, L2, L3)` where `L2` is the list to be appended with `L1` to get the resulting list `L3`.

**Program** — `concatenation_of_lists.pl`

```prolog
% conc(L1, L2, L3) : L3 is the list L1 followed by the list L2

% base case : appending anything to an empty list gives that list
conc([],L2,L2).

% recursive case : keep the head of L1, append the rest
conc([X|L],L2,[X|L3]) :-
    conc(L,L2,L3).
```

**Sample queries**

```prolog
?- conc([a, b, c], [d, e], L).
?- conc(L1, L2, [1, 2]).
```

**Output**

```prolog
?- conc([a, b, c], [d, e], L).
L = [a, b, c, d, e].

?- conc(L1, L2, [1, 2]).
L1 = [],
L2 = [1, 2]
```

**How it works.** The base clause handles the empty first list. The recursive clause peels one element `X` off the front of `L1`, concatenates the remaining tail `L`, and rebuilds the result as `[X|L3]`. Because the third argument is also a pattern, the *same* predicate can split a list as well as join one — the second query asks "which two lists concatenate to `[1, 2]`?" and PROLOG backtracks through every split.

---

### Practical 3 — Reversal of a List

**Question.** Write a PROLOG program to implement `reverse(L, R)` where list `L` is the original and list `R` is the reversed list.

**Program** — `reverse_list.pl`

```prolog
% reverse(L, R) : R is the reverse of the list L

% base case : the reverse of an empty list is empty
reverse([],[]).

% recursive case : reverse the tail, then put the head at the end
reverse([X|L],R) :-
    reverse(L,RL),
    append(RL,[X],R).
```

**Sample queries**

```prolog
?- reverse([a, b, c], R).
?- reverse([], R).
?- reverse([1, 2, 3, 4], R).
```

**Output**

```prolog
?- reverse([a, b, c], R).
R = [c, b, a].

?- reverse([], R).
R = [].

?- reverse([1, 2, 3, 4], R).
R = [4, 3, 2, 1].
```

**How it works.** Reverse the tail first, then append the old head as a one-element list at the end. For `[a, b, c]`: `reverse([b, c])` gives `[c, b]`, then `append([c, b], [a])` gives `[c, b, a]`. The cost is quadratic, because `append/3` walks the whole list at every level — the standard accumulator version does the same work in linear time.

---

### Practical 4 — Sum of Two Numbers

**Question.** Write a PROLOG program to calculate the sum of two numbers.

**Program** — `sum_of_2_numbers.pl`

```prolog
% sum(X, Y, S) : S is the sum of the numbers X and Y
sum(X, Y, S) :-
    S is X + Y.
```

**Sample queries**

```prolog
?- sum(7, 5, S).
?- sum(-3, 10, S).
```

**Output**

```prolog
?- sum(7, 5, S).
S = 12.

?- sum(-3, 10, S).
S = 7.

?- sum(2.5, 4.5, S).
S = 7.0.
```

**How it works.** `is/2` is the arithmetic evaluation operator: it computes `X + Y` and *unifies* the result with `S`. Writing `S = X + Y` instead would only build the term `7+5` without ever computing `12` — the single most common PROLOG mistake in this practical.

---

### Practical 5 — Maximum of Two Numbers

**Question.** Write a PROLOG program to implement `max(X, Y, M)` so that `M` is the maximum of two numbers `X` and `Y`.

**Program** — `max_min.pl`

```prolog
% max(X, Y, M) : M is the greater of the two numbers
max(X,Y,X) :-
    X > Y.

max(X,Y,Y) :-
    Y > X.

% min(A, B, M) : M is the smaller of the two numbers
min(A,B,A) :-
    A < B.

min(A,B,B) :-
    B < A.
```

**Sample queries**

```prolog
?- max(12, 27, M).
?- max(45, 19, M).
?- min(12, 27, M).
```

**Output**

```prolog
?- max(12, 27, M).
M = 27.

?- min(12, 27, M).
M = 12 .

?- max(45, 19, M).
M = 45
```

**How it works.** There is no computation here — the answer is expressed by **unification in the head**. In the first clause the head is `max(X, Y, X)`, so if `X > Y` succeeds, the third argument is bound to `X`; the second clause binds it to `Y` when `Y > X`. The two guards are mutually exclusive, so exactly one clause can ever succeed for distinct numbers.

---

### Practical 6 — Factorial of a Number

**Question.** Write a program in PROLOG to implement `factorial(N, F)` where `F` represents the factorial of a number `N`.

**Program** — `factorial.pl`

```prolog
% fact(N, F) : F is the factorial of the number N

% base case : 0! = 1
fact(0, 1).

% recursive case : N! = N * (N-1)!
fact(N, F) :-
    N > 0,
    N1 is N - 1,
    fact(N1, F1),
    F is N * F1.
```

**Sample queries**

```prolog
?- fact(5, F).
?- fact(0, F).
?- fact(7, F).
```

**Output**

```prolog
?- fact(5, F).
F = 120 .

?- fact(7, F).
F = 5040

?- fact(0, F).
F = 1
```

**How it works.** The guard `N > 0` prevents the recursive clause from ever meeting the base case with a negative number, so recursion terminates exactly at `fact(0, 1)`. `N1 is N - 1` *computes* the decremented value before recursing — passing `N - 1` directly would hand the next goal an unevaluated term.

---

### Practical 7 — Power of a Number

**Question.** Write a PROLOG program to implement `power(Num, Pow, Ans)` where `Num` is raised to the power `Pow` to get `Ans`.

**Program** — `power_of_number.pl`

```prolog
% power(Num, Pow, Ans) : Ans is Num raised to the power Pow

% base case : any number raised to 0 is 1
power(_, 0, 1).

% recursive case : Num^Pow = Num * Num^(Pow-1)
power(Num, Pow, Ans) :-
    Pow > 0,
    P is Pow - 1,
    power(Num, P, A),
    Ans is Num * A.
```

**Sample queries**

```prolog
?- power(2, 10, A).
?- power(5, 0, A).
?- power(3, 4, A).
```

**Output**

```prolog
?- power(2, 10, A).
A = 1024.

?- power(5, 0, A).
A = 1.

?- power(3, 4, A).
A = 81.
```

**How it works.** Identical in structure to factorial: the base clause uses `_` (anonymous variable) because the base value of the number is irrelevant when the exponent is 0. The exponent drives the recursion, and the results are multiplied on the way back out.

---

### Practical 8 — Multiplication of Two Numbers

**Question.** Write a PROLOG program to implement `multi(N1, N2, R)` where `N1` and `N2` denote the numbers to be multiplied and `R` represents the result.

**Program** — `multiply_2_numbers.pl`

```prolog
% multi(N1, N2, R) : R is the product of N1 and N2
multi(N1,N2,R) :-
    R is N1*N2.
```

**Sample queries**

```prolog
?- multi(6, 7, R).
?- multi(12, 0, R).
```

**Output**

```prolog
?- multi(6, 7, R).
R = 42.

?- multi(9, 3, R).
R = 27.

?- multi(12, 0, R).
R = 0.
```

**How it works.** A single arithmetic goal — `*` is evaluated by `is/2` exactly like `+`. Note that PROLOG has no operator for implicit multiplication, so the `*` between `N1` and `N2` is required.

---

### Practical 9 — Membership of an Element in a List

**Question.** Write a PROLOG program to implement `memb(X, L)` to check whether `X` is a member of `L` or not.

**Program** — `searching_in_lists.pl`

```prolog
% mem(X, L) : true if X is a member of the list L

% base case : X is the head of the list
mem(X, [X|L]).

% recursive case : search for X in the tail of the list
mem(X, [Y|L]) :-
    mem(X,L).
```

**Sample queries**

```prolog
?- mem(c, [a, b, c, d]).
?- mem(x, [a, b, c]).
?- mem(X, [a, b]).
```

**Output**

```prolog
?- mem(c, [a, b, c, d]).
true .

?- mem(x, [a, b, c]).
false.

?- mem(X, [a, b]).
X = a ;
X = b ;
false.
```

**How it works.** The base clause matches when the sought element *is* the head — unification does the comparison. The recursive clause discards the head and retries on the tail. The third query shows the predicate is **relational**: with the first argument free it enumerates every member, and the trailing `false` reports that backtracking has exhausted the list.

---

### Practical 10 — Sum of the Elements of a List

**Question.** Write a PROLOG program to implement `sumlist(L, S)` so that `S` is the sum of a given list `L`.

**Program** — `sum_of_elements_in_list.pl`

```prolog
% sum_list(L, S) : S is the sum of all the elements of the list L

% base case : the sum of an empty list is 0
sum_list([], 0).

% recursive case : add the head to the sum of the tail
sum_list([X|L], S) :-
    sum_list(L, S1),
    S is X + S1.
```

**Sample queries**

```prolog
?- sum_list([10, 20, 30, 40], S).
?- sum_list([], S).
```

**Output**

```prolog
?- sum_list([10, 20, 30, 40], S).
S = 100.

?- sum_list([], S).
S = 0.
```

**How it works.** This is the canonical "fold" over a list: the empty list sums to `0`, and a non-empty list sums to its head plus the sum of its tail. The addition happens *after* the recursive call returns, so the additions run from right to left.

---

### Practical 11 — Even and Odd Length of a List

**Question.** Write a PROLOG program to implement two predicates `evenlength(List)` and `oddlength(List)` so that they are true if their argument is a list of even or odd length respectively.

**Program** — `evenlength_oddlength.pl`

```prolog
% evenlength(L) : true if L has an even number of elements

% base case : an empty list has 0 elements
evenlength([]).

% remove two elements at a time and check the rest
evenlength([_,_|T]) :-
    evenlength(T).

% oddlength(L) : true if L has an odd number of elements

% base case : a list with one element
oddlength([_]).

% remove two elements at a time and check the rest
oddlength([_,_|T]) :-
    oddlength(T).
```

**Sample queries**

```prolog
?- evenlength([a, b, c, d]).
?- oddlength([a, b, c]).
```

**Output**

```prolog
?- evenlength([a, b, c, d]).
true.

?- oddlength([a, b, c]).
true .

?- oddlength([a, b, c, d]).
false.
```

**How it works.** No counting and no arithmetic are needed. The pattern `[_,_|T]` matches a list of *at least two* elements and strips both at once, so parity is decided purely by which base case the stripping eventually lands on: `[]` for even, `[_]` for odd. This is the classic illustration that in PROLOG the shape of the term can encode the control flow.

---

### Practical 12 — Maximum Element of a List

**Question.** Write a PROLOG program to implement `maxlist(L, M)` so that `M` is the maximum number in the list.

**Program** — `max_in_list.pl`

```prolog
% maxlist(L, M) : M is the largest element of the list L

% base case : a list with one element, which is the maximum
maxlist([X], X).

% recursive case : compare the head with the maximum of the tail
maxlist([X|T], M) :-
    maxlist(T, M1),
    max(X, M1, M).

% max(X, Y, M) : M is the greater of X and Y
max(X, Y, X) :-
    X >= Y.

max(X, Y, Y) :-
    Y > X.
```

**Sample queries**

```prolog
?- maxlist([12, 45, 7, 89, 23], M).
?- maxlist([5], M).
```

**Output**

```prolog
?- maxlist([12, 45, 7, 89, 23], M).
M = 89.

?- maxlist([5], M).
M = 5.
```

**How it works.** The base case is a **singleton** list, not the empty list — an empty list has no maximum, so `maxlist([], M)` is deliberately left to fail. The recursive clause finds the maximum of the tail and then keeps whichever of the head and that value is larger, using the helper `max/3`. Note the guards `X >= Y` and `Y > X`: they must partition the cases exactly, or a tie could produce two "answers".

---

### Practical 13 — Insertion of an Item at the Nth Position

**Question.** Write a PROLOG program to implement `insert(I, N, L, R)` that inserts an item `I` into the `N`th position of list `L` to generate a list `R`.

**Program** — `permutation_generator.pl`

```prolog
% insert(I, N, L, R) : R is the list L with I inserted at position N

% base case : insert at the first position
insert(I, 1, L, [I|L]).

% recursive case : move down to position N, then insert
insert(I, N, [X|L], [X|R]) :-
    N > 1,
    N1 is N - 1,
    insert(I, N1, L, R).

% permutation(L, P) : P is a permutation of the list L

% base case : the only permutation of an empty list is empty
permutation([], []).

% recursive case : permute the tail, then insert the head anywhere
permutation([X|L], P) :-
    permutation(L, R),
    length(R, N),
    N1 is N + 1,
    between(1, N1, Pos),
    insert(X, Pos, R, P).
```

**Sample queries**

```prolog
?- insert(x, 3, [a, b, c, d], R).
?- insert(x, 1, [a, b], R).
```

**Output**

```prolog
?- insert(x, 3, [a, b, c, d], R).
R = [a, b, x, c, d] .

?- insert(x, 1, [a, b], R).
R = [x, a, b] .

?- insert(x, 5, [a, b, c, d], R).
R = [a, b, c, d, x] .
```

**How it works.** `insert/4` walks the list and decrements `N` at every step. When `N` reaches `1`, the base clause puts `I` in front of whatever remains and the recursion unwinds, rebuilding the prefix it stripped. Position 1 means "before the first element", so inserting at position `N+1` appends — which is why `insert(x, 5, [a,b,c,d], R)` gives `[a, b, c, d, x]`. The second predicate, `permutation/2`, reuses `insert/4`: it permutes the tail, then inserts the head into every position `1 … N+1` using `between/3` — a generate-and-test search over all placements.

---

### Practical 14 — Deletion of the Element at the Nth Position

**Question.** Write a PROLOG program to implement `delete(N, L, R)` that removes the element at the `N`th position from a list `L` to generate a list `R`.

**Program** — `deleting_from_list.pl`

```prolog
% delete(N, L, R) : R is the list L with the element at position N removed

% base case : delete the first element
delete(1,[_|T],T).

% recursive case : move down to position N, then delete
delete(N,[X|T],[X|R]) :-
    N > 1,
    N1 is N - 1,
    delete(N1,T,R).
```

**Sample queries**

```prolog
?- delete(3, [a, b, c, d, e], R).
?- delete(1, [a, b, c], R).
```

**Output**

```prolog
?- delete(3, [a, b, c, d, e], R).
R = [a, b, d, e].

?- delete(1, [a, b, c], R).
R = [a, b, c].
```

**How it works.** The mirror image of Practical 13. The base clause matches at position `1` and *drops* the head by binding the third argument to the tail `T` alone; the recursive clause copies each element it passes onto the output list. The `_` in the base clause marks the element being deleted, which is never named because its value does not matter.

---

### Practical 15 — Greatest Common Divisor of Two Numbers

**Question.** Write a PROLOG program to compute the greatest common divisor of two numbers.

**Program** — `gcd.pl`

```prolog
% gcd(X, Y, G) : G is the greatest common divisor of X and Y

% base case : the gcd of a number and 0 is that number
gcd(X,0,X).

% recursive case : Euclid's algorithm, gcd(X,Y) = gcd(Y, X mod Y)
gcd(X,Y,G) :-
    Y =\= 0,
    R is X mod Y,
    gcd(Y,R,G).
```

**Sample queries**

```prolog
?- gcd(48, 18, G).
?- gcd(17, 5, G).
```

**Output**

```prolog
?- gcd(48, 18, G).
G = 6 .

?- gcd(17, 5, G).
G = 1 .

?- gcd(100, 0, G).
G = 100
```

**How it works.** Euclid's algorithm, stated directly as a rule: `gcd(X, Y) = gcd(Y, X mod Y)`, with `gcd(X, 0) = X` as the base case. `=\= 0` is arithmetic inequality (a numeric test), not unification. The chain 48 → gcd(18, 12) → gcd(12, 6) → gcd(6, 0) terminates at the base clause with `G = 6`. `gcd(100, 0, G)` shows the base case on its own.

---

### Practical 16 — Length of a List

**Question.** Write a PROLOG program to implement `len(L, N)` where `N` is the number of elements in the list `L`.

**Program** — `length_of_list.pl`

```prolog
% len(L, N) : N is the number of elements in the list L

% base case : an empty list has length 0
len([],0).

% recursive case : length of the tail plus one
len([X|L],N) :-
    len(L,N1),
    N is N1 + 1.
```

**Sample queries**

```prolog
?- len([a, b, c, d], N).
?- len([], N).
```

**Output**

```prolog
?- len([a, b, c, d], N).
N = 4.

?- len([], N).
N = 0.
```

**How it works.** Each recursive step consumes one element and adds `1` to the length of the tail on the way back up. The head `X` appears in the pattern only to enforce that the list is non-empty; its value is never used, so it could equally be written `[_|L]`.

---

### Practical 17 — Nth Term of the Fibonacci Series

**Question.** Write a program in PROLOG to implement `generate_fib(N, T)` where `T` represents the `N`th term of the Fibonacci series.

**Program** — `fibonacci.pl`

```prolog
% fibo(N, F) : F is the Nth term of the Fibonacci series

% base case : the 0th term of the series is 0
fibo(0, 0).

% base case : the 1st term of the series is 1
fibo(1, 1).

% recursive case : every term is the sum of the previous two
fibo(N, F) :-
    N > 1,
    N1 is N - 1,
    N2 is N - 2,
    fibo(N1, F1),
    fibo(N2, F2),
    F is F1 + F2.
```

**Sample queries**

```prolog
?- fibo(7, F).
?- fibo(0, F).
?- fibo(10, F).
```

**Output**

```prolog
?- fibo(7, F).
F = 13 .

?- fibo(0, F).
F = 0 .

?- fibo(10, F).
F = 55 .
```

**How it works.** Two base clauses anchor the series at `0` and `1`; the recursive clause asks for the two preceding terms and adds them. Unlike the linear recursion of Practicals 6, 7 and 16, this is **tree recursion** — each call spawns two more, so the number of calls grows exponentially (`fibo(10)` needs over a hundred proof steps). The guard `N > 1` is what stops the recursion from running below `0`.

---

## Concepts at a Glance

| Recursion pattern | Practicals | Signature idea |
|-------------------|:----------:|----------------|
| Knowledge base of facts + rules | 1 | Relations derived by rules, not stored |
| Structural recursion on a list | 2, 3, 9, 10, 11, 12, 16 | Split `[X|T]`, solve for `T`, combine |
| Arithmetic recursion | 6, 7, 15, 17 | A numeric argument drives termination |
| Direct evaluation with `is/2` | 4, 8 | `is` computes; `=` only unifies |
| Guarded clauses / unification in the head | 5, 12 | Answer chosen by which guard succeeds |
| Positional recursion with a counter | 13, 14 | Decrement `N` until it hits `1` |
| Generate and test | 13 | `between/3` proposes, `insert/4` verifies |

**Terms worth revising before the viva**

| Term | Meaning |
|------|---------|
| Fact | A clause with a head and no body, e.g. `male(ramesh).` |
| Rule | A clause with a body, e.g. `parent(X,Y) :- father(X,Y).` |
| Predicate | A named relation with a fixed arity, e.g. `father/2` |
| Query | A goal posed to the interpreter, written `?- goal.` |
| Unification | Matching two terms by binding variables; `=/2` |
| Backtracking | Re-entering a clause for another solution, triggered by `;` |
| Anonymous variable | `_`, matches anything and is never bound |
| `is/2` | Arithmetic evaluation and assignment |
| Cut `!` | Commits to the choices made so far (not used in these programs) |

---

## Notes and Known Issues

These are honest observations about the submitted code, kept here so the programs stay faithful to the practical file while still being useful to anyone reading the repository.

1. **`grandparent/2` uses disjunction where conjunction is meant.** As written, `grandparent(Q, R) :- parent(Q, S); parent(S, R).` succeeds whenever `Q` is a parent of anything, or `R` is a child of anything. The corrected rule is:

   ```prolog
   grandparent(Q, R) :-
       parent(Q, S),
       parent(S, R).
   ```

2. **`ancestor/2` is unrolled by hand.** The four clauses cover exactly one to four generations. A recursive definition gives the same answers for any depth and is shorter:

   ```prolog
   ancestor(X, Y) :- parent(X, Y).
   ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y).
   ```

3. **`grandmother/2` only knows paternal grandmothers.** It derives the relationship through `fatherkimother/2`, so `?- grandmother(X, raaju).` gives `amrita`, but `?- grandmother(X, daisy).` fails — no `fatherkimother` fact exists for `rahul`. A general rule needs both the father's mother and the mother's mother.

4. **Practical 13's file is named `permutation_generator.pl`.** The practical itself is insertion at the Nth position; `permutation/2` is an extra predicate that reuses `insert/4`. Both live in the same file, which is why the name differs from the practical title.

5. **Practical 7's program is unnamed in the practical file.** The repository uses `power_of_number.pl` for consistency with the other file names.

6. **Some names overlap with SWI-Prolog's own list library** (`reverse/2` and `length/2` both exist there). The definitions here are written from scratch for practice and are consulted into the `user` module. If SWI-Prolog prints a redefinition warning, it is harmless and does not change any result.

7. **Outputs were captured in SWI-Prolog.** The prompt counter (`?-` with a history number) and the trailing `true` / `false` reports are SWI-Prolog's. Some listings above include an extra query beyond the ones in the practical file, to show a boundary case.

---

## References

- *Artificial Intelligence: A Modern Approach* — Russell & Norvig (search, knowledge representation, inference).
- *PROLOG Programming for Artificial Intelligence* — Ivan Bratko (recursion, list processing, relations).
- SWI-Prolog documentation — [swi-prolog.org/pldoc](https://www.swi-prolog.org/pldoc/).

<div align="center">

---

**Nishant Das** · B.Sc. (Hons.) Computer Science · Ramanujan College, University of Delhi
Artificial Intelligence (DSC-09) · PROLOG Practical File · Session 2026–27

</div>
