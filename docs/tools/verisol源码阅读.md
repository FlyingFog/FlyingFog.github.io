# VeriSol源码阅读

代码架构

<img src="pic\VeriSol.png" alt="VeriSol" style="zoom:67%;" />

## SolidityAST

`namespace SolidityAST`

```cs
// 遍历AST数
public class BasicASTVisitor : IASTVisitor
```



```cs
// 记录编译器输出
public class CompilerOutput
// error的类型   
public class CompilerError
// 源代码
public class SoliditySourceFile
```



```cs
// 通用的visitor的接口
public interface IASTGenericVisitor<T>
```



```cs
// 遍历AST的接口
public interface IASTVisitor
```



```cs
// id to node map
public class NodeMapper : BasicASTVisitor
```



```cs
class RegressionExecutor
```



```cs
// 运行compile
public class SolidityCompiler
```



```cs
// 工具类 接收元素
public class Utils
```



```cs
public class AST
```

## BoogieAST

BoogieAST.cs

`namespace BoogieAST`

```cs
// boogie 语法树
public class BoogieAST{}
// ast节点抽象基类
public abstract class BoogieASTNode{}
// 一系列派生类 表示不同类型的节点

...

    
```



## solidity CFG

CFGBuilder.cs

```cs
using SolidityAST;

public class CFGBuilder : BasicASTVisitor
{
	public static FunctionCFG BuildFunctionCFG(CFGNodeFactory factory, FunctionDefinition func)
    {
    
    }
   private static void ConnectControlFlow(CFGNode from, CFGNode to)
   {
       //建 from --> to 的边
   }
	private CFGBuilder(CFGNodeFactory factory, FunctionCFG currentCFG){} //构造函数   
  	private void AppendControlFlow(ASTNode astNode){} // 添加 实际为接收下一个astNode
   private CFGNode CreateControlFlow(CFGNode entry, ASTNode astNode){
       // 创建控制流边 用entry和astnode
   }
	private List<CFGNode> SplitControlFlow(int num){
        //分裂控制流图，分叉 连接currentNode 和 node
   }        
   private void MergeControlFlow(List<CFGNode> nodes, CFGNode destination = null){
   		
   }
   protected override bool CommonVisit(ASTNode astNode){
       // astNode is Assignment 则加入表达式
   }
   
   public override bool Visit(BinaryOperation astNode){
   	   // 根据op做处理 例如：||  
   
		case "||":
       {
           AppendControlFlow(astNode.LeftExpression);
           List<CFGNode> nodes = SplitControlFlow(2);
           nodes[1] = CreateControlFlow(nodes[1], astNode.RightExpression);
           MergeControlFlow(nodes, nodes[0]);
       }
       break;
       case "&&":
       {
           AppendControlFlow(astNode.LeftExpression);
           List<CFGNode> nodes = SplitControlFlow(2);
           nodes[0] = CreateControlFlow(nodes[0], astNode.RightExpression);
           MergeControlFlow(nodes, nodes[1]);
       }
   }
    
    public override bool Visit(Conditional astNode){ 条件判断式 
        // create the true assignment
        nodes[0] = CreateControlFlow(nodes[0], astNode.TrueExpression);

        // create the false assignment
        nodes[1] = CreateControlFlow(nodes[1], astNode.FalseExpression);                       
    }
   
    public override bool Visit(IfStatement astNode){
        // IF 判断式
    }
}
```



`CFGNodeFactory.cs`

```cs
public class CFGNodeFactory
    {
        private int id;

        public CFGNodeFactory()
        {
            id = 1;
        }

        public CFGNode MakeNode()
        {
            return new CFGNode(id++);
        }
    }
```



SolidityCFG.cs

```cs
using SolidityAST;

public class CFG
    {
    }
public class FunctionCFG
    {
    public CFGNode Entry { get; set; }

    public CFGNode Exit { get; set; }

    public FunctionCFG()
    {
        Entry = null;
        Exit = null;
    }
}

public class CFGNode{
public int Id { get; private set; }
    public List<CFGNode> Entries { get; private set; }

    public List<CFGNode> Exits { get; private set; }

    public BasicBlock Block { get; private set; }

    public CFGNode(int id)
    {
        Id = id;
        Entries = new List<CFGNode>();
        Exits = new List<CFGNode>();
        Block = new BasicBlock();
    }
}

public class BasicBlock
{
    private List<Expression> expressions;
    // predicate based on which to split the control flow
    private Expression predicate;

    public BasicBlock()

    public List<Expression> GetExpressions()

    public void AddExpression(Expression expression)

    public Expression GetPredicate()

    public void SetPredicate(Expression predicate)
}
```

## ExternalToolsManager

```cs
public static class ExternalToolsManager
{
    private static ILogger logger;

    public static ToolManager Solc { get; private set; }
    public static ToolManager Z3 { get; private set; }
    public static ToolManager Boogie { get; private set; }
    public static ToolManager Corral { get; private set; }
}
```

下载和管理依赖工具。

## SolToBoogie

本章重点

BoogieTranslator.cs

```cs
using BoogieAST;
using SolidityAST;

public class BoogieTranslator
    {
    // set of method@contract pairs whose translatin is skipped
    public BoogieAST Translate(AST solidityAST, HashSet<Tuple<string, string>> ignoredMethods, TranslatorFlags _translatorFlags = null, String entryPointContract = "")
    {
        bool generateInlineAttributesInBpl = _translatorFlags.GenerateInlineAttributes;

        SourceUnitList sourceUnits = solidityAST.GetSourceUnits();

        TranslatorContext context = new TranslatorContext(ignoredMethods, generateInlineAttributesInBpl, _translatorFlags, entryPointContract);
        context.IdToNodeMap = solidityAST.GetIdToNodeMap();
        context.SourceDirectory = solidityAST.SourceDirectory;

        // collect the absolute source path and line number for each AST node
        SourceInfoCollector sourceInfoCollector = new SourceInfoCollector(context);
        sourceUnits.Accept(sourceInfoCollector);

        // de-sugar the solidity AST
        // will modify the AST
        SolidityDesugaring desugaring = new SolidityDesugaring(context);
        sourceUnits.Accept(desugaring);

        // collect all contract definitions
        ContractCollector contractCollector = new ContractCollector(context);
        sourceUnits.Accept(contractCollector);

        // collect all sub types for each contract
        InheritanceCollector inheritanceCollector = new InheritanceCollector(context);
        inheritanceCollector.Collect();

        // collect explicit state variables
        StateVariableCollector stateVariableCollector = new StateVariableCollector(context);
        sourceUnits.Accept(stateVariableCollector);

        // resolve state variable declarations and determine the visible ones for each contract
        StateVariableResolver stateVariableResolver = new StateVariableResolver(context);
        stateVariableResolver.Resolve();

        // collect mappings and arrays
        MapArrayCollector mapArrayCollector = new MapArrayCollector(context);
        sourceUnits.Accept(mapArrayCollector);

        // collect constructor definitions
        ConstructorCollector constructorCollector = new ConstructorCollector(context);
        sourceUnits.Accept(constructorCollector);

        // collect explicit function and event definitions
        FunctionEventCollector functionEventCollector = new FunctionEventCollector(context);
        sourceUnits.Accept(functionEventCollector);

        // resolve function and event definitions and determine the actual definition for a dynamic type
        FunctionEventResolver functionEventResolver = new FunctionEventResolver(context);
        functionEventResolver.Resolve();

        // add types, gobal ghost variables, and axioms
        GhostVarAndAxiomGenerator generator = new GhostVarAndAxiomGenerator(context);
        generator.Generate();

        // collect modifiers information
        ModifierCollector modifierCollector = new ModifierCollector(context);
        sourceUnits.Accept(modifierCollector);

        // collect all using using definitions
        UsingCollector usingCollector = new UsingCollector(context);
        sourceUnits.Accept(usingCollector);

        // translate procedures
        ProcedureTranslator procTranslator = new ProcedureTranslator(context, generateInlineAttributesInBpl);
        sourceUnits.Accept(procTranslator);

        // generate fallbacks
        FallbackGenerator fallbackGenerator = new FallbackGenerator(context);
        fallbackGenerator.Generate();

        // generate harness for each contract
        if (!context.TranslateFlags.NoHarness)
        {
            HarnessGenerator harnessGenerator = new HarnessGenerator(context, procTranslator.ContractInvariants);
            harnessGenerator.Generate();
        }

        if (context.TranslateFlags.ModelReverts)
        {
            RevertLogicGenerator reverGenerator = new RevertLogicGenerator(context);
            reverGenerator.Generate();
        }

        if (context.TranslateFlags.DoModSetAnalysis)
        {
            ModSetAnalysis modSetAnalysis = new ModSetAnalysis(context);
            modSetAnalysis.PerformModSetAnalysis();
        }

        return new BoogieAST(context.Program);
    }
}
```

## VeriSol

主程序

Program.cs

```cs
/// <summary>
/// Top level application to run VeriSol to target proofs as well as scalable counterexamples
/// </summary>
class Program
{
    
}
```

VeriSolExecutor.cs

```cs
internal class VeriSolExecutor
{
    public VeriSolExecutor(string solidityFilePath, string contractName, int corralRecursionLimit, HashSet<Tuple<string, string>> ignoreMethods, bool tryRefutation, bool tryProofFlag, ILogger logger, bool _printTransactionSequence, TranslatorFlags _translatorFlags = null)
    {
        //一堆初始化变量赋值
    }
    public int Execute(){
        
    }
    private bool FindProof(){
        
    }
    private bool RunCorralForRefutation(){
        
    }
    private void DisplayTraceUsingConcurrencyExplorer(){
        
    }
    private string[] FilterCorralTrace(string[] trace){
        
    }
    private List<Tuple<string, string>> ConvertTrace(string[] corralTrace){
        
    }
    private string ConvertFunctionName(string origName){
        
    }
    private void PrintCounterexample(){
        
    }
    private void PrintCounterexampleHelper(List<Tuple<string, string>> trace){
        
    }
    private bool ExecuteSolToBoogie(){
        
    }
    private string RunBinary(string cmdName, string arguments){
        
    }
    private bool CompareCorralOutput(string expected, string actual){
        
    }
    private bool CompareBoogieOutput(string actual){
        
    }
```

