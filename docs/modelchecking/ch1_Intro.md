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

1. 测试Testing：最快和简单的方式检测错误
2. 抽象解释Abstract interpretation：
3. 高阶理论证明Higher-order theorem proving：  
