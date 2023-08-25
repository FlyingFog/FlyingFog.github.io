---
title: "Introduction to Model Checking"
layout: "blog"
tags: ["model checking"]
series: ["model checking"]
---

# Chapter 1 Introduction to Model Checking   

## 摘要

>A computer-assisted method for the analysis of dynamical systems that can be modeled by state-transition systems.
>
>Drawing from research traditions in mathematical logic, programming languages, hardware design, and theoretical computer science, model checking is now widely used for the verification of hardware and software in industry.



## 计算机辅助验证之例

>The only effective way to raise the confidence level of a program significantly is to give a convincing proof of its correctness.  

- The main challenge is scalability: real-world software systems not only include complex control and data structures, but depend on much “context” such as libraries and interfaces to other code, including lower-level systems code.   

- This asymmetry between coding and verification is the main motivation for computer-aided verification.

- Turing’s halting problem and Rice’s Theorem tell us that computer-aided verification is, in general, an unsolvable problem.  
- If the values of integer variables are bounded, we obtain a system with finitely many different states, and verification becomes decidable.  

- However, complexity theory tells us that even for finite-state systems, many verification questions require a prohibitive effort.  

**模型检查**的发明标志着在硬件和软件行业中向实际使用逻辑进行错误发现（falsification rather than verification  ）的范式转变

- Not untypical of paradigms acquired many decades ago, the case for model checking appears simple and convincing in retrospect . 

模型检测的基本形式为：

+ 建模Modeling：**有限状态转移图**为描述有限状态系统（如硬件）以及软件和通信协议的有限状态抽象提供了充分的形式

- 规约Specification：  时序逻辑为描述状态转换系统的正确性属性提供了一个自然框架
- 算法Algorithms：存在用于确定有限状态转换结构是否是时序逻辑公式的模型的决策程序。 此外，当公式在结构中不正确时，决策程序可以产生诊断性反例。

![image-20220310103233637](\pic\image-20220310103233637.png)

**两大主题：**

> 两个研究分类，两个主题推动了模型检测大部分研究议程。

1. 算法：设计模型检测算法扩展到现实中的问题

   > 状态组合爆炸
   >
   > unbounded情景下，状态空间是无限的
   >
   > 在实践中，非常大和无限的状态转换系统通过有效抽象来逼近，例如有限状态抽象，或者决策过程通过所谓的“半算法”来逼近

2. 建模：拓展模型检测的框架

   >建模无界情景
   >
   >保存可判定性，构造优先状态抽象 ==> 1
   >
   >牺牲可判定性，保持模型检测的功能

**三大优势：**

>  We believe that the main strengths of model checking are threefold  

1. 可计算机化、理想的、自动的 系统、可算法化的方法
2. 模型检测可以被用于设计过程中的不同状态
3. 模型检查处理并发性特别好。 并发错误特别微妙、偶然，因此难以重现； 它们很难通过测试系统找到，并且它们的缺失很难通过逻辑论证或程序分析来证明，因为并行进程之间可能存在极大量的交互。

与其他相关方法的**区别**：

**efficiency versus expressiveness (or precision)**   

1. 测试Testing：最快和简单的方式检测错误。While testing is only debugging, model checking is systematic debugging aimed at model verification.  Chapter 19 will discuss the cross-fertilization of modern model-checking and white-box testing techniques.
2. 抽象解释Abstract interpretation: Abstract interpretation and other program analyses are similar to model checking in that they employ algorithms to prove program properties automatically.遵循编程语言语义而非逻辑的传统，静态分析框架（如数据流分析、抽象解释和富类型系统）是基于格理论构建的。In comparison to model checking, the focus of program analyses is on efficiency rather than expressiveness, and on code rather than models. In recent research on software verification, abstract interpretation and model checking have converged to a large extent.
3. 高阶理论证明Higher-order theorem proving：  is a powerful and proven approach for the verification of complex systems. In comparison to model checking, the focus of higher-order theorem proving is on expressiveness rather than efficiency: it handles data manipulation precisely and typically aims at full functional correctness. 

模型检查的特点不是单纯的方法，而是以调试和分析现实世界中存在的、可以建模为状态转换系统的动态系统为目标。

## 时序逻辑模型检测初探

### 克里普克结构 kripke structure

- 顶点用原子命题集标记的有限有向图
- 图的顶点和边被称为“状态”和“转移”
- $K=<S,R,L>, S状态集，R \subseteq S \times S转移关系，L:S \to 2^A把每个状态与一个原子命题集合关联。$ $L(s)代表s状态下为真的原子命题集合。$ 假设R是完全的。  

### CTL*

A propositional modal logic with path quantifiers, which are interpreted over states, and temporal operators, which are interpreted over paths.  

![image-20220314000731592](\pic\image-20220314000731592.png)	

The formal semantics of CTL* is based on this syntactic distinction：

![image-20220314000839323](\pic\image-20220314000839323.png)

两个时序操作**X 和 U **代表了CTL*的所有表示能力。

给定一个Kripke结构K，状态s，状态公式f，模型检测算法是对$K,s \models f$的决策过程。

Table 2 gives common examples of CTL* formulas. For each formula, the table describes the semantics of the formula and the structure of possible counterexamples.

Note that a counterexample is a witness for the negated formula.   

Intuitively, CTL is a logic based on state formulas, and LTL is a logic avoiding state formulas.   While CTL can be model checked particularly efficiently, LTL allows the natural specification of properties of dynamic behaviors, which correspond to paths.  

![image-20220314001032613](\pic\image-20220314001032613.png)

![image-20220314000907037](\pic\image-20220314000907037.png)

### CTL

CTL (Computation Tree Logic)  is the syntactic fragment of CTL in which every path quantifier is immediately followed by a temporal operator.

![image-20220314103301124](\pic\image-20220314103301124.png)

> **Theorem 1** *There is a CTL model-checking algorithm whose running time depends linearly on the size of the Kripke structure and on the length of the CTL formula (if the other parameter is fixed).*  

### LTL

LTL (Linear-time Temporal Logic) is the syntactic fragment of CTL* that contains no path quantifiers except a leading A.

For a Kripke structure K and LTL- formula ψ, a counterexample of ψ in K is an infinite path π of K such that K,π $\models$ ψ. Then K,s $\not\models$ Aψ for the initial state s of π, and, thus, the infinite path π is also a counterexample for the LTL formula Aψ.   

Moreover, an inductive proof shows that the counterexample π can w.l.o.g. be restricted to have a “lasso” shape $v \cdot w^\omega$, i.e., an initial finite path (prefix) v followed by an infinitely repeated finite path (cycle) w.   

Safety properties always have finite paths as counterexamples: they specify a finite, “bad” series of events that must never happen. The simplest safety properties are invariants, that is, LTL formulas of the form AGf , where f is a boolean combination of atomic propositions.  

Most LTL properties specify a combination of safety and so-called liveness properties, and thus may require infinite paths (lassos) as counterexamples.   

Let p be an atomic proposition that signals that a process is ready to be scheduled, and let q signal that the process is being scheduled. A scheduler is weakly fair if it does not forever neglect scheduling a process that is continuously ready to be scheduled: GF(¬p ∨ q) . By contrast, the LTL- formula (GFp) → GFq specifies that, along an infinite path, if the process is ready infinitely often, then it is scheduled infinitely often. This requirement defines a strongly fair scheduler, which must not forever neglect scheduling a process that is ready infinitely often (but not necessarily continuously) .

Using a tableau construction for modal logics, it is possible to translate an LTL- specification ψ into a Büchi automaton Bψ over the alphabet $2^A$ (where A is the set of atomic propositions) such that for all Kripke structures K and infinite paths π, the infinite word L(π) is accepted by the automaton Bψ iff π is a counterexample of ψ in K.  

> **Theorem 2** *There is an LTL model-checking algorithm whose running time depends linearly on the size of the Kripke structure and exponentially on* the length of the LTL formula.   

## 本书章节概述

Over the span of more than three decades, model checking has developed from a niche subject in theoretical computer science into a large family of formalisms methods, tools, subcommunities, and application areas.

### 算法挑战

A significant part of model-checking research has focused on algorithmic methods to deal with state explosion. These methods avoid the explicit construction of the complete Kripke structure.

- 结构化方法，Structural methods for model checking exploit the structure of the syntactic expression (the “code”) that defines the system.  

- 符号方法，Symbolic methods represent state sets and the transition relation of a Kripke structure by an expression in a symbolic logic, rather than by an explicit enumeration of states or transitions.   

- 抽象，Abstraction (Chapters 13–15) is a more aggressive approach to state explosion. Abstraction reduces a Kripke structure K to a smaller homomorphic image Kˆ —the abstract model—which preserves certain properties of the original structure and can be analyzed more efficiently.   Formally, if Kˆ simulates K, then for all ACTL* specifications ϕ, if Kˆ$\models$ ϕ then K $\models $ϕ, but not vice versa.  

  A key ingredient of many modern model checkers is counterexample-guided abstraction refinement .

  ![image-20220314111221420](\pic\image-20220314111221420.png)

### 建模挑战

For modeling software, there are four paradigmatic extensions of finite state-transition systems that are essential for modeling certain important classes of systems

- 安全协议Security protocols 为形式化方法提供了一个完美的应用领域:它们通常很小，但很难得到正确的处理，而且它们的正确性是至关重要的。然而，任何安全协议的方法都必须处理非有限状态的概念，如nonces和密钥、加密和解密以及未知的攻击者
- 图博弈Graph games are an extension of state-transition systems with multiple actors. In each state, one or more of the actors choose actions that, taken together, determine the next state of the system.   图博弈需要为具有多个组件、过程、参与者或代理的系统建模，这些组件、过程、参与者或代理具有不同的、有时相互冲突的目标。即使系统是单一的，它的环境有时也必须被视为独立的，就像双人游戏中的对手。
- 概率系统Probabilistic systems such as discrete-time Markovchains or Markov decision processes, where from certain states the next state is chosen according to a probability distribution.   
- 实时和混合系统Real-time and hybrid systems are extensions of discrete state-transition systems with continuous components  

## 模型检测的未来

**Three motivation**

The ubiquity and complexity of hardware- and software-based systems continue to grow.   In addition, hardware- and softwarebased systems are increasingly deployed in safety-critical situations, to control and connect physical systems from cardiac pacemakers to aircraft. Model checking makes similar inroads into the practice of software development, especially in error-prone controlcentric—as opposed to data-centric—software domains, including systems software (kernels, schedulers, device drivers, memory and communication protocols, etc.), distributed algorithms and concurrent data structures, and the rapidly growing areas of software-defined networking and cyber-physical systems.

In the future the correctness, reliability, security, and overall robustness of systems will play an ever-increasing role, also as a differentiating factor for software and hardware products.  Correctness has moved recently to center stage in compilers, operating systems, and distributed systems research.  

With the rise of digital technology, models based on state-transition systems (or “discrete-event systems”) have become ubiquitous in systems engineering.   Model checking—as the central paradigm for analyzing state-transition systems—is therefore only at the beginning of its application to a wide range of different domains, from engineering to science, business, law, etc.  

**two of the many currently active directions of model-checking research**  

Much current research is devoted to moving beyond verification to synthesis  

> Verification is the task of checking whether a given system satisfies a given specification; 
>
> Synthesis is the task of constructing a system that satisfies a given specification.   
>
> 虽然全自动的功能合成只在限制的情况下是可行的，例如从高级语言编译，或从给定的构建块合成电路，一般的合成任务已经看到了许多最近的进展，需要改进或完成给定的，部分系统描述，以满足某些性质。Often the synthesized properties are non-functional. More generally, the goal of “computer-aided programming” is to relieve the programmer from tedious but error-prone implementation details so that they can focus on the functionality of the design, while automating the fulfillment of other requirements on the code such as security and fault tolerance.  Much effort has been devoted to template- and syntax-guided approaches to synthesis, and to the use of inductive and learning techniques in synthesis.

A second important trend that receives much current attention generalizes   the classical boolean setting of model checking towards a quantitative setting of model measuring.   

>Model checking answers the boolean question of whether a system does or does not satisfy a specification;
>
>Model measuring quantifies the quality of a system, e.g., by computing a distance between the system and the specification.
>
>Quantitative measures can be used to distinguish different systems that satisfy the same functional specification: they may measure the performance of the system, its resource consumption, its cost, or other non-functional properties such as different notions of robustness. Quantitative measures can also be used to distinguish different systems that do not satisfy a given specification.
>
>Several such extensions have been proposed, including quantitative temporal logics. Statistical model checking replaces absolute guarantees with probabilistic guarantees.  

**two of the many potential new application areas for model checking**  

A recent, perhaps unexpected application scenario for model checking is the modeling and analysis of biological phenomena using state-transition systems.

An equally speculative and perhaps even more important new application domain for model checking is the analysis of learning-based software and other systems built on modern artificial intelligence.   虽然传统的基于逻辑的人工智能与基于数学逻辑共同基础的形式验证有直接联系，但现代人工智能的许多成功似乎逃脱了理性解释和定量分析。虽然有基于概率模型、数理统计和优化的定量方法试图解释和量化机器学习系统，但要保证这些系统的特性，将需要一种更正式的方法。许多基于学习的系统——比如自动驾驶汽车中用于计算机视觉的系统——都是安全的关键，即使是有限的正式保证也会有巨大的价值。对可核查性的要求可以指导这种系统的设计，例如纳入监测机制，确保the system does not leave a safe envelope of operation , 基于学习的技术已经开始进入模型检查技术

**This shows that model checking is a vibrant, expanding field of research with no lack of challenges and impact for many years to come.**  
