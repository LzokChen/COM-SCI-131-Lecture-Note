#### Grammar notation
* BNF
* EBNF
  * RFC EBNF
  * ISO EBNF
    | Notation | Meaning          |
    | -------- | ---------------- |
    | A,B      | concatenation    |
    | {A}      | zero of more A's |
    | A-B      | A except B       |
    * identifier = letter list - reserved word;
    * identifier = 'a' | 'b' | 'c' | .... | 'z' | '-' | ... 
                   | 'a','a' | ... | 'i','e'| 'i', 'g' |...
      * Note that we skip the "'i', 'f' ", because "if" is reserved keyword. 

  * Syntax charts, diagrams, racetracks
  
  * Ex: Scheme grammar
    * <cand> ->   (cand <cand clause>+)
                | (cand <cand clause>* (else<sequence>))
    * redo as a diagrams
       <img src="./pic/week2-01.jpg" width = "450" height = "280" align=center /> 
      * reduce duplication
-------------------

#### Grammar notation trouble
* Grammars are programs-ish
* Ex:
    ```
    S->a 
    S->T b (problem 1)
    S->S c 
    G-> F b c (problem 2)
    F-> e f g (problem 3)
    ```
  1. use(on RHS) a nonterminal without defining it (on LHS), then there is a bug.
      * Means the containing rule is useless -> you can remove it.
  2. Define a nonterminal without using it.
  3. Nonterminal cannot be reached from start symbol.
  4. Duplicate rules (line 1 2 3)
  5. Extra: your grammar doesn't contain grammatical constraints that (grammar is too generous)
     ```
     S -> NP VP 
     NP -> N
     NP -> Adj NP
     VP -> V
     VP -> VP Adv
     N -> 'dog'
     N -> 'cats'
     V -> 'prowl'
     V -> 'jump'
     Adj -> 'blue'
     Adv -> 'quickly'
     ``` 
     *      blue cats prowl --> correct
     *      blue dog prowl --> wrong
     * How to fix it: complicate grammar to captions (plural-vs-singular matching)
        ```
        S -> SNP SVP
        S -> PNP PVP
        SNP -> SN
        SNP -> Adj SNP
        PNP  ->  PN
        PNP -> Adj PNP
        ...
        N -> 'dog'
        N -> 'cats'
        ...
        ``` 
  6. Ambiguity (not always obvious)
     * In programming language the Ambiguity usually come from **precedence优先级, associativity结合性, and dangling problem**
     * Ex: 
       * <img src="./pic/week2-02.jpg" width = "450" height = "250" align=center /> 
       * E -> expression, T -> term, F-> factor  
       * How to fix: 
         1. **operator precedence** (by complicating grammar).
            <img src="./pic/week2-03.jpg" width = "450" height = "250" align=center /> 
         2. **Associativity**
             <img src="./pic/week2-04.jpg" width = "450" height = "250" align=center />

    
     * More Ex: **C grammar**
        ``` C
        stmt : 
                ;
                expr ;
                goto ID ;
                return expr ;
                break ;
                continue ;
                if (expr) stmt
                if (expr) stmt else stmt
                while (expr) stmt
                do stmt while (expr);
                {stmt list}
        ``` 
       * why we add () in if and while but not others?
         * because without () in if and while, C will be ambiguous.
         * Ex of if: 
            ```C
            if expr s dtmt

            Problem: if a - b - c;

            2 Partitions: if (a) -b-c; | if (a-b) -c; 
            ``` 
         * for do-while, the () is not necessary for avoiding ambiguous, because the ; can tell us where to end.
  
       * dangling problem:
         ```C
         if (a==b) if (b==1) c=2; else d = 4;

         two partitions:
         if (a==b)
         |   if (b==1)
         |   |  c = 2;
         else d = 4;
         
         if (a==b)      //the partition that
         |  if (b==1)   //complier will approach usually
         |  |   c = 2;
         |  else d = 4;
         ``` 
         * Fix: remove rule: ```if (expr) stmt```

     * Resolving ambiguity by complicating the grammar also complicate the parse tree.

     * Prolog let you define your own operator.
      ```Prolog
      :-op (500,yfx,[+,-])
      :-op (400,yfx, [*,/])
      :-op (200,fy, [+,-])
      :-op (700,xfx, [=,==,<,>])
      ...
      ```  
       * yfx = binary, left associativity
         *  y means operand can have equal precedence
         *  f means function
         *  x means operand must have better precedence 


------------------------------
#### ML intro 
* **example situation**
  * big backend queries executed in a distributed environment.
  * programming in C++ didn't scale.
  * **Map reduce**
    * the programing style came from **functional programming**
  

###3 categories of languages

| type                | Program contains    | How to hook together | what you give up |
| :-------------------: | :-------------------: | :--------------------: | :----------------: |
| imperative          | commands, statement | C1;c2;c3;            |
| Functional Language | Expressions         | f(g(x),h(y))         | Side effects     |
| Logic Language      | Predicates          | p1 & p2, p1          | p2               | Functions |

----------------

#### Functional programming basics Motivation
1. **Clarity** (J Backus (Fortran))
2. **Parallelizability** escaping from the von neumann bottleneck(serial execution)
3. Function
   1. relation -set of ordered pair(a, b): ("domain", "range")
      1. function is a relation where no two pairs have the same 1st element.
   2. mapping from domain to range
   * Partial function doesn't care about the domain.
   * ```'+' (f(y),g(z))``` ordering is controlled by ```calls+args``` first, then ```fn```.
   * Function form
       <img src="./pic/week2-06.JPg" width = "350" height = "200" align=center />
   * **No side effects**
     * No assignment
     * No copying
     * No I/O
   * Referential transparency 引用透明性
        <img src="./pic/week2-07.JpG" width = "350" height = "200" align=center />


#### ML & Ocaml
* **Functional Programming**
* **Type inference** 
  * Compile time type checking (a.k.a Static type checking)
  * (ML\Ocaml, C++, Java)
    * for reliability + performance(more faster)
  * you need not write down names of type, all the time: compile unit or complier will enter.
* **Good support for memory management**
   ```
      C:       malloc      free
      C++      new         del
      Ocaml    (have)     （no need）
    ```
* **Good support for higher order functions**
* **Example**
   ```Ocaml
   # if 1 < 2 then "a" else "b";; (*it will return "a" and will not check "b"*)
   -: string = "a"

   # if 1< 2 then "a" else 2.1;; (*it will cause syntax error,because Ocaml do the static type checking, and it want "a" and 2.1 be the same type*)

   -------------
   (* tuple, 
   * Heterogeneous(element can be different type)
   * fix length(once a tuple is declared, the size is fixed,we cannot add or delete element)*)
   # 1, "a";; 
   -: int * string = (1,"a") 
   ------------
   (* list
   * Homogeneous (element must be same type)
   * length unspecified (the size can be changed, we can add or delete elements)*)
   # [1;3;-5]
   -: int list = [1;3;-5]
   
   # ["a", 3];; (*syntax error elements are not in the same type*)
  
   # [];;
   -: 'a list = [] (*undeclared type, this list and @ with any kind of list*)

   -----------
   (**function**)
   # let f x = x+1;;
   val f: int->int = <fun> (*the 1st "int" is domain, 2nd "int" is range*)

   # let cons h t = h::t
   cons:'a -> 'a list-> 'a list = <fun>
   (*         (                )     *)
   (*currying, this is a two args function, but it can be treated as two higher level single element function*)
   (*every function actually is a single arg function*)

   # let uccons (h,t) = h::t;;
   uccons: 'a*'a list -> 'a list = <fun>
   (*      (  (      ))  (      )    *)

  ----------
   #let triplist = cons 3;; (*equivalent to: let triplist mylist = cons 3 mylist *)
   triplist int list -> int list = <fun>

   #triplist [19];;
   -: int list [3; 19]

   # cons 3 [4;-1]
   -: int list [3;4;-1]

   # uccons (3, [4;-1]);;
   -: int list [3;4;-1]
   (*  ex: m+n*3 is equivalent to (+)m((* n 3)) *)
   ```

* IN Ocaml
  1. function can be anonymous. 
   ```ocaml
    fun x -> x+1  (*x is is a arg not fun. name*)
   ```
   1. Function defines often use shorthands. 
   ```ocaml
   let f x = x+1
   let f = (fun x->x+1) (* val f : int -> int = fun *)
   let cons xy = x::y
   let con = (fun x ->(fun y ->(x::y)))
   ``` 

* Patterns in OCaml
  ``` Ocaml
  match E with
  | P1 -> E1
  | P2 -> E2
  ...
  | Pn -> En
  ``` 

  * ```_``` (underscore):  matchs with anything
  * ```a```(identifer) match  with a and binds a to that value
  * ```[]``` empty list of any type
  * ```[P1;P2;p3]```  3-element lists
  * ```P1,P2,P3``` 3-triple
  * ```P1::P2``` a list of length > 1

 ```Ocaml
 function
  | P1 -> E1
  | P2 -> E2
  ...
  | Pn -> En

  (*equivalent to*)
fun x -> match x with 
  | P1 -> E1
  | P2 -> E2
  ...
  | Pn -> En
--------
let uccons p = match p with 
 | (h,t) -> h::t

let uccons = function (h,t) -> h::t
---------
 ```
 * **function vs. fun**
 ```ocaml
 (*fun is for currying*)
 fun x y z  -> x + y * z;;
 fun x -> fun y -> fun z -> x + y * z;;
 int -> int -> int -> int 

 (*function is for pattern matching*)
 function 
 |1 -> "one"
 |0 -> "zero"
 
 int -> string
-------
fun x-> x+1;;
function |x-> x+1;;
--------
let car = function | h::_ -> h
let car l = match l with | h::_ ->h;;
(* Notice: let car [] will not work!!! *)
 ``` 
   #### before next time, Webber 4,6,8