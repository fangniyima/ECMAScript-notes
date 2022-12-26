## 6.2 ECMAScript Specification Types(ECMAScript规范类型)

规范类型对应于算法中用于描述ECMAScript语言构造和ECMAScript语言类型语义的元值。规范类型包括引用(Reference)、列表(List)、完成记录(Completion Record)、属性描述符(Property Descriptor)、环境记录(Environment Record)、抽象闭包(Abstract Closure)和数据块(Data Block)。规范类型值是不一定对应于ECMAScript实现中任何特定实体的规范产物。规范类型值可用于描述ECMAScript表达式计算的中间结果，但这些值不能作为对象的属性或ECMAScript语言变量的值存储。

### 6.2.1 The Enum Specification Type

枚举是规范内部的值，不能从ECMAScript代码中直接观察到。枚举使用无衬线字体表示。例如，Completion Record的[[Type]]字段采用normal、return或throw等值。枚举除了名称之外没有其他特征。枚举的名称除了用于将其与其他枚举区分之外，没有任何其他用途，也不暗示其在上下文中的用法或含义。

### 6.2.2 列表和记录规范类型

List类型用于解释在new表达式，函数调用以及其他需要简单的值列表的算法中对参数列表(请参见13.3.8)的求值。 List类型的值是包含各个值的list元素的简单排序序列。 这些序列可以是任何长度。 列表的元素可以使用0起点索引随机访问。 为了符号上的方便，可以使用类似数组的语法来访问List元素。 例如，arguments [2]是表示List参数的第3个元素的简写。

当算法在没有指定顺序的情况下迭代List的元素时，使用的顺序是List中元素的顺序。

为方便起见，在本规范中，可以使用文字语法来表示新的List值。例如，«1，2»定义具有两个元素的List值，每个元素都被初始化为特定值。一个新的空列表可以表示为«»。

在本规范中，短语“A、B……的列表串联”（其中每个参数可能是空列表）表示一个新的列表值，其元素是每个参数（顺序）的元素（顺序）的串联。

记录类型用于描述本规范算法内的数据聚合。记录类型值包含一个或多个命名字段。每个字段的值可以是ECMAScript值，也可以是由与Record类型关联的名称表示的抽象值。字段名称始终用双括号括起来，例如[[Value]]。

为方便起见，在本规范中，可以使用类似于对象字面量的语法来表示Record值。例如，{[[Field1]]：42，[[Field2]]：false，[[Field3]]：empty}定义了一个Record值，该值具有三个字段，每个字段都被初始化为特定值。字段名称顺序不重要。没有明确列出的任何字段都被视为不存在。

在规范文本和算法中，点符号可用于指代Record值的特定字段。例如，如果R是上一段中显示的记录，则R.[[Field2]]是" R的名为[[Field2]]的字段"的简写。

可以命名常用记录字段组合的模式，并且该名称可以用作文字记录值的前缀，以标识所描述的特定类型的聚合。例如：PropertyDescriptor {[[Value]]：42，[[Writable]]：false，[[Configurable]]：true}。

### 6.2.2集合和关系规范类型

Set类型用于解释在内存模型中使用的无序元素的集合。 Set类型的值是元素的简单集合，其中没有元素出现一次以上。 元素可以添加到集合中或从集合中删除。 集合可以合并，相交或彼此减去。

关系类型用于解释对集合的约束。 关系类型的值是其值域中的有序值对的集合。 例如，事件关联是一组有序的事件对。 对于一个关系R以及R的值域中的两个值a和b，a R b是表示有序对(a，b)是R的成员的简写。 当关系是满足这些条件的最小关系时，它相对于某些条件而言最少。

严格的偏序是满足以下条件的关系值R。

- For all a, b, and c in R's domain:
  - It is not the case that a R a, and
  - If a R b and b R c, then a R c.


>NOTE1 上面的两个属性分别称为不可反射性和可传递性。


    拓展：
    
    偏序集合(英语：Partiallyordered set，简写poset)是数学中，特别是序理论中，指配备了部分排序关系的集合。 这个理论将排序、顺序或排列这个集合的元素的直觉概念抽象化。

    严格偏序，反自反偏序
        给定集合S，"<"是S上的二元关系，若"<"满足：
        反自反性：∀a∈S，有a≮a；
        非对称性：∀a，b∈S，a<b ⇒ b≮a；
        传递性：∀a，b，c∈S，a<b且b<c，则a<c；
        则称"<"是S上的严格偏序或反自反偏序。

    非严格偏序，自反偏序
        给定集合S，"≤"是S上的二元关系，若"≤"满足：
        自反性：∀a∈S，有a≤a；
        反对称性：∀a，b∈S，a≤b且b≤a，则a=b；
        传递性：∀a，b，c∈S，a≤b且b≤c，则a≤c；
        则称"≤"是S上的非严格偏序或自反偏序。
    

严格全序是满足以下条件的关系值R。

- For all a, b, and c in R's domain:

  - a is identical to b or a R b or b R a, and
  - It is not the case that a R a, and
  - If a R b and b R c, then a R c.


>NOTE2 上面的三个属性分别称为总体性、不可逆性和及物性。

    拓展：
        
    严格全序关系(strictly totally ordered rela-tion)亦称严格线性序关系、严格有序关系一种重要的全序关系.指集合A上的不对称的、传递的、弱连通的二元关系R. A称为严格全序集.例如，整数集上的小于关系<就是一个严格全序的.

    这部分知识不属于本规范内容，了解更多请查阅相关资料

### 6.2.4 The Completion Record Specification Type(完成记录规范类型)

完成类型是一种记录，用于解释值和控制流的运行时传播，例如执行非本地控制转移的语句(break, continue, return and throw)的行为。

"完成"类型的值是"记录"值，其字段由表7定义。此类值称为"完成记录"。

<center>Table 7: Completion Record Fields</center>

|Field Name     |Value                                              |Meaning|
|--|--|--|
|[[Type]]	    |One of normal, break, continue, return, or throw   |发生的完成类型|
|[[Value]]	    |any ECMAScript language value or empty	            |产生的值
[[Target]]      |any ECMAScript string or empty                     |定向控制转移的目标标签

术语"突然完成"是指具有[[Type]]值而不是正常值的任何完成。

以下的速记术语有时被用来指代完成记录。

- normal completion refers to any Completion Record with a [[Type]] value of normal.
- break completion refers to any Completion Record with a [[Type]] value of break.
- continue completion refers to any Completion Record with a [[Type]] value of continue.
- return completion refers to any Completion Record with a [[Type]] value of return.
- throw completion refers to any Completion Record with a [[Type]] value of throw.
- abrupt completion refers to any Completion Record with a [[Type]] value other than normal.
- a normal completion containing some type of value refers to a normal completion that has a value of that type in its [[Value]] field.


#### 6.2.4.1 NormalCompletion ( value )

抽象操作NormalCompletion接受参数value（除了Completion Record之外的任何值）并返回正常完成。调用时执行以下步骤：

1. Return Completion Record { [[Type]]: normal, [[Value]]: value, [[Target]]: empty }.

#### 6.2.4.2 ThrowCompletion ( value )

抽象操作ThrowCompletion接受参数value（ECMAScript语言值）并返回抛出完成。调用时执行以下步骤：

1. Return Completion Record { [[Type]]: throw, [[Value]]: value, [[Target]]: empty }.

#### 6.2.4.3 UpdateEmpty ( completionRecord, value )

抽象操作UpdateEmpty接受参数completionRecord（一个完成记录）和value（除完成记录外的任何值）并返回一个完成记录。它在被调用时执行以下步骤。

1. 断言：如果completeRecord.[[[Type]]是return或throw，则completeRecord.[[Value]]不为空。
2. 如果completionRecord.[[Value]]不为空，则返回? completionRecord.
3. 返回完成{[[[Type]]：completeRecord.[[Type]]，[[Value]]：value，[[Target]]：completionRecord.[[Target]]}。


### 6.2.5 The Reference Record Specification Type (引用记录规范类型)

引用记录类型用于解释诸如删除，typeof，赋值运算符，super关键字和其他语言功能之类的运算符的行为。 例如，赋值的左操作数应产生引用记录。

引用记录是解析的名称或属性绑定； 其字段由表8定义。

<center>Table 8: Reference Record Fields</center>

| Field Name   | Value   | Meaning |
|  ----  | ----  | ----  |
| [[Base]]	|一个ECMAScript语言值，一个环境记录，或unresolvable| 持有该绑定的值或环境记录。一个[[Base]]的unresolvable表示该绑定不能被解决。|
|[[ReferencedName]]	|String or Symbol	|绑定的名称。 如果[[Base]]值是环境记录，则始终为字符串。|
|[[Strict]]	|Boolean |	如果引用记录源自严格模式代码，则为true，否则为false。|
|[[ThisValue]]|	any ECMAScript language value or empty|如果不为空，则引用记录表示使用super关键字表示的属性绑定。 它称为super引用记录，并且其[[Base]]值永远不会是环境记录。 在这种情况下，[[ThisValue]]字段将在创建引用记录时保留此值。|

本规范中使用以下抽象操作对引用进行操作：

#### 6.2.5.1 IsPropertyReference ( V )

抽象操作IsPropertyReference采用参数V (引用记录)。并返回一个布尔值。它在被调用时执行以下步骤。

1. 如果V.[Base]]是unresolvable，则返回false。
2. 如果V.[Base]]是环境记录返回false,否则返回true


#### 6.2.5.2 IsUnresolvableReference(V)(不可解析引用)

抽象操作IsUnsolvableReference接受参数V（引用记录）并返回布尔值。调用时执行以下步骤：

1. If V.[[Base]] is unresolvable, return true; otherwise return false.


#### 6.2.5.3 IsSuperReference(V)

抽象操作IsSuperReference接受参数V（引用记录）并返回布尔值。调用时执行以下步骤：

1. If V.[[ThisValue]] is not empty, return true; otherwise return false.

#### 6.2.5.4 IsPrivateReference ( V )

抽象操作IsPrivateReference接受参数V（引用记录）并返回布尔值。调用时执行以下步骤：

1. If V.[[ReferencedName]] is a Private Name, return true; otherwise return false.

#### 6.2.5.5 GetValue(V)

抽象操作GetValue接受参数V（引用记录或ECMAScript语言值），并返回包含ECMAScript语言值的正常完成或突然完成。调用时执行以下步骤：

1. If V is not a Reference Record, return V.
2. If IsUnresolvableReference(V) is true, throw a ReferenceError exception.
3. If IsPropertyReference(V) is true, then
    - Let baseObj be ? ToObject(V.[[Base]]).
    - If IsPrivateReference(V) is true, then
        1. Return ? PrivateGet(baseObj, V.[[ReferencedName]]).
    - Return ? baseObj.[[Get]](V.[[ReferencedName]], GetThisValue(V)).
4. Else,
    - Let base be V.[[Base]].
    - Assert: base is an Environment Record.
    - Return ? base.GetBindingValue(V.[[ReferencedName]], V.[[Strict]]) (see 9.1).


1. 如果V不是引用记录，则返回V。
2. 如果IsUnresolvableReference(V)为true，则引发ReferenceError异常。
3. 如果IsPropertyReference(V)为true，则
    1. 令baseObj为！ ToObject(V.[[Base]])。
    2. 返回？ baseObj.[[Get]](V.[[ReferencedName]]，GetThisValue(V))。
4. 否则，
    1. 令base为V。[[Base]]。
    2. 断言：base是一个环境记录。
    3. 返回？ base.GetBindingValue(V.[[ReferencedName]], V.[[Strict]]) (see 9.1).

>NOTE 步骤 3.a 中可能创建的对象在上述抽象操作和普通对象 [[Get]] 内部方法之外是不可访问的。实现可能会选择避免实际创建对象。

#### 6.2.5.6 PutValue(V，W)

抽象操作PutValue接受参数V（引用记录或ECMAScript语言值）和W（ECMAScript语言值），并返回包含未使用的正常完成或突然完成。调用时执行以下步骤：

1. If V is not a Reference Record, throw a ReferenceError exception.
2. If IsUnresolvableReference(V) is true, then
    - If V.[[Strict]] is true, throw a ReferenceError exception.
    - Let globalObj be GetGlobalObject().
    - Perform ? Set(globalObj, V.[[ReferencedName]], W, false).
    - Return unused.
3. If IsPropertyReference(V) is true, then
    - Let baseObj be ? ToObject(V.[[Base]]).
    - If IsPrivateReference(V) is true, then
        1. Return ? PrivateSet(baseObj, V.[[ReferencedName]], W).
    - Let succeeded be ? baseObj.[[Set]](V.[[ReferencedName]], W, GetThisValue(V)).
    - If succeeded is false and V.[[Strict]] is true, throw a TypeError exception.
    - Return unused.
4. Else,
    - Let base be V.[[Base]].
    - Assert: base is an Environment Record.
    - Return ? base.SetMutableBinding(V.[[ReferencedName]], W, V.[[Strict]]) (see 9.1).

说明

1. 如果V不是引用记录，则引发ReferenceError异常
2. 如果IsUnresolvableReference(V)是true，那么
    1. 如果V.[[Strict]]是true,则引发ReferenceError异常
    2. 让globalObj为GetGlobalObject()<sup>1</sup>
    3. 执行Set(globalObj, V.[[ReferencedName]], W, false)
    4. 返回unused
3. 如果IsPropertyReference(V)为true,那么
    1. 令baseObj为? ToObject(V.[[Base]]).
    2. 如果IsPrivateReference(V)为true
        - 返回? PrivateSet(baseObj, V.[[ReferencedName]], W).
        令succeeded为 ? baseObj.[[Set]](V.[[ReferencedName]], W, GetThisValue(V)).
    - 如果succeeded是false and V.[[Strict]] is true, throw a TypeError exception.
    - 返回unused.
4. Else,
    - Let base be V.[[Base]].
    - 断言：base是一个环境记录
    - 返回? base.SetMutableBinding(V.[[ReferencedName]], W, V.[[Strict]]) (see 9.1).

>GetGlobalObject()返回当前运行的执行上下文使用的全局对象

>NOTE 在上述抽象操作和普通对象[[Set]]内部方法之外，无法访问可能在步骤3.a中创建的对象。 一个实现可能选择避免实际创建该对象。

#### 6.2.5.7 GetThisValue(V)

抽象操作GetThisValue接受参数V（引用记录）并返回ECMAScript语言值。调用时执行以下步骤：

1. Assert: IsPropertyReference(V) is true.
2. If IsSuperReference(V) is true, return V.[[ThisValue]]; otherwise return V.[[Base]].

#### 6.2.5.8 InitializeReferencedBinding(V，W)

抽象操作 InitializeReferencedBinding 采用参数 V（参考记录）和 W（ECMAScript 语言值）并返回包含未使用的正常完成或突然完成。 它在调用时执行以下步骤：

1. Assert: IsUnresolvableReference(V) is false.
2. Let base be V.[[Base]].
3. Assert: base is an Environment Record.
4. Return ? base.InitializeBinding(V.[[ReferencedName]], W).

#### 6.2.5.9 MakePrivateReference ( baseValue, privateIdentifier )

抽象操作MakePrivateReference接受参数baseValue（ECMAScript语言值）和privateIdentifier（字符串），并返回一个Reference Record。调用时执行以下步骤：

1. Let privEnv be the running execution context's PrivateEnvironment.
2. Assert: privEnv is not null.
3. Let privateName be ResolvePrivateIdentifier(privEnv, privateIdentifier).
4. Return the Reference Record { [[Base]]: baseValue, [[ReferencedName]]: privateName, [[Strict]]: true, [[ThisValue]]: empty }.

### 6.2.6属性描述符规范类型

属性描述符类型用于解释对象属性property的操作和具体化。属性描述符类型的值是记录。每个字段的名称是一个属性名，其值是6.1.7.1中指定的相应属性值。此外，任何字段都可以存在或不存在。此规范中用于标记属性描述符记录的文本描述的架构名称为"PropertyDescriptor"。

属性描述符类型用于解释Object属性属性的操作和具体化。，属性描述符值可以进一步分类为数据属性描述符和访问器属性描述符。数据属性描述符包含名为[[Value]]或[[Writable]]的任何字段。访问器属性描述符包含名为[[Get]]或[[Set]]的任何字段。任何属性描述符都可以有名为[[Enumerable]]和[[Configurable]]的字段。属性描述符值不能同时是数据属性描述符和访问器属性描述符；然而，它可能两者都不是（在这种情况下，它是一个通用属性描述符）。完全填充的属性描述符是一个访问器属性描述符或数据属性描述符，并且具有表3中定义的所有对应字段。

本规范中使用以下抽象操作对属性描述符值进行操作：

#### 6.2.6.1 IsAccessorDescriptor(Desc)

抽象操作IsAccessorDescriptor采用参数Desc(a Property Descriptor or undefined)。它在调用时执行以下步骤：

1. If Desc is undefined, return false.
2. If Desc has a [[Get]] field, return true.
3. If Desc has a [[Set]] field, return true.
4. Return false.

#### 6.2.6.2 IsDataDescriptor(Desc)

抽象操作IsDataDescriptor带有参数Desc(a Property Descriptor or undefined)。 调用时，它将执行以下步骤：

1. If Desc is undefined, return false.
2. If Desc has a [[Value]] field, return true.
3. If Desc has a [[Writable]] field, return true.
4. Return false.

#### 6.2.6.3 IsGenericDescriptor(Desc)

抽象操作IsGenericDescriptor带有参数Desc(a Property Descriptor or undefined)。 调用时，它将执行以下步骤：

1. If Desc is undefined, return false.
2. If IsAccessorDescriptor(Desc) is true, return false.
3. If IsDataDescriptor(Desc) is true, return false.
4. Return true.


#### 6.2.6.4 FromPropertyDescriptor(Desc)

抽象操作FromPropertyDescriptor采用参数Desc(a Property Descriptor or undefined)。调用时，它将执行以下步骤：

1. If Desc is undefined, return undefined.
2. Let obj be ! OrdinaryObjectCreate(%Object.prototype%).
3. Assert: obj is an extensible ordinary object with no own properties.
4. If Desc has a [[Value]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "value", Desc.[[Value]]).
5. If Desc has a [[Writable]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "writable", Desc.[[Writable]]).
6. If Desc has a [[Get]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "get", Desc.[[Get]]).
7. If Desc has a [[Set]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "set", Desc.[[Set]]).
8. If Desc has an [[Enumerable]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "enumerable", Desc.[[Enumerable]]).
9. If Desc has a [[Configurable]] field, then
    1. Perform ! CreateDataPropertyOrThrow(obj, "configurable", Desc.[[Configurable]]).
10. Return obj.

说明:

1. 如果Desc是undefined,返回undefined
2. 让obj为OrdinaryObjectCreate(%Object.prototype%)
3. 断言obj是一个可拓展且不具有自身属性的普通对象
4. 然后将Desc存在的字段设置到obj并返回

Object.getOwnPropertyDescriptor ( O, P )方法用到了该抽象操作

#### 6.2.6.5 ToPropertyDescriptor ( Obj )

抽象操作ToPropertyDescriptor带有参数Obj。 调用时，它将执行以下步骤：


1. If Type(Obj) is not Object, throw a TypeError exception.
2. Let desc be a new Property Descriptor that initially has no fields.
3. Let hasEnumerable be ? HasProperty(Obj, "enumerable").
4. If hasEnumerable is true, then
    1. Let enumerable be ! ToBoolean(? Get(Obj, "enumerable")).
    2. Set desc.[[Enumerable]] to enumerable.
5. Let hasConfigurable be ? HasProperty(Obj, "configurable").
6. If hasConfigurable is true, then
    1. Let configurable be ! ToBoolean(? Get(Obj, "configurable")).
    2. Set desc.[[Configurable]] to configurable.
7. Let hasValue be ? HasProperty(Obj, "value").
8. If hasValue is true, then
    1. Let value be ? Get(Obj, "value").
    2. Set desc.[[Value]] to value.
9. Let hasWritable be ? HasProperty(Obj, "writable").
10. If hasWritable is true, then
    1. Let writable be ! ToBoolean(? Get(Obj, "writable")).
    2. Set desc.[[Writable]] to writable.
11. Let hasGet be ? HasProperty(Obj, "get").
12. If hasGet is true, then
    1. Let getter be ? Get(Obj, "get").
    2. If IsCallable(getter) is false and getter is not undefined, throw a TypeError exception.
    3. Set desc.[[Get]] to getter.
13. Let hasSet be ? HasProperty(Obj, "set").
14. If hasSet is true, then
    1. Let setter be ? Get(Obj, "set").
    2. If IsCallable(setter) is false and setter is not undefined, throw a TypeError exception.
    3. Set desc.[[Set]] to setter.
15. If desc.[[Get]] is present or desc.[[Set]] is present, then
    If desc.[[Value]] is present or desc.[[Writable]] is present, throw a TypeError exception.
16. Return desc.


说明：
1. 如果Obj不是对象，则引发TypeError异常
2. 让desc成为一个新的属性描述符，初始没有字段
3. 3-10设置检查并对象的Enumerable，Configurable，Value，Writable四个属性
4. 当get和set其中一个存在但不具有[[Call]]且另一个未定义，引发TypeError异常
5. 14保证了要么是数据属性描述符要么是访问器属性描述符

Object.defineProperties会用到此方法

#### 6.2.6.6 CompletePropertyDescriptor ( Desc )

抽象操作CompletePropertyDescriptor采用参数Desc(属性描述符)。它在调用时执行以下步骤：

1. Assert: Desc is a Property Descriptor.
2. Let like be the Record { [[Value]]: undefined, [[Writable]]: false, [[Get]]: undefined, [[Set]]: undefined, [[Enumerable]]: false, [[Configurable]]: false }.
3. If IsGenericDescriptor(Desc) is true or IsDataDescriptor(Desc) is true, then
    1. If Desc does not have a [[Value]] field, set Desc.[[Value]] to like.[[Value]].
    2. If Desc does not have a [[Writable]] field, set Desc.[[Writable]] to like.[[Writable]].
4. Else,
    1. If Desc does not have a [[Get]] field, set Desc.[[Get]] to like.[[Get]].
    3. If Desc does not have a [[Set]] field, set Desc.[[Set]] to like.[[Set]].
    3. If Desc does not have an [[Enumerable]] field, set Desc.[[Enumerable]] to like.[[Enumerable]].
5. If Desc does not have a [[Configurable]] field, set Desc.[[Configurable]] to like.[[Configurable]].
6. Return Desc.

### 6.2.7环境记录规范类型

环境记录类型用于解释嵌套函数和块中名称解析的行为。 此类型及其操作在9.1中定义。

### 6.2.8 抽象闭包规范类型

抽象闭包规范类型用于与值的集合一起引用算法步骤。 抽象闭包是元值，并使用函数应用程序样式(如closure(arg1，arg2))调用。 像抽象操作一样，调用执行抽象闭包描述的算法步骤。

在创建抽象闭包的算法步骤中，使用动词"capture"捕获值，后跟别名列表。 创建抽象闭包后，它将捕获当时与每个别名关联的值。 在指定调用Abstract Closure时要执行的算法的步骤中，每个捕获的值均由用于捕获该值的别名引用。

如果抽象闭包返回完成记录，则该完成记录的[[Type]]必须为normal或throw。

抽象闭包是作为其他算法的一部分内联创建的，如以下示例所示。

1. Let addend be 41.
2. Let closure be a new Abstract Closure with parameters (x) that captures addend and performs the following steps when called:
    1. Return x + addend.
3. Let val be closure(1).
4. Assert: val is 42.

### 6.2.9数据块

数据块规范类型用于描述字节大小(8位)数值的不同和可变序列。字节值是0到255之间的整数值(包括0到255)。数据块值是用固定字节数创建的，每个字节的初始值为0。

为了方便本规范中的注释，可以使用类似数组的语法来访问数据块值的各个字节。这种表示法将数据块值表示为0原始整数索引字节序列。例如，如果db是一个5字节的数据块值，那么db[2]可以用来访问它的第3个字节。

驻留在内存中且可以从多个代理并发引用的数据块被指定为共享数据块。共享数据块有一个没有地址的标识(出于对共享数据块值进行相等性测试的目的)：它不与块在任何进程中映射到的虚拟地址相关联，而是与块表示的内存中的一组位置相关联。只有当两个数据块包含的位置集相等时，两个数据块才相等；否则，它们就不相等，并且它们包含的位置集的交集为空。最后，可以将共享数据块和数据块区分开。

共享数据块的语义由内存模型使用共享数据块事件定义。下面的抽象操作引入共享数据块事件，并充当内存模型的评估语义和事件语义之间的接口。事件形成一个候选执行，内存模型在其中充当过滤器。请参考内存模型以获得完整的语义。

共享数据块事件由内存模型中定义的记录建模。

本规范使用以下抽象操作对数据块值进行操作：

#### 6.2.9.1 CreateByteDataBlock ( size )

抽象操作CreateByteDataBlock采用参数大小（非负整数），并返回包含数据块的正常完成或抛出完成。调用时执行以下步骤：

1. Let db be a new Data Block value consisting of size bytes. If it is impossible to create such a Data Block, throw a RangeError exception.
2. Set all of the bytes of db to 0.
3. Return db.

#### 6.2.9.2 CreateSharedByteDataBlock ( size )

抽象操作CreateSharedByteDataBlock采用参数大小（非负整数），并返回包含共享数据块的正常完成或抛出完成。调用时执行以下步骤：

1. Assert: size ≥ 0.
2. Let db be a new Shared Data Block value consisting of size bytes. If it is impossible to create such a Shared Data Block, throw a RangeError exception.
3.Let execution be the [[CandidateExecution]] field of the surrounding agent's Agent Record.
4. Let eventList be the [[EventList]] field of the element in execution.[[EventsRecords]] whose [[AgentSignifier]] is AgentSignifier().
5. Let zero be « 0 ».
6. For each index i of db, do
    7. Append WriteSharedMemory { [[Order]]: Init, [[NoTear]]: true, [[Block]]: db, [[ByteIndex]]: i, [[ElementSize]]: 1, [[Payload]]: zero } to eventList.
8. Return db.


#### 6.2.9.3 CopyDataBlockBytes ( toBlock, toIndex, fromBlock, fromIndex, count )

抽象操作CopyDataBlockBytes接受toBlock、toIndex(非负整数)、fromBlock、fromIndex(非负整数)和count(非负整数)。它在调用时执行以下步骤：

1. Assert: fromBlock and toBlock are distinct Data Block or Shared Data Block values.
2. Let fromSize be the number of bytes in fromBlock.
3. Assert: fromIndex + count ≤ fromSize.
4. Let toSize be the number of bytes in toBlock.
5. Assert: toIndex + count ≤ toSize.
6. Repeat, while count > 0,
    1. If fromBlock is a Shared Data Block, then
        1. Let execution be the [[CandidateExecution]] field of the surrounding agent's Agent Record.
        2. Let eventList be the [[EventList]] field of the element in execution.[[EventsRecords]] whose [[AgentSignifier]] is AgentSignifier().
        3. Let bytes be a List whose sole element is a nondeterministically chosen byte value.
        4. NOTE: In implementations, bytes is the result of a non-atomic read instruction on the underlying hardware. The nondeterminism is a semantic prescription of the memory model to describe observable behaviour of hardware with weak consistency.
        5. Let readEvent be ReadSharedMemory { [[Order]]: Unordered, [[NoTear]]: true, [[Block]]: fromBlock, [[ByteIndex]]: fromIndex, [[ElementSize]]: 1 }.
        6. Append readEvent to eventList.
        7. Append Chosen Value Record { [[Event]]: readEvent, [[ChosenValue]]: bytes } to execution.[[ChosenValues]].
        8. If toBlock is a Shared Data Block, then
            1. Append WriteSharedMemory { [[Order]]: Unordered, [[NoTear]]: true, [[Block]]: toBlock, [[ByteIndex]]: toIndex, [[ElementSize]]: 1, [[Payload]]: bytes } to eventList.
        9. Else,
            1. Set toBlock[toIndex] to bytes[0].
    2. Else,
        1. Assert: toBlock is not a Shared Data Block.
        2. Set toBlock[toIndex] to fromBlock[fromIndex].
    3. Set toIndex to toIndex + 1.
    4. Set fromIndex to fromIndex + 1.
    5. Set count to count - 1.
7. Return NormalCompletion(empty).


说明：ArrayBuffer.prototype.slice使用该算法步骤，下面是一个具体的例子，具体在24章会有详细介绍

1. 断言:fromBlock和toBlock是不同的数据块或共享数据块值。
2. 令fromSize是fromBlock中的字节数。
3. 断言：fromIndex+count≤fromSize。
4. 令toSize为toBlock中的字节数。
5. 断言：toIndex+count≤toSize。
6. 重复，while count>0，

    const buffer = new ArrayBuffer(16);
    const int32View = new Int32Array(buffer);
    int32View[1] = 42;
    const sliced = new Int32Array(buffer.slice(4, 12));
    console.log(sliced[0]);

### 6.2.10 The PrivateElement Specification Type (PrivateElement 规范类型)

PrivateElement 类型是用于规范私有类字段、方法和访问器的 Record。 虽然属性描述符不用于私有元素，但私有字段的行为类似于不可配置、不可枚举、可写的数据属性，私有方法的行为类似于不可配置、不可枚举、不可写的数据属性，私有访问器的行为 类似于不可配置、不可枚举的访问器属性。

PrivateElement类型的值是记录值，其字段由表9定义。这些值被称为私有元素。

<center>Table 9: PrivateElement Fields</center>

|Field Name|Values of the [[Kind]] field for which it is present|Value|Meaning|
|--|--|--|--|
|[[Key]]|All|a Private Name|字段、方法或访问器的名称。|
|[[Kind]]|All|field, method, or accessor|元素的种类。|
|[[Value]]|field and method|an ECMAScript language value|字段的值|
|[[Get]]|accessor|a function object or undefined|私有访问器的getter|
|[[Set]]|accessor|a function object or undefined|私有访问器的setter|

### 6.2.11 The ClassFieldDefinition Record Specification Type ClassFieldDefinition 记录规范类型

ClassFieldDefinition 类型是在类字段规范中使用的 Record。

ClassFieldDefinition 类型的值是记录值，其字段由表 10 定义。这些值称为 ClassFieldDefinition Records。

<center>Table 10: ClassFieldDefinition Record Fields</center>

|Field Name|Value|Meaning|
|--|--|--|
|[[Name]]|a Private Name, a String, or a Symbol|字段名|
|[[Initializer]]|a function object or empty|字段的初始值设定项（如果有）。|

### 6.2.12 Private Names

私有名称规范类型用于描述表示私有类元素（字段、方法或访问器）的键的全局唯一值（与任何其他私有名称不同，即使它们在其他方面无法区分）。每个私有名称都有一个相关的不可变[[Description]]，它是一个字符串值。可以使用PrivateFieldAdd或PrivateMethodOrAccessorAdd在任何ECMAScript对象上安装Private Name，然后使用PrivateGet和PrivateSet进行读取或写入。

### 6.2.13 The ClassStaticBlockDefinition Record Specification Type

ClassStaticBlockDefinition Record 是一个 Record 值，用于封装类静态初始化块的可执行代码。

ClassStaticBlockDefinition 记录具有表 11 中列出的字段。

<center>Table 11: ClassStaticBlockDefinition Record Fields</center>

|Field Name|Value|Meaning|
|--|--|--|
|[[BodyFunction]]|一个函数对象|在类的静态初始化期间要调用的函数对象|
