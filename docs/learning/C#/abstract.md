# C#

## 体系结构

C# 程序在 .NET 上运行，而 .NET 是名为公共语言运行时 (CLR) 的虚执行系统和一组类库。 CLR 是 Microsoft 对公共语言基础结构 (CLI) 国际标准的实现。 CLI 是创建执行和开发环境的基础，语言和库可以在其中无缝地协同工作。

用 C# 编写的源代码被编译成符合 CLI 规范的[中间语言 (IL)](https://docs.microsoft.com/zh-cn/dotnet/standard/managed-code)。 IL 代码和资源（如位图和字符串）存储在扩展名通常为 .dll 的程序集中。 程序集包含一个介绍程序集的类型、版本和区域性的清单。

执行 C# 程序时，程序集将加载到 CLR。 CLR 会直接执行实时 (JIT) 编译，将 IL 代码转换成本机指令。 CLR 可提供其他与自动垃圾回收、异常处理和资源管理相关的服务。 CLR 执行的代码有时称为“托管代码”。而“非托管代码”被编译成面向特定平台的本机语言。

语言互操作性是 .NET 的一项重要功能。 C# 编译器生成的 IL 代码符合公共类型规范 (CTS)。 通过 C# 生成的 IL 代码可以与通过 .NET 版本的 F#、Visual Basic、C++ 生成的代码进行交互。 还有 20 多种与 CTS 兼容的语言。 单个程序集可包含多个用不同 .NET 语言编写的模块。 这些类型可以相互引用，就像它们是用同一种语言编写的一样。

除了运行时服务之外，.NET 还包含大量库。 这些库支持多种不同的工作负载。 它们已整理到命名空间中，这些命名空间提供各种实用功能。 这些功能包括文件输入输出、字符串控制、XML 分析、Web 应用程序框架和 Windows 窗体控件。 典型的 C# 应用程序广泛使用 .NET 类库来处理常见的“管道”零碎工作。

## 类型和变量

对于引用类型，两个变量可以引用同一个对象；对一个变量执行的运算可能会影响另一个变量引用的对象。 借助值类型，每个变量都有自己的数据副本；因此，对一个变量执行的运算不会影响另一个变量（`ref` 和 `out` 参数变量除外）。

- 值类型
  - 简单类型
    - [有符号整型](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)：、`short`、`int`、`long`
    - [无符号整型](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)：、`ushort`、`uint`、`ulong`
    - [Unicode 字符](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/character-encoding-introduction)：，表示 UTF-16 代码单元
    - [IEEE 二进制浮点](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)：、`double`
    - [高精度十进制浮点数](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)：
    - 布尔值：`bool`，表示布尔值（`true` 或 `false`）
  - 枚举类型
    - `enum E {...}` 格式的用户定义类型。 `enum` 类型是一种包含已命名常量的独特类型。 每个 `enum` 类型都有一个基础类型（必须是八种整型类型之一）。 `enum` 类型的值集与基础类型的值集相同。
  - 结构类型
    - 格式为 `struct S {...}` 的用户定义类型
  - 可以为 null 的值类型
    - 值为 `null` 的其他所有值类型的扩展
  - 元组值类型
    - 格式为 `(T1, T2, ...)` 的用户定义类型
- 引用类型
  - 类类型
    - 其他所有类型的最终基类：`object`
    - [Unicode 字符串](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/character-encoding-introduction)：，表示 UTF-16 代码单元序列
    - 格式为 `class C {...}` 的用户定义类型
  - 接口类型
    - 格式为 `interface I {...}` 的用户定义类型
  - 数组类型
    - 一维、多维和交错。 例如：`int[]`、`int[,]` 和 `int[][]`
  - 委托类型
    - 格式为 `delegate int D(...)` 的用户定义类型

C# 程序使用类型声明创建新类型。 类型声明指定新类型的名称和成员。 用户可定义以下六种 C# 类型：类类型、结构类型、接口类型、枚举类型、委托类型和元组值类型。 还可以声明 record 类型（record struct 或 record class）。 记录类型具有编译器合成成员。 记录主要用于存储值，关联行为最少。

class 类型定义包含数据成员（字段）和函数成员（方法、属性及其他）的数据结构。 类类型支持单一继承和多形性，即派生类可以扩展和专门针对基类的机制。
struct 类型定义包含数据成员和函数成员的结构，这一点与类类型相似。 不过，与类不同的是，结构是值类型，通常不需要进行堆分配。 结构类型不支持用户指定的继承，并且所有结构类型均隐式继承自类型 object。
interface 类型将协定定义为一组已命名的公共成员。 实现 interface 的 class 或 struct 必须提供接口成员的实现代码。 interface 可以继承自多个基接口，class 和 struct 可以实现多个接口。
delegate 类型表示引用包含特定参数列表和返回类型的方法。 通过委托，可以将方法视为可分配给变量并可作为参数传递的实体。 委托类同于函数式语言提供的函数类型。 它们还类似于其他一些语言中存在的“函数指针”概念。 与函数指针不同，委托是面向对象且类型安全的。
class、struct、interface 和 delegate 类型全部都支持泛型，因此可以使用其他类型对它们进行参数化。

数组类型是通过在类型名称后面添加方括号构造而成。 例如，int[] 是 int 类型的一维数组，int[,] 是 int 类型的二维数组，int[][] 是由 int 类型的一维数组或“交错”数组构成的一维数组。

C# 采用统一的类型系统，因此任意类型的值都可视为 `object`。每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。

*装箱*和*取消装箱*

```csharp
int i = 123;
object o = i;    // Boxing
int j = (int)o;  // Unboxing
```

将值类型的值分配给 `object` 对象引用时，会分配一个“箱”来保存此值。 该箱是引用类型的实例，此值会被复制到该箱。 相反，当 `object` 引用被显式转换成值类型时，将检查引用的 `object` 是否是具有正确值类型的箱。

## 程序结构

C# 中的关键组织结构概念包括程序、命名空间、类型、成员和程序集。 

```csharp
//此类的完全限定的名称为 Acme.Collections.Stack。 此类包含多个成员：一个 _top 字段、两个方法（Push 和 Pop）和一个 Entry 嵌套类。
namespace Acme.Collections;

public class Stack<T>
{
    Entry _top;

    public void Push(T data)
    {
        _top = new Entry(_top, data);
    }
    
    public T Pop()
    {
        if (_top == null)
        {
            throw new InvalidOperationException();
        }
        T result = _top.Data;
        _top = _top.Next;
    
        return result;
    }
    
    class Entry
    {
        public Entry Next { get; set; }
        public T Data { get; set; }
    
        public Entry(Entry next, T data)
        {
            Next = next;
            Data = data;
        }
    }
}
```
由于程序集是包含代码和元数据的自描述功能单元，因此无需在 C# 中使用 `#include` 指令和头文件。 只需在编译程序时引用特定的程序集，即可在 C# 程序中使用此程序集中包含的公共类型和成员。

C# 程序可以存储在多个源文件中。 在编译 C# 程序时，将同时处理所有源文件，并且源文件可以自由地相互引用。

## 类型和成员

*类*是最基本的 C# 类型。 类是一种数据结构，可在一个单元中就将状态（字段）和操作（方法和其他函数成员）结合起来。 类为类实例（亦称为“对象”）提供了定义 。 类支持*继承*和*多形性*，即*派生类*可以扩展和专门针对*基类*的机制。

### 类

新类使用类声明进行创建。 类声明以标头开头。 标头指定以下内容：

- 类的特性和修饰符
- 类的名称
- 基类（从[基类](https://docs.microsoft.com/zh-cn/dotnet/csharp/tour-of-csharp/types#base-classes)继承时）
- 接口由该类实现。

标头后面是类主体，由在分隔符 `{` 和 `}` 内编写的成员声明列表组成。

以下代码展示的是简单类 `Point` 的声明：

C#复制

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }
    
    public Point(int x, int y) => (X, Y) = (x, y);
}
```

类实例是使用 `new` 运算符进行创建，此运算符为新实例分配内存，调用构造函数来初始化实例，并返回对实例的引用。 以下语句创建两个 `Point` 对象，并将对这些对象的引用存储在两个变量中：

```csharp
var p1 = new Point(0, 0);
var p2 = new Point(10, 20);
```

### 类型参数

```csharp
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; }
    public TSecond Second { get; }
    
    public Pair(TFirst first, TSecond second) => 
        (First, Second) = (first, second);
}
```

声明为需要使用类型参数的类类型被称为*泛型类类型*。 结构、接口和委托类型也可以是泛型。 使用泛型类时，必须为每个类型参数提供类型自变量：

```csharp
var pair = new Pair<int, string>(1, "two");
int i = pair.First;     //TFirst int
string s = pair.Second; //TSecond string
```

### 基类

### 结构

类定义可支持继承和多形性的类型。 它们使你能够基于派生类的层次结构创建复杂的行为。 相比之下，[结构](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/struct)类型是较为简单的类型，其主要目的是存储数据值。 结构不能声明基类型；它们从 [System.ValueType](https://docs.microsoft.com/zh-cn/dotnet/api/system.valuetype) 隐式派生。 不能从 `struct` 类型派生其他 `struct` 类型。 这些类型已隐式密封。

```csharp
public struct Point
{
    public double X { get; }
    public double Y { get; }
    
    public Point(double x, double y) => (X, Y) = (x, y);
}
```

### 接口

类声明可以指定基类。 在类名和类型参数后面加上冒号和基类的名称。 省略基类规范与从 `object` 类型派生相同。 在以下示例中，`Point3D` 的基类是 `Point` 在第一个示例中，`Point` 的基类是 `object`：

```csharp
public class Point3D : Point
{
    public int Z { get; set; }
    
    public Point3D(int x, int y, int z) : base(x, y)
    {
        Z = z;
    }
}
```

类继承其基类的成员。 继承意味着一个类隐式包含其基类的几乎所有成员。 类不继承实例、静态构造函数以及终结器。 派生类可以在其继承的成员中添加新成员，但无法删除继承成员的定义。 在上面的示例中，`Point3D` 从 `Point` 继承了 `X` 和 `Y` 成员，每个 `Point3D` 实例均包含三种属性（`X`、`Y` 和 `Z`）。

可以将类类型隐式转换成其任意基类类型。 类类型的变量可以引用相应类的实例或任意派生类的实例。 例如，类声明如上，`Point` 类型的变量可以引用 `Point` 或 `Point3D`：

```csharp
Point a = new(10, 20);
Point b = new Point3D(10, 20, 30);
```

### 枚举

[枚举](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/enum)类型定义了一组常数值。 以下 `enum` 声明了定义不同根蔬菜的常数：

```csharp
public enum SomeRootVegetable
{
    HorseRadish,
    Radish,
    Turnip
}
```

还可以定义一个 `enum` 作为标志组合使用。 以下声明为四季声明了一组标志。 可以随意搭配季节组合，包括 `All` 值（包含所有季节）：

```csharp
[Flags]
public enum Seasons
{
    None = 0,
    Summer = 1,
    Autumn = 2,
    Winter = 4,
    Spring = 8,
    All = Summer | Autumn | Winter | Spring
}
```

以下示例显示了前面两个枚举的声明：

```csharp
var turnip = SomeRootVegetable.Turnip;

var spring = Seasons.Spring;
var startingOnEquinox = Seasons.Spring | Seasons.Autumn;
var theYear = Seasons.All;
```

### null

任何类型的变量都可以声明为“不可为 null”或“可为 null”***_***。 可为 null 的变量包含一个额外的 `null` 值，表示没有值。 可为 null 的值类型（结构或枚举）由 [System.Nullable](https://docs.microsoft.com/zh-cn/dotnet/api/system.nullable-1) 表示。 不可为 null 和可为 null 的引用类型都由基础引用类型表示。 这种区别由编译器和某些库读取的元数据体现。 当可为 null 的引用在没有先对照 `null` 检查其值的情况下取消引用时，编译器会发出警告。 当对不可为 null 的引用分配了可能为 `null` 的值时，编译器也会发出警告。 以下示例声明了“可为 null 的 int”，并将其初始化为 。 然后将值设置为 `5`。 该示例通过“可为 null 的字符串”演示了同一概念。 有关详细信息，请参阅[可为 null 的值类型](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/nullable-value-types)和[可为 null 的引用类型](https://docs.microsoft.com/zh-cn/dotnet/csharp/nullable-references)。

```csharp
int? optionalInt = default; 
optionalInt = 5;
string? optionalText = default;
optionalText = "Hello World.";
```

### 元组

C# 支持[元组](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/value-tuples)，后者提供了简洁的语法来将多个数据元素分组成一个轻型数据结构。 通过声明 `(` 和 `)` 之间的成员的类型和名称来实例化元组，如下例所示：

```csharp
(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
//Output:
//Sum of 3 elements is 4.5.
```

### 成员

`class` 的成员要么是静态成员，要么是实例成员`class`。 静态成员属于类，而实例成员则属于对象（类实例）。

以下列表概述了类可以包含的成员类型。

- **常量**：与类相关联的常量值
- 字段：与类关联的变量
- 方法：类可执行的操作
- **属性**：与读取和写入类的已命名属性相关联的操作
- **索引器**：与将类实例编入索引（像处理数组一样）相关联的操作
- **事件**：类可以生成的通知
- **运算符**：类支持的转换和表达式运算符
- **构造函数**：初始化类实例或类本身所需的操作
- 终结器：永久放弃类的实例之前完成的操作
- **类型**：类声明的嵌套类型

#### 辅助功能

每个类成员都有关联的可访问性，用于控制能够访问成员的程序文本区域。 可访问性有六种可能的形式。 以下内容对访问修饰符进行了汇总。

- `public`：访问不受限制。
- `private`：访问仅限于此类。
- `protected`：访问仅限于此类或派生自此类的类。
- `internal`：仅可访问当前程序集（`.exe` 或 `.dll`）。
- `protected internal`：仅可访问此类、从此类中派生的类，或者同一程序集中的类。
- `private protected`：仅可访问此类或同一程序集中从此类中派生的类。

**示例**

```csharp
public class Color
{
    public static readonly Color Black = new(0, 0, 0);
    public static readonly Color White = new(255, 255, 255);
    public static readonly Color Red = new(255, 0, 0);
    public static readonly Color Green = new(0, 255, 0);
    public static readonly Color Blue = new(0, 0, 255);
    
    public byte R;
    public byte G;
    public byte B;

    public Color(byte r, byte g, byte b)
    {
        R = r;
        G = g;
        B = b;
    }
}

// 当方法主体是单个表达式时，可使用紧凑表达式格式定义方法，如下例中所示：
public override string ToString() => "This is an object";


// 引用参数用于按引用传递自变量。 
static void Swap(ref int x, ref int y)
{
    int temp = x;
    x = y;
    y = temp;
}

public static void SwapExample()
{
    int i = 1, j = 2;
    Swap(ref i, ref j);
    Console.WriteLine($"{i} {j}");    // "2 1"
}

// 输出参数用于按引用传递自变量。 输出参数与引用参数类似，不同之处在于，不要求向调用方提供的自变量显式赋值。 输出参数使用 out 修饰符进行声明。
static void Divide(int x, int y, out int result, out int remainder)
{
    result = x / y;
    remainder = x % y;
}

public static void OutUsage()
{
    Divide(10, 3, out int res, out int rem);
    Console.WriteLine($"{res} {rem}");	// "3 1"
}



public class Console
{
    public static void Write(string fmt, params object[] args) { }
    public static void WriteLine(string fmt, params object[] args) { }
    // ...
}



int x, y, z;
x = 3;
y = 4;
z = 5;
Console.WriteLine("x={0} y={1} z={2}", x, y, z);




int x = 3, y = 4, z = 5;

string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);

```

