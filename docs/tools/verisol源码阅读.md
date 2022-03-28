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

