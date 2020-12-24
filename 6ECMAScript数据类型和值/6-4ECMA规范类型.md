## 6.2 ECMAScript Specification Types(ECMAScript规范类型)

规范类型对应于算法中用于描述ECMAScript语言构造和ECMAScript语言类型语义的元值。规范类型包括引用(Reference)、列表(List)、完成(Completion)、属性描述符(Property Descriptor)、环境记录(Environment Record)、抽象闭包(Abstract Closure)和数据块(Data Block)。规范类型值是不一定对应于ECMAScript实现中任何特定实体的规范产物。规范类型值可用于描述ECMAScript表达式计算的中间结果，但这些值不能作为对象的属性或ECMAScript语言变量的值存储。

### 6.2.1 列表和记录规范类型

List类型用于解释在new表达式，函数调用以及其他需要简单的值列表的算法中对参数列表(请参见12.3.8)的求值。 List类型的值是包含各个值的list元素的简单排序序列。 这些序列可以是任何长度。 列表的元素可以使用0起点索引随机访问。 为了符号上的方便，可以使用类似数组的语法来访问List元素。 例如，arguments [2]是表示List参数的第3个元素的简写。
为方便起见，在本规范中，可以使用字面量语法来表示新的List值。例如，«1，2»定义具有两个元素的List值，每个元素都被初始化为特定值。一个新的空列表可以表示为«»。

记录类型用于描述本规范算法内的数据聚合。记录类型值包含一个或多个命名字段。每个字段的值可以是ECMAScript值，也可以是由与Record类型关联的名称表示的抽象值。字段名称始终用双括号括起来，例如[[Value]]。

为方便起见，在本规范中，可以使用类似于对象字面量的语法来表示Record值。例如，{[[Field1]]：42，[[Field2]]：false，[[Field3]]：empty}定义了一个Record值，该值具有三个字段，每个字段都被初始化为特定值。字段名称顺序不重要。没有明确列出的任何字段都被视为不存在。

在规范文本和算法中，点符号可用于指代Record值的特定字段。例如，如果R是上一段中显示的记录，则R.[[Field2]]是"R的名为[[Field2]]的字段"的简写。

可以命名常用记录字段组合的模式，并且该名称可以用作文字记录值的前缀，以标识所描述的特定类型的聚合。例如：PropertyDescriptor {[[Value]]：42，[[Writable]]：false，[[Configurable]]：true}。

### 6.2.2集合和关系规范类型

Set类型用于解释在内存模型中使用的无序元素的集合。 Set类型的值是元素的简单集合，其中没有元素出现一次以上。 元素可以添加到集合中或从集合中删除。 集合可以合并，相交或彼此减去。

Relation类型用于解释对集合的约束。 关系类型的值是其值域中的有序值对的集合。 例如，事件关联是一组有序的事件对。 对于一个关系R以及R的值域中的两个值a和b，a R b是表示有序对(a，b)是R的成员的简写。 当关系是满足这些条件的最小关系时，它相对于某些条件而言最少。

严格的偏序是满足以下条件的关系值R。
- For all a, b, and c in R's domain:
  - It is not the case that a R a, and
  - If a R b and b R c, then a R c.

<table><tr><td bgcolor=#E9FBE9 width=10%>
注1
上面的两个属性分别称为不可反射性和可传递性。
</td></tr></table>

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
    严格全序关系(strictly totally ordered rela-tion)亦称严格线性序关系、严格有序关系一种重要的全序关系.指集合A上的不对称的、传递的、弱连通的二元关系R. A称为严格全序集.例如，整数集上的小于关系<就是一个严格全序的.

    这部分知识不属于本规范内容，了解更多请查阅相关资料    
  
严格全序是满足以下条件的关系值R。

- For all a, b, and c in R's domain:

  - a is identical to b or a R b or b R a, and
  - It is not the case that a R a, and
  - If a R b and b R c, then a R c.

<table><tr><td bgcolor=#E9FBE9 width=10%>
NOTE2

上面的三个属性分别称为总体性、不可逆性和及物性。
</td></tr></table>

### 6.2.3 完成记录规范类型(The Completion Record Specification Type)

完成类型是一种记录，用于解释值和控制流的运行时传播，例如执行非本地控制转移的语句(break, continue, return and throw)的行为。

"完成"类型的值是"记录"值，其字段由表9定义。此类值称为"完成记录"。

<center>Table 9: Completion Record Fields</center>

|Field Name     |Value                                              |Meaning|
|--|--|--|
|[[Type]]	    |One of normal, break, continue, return, or throw   |发生的完成类型|
|[[Value]]	    |any ECMAScript language value or empty	            |产生的值
[[Target]]      |any ECMAScript string or empty                     |定向控制转移的目标标签

术语"突然完成"是指具有[[Type]]值而不是正常值的任何完成。

#### 6.2.3.1 Await

算法步骤

1.Let completion be Await(value).

意思与：

    1. Let asyncContext be the running execution context.
    2. Let promise be ? PromiseResolve(%Promise%, value).
    3. Let stepsFulfilled be the algorithm steps defined in Await Fulfilled Functions.
    4. Let onFulfilled be ! CreateBuiltinFunction(stepsFulfilled, « [[AsyncContext]] »).
    5. Set onFulfilled.[[AsyncContext]] to asyncContext.
    6. Let stepsRejected be the algorithm steps defined in Await Rejected Functions.
    7. Let onRejected be ! CreateBuiltinFunction(stepsRejected, « [[AsyncContext]] »).
    8. Set onRejected.[[AsyncContext]] to asyncContext.
    9. Perform ! PerformPromiseThen(promise, onFulfilled, onRejected).
    10. Remove asyncContext from the execution context stack and restore the execution context that is at the top of the execution context stack as the running execution context.
    11. Set the code evaluation state of asyncContext such that when evaluation is resumed with a Completion completion, the following steps of the algorithm that invoked Await will be performed, with completion available.
    12. Return.
    13. NOTE: This returns to the evaluation of the operation that had most previously resumed evaluation of asyncContext.

其中，除完成之外，上述步骤中的所有别名都是短暂的，仅在与Await相关的步骤中可见。

    说明
    
    1. 令asyncContext为正在执行的上下文
    2. 令promise为? PromiseResolve(%Promise%, value)
    3. 令stepsFulfilled为Await Fulfilled函数中定义的算法步骤。
    4. 令onFulfilled为CreateBuiltinFunction(stepsFulfilled, « [[AsyncContext]] »)
    5. 将onFulfilled.[[AsyncContext]]的值设为asyncContext
    6. 令stepsRejected为Await Rejected函数中定义的算法步骤
    7. 令onRejected为CreateBuiltinFunction(stepsRejected, « [[AsyncContext]] »)
    8. 将onRejected.[[AsyncContext]]设置为asyncContext
    9. 执行! PerformPromiseThen(promise, onFulfilled, onRejected).
    10. 从执行上下文堆栈中删除asyncContext，并将位于执行上下文堆栈顶部的执行上下文恢复为正在运行的执行上下文
    11. 设置asyncContext的代码求值状态，以便在完成完成后继续求值时，将执行调用Await的算法的以下步骤，完成可用。
    12. 返回
    13. 注意：这将返回对先前已恢复asyncContext求值的操作的计算。

<table><tr><td bgcolor=#E9FBE9 width=10%>
NOTE

Await 可以和 ? and ! 组合使用, 例如

Let result be ? Await(value).

与以下内容含义相同：

Let result be Await(value).

ReturnIfAbrupt(result).
</td></tr></table>

await fulfilled函数的"length"属性为1<sub>𝔽</sub>。

##### 6.2.3.1.1 Await Fulfilled Functions

Await Fulfilled函数是一个匿名内置函数，用作"Await"规范方法的一部分，以将"承诺完成"值作为正常完成传递给调用方。 每个Await fulfilled函数都有一个[[AsyncContext]]内部插槽。

当使用参数value调用Await fulfilled函数时，将执行以下步骤：

1. Let F be the active function object.
2. Let asyncContext be F.[[AsyncContext]].
3. Let prevContext be the running execution context.
4. Suspend prevContext.
5. Push asyncContext onto the execution context stack; asyncContext is now  the running execution context.
6. Resume the suspended evaluation of asyncContext using NormalCompletion(value) as the result of the operation that suspended it.
7. Assert: When we reach this step, asyncContext has already been removed from the execution context stack and prevContext is the currently running execution context.
8. Return undefined.

Await-rejected函数的"length"属性为1<sub>𝔽</sub>。

说明:

1. 令F为活动函数对象
2. 令asyncContext为F.[[AsyncContext]]
3. 让prevContext成为正在运行的执行上下文
4. 挂起prevContext
5. 将asyncContext推送到执行上下文堆栈；asyncContext现在是正在运行的执行上下文
6. 使用NormalCompletion(value)作为暂停asyncContext的操作的结果，恢复其暂停的计算。
7. 当我们到达这个步骤时，asyncContext已经从执行上下文堆栈中移除，prevContext是当前正在运行的执行上下文
8. 返回undefined

##### 6.2.3.1.2 Await Rejected Functions

Await rejected函数是一个匿名的内置函数，用作Await规范方法的一部分，用于将promise拒绝原因作为一个突然抛出完成传递给调用方。每个await rejected函数都有一个[[AsyncContext]]内部槽。



当使用参数reason调用Await rejected函数时，将执行以下步骤：

    Let F be the active function object.
    Let asyncContext be F.[[AsyncContext]].
    Let prevContext be the running execution context.
    Suspend prevContext.
    Push asyncContext onto the execution context stack; asyncContext is now the running execution context.
    Resume the suspended evaluation of asyncContext using ThrowCompletion(reason) as the result of the operation that suspended it.
    Assert: When we reach this step, asyncContext has already been removed from the execution context stack and prevContext is the currently running execution context.
    Return undefined.


Await-rejected函数的"length"属性为1<sub>𝔽</sub>。

#### 6.2.3.2 NormalCompletion
例如一个带有一个抽象参数的完成：

1. Return NormalCompletion(argument).

是一种速记，定义如下：

1. Return Completion { [[Type]]: normal, [[Value]]: argument, [[Target]]: empty }.

#### 6.2.3.3 ThrowCompletion

通过单个参数完成的抽象操作，例如：

1. Return ThrowCompletion(argument).

是一种速记，定义如下：

1. Return Completion { [[Type]]: throw, [[Value]]: argument, [[Target]]: empty }.

#### 6.2.3.4 UpdateEmpty ( completionRecord, value )

抽象操作UpdateEmpty接受参数completionRecord和value。它在调用时执行以下步骤：

1. Assert: If completionRecord.[[Type]] is either return or throw, then completionRecord.[[Value]] is not empty.

2. If completionRecord.[[Value]] is not empty, return Completion(completionRecord).

3. Return Completion { [[Type]]: completionRecord.[[Type]], [[Value]]: value, [[Target]]: completionRecord.[[Target]] }.

说明：

1. 断言：如果completeRecord.[[[Type]]是return或throw，则completeRecord.[[Value]]不为空。

2. 如果completionRecord.[[Value]]不为空，则返回Completion(completionRecord)。

3. 返回完成{[[[Type]]：completeRecord.[[Type]]，[[Value]]：value，[[Target]]：completionRecord.[[Target]]}。


### 6.2.4 引用记录规范类型 (The Reference Record Specification Type)

引用记录类型用于解释诸如删除，typeof，赋值运算符，super关键字和其他语言功能之类的运算符的行为。 例如，赋值的左操作数应产生引用记录。

引用记录是解析的名称或属性绑定； 其字段由表10定义。
<center>Table 10: Reference Record Fields</center>

| Field Name   | Value   | Meaning |
|  ----  | ----  | ----  |
| [[Base]]	|其中之一：任何ECMAScript语言值，但undefined或null、环境记录或无法解析的值除外。| 包含绑定的值或环境记录。 [[Base]]不可解析表示无法解析绑定。|
|[[ReferencedName]]	|String or Symbol	|绑定的名称。 如果[[Base]]值是环境记录，则始终为字符串。|
|[[Strict]]	|Boolean |	如果引用记录源自严格模式代码，则为true，否则为false。|
|[[ThisValue]]|	any ECMAScript language value or empty|如果不为空，则引用记录表示使用super关键字表示的属性绑定。 它称为super引用记录，并且其[[Base]]值永远不会是环境记录。 在这种情况下，[[ThisValue]]字段将在创建引用记录时保留此值。|

本规范中使用以下抽象操作对引用进行操作：

#### 6.2.4.1 IsPropertyReference ( V )

抽象操作IsPropertyReference采用参数V。调用时它执行以下步骤：


1. Assert: V is a Reference Record.

2. If V.[[Base]] is unresolvable, return false.

3. If Type(V.[[Base]]) is Boolean, String, Symbol, BigInt, Number, or Object, return true; otherwise return false.

说明：

1. 断言：V是一个引用记录。

2. 如果V.[Base]]不可解析，则返回false。

3. 如果类型(V.[Base]])是Boolean、String、Symbol、BigInt、Number或Object，则返回true；否则返回false。


#### 6.2.4.2 IsUnresolvableReference(V)(不可解析引用)

抽象操作IsUnresolvableReference带有参数V。被调用时，它执行以下步骤：

1. Assert: V is a Reference Record.
2. If V.[[Base]] is unresolvable, return true; otherwise return false.

说明：

1. 断言:V是一个引用记录
2. 如果V.[[Base]],否则返回false


#### 6.2.4.3 IsSuperReference(V)

抽象操作IsSuperReference的参数为V。调用时，它将执行以下步骤：

1. Assert: V is a Reference Record.
2. If V.[[ThisValue]] is not empty, return true; otherwise return false.

说明：

1. 断言:V是一个引用记录
2. 如果V.[[ThisValue]]不是空返回true,否则返回false

#### 6.2.4.4 GetValue(V)

抽象操作GetValue接受参数V。被调用时，它执行以下步骤：

1. ReturnIfAbrupt(V).
2. If V is not a Reference Record, return V.
3. If IsUnresolvableReference(V) is true, throw a ReferenceError exception.
4. If IsPropertyReference(V) is true, then
    1. Let baseObj be ! ToObject(V.[[Base]]).
    2. Return ? baseObj.[[Get]](V.[[ReferencedName]], GetThisValue(V)).
5. Else,
    1. Let base be V.[[Base]].
    2. Assert: base is an Environment Record.
    3. Return ? base.GetBindingValue(V.[[ReferencedName]], V.[[Strict]]) (see 8.1).

1. ReturnIfAbrupt(V)。
2. 如果V不是引用记录，则返回V。
3. 如果IsUnresolvableReference(V)为true，则引发ReferenceError异常。
4. 如果IsPropertyReference(V)为true，则
    1. 令baseObj为！ ToObject(V.[[Base]])。
    2. 返回？ baseObj.[[Get]](V.[[ReferencedName]]，GetThisValue(V))。
5. 否则，
    1. 令base为V。[[Base]]。
    2. 断言：base是一个环境记录。
    3. 返回？ base..[[ReferencedName]]，V.[[Strict]])(请参阅8.1)。


#### 6.2.4.5 PutValue(V，W)
抽象操作PutValue接受参数V和W。调用时，它将执行以下步骤：

1. ReturnIfAbrupt(V).
2. ReturnIfAbrupt(W).
3. If V is not a Reference Record, throw a ReferenceError exception.
4.  If IsUnresolvableReference(V) is true, then
    1. If V.[[Strict]] is true, throw a ReferenceError exception.
    2. Let globalObj be GetGlobalObject().
    3. Return ? Set(globalObj, V.[[ReferencedName]], W, false).
5. If IsPropertyReference(V) is true, then
    1. Let baseObj be ! ToObject(V.[[Base]]).
    2. Let succeeded be ? baseObj.[[Set]](V.[[ReferencedName]], W, GetThisValue(V)).
    3. If succeeded is false and V.[[Strict]] is true, throw a TypeError exception.
    4. Return.
6.  Else,
    1. Let base be V.[[Base]].
    2. Assert: base is an Environment Record.
    3. Return ? base.SetMutableBinding(V.[[ReferencedName]], W, V.[[Strict]]) (see 8.1).

说明

1. ReturnIfAbrupt(V)
2. ReturnIfAbrupt(W)
3. 如果V不是引用记录，则引发ReferenceError异常
4. 如果IsUnresolvableReference(V)是true，那么
    1. 如果V.[[Strict]]是true,则引发ReferenceError异常
    2. 让globalObj为GetGlobalObject()<sup>1</sup>
    3. 返回Set(globalObj, V.[[ReferencedName]], W, false)<sup>2</sup>

1.GetGlobalObject()返回当前运行的执行上下文使用的全局对象
2.对象的set详见7.3.4

注意

在上述抽象操作和普通对象[[Set]]内部方法之外，无法访问可能在步骤5.a中创建的对象。 一个实现可能选择避免实际创建该对象。

#### 6.2.4.6 GetThisValue(V)
抽象操作GetThisValue接受参数V。被调用时，它执行以下步骤：

    1.Assert: IsPropertyReference(V) is true.
    2.If IsSuperReference(V) is true, return V.[[ThisValue]]; otherwise return V.[[Base]].

#### 6.2.4.7 InitializeReferencedBinding(V，W)
抽象操作InitializeReferencedBinding使用参数V和W。调用时，它将执行以下步骤：

    ReturnIfAbrupt(V).
    ReturnIfAbrupt(W).
    Assert: V is a Reference Record.
    Assert: IsUnresolvableReference(V) is false.
    Let base be V.[[Base]].
    Assert: base is an Environment Record.
    Return base.InitializeBinding(V.[[ReferencedName]], W).

### 6.2.5属性描述符规范类型

属性描述符类型用于解释对象属性property的操作和具体化。属性描述符类型的值是记录。每个字段的名称是一个属性名，其值是6.1.7.1中指定的相应属性值。此外，任何字段都可以存在或不存在。此规范中用于标记属性描述符记录的文本描述的架构名称为"PropertyDescriptor"。

基于某些字段的存在或使用，属性描述符值可以进一步分类为数据属性描述符和访问器属性描述符。数据属性描述符包含名为[[Value]]或[[Writable]]的任何字段。访问器属性描述符包含名为[[Get]]或[[Set]]的任何字段。任何属性描述符都可以有名为[[Enumerable]]和[[Configurable]]的字段。属性描述符值不能同时是数据属性描述符和访问器属性描述符；但是，也可能两者都不是。泛型属性描述符是既不是数据属性描述符也不是访问器属性描述符的属性描述符值。完全填充的属性描述符是一个访问器属性描述符或数据属性描述符，它具有与表3或表4中定义的属性属性相对应的所有字段。

本规范中使用以下抽象操作对属性描述符值进行操作：

#### 6.2.5.1 IsAccessorDescriptor(Desc)

抽象操作IsAccessorDescriptor采用参数Desc(a Property Descriptor or undefined)。它在调用时执行以下步骤：

1. If Desc is undefined, return false.
2. If both Desc.[[Get]] and Desc.[[Set]] are absent, return false.
3. Return true.

说明:

1. 如果Desc是undefined,返回false
2. 如果Desc.[[Get]]和Desc.[[Set]]都不存在,返回false
3. 返回true

#### 6.2.5.2 IsDataDescriptor(Desc)

抽象操作IsDataDescriptor带有参数Desc(a Property Descriptor or undefined)。 调用时，它将执行以下步骤：

    If Desc is undefined, return false.
    If both Desc.[[Value]] and Desc.[[Writable]] are absent, return false.
    Return true.

说明:

1. 如果Desc是undefined,返回false
2. 如果Desc.[[Value]]和Desc.[[Writable]]都不存在,返回false
3. 返回true

#### 6.2.5.3 IsGenericDescriptor(Desc)
抽象操作IsGenericDescriptor带有参数Desc(a Property Descriptor or undefined)。 调用时，它将执行以下步骤：

1. If Desc is undefined, return false.
2. If IsAccessorDescriptor(Desc) and IsDataDescriptor(Desc) are both false, return true.
3. Return false.

说明:

1. 如果Desc是undefined,返回false
2. 如果IsAccessorDescriptor(Desc)和IsAccessorDescriptor(Desc)都是假,返回true
3. 返回false

#### 6.2.5.4 FromPropertyDescriptor(Desc)

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
3. 断言obj是一个可拓展切不具有自身属性的普通对象
4. 然后将Desc存在的字段设置到obj并返回

Object.getOwnPropertyDescriptor ( O, P )方法用到了该抽象操作

#### 6.2.5.5 ToPropertyDescriptor ( Obj )

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
14. If desc.[[Get]] is present or desc.[[Set]] is present, then
    If desc.[[Value]] is present or desc.[[Writable]] is present, throw a TypeError exception.
15. Return desc.


说明：
1. 如果Obj不是对象，则引发TypeError异常
2. 让desc成为一个新的属性描述符，初始没有字段
3. 3-10设置检查并对象的Enumerable，Configurable，Value，Writable四个属性
4. 当get和set其中一个存在但不具有[[Call]]且另一个未定义，引发TypeError异常
5. 14保证了要么是数据属性描述符要么是访问器属性描述符

Object.defineProperties会用到此方法

#### 6.2.5.6 CompletePropertyDescriptor ( Desc )

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

### 6.2.6环境记录规范类型

环境记录类型用于解释嵌套函数和块中名称解析的行为。 此类型及其操作在8.1中定义。

### 6.2.7 抽象闭包规范类型

抽象闭包规范类型用于与值的集合一起引用算法步骤。 抽象闭包是元值，并使用函数应用程序样式(如closure(arg1，arg2))调用。 像抽象操作一样，调用执行抽象闭包描述的算法步骤。

在创建抽象闭包的算法步骤中，使用动词"capture"捕获值，后跟别名列表。 创建抽象闭包后，它将捕获当时与每个别名关联的值。 在指定调用Abstract Closure时要执行的算法的步骤中，每个捕获的值均由用于捕获该值的别名引用。

如果抽象闭包返回完成记录，则该完成记录的[[Type]]必须为normal或throw。

抽象闭包是作为其他算法的一部分内联创建的，如以下示例所示。

1. Let addend be 41.
2. Let closure be a new Abstract Closure with parameters (x) that captures addend and performs the following steps when called:
    1. Return x + addend.
3. Let val be closure(1).
4. Assert: val is 42.

### 6.2.8数据块

数据块规范类型用于描述字节大小(8位)数值的不同和可变序列。字节值是0到255之间的整数值(包括0到255)。数据块值是用固定字节数创建的，每个字节的初始值为0。

为了方便本规范中的注释，可以使用类似数组的语法来访问数据块值的各个字节。这种表示法将数据块值表示为0原始整数索引字节序列。例如，如果db是一个5字节的数据块值，那么db[2]可以用来访问它的第3个字节。

驻留在内存中且可以从多个代理并发引用的数据块被指定为共享数据块。共享数据块有一个没有地址的标识(出于对共享数据块值进行相等性测试的目的)：它不与块在任何进程中映射到的虚拟地址相关联，而是与块表示的内存中的一组位置相关联。只有当两个数据块包含的位置集相等时，两个数据块才相等；否则，它们就不相等，并且它们包含的位置集的交集为空。最后，可以将共享数据块和数据块区分开。

共享数据块的语义由内存模型使用共享数据块事件定义。下面的抽象操作引入共享数据块事件，并充当内存模型的评估语义和事件语义之间的接口。事件形成一个候选执行，内存模型在其中充当过滤器。请参考内存模型以获得完整的语义。

共享数据块事件由内存模型中定义的记录建模。

本规范使用以下抽象操作对数据块值进行操作：

#### 6.2.8.1 CreateByteDataBlock ( size )

抽象操作createBytedDataBlock采用参数大小(整数)。它在调用时执行以下步骤：

1. Assert: size ≥ 0.
2. Let db be a new Data Block value consisting of size bytes. If it is impossible to create such a Data Block, throw a RangeError exception.
3. Set all of the bytes of db to 0.
4. Return db.

说明：ArrayBuffer 对象用来表示通用的、固定长度的原始二进制数据缓冲区。它是一个字节数组，通常在其他语言中称为“byte array”

1. 断言：大小≥0。
2. 令db为由size字节组成的新数据块值。 如果不可能创建这样的数据块，则引发RangeError异常。
3. 将db的所有字节设置为0。
4. 返回db。



#### 6.2.8.2 CreateSharedByteDataBlock ( size )

抽象操作CreateSharedByteDataBlock采用参数大小(非负整数)。它在调用时执行以下步骤：

1. Assert: size ≥ 0.
2. Let db be a new Shared Data Block value consisting of size bytes. If it is impossible to create such a Shared Data Block, throw a RangeError exception.
3.Let execution be the [[CandidateExecution]] field of the surrounding agent's Agent Record.
4. Let eventList be the [[EventList]] field of the element in execution.[[EventsRecords]] whose [[AgentSignifier]] is AgentSignifier().
5. Let zero be « 0 ».
6. For each index i of db, do
    7. Append WriteSharedMemory { [[Order]]: Init, [[NoTear]]: true, [[Block]]: db, [[ByteIndex]]: i, [[ElementSize]]: 1, [[Payload]]: zero } to eventList.
8. Return db.

说明：

1. 断言：大小≥0。
2. 令db为由大小字节组成的新共享数据块值。 如果不可能创建这样的共享数据块，则引发RangeError异常。
3. 让执行成为周围业务代表的业务代表记录的[[CandidateExecution]]字段。
让执行为周围业务代表的业务代表记录的[[CandidateExecution]]字段。
4. 令eventList为正在执行的元素的[[EventList]]字段。[[EventsRecords]]的[[AgentSignifier]]是AgentSignifier（）。
5. 设zero为«0»。
6. 对于db的每个索引i，执行
    7.附加WriteSharedMemory {[[Order]]：Init，[[NoTear]]：true，[[Block]]：db，[[ByteIndex]]：i，[[ElementSize]]：1，[[Payload]]：0 }到eventList。
8. 返回db。

#### 6.2.8.3 CopyDataBlockBytes ( toBlock, toIndex, fromBlock, fromIndex, count )

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

