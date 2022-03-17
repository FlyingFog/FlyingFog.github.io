# 时序逻辑和公平离散系统

> Temporal Logic and Fair Discrete Systems

## 摘要

- Temporal logics can be classified by their view of the evolution of time as either linear or branching.  

- In the linear-time view, we see time ranging over a linear total order and executions are sequences of states.  When the system has multiple possible executions (due to nondeterminism or reading input) we view them as separate possible evolutions and the system has a set of possible behaviors.  
- In the branching-time view, a point in time may have multiple possible successors and accordingly executions are tree-like structures.   According to this view, a system has exactly one execution, which takes the form of a tree.   
- Fair Discrete Structures, the model on which we evaluate the truth and falsity of temporal logic formulas. Fair Discrete Structures describe the states of a system and their possible evolution.   
- Propositional Linear Temporal Logic  
- We explain the distinction between safety and liveness properties and introduce a hierarchy of liveness properties of increasing expressiveness.   
- The expressive power of full LTL and cover extensions that increase its expressive power.
- Algorithms for checking the satisfiability of LTL and model checking LTL.   
- Computation Tree Logic (CTL) ：expressive power, consider extensions, and cover satisfiability and model checking.
- Examples of formulas in both LTL and CTL and stress the differences between the two.
- CTL*, a hybrid of LTL and CTL that combines the linear and branching views into one logic.  

## Introduction

>In the late 1950s Arthur Prior introduced what we call today tense logic; essentially, introducing the operators Fp meaning “it will be the case that p” and its dual Gp meaning “it will always be the case that p”. 
>
>Roughly at the same time, working on modal logic, Saul Kripke introduced a semantics, which we call today Kripke semantics, to interpret modal logic. His suggestion was to interpret modal logic over a set of possible worlds and an accessibility relation between worlds.   
>
>Pnueli introduced Prior’s tense operators to verification and suggested that they can be used for checking the correctness of programs.   In particular, Pnueli’s ideas considered ongoing and non-terminating computations of programs.
>
>The existing paradigm of verification was that of pre- and post-conditions matching programs that get an input and produce an output upon termination.  Instead, what were later termed as reactive systems continuously interact with their environment, receive inputs, send outputs, and importantly do not terminate.   
>
>Temporal logic proved a convenient way to do this. It can be used to capture the specification of programs in a formal notation that can then be checked on programs.  
>
>Linear Temporal Logic (abbreviated LTL), the logic introduced by Pnueli, is linear in the sense that at every moment in time there is only one possible future.  
>
>Clarke and Emerson and Queille and Sifakis introduced a more elaborate branching-time logic, Computation Tree Logic (abbreviated  CTL), and the idea of model checking: Model checking is the algorithmic analysis for determining whether a program satisfies a given temporal logic specification.  

## 公平离散系统

In order to be able to formally reason about systems and programs, we have to agree on a formal mathematical model in which reasoning can be applied. Transition systems are, by now, a standard tool in computer science for representing programs.   

We define Kripke structures, one of the most popular versions of transition systems used in modeling.
Then, we present a symbolic version of transition systems, which we call fair discrete systems or FDS for short, where the states arise as interpretations of variables and transitions correspond to changes in variables’ values.   

One of the most important features of programs and systems is concurrency, or the ability to communicate with other programs.  



### Kripke Structures  

<img src="\pic\image-20220315155733549.png" alt="image-20220315155733549" style="zoom:85%;" />	

However, in model checking, higher-level representations of transition systems provide a more direct relation to programs and enable us to reason about systems using sets of states rather than individual states.   

- The idea is that states are obtained by interpreting the values of variables of the system  
- In addition, in the context of model checking it is sometimes necessary to ignore some “not interesting” runs of the system.   

### Definition of Fair Discrete System  

![image-20220316160809892](\pic\image-20220316160809892.png)	

![image-20220316160857898](\pic\image-20220316160857898.png)	

![image-20220316160923975](\pic\image-20220316160923975.png)	

<img src="\pic\image-20220317101250877.png" alt="image-20220317101250877" style="zoom:100%;" />	

![image-20220317101343076](\pic\image-20220317101343076.png)	

![image-20220317101405894](\pic\image-20220317101405894.png)	

$\mathscr{D}_{1} ||| \mathscr{D}_{2}=\left\langle\mathscr{V} 1 \cup \mathscr{V}_{2}, \theta_{1} \wedge \theta_{2}, \rho_{1} \wedge \rho_{2}, \mathscr{J}_{1} \cup \mathscr{J}_{2}, \mathscr{C}_{1} \cup \mathscr{C}_{2}\right\rangle$

### Representing Programs  

We do not formally define a programming language, however, the meaning of commands and constructs will be clear from the translation to FDSs. 

考虑如图1所示的程序，它可以表示为一个FDS，运用 $\pi$ 代表程序在$\{l_0,...,l3 \}$的程序变量 和 $n$是一个从10开始的整数。

形式化的，$\mathscr{D} = \langle \{\pi,n\},\theta, \rho, \mathscr{}  \rangle$

<img src="\pic\image-20220317104519396.png" alt="image-20220317104519396" style="zoom:90%;" />	

### Algorithms  

