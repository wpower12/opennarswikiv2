> ### InputOutputFormat  
> The input/output format of the system 

***

### I/O Format

Each line in the system's input and output is either a task (as defined in the following grammar, in BNF notation), or an integer, indicating the number of inference steps between tasks.

In a task, all the space characters are optional, and will be ignored by the system in processing.

### Narsese Grammar

```
             task ::= [budget] sentence                       (* task to be processed *)

         sentence ::= statement"." [tense] [truth]            (* judgement to be remembered *)
                    | statement"?" [tense] [truth]            (* question to be answered, tense added in OpenNARS 1.7 *)
                    | statement"@" [tense] [truth]            (* question on desire value to be answered, tense added in OpenNARS 1.7 *)
                    | statement"!" [tense] [truth]            (* goal to be realized, tense added in OpenNARS 1.7 *)

        statement ::= <"<">term copula term<">">              (* two terms related to each other *)
                    | <"(">term copula term<")">              (* two terms related to each other, new notation *)
                    | term                                    (* a term can name a statement *)
                    | "(^"word {","term} ")"                  (* an operation to be executed *)
                    | word"("term {","term} ")"               (* an operation to be executed, new notation *)

           copula ::= "-->"                                   (* inheritance *)
                    | "<->"                                   (* similarity *)
                    | "{--"                                   (* instance *)
                    | "--]"                                   (* property *)
                    | "{-]"                                   (* instance-property *)
                    | "==>"                                   (* implication *)
                    | "=/>"                                   (* predictive implication *)
                    | "=|>"                                   (* concurrent implication *)
                    | "=\\>"                                  (* =\> retrospective implication *)
                    | "<=>"                                   (* equivalence *)
                    | "</>"                                   (* predictive equivalence *)
                    | "<|>"                                   (* concurrent equivalence *)

             term ::= word                                    (* an atomic constant term *)
                    | variable                                (* an atomic variable term *)
                    | compound-term                           (* a term with internal structure *)
                    | statement                               (* a statement can serve as a term *)
                    | interval                                (* time measure between events *)

    compound-term ::= op-ext-set term {"," term} "}"          (* extensional set *)
                    | op-int-set term {"," term} "]"          (* intensional set *)
                    | "("op-multi"," term {"," term} ")"      (* with prefix operator *)
                    | "("op-single"," term "," term ")"       (* with prefix operator *)
                    | "(" term {op-multi term} ")"            (* with infix operator *)
                    | "(" term op-single term ")"             (* with infix operator *)
                    | "(" term {","term} ")"                  (* product, new notation *)
                    | "(" op-ext-image "," term {"," term} ")"(* special case, extensional image *)
                    | "(" op-int-image "," term {"," term} ")"(* special case, \ intensional image *)
                    | "(" op-negation "," term ")"            (* negation *)
                    | op-negation term                        (* negation, new notation *)

        op-int-set::= "["                                     (* extensional set *)
        op-ext-set::= "{"                                     (* intensional set *)
       op-negation::= "--"                                    (* negation *)
      op-int-image::= "\\"                                    (* \ intensional image *)
      op-ext-image::= "/"                                     (* extensional image *)
         op-multi ::= "&&"                                    (* conjunction *)
                    | "*"                                     (* product *)
                    | "||"                                    (* disjunction *)
                    | "&|"                                    (* parallel events *)
                    | "&/"                                    (* sequential events *)
                    | "|"                                     (* intensional intersection *)
                    | "&"                                     (* extensional intersection *)
        op-single ::= "-"                                     (* extensional difference *)
                    | "~"                                     (* intensional difference *)

         variable ::= "$"word                                 (* independent variable *)
                    | "#"word                                 (* dependent variable *)
                    | "?"word                                 (* query variable in question *)

            tense ::= ":/:"                                   (* future event *)
                    | ":|:"                                   (* present event *)
                    | ":\\:"                                  (* :\: past event *)
                    | <":">#"\d+"<":">                        (* defined event, output only *)

         interval ::= <"/">#"\d+"                             (* integer *)

            truth ::= <"%">frequency[<";">confidence]<"%">    (* two numbers in [0,1]x(0,1) *)
           budget ::= <"$">priority[<";">durability][<";">quality]<"$"> (* three numbers in [0,1]x(0,1)x[0,1] *)

               word : #"\w+"                                  (* unicode string *)    
           priority : #"([0]?\.[0-9]+|1\.[0]*|1|0)"           (* 0 <= x <= 1 *)
         durability : #"[0]?\.[0]*[1-9]{1}[0-9]*"             (* 0 <  x <  1 *)
            quality : #"([0]?\.[0-9]+|1\.[0]*|1|0)"           (* 0 <= x <= 1 *)
          frequency : #"([0]?\.[0-9]+|1\.[0]*|1|0)"           (* 0 <= x <= 1 *)
         confidence : #"[0]?\.[0]*[1-9]{1}[0-9]*"             (* 0 <  x <  1 *)
```

### Example Usage

* Tim is dead.

  `<{Tim} --> [dead]>.`

* Tim is a human.

  `<{Tim} --> human>.`

* Humans are a lifeform.

  `<human --> lifeform>.`

* Lifeforms are like machines.

  `<lifeform <-> machine>.`

* Tom eats chocolate.
  
  ```
  <(*,{Tom},chocolate) --> eat>.

  <{Tom} --> (/,eat,_,chocolate)>.

  <chocolate --> (/,eat,{Tom},_)>.
  ```