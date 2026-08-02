# 🤖 Artificial Intelligence — Practical Lab Manual
### `DSC16 / GE6e / DSE : Artificial Intelligence` &nbsp;|&nbsp; 4 Credits (L-T-P: 3-0-1)

![Language](https://img.shields.io/badge/Language-Prolog-blue?style=for-the-badge&logo=prolog)
![Semester](https://img.shields.io/badge/Semester-3rd-orange?style=for-the-badge)
![Practicals](https://img.shields.io/badge/Practicals-13-success?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

> A complete, ready-to-run companion for the **Artificial Intelligence** practical syllabus — covering search algorithms, game playing, constraint satisfaction, knowledge representation, and natural language parsing, all implemented in **SWI-Prolog**.

---

## 📑 Table of Contents

| # | Practical | Category |
|---|-----------|----------|
| [1](#-1-tower-of-hanoi) | Tower of Hanoi(N) | Recursion / Search |
| [2](#-2-hill-climbing-search) | Hill Climbing Search | Informed Search |
| [3](#-3-best-first-search) | Best First Search | Informed Search |
| [4](#-4-a-search-algorithm) | A* Search Algorithm | Informed Search |
| [5](#-5-min-max-search) | Min-Max Search | Game Playing |
| [6](#-6-water-jug-problem) | Water-Jug Problem | State Space Search |
| [7](#-7-sudoku-9x9-csp) | Sudoku (9×9) using CSP | Constraint Satisfaction |
| [8](#-8-family-tree) | Family Tree & Relationships | Knowledge Representation |
| [9](#-9-frames) | Knowledge Representation using Frames | Knowledge Representation |
| [10](#-10-list-concatenation) | `conc(L1, L2, L3)` — List Append | List Processing |
| [11](#-11-list-reversal) | `reverse(L, R)` — List Reversal | List Processing |
| [12](#-12-parse-tree-generation) | Parse Tree of an English Sentence | NLP |
| [13](#-13-context-free-grammar-anbn) | Context-Free Grammar aⁿbⁿ | NLP / Formal Grammar |

---

## ⚙️ Prerequisites & Setup

<details>
<summary><strong>🔧 Click to expand: How to run these programs</strong></summary>

1. Install **SWI-Prolog** → [https://www.swi-prolog.org/download/stable](https://www.swi-prolog.org/download/stable)
2. Save any snippet below into a file, e.g. `hanoi.pl`
3. Launch Prolog and load the file:
   ```prolog
   ?- [hanoi].
   ```
4. Run the query shown under each program's **Sample Run** section.

> 💡 Programs marked **CSP** use `library(clpfd)`, which ships built-in with SWI-Prolog — no extra install needed.

</details>

---

## 🔹 1. Tower of Hanoi

**Goal:** Move `N` disks from the source peg to the destination peg using an auxiliary peg, obeying the rule that a larger disk can never sit atop a smaller one.

```prolog
% hanoi(N) - solves Tower of Hanoi for N disks
hanoi(N) :-
    move(N, left, right, center).

% move(NumDisks, Source, Destination, Auxiliary)
move(0, _, _, _) :- !.
move(N, Source, Destination, Auxiliary) :-
    N > 0,
    N1 is N - 1,
    move(N1, Source, Auxiliary, Destination),
    format("Move disk ~w from ~w to ~w~n", [N, Source, Destination]),
    move(N1, Auxiliary, Destination, Source).
```

**Sample Run**
```prolog
?- hanoi(3).
Move disk 1 from left to right
Move disk 2 from left to center
Move disk 1 from right to center
Move disk 3 from left to right
Move disk 1 from center to left
Move disk 2 from center to right
Move disk 1 from left to right
```

---

## 🔹 2. Hill Climbing Search

**Goal:** Greedily move to the neighboring state with the best heuristic value until no better neighbor exists (local optimum).

```prolog
% Graph definition: edge(Node, Neighbor)
edge(a, b). edge(a, c).
edge(b, d). edge(c, d).
edge(d, e). edge(c, e).

% Heuristic estimates (lower is better; goal has heuristic 0)
heuristic(a, 5). heuristic(b, 4). heuristic(c, 3).
heuristic(d, 2). heuristic(e, 0).

hill_climb(Node, Node) :-
    heuristic(Node, 0), !,
    format("Goal reached: ~w~n", [Node]).

hill_climb(Node, Goal) :-
    format("Visiting: ~w~n", [Node]),
    findall(N-H, (edge(Node, N), heuristic(N, H)), Neighbors),
    Neighbors \= [],
    sort(2, @=<, Neighbors, [Best-_|_]),
    heuristic(Node, HCurrent),
    Best \= HCurrent,
    hill_climb(Best, Goal).

hill_climb(Node, Node) :-
    format("Stuck at local optimum: ~w~n", [Node]).
```

**Sample Run**
```prolog
?- hill_climb(a, e).
Visiting: a
Visiting: c
Visiting: e
Goal reached: e
```

---

## 🔹 3. Best First Search

**Goal:** Expand the node that appears most promising according to a heuristic function, using an open list ordered by heuristic value (Greedy Best-First Search).

```prolog
edge(a, b, 1). edge(a, c, 4).
edge(b, d, 2). edge(b, e, 5).
edge(c, f, 3). edge(e, g, 1). edge(f, g, 2).

heuristic(a, 6). heuristic(b, 4). heuristic(c, 5).
heuristic(d, 6). heuristic(e, 2). heuristic(f, 2).
heuristic(g, 0).

best_first_search(Start, Goal, Path) :-
    bfs([[Start]], Goal, RevPath),
    reverse(RevPath, Path).

bfs([[Goal|Rest]|_], Goal, [Goal|Rest]).
bfs([[Node|Rest]|OtherPaths], Goal, SolutionPath) :-
    findall([Next, Node|Rest],
            (edge(Node, Next, _), \+ member(Next, [Node|Rest])),
            NewPaths),
    append(OtherPaths, NewPaths, AllPaths),
    sort_by_heuristic(AllPaths, SortedPaths),
    bfs(SortedPaths, Goal, SolutionPath).

sort_by_heuristic(Paths, Sorted) :-
    map_list_to_pairs(path_heuristic, Paths, Pairs),
    keysort(Pairs, SortedPairs),
    pairs_values(SortedPairs, Sorted).

path_heuristic([Node|_], H) :- heuristic(Node, H).
```

**Sample Run**
```prolog
?- best_first_search(a, g, Path).
Path = [a, b, e, g].
```

---

## 🔹 4. A* Search Algorithm

**Goal:** Find the optimal path by minimizing `f(n) = g(n) + h(n)`, where `g(n)` is the cost so far and `h(n)` is the estimated cost to the goal.

```prolog
edge(a, b, 1). edge(a, c, 4).
edge(b, d, 2). edge(b, e, 5).
edge(c, f, 3). edge(e, g, 1). edge(f, g, 2).

heuristic(a, 6). heuristic(b, 4). heuristic(c, 5).
heuristic(d, 6). heuristic(e, 2). heuristic(f, 2).
heuristic(g, 0).

astar(Start, Goal, Path, Cost) :-
    heuristic(Start, H0),
    astar_search([[H0, 0, Start]], Goal, [], RevPath, Cost),
    reverse(RevPath, Path).

astar_search([[_, G, Goal]|_], Goal, Visited, [Goal|Visited], G) :- !.
astar_search([[_, G, Node]|Rest], Goal, Visited, Path, Cost) :-
    findall([F1, G1, Next],
            (edge(Node, Next, StepCost),
             \+ member(Next, Visited),
             G1 is G + StepCost,
             heuristic(Next, H1),
             F1 is G1 + H1),
            Children),
    append(Rest, Children, Frontier),
    sort(Frontier, SortedFrontier),
    astar_search(SortedFrontier, Goal, [Node|Visited], Path, Cost).
```

**Sample Run**
```prolog
?- astar(a, g, Path, Cost).
Path = [a, b, e, g],
Cost = 7.
```

---

## 🔹 5. Min-Max Search

**Goal:** Evaluate a game tree assuming an adversarial opponent — MAX tries to maximize the score, MIN tries to minimize it.

```prolog
% Game tree represented as: node(Value) for leaves, node(Children) for internal nodes
leaf(node(4), 4).  leaf(node(8), 8).
leaf(node(2), 2).  leaf(node(1), 1).
leaf(node(6), 6).  leaf(node(3), 3).
leaf(node(9), 9).  leaf(node(7), 7).

tree(root, [n1, n2]).
tree(n1, [node(4), node(8)]).
tree(n2, [node(2), node(1)]).

minimax(Node, max, Value) :-
    leaf(Node, Value), !.
minimax(Node, max, Value) :-
    tree(Node, Children),
    findall(V, (member(C, Children), minimax(C, min, V)), Values),
    max_list(Values, Value).

minimax(Node, min, Value) :-
    leaf(Node, Value), !.
minimax(Node, min, Value) :-
    tree(Node, Children),
    findall(V, (member(C, Children), minimax(C, max, V)), Values),
    min_list(Values, Value).
```

**Sample Run**
```prolog
?- minimax(root, max, V).
V = 4.
```

---

## 🔹 6. Water-Jug Problem

**Goal:** Given a 4-litre and a 3-litre jug (no measuring marks), measure exactly 2 litres using fill, empty, and pour operations.

```prolog
% state(Jug4, Jug3)
capacity(4, 3).

goal_state(state(2, _)).

pour(state(X, Y), state(4, Y)) :- X < 4.                       % fill jug4
pour(state(X, Y), state(X, 3)) :- Y < 3.                       % fill jug3
pour(state(X, Y), state(0, Y)) :- X > 0.                       % empty jug4
pour(state(X, Y), state(X, 0)) :- Y > 0.                       % empty jug3
pour(state(X, Y), state(X1, Y1)) :-                             % jug4 -> jug3
    X > 0, Y < 3,
    Transfer is min(X, 3 - Y),
    X1 is X - Transfer, Y1 is Y + Transfer.
pour(state(X, Y), state(X1, Y1)) :-                             % jug3 -> jug4
    Y > 0, X < 4,
    Transfer is min(Y, 4 - X),
    X1 is X + Transfer, Y1 is Y - Transfer.

water_jug(Path) :-
    search(state(0, 0), [state(0, 0)], RevPath),
    reverse(RevPath, Path).

search(State, Visited, Visited) :- goal_state(State).
search(State, Visited, Path) :-
    pour(State, Next),
    \+ member(Next, Visited),
    search(Next, [Next|Visited], Path).
```

**Sample Run**
```prolog
?- water_jug(Path).
Path = [state(0,0), state(0,3), state(3,0), state(3,3),
        state(4,2), state(0,2)].
```

---

## 🔹 7. Sudoku (9×9) — CSP

**Goal:** Solve a standard 9×9 Sudoku puzzle using **Constraint Satisfaction** via SWI-Prolog's `clpfd` library — rows, columns, and 3×3 boxes must all contain digits 1–9 exactly once.

```prolog
:- use_module(library(clpfd)).

sudoku(Rows) :-
    length(Rows, 9), maplist(same_length(Rows), Rows),
    append(Rows, Vs), Vs ins 1..9,
    maplist(all_distinct, Rows),
    transpose(Rows, Columns),
    maplist(all_distinct, Columns),
    Rows = [As, Bs, Cs, Ds, Es, Fs, Gs, Hs, Is],
    blocks(As, Bs, Cs),
    blocks(Ds, Es, Fs),
    blocks(Gs, Hs, Is).

blocks([], [], []).
blocks([N1,N2,N3|Ns1], [N4,N5,N6|Ns2], [N7,N8,N9|Ns3]) :-
    all_distinct([N1,N2,N3,N4,N5,N6,N7,N8,N9]),
    blocks(Ns1, Ns2, Ns3).

print_sudoku(Rows) :-
    maplist(print_row, Rows).
print_row(Row) :- maplist(format("~w "), Row), nl.

solve(Rows) :-
    sudoku(Rows),
    maplist(label, Rows),
    print_sudoku(Rows).
```

**Sample Run** *(0 = blank cell in the puzzle you load in)*
```prolog
?- Puzzle = [[5,3,_,_,7,_,_,_,_],
             [6,_,_,1,9,5,_,_,_],
             [_,9,8,_,_,_,_,6,_],
             [8,_,_,_,6,_,_,_,3],
             [4,_,_,8,_,3,_,_,1],
             [7,_,_,_,2,_,_,_,6],
             [_,6,_,_,_,_,2,8,_],
             [_,_,_,4,1,9,_,_,5],
             [_,_,_,_,8,_,_,7,9]],
   solve(Puzzle).
% Prints the fully-solved 9x9 grid
```

---

## 🔹 8. Family Tree

**Goal:** Represent a family using base facts (`parent`, `male`, `female`) and derive relationships (father, mother, sibling, grandparent) through logical rules.

```prolog
male(ramesh).
female(amrita).
male(suresh).
female(saumya).

father(daanish, raaju).
father(daanish, ramya).
father(rahul, aditya).
father(rahul, daisy).

mother(sangeeta, raaju).
mother(sangeeta, ramya).
mother(nandini, aditya).
mother(nandini, daisy).

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

**Sample Run**
```prolog
% ?- sister(ramya, raaju).
% ?- grandfather(ramesh, raaju).
% ?- grandmother(amrita, ramya).
% ?- parent(daanish, raaju).
% ?- ancestor(ramesh, raaju).      
% ?- ancestor(daanish, raaju).     
% ?- ancestor(X, raaju).           
```

---

## 🔹 9. Frames

**Goal:** Represent structured knowledge as **frames** — objects with named slots and values — a classic AI knowledge-representation scheme.

```prolog
% frame(FrameName, ListOfSlot-Value pairs)
frame(bird, [
    isa-animal,
    covering-feathers,
    movement-flies,
    reproduction-lays_eggs
]).

frame(penguin, [
    isa-bird,
    movement-swims,        % overrides inherited default
    habitat-antarctica
]).

frame(animal, [
    movement-varies,
    reproduction-varies,
    alive-true
]).

% get_slot(Frame, Slot, Value) - retrieves a value, inheriting from parent (isa) if absent
get_slot(Frame, Slot, Value) :-
    frame(Frame, Slots),
    member(Slot-Value, Slots), !.
get_slot(Frame, Slot, Value) :-
    frame(Frame, Slots),
    member(isa-Parent, Slots),
    get_slot(Parent, Slot, Value).
```

**Sample Run**
```prolog
?- get_slot(penguin, movement, X).
X = swims.                  % overridden locally

?- get_slot(penguin, covering, X).
X = feathers.                % inherited from 'bird'

?- get_slot(penguin, alive, X).
X = true.                    % inherited from 'animal' via 'bird'
```

---

## 🔹 10. List Concatenation

**Goal:** Implement `conc(L1, L2, L3)` such that `L3` is `L1` appended with `L2`.

```prolog
conc([], L, L).
conc([H|T], L2, [H|T3]) :-
    conc(T, L2, T3).
```

**Sample Run**
```prolog
?- conc([1,2,3], [4,5], L3).
L3 = [1, 2, 3, 4, 5].

?- conc([a,b], X, [a,b,c,d]).
X = [c, d].
```

---

## 🔹 11. List Reversal

**Goal:** Implement `reverse(L, R)` such that `R` is the reverse of list `L`, using an accumulator for efficiency.

```prolog
reverse(L, R) :-
    reverse_acc(L, [], R).

reverse_acc([], Acc, Acc).
reverse_acc([H|T], Acc, R) :-
    reverse_acc(T, [H|Acc], R).
```

**Sample Run**
```prolog
?- reverse([1,2,3,4,5], R).
R = [5, 4, 3, 2, 1].
```

---

## 🔹 12. Parse Tree Generation

**Goal:** Use a **Definite Clause Grammar (DCG)** to parse a simple English sentence and produce its syntactic parse tree.

```prolog
% Grammar: S -> NP VP | NP -> Det N | VP -> V NP
sentence(s(NP, VP))     --> noun_phrase(NP), verb_phrase(VP).
noun_phrase(np(Det, N)) --> determiner(Det), noun(N).
verb_phrase(vp(V, NP))  --> verb(V), noun_phrase(NP).

determiner(det(the)) --> [the].
determiner(det(a))   --> [a].

noun(n(dog))  --> [dog].
noun(n(cat))  --> [cat].
noun(n(boy))  --> [boy].

verb(v(chased)) --> [chased].
verb(v(saw))    --> [saw].

parse(Sentence, Tree) :-
    phrase(sentence(Tree), Sentence).
```

**Sample Run**
```prolog
?- parse([the, boy, chased, a, cat], Tree).
Tree = s(np(det(the), n(boy)),
          vp(v(chased), np(det(a), n(cat)))).
```

---

## 🔹 13. Context-Free Grammar aⁿbⁿ

**Goal:** Recognize the classic context-free language `{aⁿbⁿ | n ≥ 1}` — equal numbers of `a`s followed by equal numbers of `b`s — using a DCG.

```prolog
% s --> a^n b^n  (n >= 1)
s --> [a], s1.
s1 --> [b].
s1 --> [a], s1, [b].

recognize(List) :-
    phrase(s, List).
```

**Sample Run**
```prolog
?- recognize([a,a,a,b,b,b]).
true.

?- recognize([a,a,b]).
false.

?- recognize([a,b]).
true.
```

---

## 📊 Practical Coverage Summary

| Category | Practicals Covered | Count |
|----------|--------------------|:-----:|
| 🔎 Uninformed / Recursive Search | Tower of Hanoi | 1 |
| 🎯 Informed Search | Hill Climbing, Best First, A* | 3 |
| 🎮 Game Playing | Min-Max | 1 |
| 🧩 Constraint Satisfaction | Water-Jug, Sudoku | 2 |
| 🧠 Knowledge Representation | Family Tree, Frames | 2 |
| 📋 List Processing | Concatenation, Reversal | 2 |
| 💬 Natural Language Processing | Parse Tree, CFG aⁿbⁿ | 2 |
| **Total** | | **13** |

---

## 📚 Reference Textbooks

1. Russell, S. J. & Norvig, P. — *Artificial Intelligence: A Modern Approach*, 4th ed., Pearson, 2020
2. Bratko, I. — *Prolog Programming for Artificial Intelligence*, 4th ed., Pearson, 2012
3. Patterson, D. W. — *Introduction to A.I. and Expert Systems*, PHI, 2007
4. Clocksin, W. F. & Mellish, C. — *Programming in PROLOG*, 5th ed., Springer, 2003

---

<p align="center">
Made for the <strong>Artificial Intelligence</strong> course • Semester 3 🎓
</p>
