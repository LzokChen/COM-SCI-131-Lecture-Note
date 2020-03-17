## Lecture 1
#### Jan 6, 2020

* D.E. Knuth -- The art of computer programming.
  
##### before next lecture
  * Read Modern programming languages : a practical introduction / Adam Brooks Webber 
  * 1-3 syntax, 5,7,9 ML

##### Programming Language Resources
  * OCaml
  * Real world OCaml (advanced)
  * OCaml Ref
  * ML vs OCaml

##### Resources for written reports
  * citation

##### IEEE spectrum
1.  Python
2.  Java
3.  C
4.  C++
5.  R
6.  javascript
7.  C#
8.  Matlab
9.  swift
10. GO


#### Core of Programing language
* principle & limitation of programming models
* Notation for these models
  * Design, use, support, etc.
* Methods to evaluate strengths + weakness 


#### Language design principles
* orthogonality
  * (C: a function can return any type except arrays, because for a array x[10], return x means return x[0])
* Efficiency
  * (CPU time, space, energy, network, ...)
* Simplicity
* Convenience
* Safety
  * if array is out of bound.
* concurrency
* Exceptions handling
* Mutability
* Abstraction


-------

## Lecture 2
####  Jan 8, 2020

##### Syntax (theory)
  * the form and meaning are independent
  * Why we need to learn syntax?
    1. easier
    2. respectability for CS
   
  > "Colorless green ideas sleep furiously" -- it follows the syntax but without meaning
  > "Time flies" -->**ambiguity** --> N + V or V + N

  * **Reasons for choosing syntax over another**
    * Inertia (pick something that people are already used to)
      * Ex: (a + 5)/ 3
    * Simple + regular
      * Ex: (/(+ a 5) 3)    -->Scheme
      * Ex: a 5 + 3 /       -->postscript 
    * Readable 
    * Writeable 
    * redundant
    * unambiguous
  
  ###### Internet RFC (Request For Comment)
      * RFC 5322, current standard for email http://rumkin.com/software/email/rfc.php
        * Grammar
          * msg-id = "<" dot-atom-text "@" id-right ">"
            * dot-atom-text =  1*atext *("." 1*atext)
            * atext = ALPHA / DIGIT /
                      "!" / "#" /
                      "$" / "%" /
                      "&" / "'" /
                      "*" / "+" /
                      "-" / "/" /
                      "=" / "?" /
                      "^" / "_" /
                      "`" / "{" /
                      "|" / "}" /
                      "~"
              * Any character except controls, SP, and specials used for atoms
            * id-right = dot-atom-text / no-fold-literal
            * No-ford-literal = "[" *atext "]"
            * dtext =   NO-WS-CTL /     ; Non white space controls
                        %d33-90 /       ; The rest of the US-ASCII
                        %d94-126        ;  characters not including "[", "]", or "\"

        * ex: Message-ID: <139.57a$b@cs.ucla.edu>


##### Tokens: individual symbols in programs
  * (In our case, %133 -- %126) 94 tokens
  * **language** = set of strings
  * **string** = finite sequence of tokens
  * **token** = number of a specified finite set
  * ex: Sample language: a^n b^m, n a's followed by m b's
    * "aaabb"--> correct "bba"--> wrong

  * **byte stream** --> character stream
    * 256 char is not enough 
  * UTF-8 (encoded) --> unicode chars ~2^23 possibilities
  
  * **What is not a token**:
    * spaces, tabs, newlines, (white space)
    * comment

  * ex: int xyz = 321;
    * 5 tokens, |int|xyz|=|321|;|
  * ex: int a = b+++c ==> b++ + c
  * ex: int x = a+++++b
    * interpretation 1: a++ ++ + b --> invalid
    * interpretation 2: a++ + ++b --> valid

  * **Tokenizers are greedy**
    * operators, identifiers, numbers

##### Keywords vs. Reserved Keywords
  * **reserved keywords**: if, else, while, return ...
  * ex: int return = 27; //it is invalid.

  * **keywords need not be reserved**
    * ex: in PL/I : if(if(3) = 5) then if(7)=29;
    * result: syntax tends to be complicated.


##### Context-free grammar 
  1. finite set of terminal symbols(tokens)     | ex in English: "dog"
  2. finite set of nonterminal symbols.        | ex in English: non-phrase
  3. finite set of grammar rules, each rule has following:
     1. a left-hand side that is a nonterminal symbol
     2. a right-hand side that is finite sequence of symbols. (terminal or nonterminal)
  4. a start symbol(a nonterminal)

  * ex:
    * msg-id = "<" dot-atom-text "@" id-right ">"
    * **EBNF(extended Backus Normal Form)**
      * id-right = dot-atom-text/No-ford-literal 
    * **BNF(BNF(Backus Normal Form/Backus-Naur Form)**
      * id-right = dot-atom-text
      * id-right = no-ford-literal
  
    * EBNF ==> no-fold-literal = "[" *dtext "]"
    * BNF
      * no-fold-literal = "[" dtexts "]"
      * dtexts = 
      * dtexts = dtext dtexts

    * EBNF ==> dot-atom-text = 1*atext *("." 1*atext)
    * BNF
      * atextplus = atext
      * atextplus = atext atext atextplus
      * dotwords = 
      * dotwords = "." atextplus dotwords
      * dot-atom-text = atextplus dotswords

##### ISO EBNF
  *  ISO: intl org. for standardization
  * 
  *  syntax = syntax rule, {syntax rule};
  *  syntax rule = meta id, '=',def list,';';
     *  "syntax rule" --> meta id
     *   = --> '=' 
     *   meta id, '=',def list,';' --> def list
     *   ; --> ';'
  *  def list = def {'|' def};
  *  def = term, {',',terms};

##### [terminal & nonterminal symbols](https://en.wikipedia.org/wiki/Terminal_and_nonterminal_symbols)

