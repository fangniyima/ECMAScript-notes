# <center>6 ECMAScript数据类型和值</center>

## 概要

此规范中的算法操作的每个值都有关联类型。可能的值类型正是本节中定义的值类型。类型被进一步分为ECMAScript语言类型(language types)和规范类型(specification types)。

在本规范中，符号"Type(x)"被用作"typeof x"的简写，其中"Type"指的是ECMAScript语言和本节中定义的规范类型。当术语"empty"被用作命名值时，它相当于说"没有任何类型的值"。

## 6.1 ECMAScript语言类型(ECMAScript Language Types)

ECMAScript语言类型对应于ECMAScript程序员使用ECMAScript语言直接操作的值。ECMAScript语言类型包括Undefined、Null、Boolean、String、Symbol、Number、BigInt和Object。ECMAScript语言值是由ECMAScript语言类型特征的值。

## 6.1.1 Undefined类型

Undefined类型只有一个值，为undefined。任何未赋值的变量都是undefined。

## 6.1.2 Null类型

Null类型只有一个值，为null。

## 6.1.3 Boolean类型

Boolean类型表示具有两个值的逻辑实体，为true和false。

## 6.1.4 String字符串类型

字符串类型是由零个或多个16位无符号整数值('元素')组成的所有有序序列的集合，最大长度为2<sup>53</sup>-1个<sup>1</sup>。字符串类型通常用于表示正在运行的ECMAScript程序中的文本数据，在这种情况下，字符串中的每个元素都被视为UTF-16代码单元值。每个元素都被认为占据了序列中的一个位置。这些位置用非负整数索引。第一个元素(如果有)位于索引0，下一个元素(如果有)位于索引1，依此类推。字符串的长度是其中的元素数(即16位值)。空字符串的长度为零，因此不包含任何元素。

>注:不同引擎实现不同，可用如下方式测试'a'.repeat(2**27);

不解释String内容的ECMAScript操作不应用进一步的语义。解释字符串值的操作将每个元素视为单个UTF-16代码单元。但是，ECMAScript不限制这些代码单元的值或它们之间的关系，因此，将字符串内容进一步解释为以UTF-16编码的Unicode码位序列的操作必须考虑到格式错误的子序列。这样的操作对每个包含范围0xD800到0xDBFF(由Unicode标准定义为前导代理项，或更正式地定义为高代理项代码单元)的每个代码单元以及包含范围0xDC00到0xDFFF(定义为尾随代理项)的每个代码单元应用特殊处理，或者更正式地说是低代理代码单元)，使用以下规则：

- 不是前导代理项也不是尾随代理项的代码单元被解释为具有相同值的代码点。

- 一种由两个代码单元组成的序列，其中第一个代码单元c1是前导代理，第二个代码单元c2是一个尾随代理项，它被解释为一个值为(c1-0xD800)×0x400+(c2-0xDC00)+0x10000的码位。(见11.1.3)

- 作为前导代理或尾随代理但不属于代理对的代码单元，将被解释为具有相同值的代码点。

    拓展：

    1.在Unicode标准下，代表某个字符的一串二进制数称为这个字符的"码点"，给一个字符指定一串二进制数的行为就叫做"编码"；为了能统一编码世界上所有的文字及符号以及实现一些信息处理功能，Unicode标准共"准备"了17*65536个码位，其中前面的17是指统一码标准将这些码位分为17个集合，每一个集合称为一个平面(plane)。

    2.第0平面又称为基本多文种平面(BMP，Basic Multilingual Plane)，若使用UTF，其中码位对应的字符在可以只用8位的码元(code unit)表示，而第1-16平面中码位的字符必须用16位或32位的码元来表示。

    代码点(Code Point)：
    Unicode 代码空间中的任何值，即从 0 到   的整数范围。但并非所有代码点都分配给编码字符。
    一个字符在任何编码字符集中的值或位置。

    代码单元(Code Unit)：最小的数位组合，可以表示用于处理或交换的编码文本的单位。在 Unicode 标准中，UTF-8 编码格式采用 8 位编码单元，UTF-16 编码格式采用 16 位编码单元，UTF-32 编码格式采用 32 位编码单元。

    代理项(Surrogate)，是一种仅在 UTF-16 中用来表示补充字符的方法。在 UTF-16 中，为补充字符分配两个 16 位的 Unicode 代码单元：
    </br>第一个代码单元，被称为高代理项代码单元或前导代码单元；
    </br>第二个代码单元，被称为低代理项代码单元或尾随代码单元。
    </br>这两个代码单元组合在一起，就被称为代理项对


方法String.prototype.normalize(见22.1.3.14)可用于显式规范化字符串值。String.prototype.localeCompare(见22.1.3.11)在内部规范化字符串值，但没有其他操作隐式规范化它们所操作的字符串。只有显式指定为对语言或区域设置敏感的操作才会产生对语言敏感的结果。


    '\u01D1'                        // 'Ǒ'
    '\u004F\u030C'                  // 'Ǒ'
    '\u01D1' == '\u004F\u030C'      //false
    '\u01D1'.normalize() == '\u004F\u030C'.normalize()  //true


>NOTE 这种设计的基本原理是使字符串的实现尽可能简单和高性能。如果ECMAScript源文本是标准化形式C，那么字符串文本也保证是规范化的，只要它们不包含任何Unicode转义序列。


在本规范中，短语"A，B，…"的字符串连接(其中每个参数是字符串值、代码单元或代码单元序列)表示字符串值，其代码单元序列是每个参数(按顺序)的代码单元(按顺序)的串联。

短语"S从inclusiveStart到exclusiveEnd的子串"(其中S是字符串值或一系列代码单元，inclusiveStart和exclusiveEnd是整数)表示由S的连续代码单元组成的字符串值，从索引inclusiveStart开始，到索引exclusiveEnd之前结束(不包含它)当inclusiveStart=exclusiveEnd时的字符串。如果省略"to"后缀，则使用S的长度作为exclusiveEnd的值。

短语 "ASCII文字字符 "表示以下字符串值，它完全由Unicode基本拉丁语块中的每个字母和数字以及U+005F（LOW LINE）组成。

"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_".

由于历史原因，它对各种算法都有意义。

#### 6.1.4.1 StringIndexOf(string， searchValue， fromIndex )

抽象操作StringIndexOf接受参数string(字符串)、searchValue(字符串)和fromIndex(非负整数)。调用时执行以下步骤：


    1. Let len be the length of string.
    2. If searchValue is the empty String and fromIndex ≤ len, return fromIndex.
    3. Let searchLen be the length of searchValue.
    4. For each integer i such that fromIndex ≤ i ≤ len - searchLen, in ascending order, do
        a. Let candidate be the substring of string from i to i + searchLen.
        b. If candidate is the same sequence of code units as searchValue, return i.
    5. Return -1.

    说明:
    这是我们第一个解读的抽象操作，所以做了详细的分析
    1. 让len为字符串的长度
    2. 如果searchValue为空字符串，并且fromIndex≤len，则返回fromIndex。
    3. 让searchLen成为检索字符串的长度
    4. 对于每个整数i，使fromIndex≤i≤len-searchLen，按升序排列，做
        a. 让candidate成为从i到i + searchLen的字符串的子串。
        b. 如果candidate是与searchValue相同的代码单元序列，则返回i。
    6. 返回-1

    由于抽象操作只是定义了算法，并不是具体实现，所以并没有一个完全相同的方法，这里我们可以根据算法内容猜测到他的用处。第一个想到的就是String.prototype.indexOf，这个过程类似下面的伪代码

    String.prototype.indexOf = function(string， searchValue， fromIndex){

        let string = operation_str(string)
        let searchValue = operation_str(searchValue)
        let fromIndex = operation_int(fromIndex)
        return operation_fun(StringIndexOf(string， searchValue， fromIndex))

    }

    再来看个特殊的例子：

    "Hello World".indexOf(''， -1)  //0
    "Hello World".indexOf(''， 5)  //5

    当searchValue为''的时候会有一些特殊，他会返回最接近fromIndex且不大于len的非负整数(此为String.prototype.indexOf的算法)

>NOTE1 如果searchValue是空字符串，并且fromIndex小于或等于字符串的长度，则此算法返回fromIndex。空字符串可以有效地在字符串中的每个位置找到，包括在最后一个代码单元之后。

> NOTE2 如果fromIndex>字符串的长度，则此算法始终返回-1。  

### 6.1.5  符号类型/Symbol类型（The Symbol Type）

符号类型是可以用作对象属性(6.1.7)键的所有非字符串值的集合。

每个可能的符号值都是唯一且不可变的。

每个符号值不变地保存一个名为[[Description]]的关联值，该值要么是undefined，要么是一个字符串值。

#### 6.1.5.1 已知符号

已知符号是本规范的算法明确引用的内置符号值。它们通常用作属性的键，其值用作规范算法的扩展。除非另有规定，已知符号值由所有域(realm)共享(8.2)。

在这个规范中，通过使用@@name的符号来引用一个已知的符号，其中"name"是表1中列出的值之一。

<center>Table 1: Well-known Symbols</center>

|Specification Name         | [[Description]]                   |Value and Purpose|
|----|----|----|
@@asyncIterator             |"Symbol.asyncIterator"             |返回对象的默AsyncIterator的方法。由for await of语句的语义调用。
@@hasInstance               |"Symbol.hasInstance"               |一个确定构造函数对象是否将对象识别为构造函数实例之一的方法。由instanceof操作符的语义调用。
@@isConcatSpreadable        |"Symbol.isConcatSpreadable"        |布尔值属性，如果为true，则指示应通过Array.prototype.concat将对象展平为其数组元素。
@@iterator                  |"Symbol.iterator"                  |返回对象的默认迭代器的方法。由for-of语句的语义调用。
@@match	                    |"Symbol.match"                     |匹配将正则表达式与字符串匹配的正则表达式方法。由String.prototype.match方法。
@@matchAll	                |"Symbol.matchAll" matchAll         |返回迭代器的正则表达式方法，该方法根据字符串生成正则表达式的匹配项。由String.prototype.matchAll方法调用。
@@replace	                |"Symbol.replace"                   |替换替换字符串匹配子字符串的正则表达式方法。由String.prototype.replace方法调用。
@@search	                |"Symbol.search"                    |一个正则表达式方法，返回与正则表达式匹配的字符串中的索引。由String.prototype.search方法调用    。
@@species	                |"Symbol.species"                   |一个函数值属性，它是用于创建派生对象的构造函数。
@@split	                    |"Symbol.split"                     |在与正则表达式匹配的索引处拆分字符串的正则表达式方法。由String.prototype.split方法调用。
@@toPrimitive	            |"Symbol.toPrimitive"               |将对象转换为相应的原始值的方法。由TopPrimitive抽象操作调用。
@@toStringTag               |"Symbol.toStringTag"               |一个字符串值属性，用于创建对象的默认字符串描述。通过内置方法访问 Object.prototype.toString.
@@unscopables	            | "Symbol.unscopables"              |一个对象值属性，其自身和继承的属性名是从关联对象的with环境绑定中排除的属性名。

### 6.1.6 数字类型(Numeric Types)

ECMAScript有两个内置的数字类型：Number和BigInt。在这个规范中，每个数字类型T都包含一个表示为T:：unit的乘法标识值。规范类型还具有以下抽象操作，对于具有规范名称op的给定操作，同样表示为T:：op。所有参数类型都是T。"Result"列显示返回类型，并指示某些操作调用是否可能返回突然完成。

|Invocation Synopsis|Example source	|Invoked by the Evaluation semantics of ...	|Result|
|----|----|----|----|
T::unaryMinus(x)    |-x	            |Unary - Operator	                        |T
T::bitwiseNOT(x)	|~x             |Bitwise NOT Operator ( ~ )                 |T
T::exponentiate(x， y)|x ** y	    |Exponentiation Operator and Math.pow ( base， exponent )	|T， may throw RangeError
T::multiply(x， y)   |x * y          |Multiplicative Operators                   |T
T::divide(x， y)	    |x / y	        |Multiplicative Operators	                |T， may throw RangeError
T::remainder(x， y)	|x % y	        |Multiplicative Operators	                |T， may throw RangeError
T::add(x， y)	    |x++，++x，x + y  |Postfix Increment Operator， Prefix Increment Operator， and The Addition Operator ( + ) |	T
T::subtract(x， y)	|x--，--x，x - y	|Postfix Decrement Operator， Prefix Decrement Operator， and The Subtraction Operator ( - )|	T
T::leftShift(x， y)	|x << y	T       |he Left Shift Operator ( << )	            |T
T::signedRightShift(x， y)	|x >> y	|The Signed Right Shift Operator ( >> )	    |T
T::unsignedRightShift(x， y)	|x >>> y|	The Unsigned Right Shift Operator ( >>> )|T， may throw TypeError
T::lessThan(x， y)	|x< yx > y，x <= y，x >= y	|Relational Operators， via Abstract Relational Comparison	|Boolean or undefined (for unordered inputs)
T::equal(x， y)	    |x == y，x != y，x === y，x !== y	|Equality Operators， via Strict Equality Comparison |Boolean
T::sameValue(x， y)	||Object internal methods， via SameValue ( x， y )， to test exact value equality	|Boolean
T::sameValueZero(x， y)||Array， Map， and Set methods， via SameValueZero ( x， y )， to test value equality ignoring differences among members of the zero cohort (i.e.， -0<sub>𝔽</sub> and +0<sub>𝔽</sub>)	|Boolean
T::bitwiseAND(x， y)	|x & y          |Binary Bitwise Operators                   |T
T::bitwiseXOR(x， y)	|x ^ y	        |Binary Bitwise Operators	                |T
T::bitwiseOR(x， y)	|x | y	        |Binary Bitwise Operators	                |T
T::toString(x)	    |String(x)	    |Many expressions and built-in functions， via ToString ( argument )	|String

因为数字类型通常在不损失精度或截断的情况下是不可转换的，所以ECMAScript语言在这些类型之间不提供隐式转换。 程序员在调用需要其他类型的函数时必须显式调用Number和BigInt函数以在类型之间进行转换。

>NOTE ECMAScript的第一版和后续版本为某些运算符提供了可能会失去精度或截断的隐式数值转换。 保留这些传统的隐式转换是为了实现向后兼容性，但BigInt并未提供这些转换，以最大程度地减少程序员出错的机会，并在以后的版本中保留通用值类型的选项。


#### 6.1.6.1 Number类型（The Number Type）

Number类型正好具有18，437，736，874，454，810，627(即2<sup>64</sup>-2<sup>53</sup> + 3)个值，表示IEEE二进制浮点算术标准中指定的双精度64位格式IEEE 754-2019值，除了9，007，199，254，740，990( 也就是说，2<sup>53</sup>-2)IEEE标准中不同的"非数字"值在ECMAScript中表示为单个特殊的NaN值。 (请注意，NaN值是由程序表达式NaN产生的。)在某些实现中，外部代码可能能够检测到各种Not-a-Number值之间的差异，但是这种行为是实现定义的。 在ECMAScript代码中，所有NaN值彼此之间是无法区分的。

>NOTE 在将Number值存储到ArrayBuffer(请参阅25.1)或SharedArrayBuffer(请参阅25.2)中可能会观察到的位模式不一定与ECMAScript实现使用的Number值的内部表示相同。

还有另外两个特殊值，称为正无穷大和负无穷大。为了简洁起见，为了便于解释，这些值也分别由符号+∞<sub>𝔽</sub>和-∞<sub>𝔽</sub>引用。(请注意，这两个无穷数值由程序表达式+Infinity(或简单地说是Infinity)和-Infinity生成。)

其他18，437，736，874，454，810，624(即2<sup>64</sup>-2<sup>53</sup>)值称为有限数。其中一半是正数，一半是负数；对于每个有限的正数，都有一个对应的负值，其大小相同。

注意，既有正零也有负零。为简便起见，出于说明目的，这些值也分别用符号+0<sub>𝔽</sub>和-0<sub>𝔽</sub>表示。 (请注意，这两个不同的零数字值是由程序表达式+0(或简单地为0)和-0产生的。)

18，437，736，874，454，810，622(即2<sup>64</sup>-2<sup>53</sup>-2)有限的非零值有两种：

其中18，428，729，675，200，069，632(即2<sup>64</sup>-2<sup>54</sup>)被规范化，其形式为

s×m×2<sup>e</sup>

其中s为1或-1，m是使得2<sup>52</sup>≤m <2<sup>53</sup>的整数，并且e是使得-1074≤e≤971的整数。

其余的9，007，199，254，740，990(即253-2)值被非规范化，其形式为

s×m×2<sup>e</sup>

其中s为1或-1，m为整数，使得0 < m < 252，e为-1074。

请注意，所有不大于2<sup>53</sup>的正整数和负整数都可以在Number类型中表示。整数0在数字类型中具有两种表示形式：+0<sub>𝔽</sub>和-0<sub>𝔽</sub>。

如果一个有限的数是非零的，并且用来表示它的整数m(以上面所示的两种形式之一)是奇数，那么它的有效位是奇数。否则，它具有偶数意义。

在本规范中，短语" x的数值"(其中x表示确切的实际数学量(甚至可能是无理数，例如π))表示按以下方式选择的数值。考虑数字类型的所有有限值的集合，其中删除了-0<sub>𝔽</sub>并添加了两个在数字类型中无法表示的附加值，即2<sup>1024</sup>(即+1×253×2971)和-2<sup>1024</sup>(即-1×253×2971)。选择此集合中值最接近x的成员。如果该集合的两个值相等接近，则选择有效位为偶数的那一个；为此，额外的两个值2<sup>1024</sup>和-2<sup>1024</sup>被视为具有偶数个有效位。最后，如果选择了2<sup>1024</sup>，则将其替换为+∞<sub>𝔽</sub>；如果选择了-2<sup>1024</sup>，则将其替换为-∞<sub>𝔽</sub>；如果选择了+0<sub>𝔽</sub>，则当且仅当x<0时，将其替换为-0<sub>𝔽</sub>；任何其他选择的值不变地使用。结果是x的数值(此程序与IEEE 754-2019 RoundtiesToEvent模式的行为完全对应。)

+∞的Number值为+∞<sub>𝔽</sub>，-∞的Number值为-∞<sub>𝔽</sub>。

某些ECMAScript运算符仅处理特定范围内的整数，例如-2<sup>31</sup>至2<sup>31</sup>-1(含端点)或0至2<sup>16</sup>-1(含端点)范围。这些运算符接受Number类型的任何值，但首先将每个此类值转换为期望范围内的整数值。请参阅7.1中数字转换操作的说明。

#### 6.1.6.1.1 Number::unaryMinus ( x ) 

抽象操作Number::unaryMinus采用参数x(一个Number)。 调用时，它将执行以下步骤：

1. 操作数x若为NaN 返回NaN
2. 返回x相反的结果；也就是说，计算一个大小相同但符号相反的数。

#### 6.1.6.1.2 Number::bitwiseNOT ( x ) 

抽象操作Number::bitwiseNOT接受参数x(一个Number)。 调用时，它将执行以下步骤：

1. 令oldValue为!ToInt32(x).
2. 返回对oldValue应用位补的结果。结果的数学值可以精确地表示为32位的二进制补码字符串。

    说明:

    来看2个例子
    ~1      //-2
    ~-2     //1
    ~3      //-4

    结果就是操作数+1取反

#### 6.1.6.1.3 Number::exponentiate ( base， exponent ) 

抽象操作Number:：exponentiate接受参数base(一个数字)和exponent(一个数字)。它返回一个实现近似值，表示将基数提高为幂指数的结果，但必须满足以下要求：

1. If exponent is NaN, return NaN.
2. If exponent is +0𝔽 or exponent is -0𝔽, return 1𝔽.
3. If base is NaN, return NaN.
4. If base is +∞𝔽, then
    - If exponent > +0𝔽, return +∞𝔽. Otherwise, return +0𝔽.
5. If base is -∞𝔽, then
    - If exponent > +0𝔽, then
        - If exponent is an odd integral Number, return -∞𝔽. Otherwise, return +∞𝔽.
    - Else,
        - If exponent is an odd integral Number, return -0𝔽. Otherwise, return +0𝔽.
6. If base is +0𝔽, then
    - If exponent > +0𝔽, return +0𝔽. Otherwise, return +∞𝔽.
7. If base is -0𝔽, then
    - If exponent > +0𝔽, then
        - If exponent is an odd integral Number, return -0𝔽. Otherwise, return +0𝔽.
    - Else,
        - If exponent is an odd integral Number, return -∞𝔽. Otherwise, return +∞𝔽.
8. Assert: base is finite and is neither +0𝔽 nor -0𝔽.
9. If exponent is +∞𝔽, then
    - If abs(ℝ(base)) > 1, return +∞𝔽.
    - If abs(ℝ(base)) is 1, return NaN.
    - If abs(ℝ(base)) < 1, return +0𝔽.
10. If exponent is -∞𝔽, then
    - If abs(ℝ(base)) > 1, return +0𝔽.
    - If abs(ℝ(base)) is 1, return NaN.
    - If abs(ℝ(base)) < 1, return +∞𝔽.
11. Assert: exponent is finite and is neither +0𝔽 nor -0𝔽.
12. If base < -0𝔽 and exponent is not an integral Number, return NaN.
13. Return an implementation-approximated Number value representing the result of raising ℝ(base) to the ℝ(exponent) power.


    说明：最后一步返回一个近似于实现的Number值，代表将ℝ(base)提高到ℝ(exponent)的幂的结果。
    
    先来看两个常见的写法：

    Math.pow(2，5)           //32

    (-2)**2                 //4  注意，此处必须使用()消除优先级歧义

    再来看下特殊的写法

    Math.pow({a:1}，null)    //1   

    Math.pow方法稍有不同，在进行上面的算法步骤之前会先进行类型转换，null会先转换为0，也就是第二种情况直接返回1，无视base的值


> NOTE 当基数为1<sub>𝔽</sub>或-1<sub>𝔽</sub>且指数为+∞<sub>𝔽</sub>或-∞<sub>𝔽</sub>时，或当基数为1<sub>𝔽</sub>且指数为NaN时，base ** exponent 的结果与IEEE 754-2019不同。ECMAScript的第一版为该操作指定了NaN的结果，而ieee754-2019的后续版本指定了1<sub>𝔽</sub>。出于兼容性的原因，保留了历史ECMAScript行为。


#### 6.1.6.1.4 Number::multiply ( x， y ) 

抽象操作Number::multiply接受参数x(一个Number)和y(一个Number)。 根据IEEE 754-2019二进制双精度算法的规则确定，它执行乘法运算，生成x和y的乘积：

1. If x is NaN or y is NaN, return NaN.
2. If x is +∞𝔽 or x is -∞𝔽, then
    - If y is +0𝔽 or y is -0𝔽, return NaN.
    - If y > +0𝔽, return x.
    - Return -x.
3. If y is +∞𝔽 or y is -∞𝔽, then
    - If x is +0𝔽 or x is -0𝔽, return NaN.
    - If x > +0𝔽, return y.
    - Return -y.
4. If x is -0𝔽, then
    - If y is -0𝔽 or y < -0𝔽, return +0𝔽.
    - Else, return -0𝔽.
5. If y is -0𝔽, then
    - If x < -0𝔽, return +0𝔽.
    - Else, return -0𝔽.
6. Return 𝔽(ℝ(x) × ℝ(y)).


    说明

    如果任一操作数为NaN，则结果为NaN。
    如果其中一个是无穷(正负皆可)，且另一个为0(正负皆可)，返回NaN,如果另一个数大于0则返回这个数，否则返回这个数的相反数
    如果x为-0，y是-0或者小于-0返回0，否则返回-0
    如果y是﹣-，x小于-0返回0否则返回-0
    返回𝔽(ℝ(x) × ℝ(y))

    Infinity*0      //NaN
    1*NaN           //NaN

    这里还有个特殊的情况是针对于下方结和性的说明
    0.1*0.2*0.3     //0.006000000000000001
    0.2*0.3*0.1     //0.006


>注意 有限精度乘法是可交换的，但并不总是结合的。


#### 6.1.6.1.5 Number::divide(x，y)

抽象操作Number::divide接受参数x(一个数字)和y(一个数字)。 它执行除法，产生x和y的商； x是被除数，y是除数。 结果由IEEE 754-2019算法的规范确定：

1. If x is NaN or y is NaN, return NaN.
2. If x is +∞𝔽 or x is -∞𝔽, then
    - If y is +∞𝔽 or y is -∞𝔽, return NaN.
    - If y is +0𝔽 or y > +0𝔽, return x.
    - Return -x.
3. If y is +∞𝔽, then
    - If x is +0𝔽 or x > +0𝔽, return +0𝔽. Otherwise, return -0𝔽.
4. If y is -∞𝔽, then
    - If x is +0𝔽 or x > +0𝔽, return -0𝔽. Otherwise, return +0𝔽.
5. If x is +0𝔽 or x is -0𝔽, then
    - If y is +0𝔽 or y is -0𝔽, return NaN.
    - If y > +0𝔽, return x.
    - Return -x.
6. If y is +0𝔽, then
    - If x > +0𝔽, return +∞𝔽. Otherwise, return -∞𝔽.
7. If y is -0𝔽, then
    - If x > +0𝔽, return -∞𝔽. Otherwise, return +∞𝔽.
8. Return 𝔽(ℝ(x) / ℝ(y)).

    注：除法
    如果任一操作数为NaN，则结果为NaN。
    如果两个操作数具有相同的符号，则结果的符号为正；如果两个操作数具有不同的符号，则结果为负。
    无限除以无限会得到NaN。
    无穷大除以零会导致无穷大。该标志由上述规则确定。
    无穷大除以非零有限值会导致有符号无穷大。该标志由上述规则确定。
    有限值除以无穷大将得出零。该标志由上述规则确定。
    将零除以零会产生NaN；将零除以任何其他有限值将得出零，其符号由上述规则确定。
    将非零有限值除以零会导致有符号无穷大。该标志由上述规则确定。
    在其余情况下，既不涉及无穷大，也不涉及零，也不涉及NaN，则使用IEEE 754-2019 roundTiesToEven模式计算商并将其四舍五入为最接近的可表示值。如果幅度太大而无法表示，则操作会溢出；结果就是适当符号的无穷大。如果幅度太小而无法表示，则操作会下溢，并且结果为适当符号的零。 ECMAScript语言要求支持IEEE 754-2019所定义的渐进式下溢。

    此处和乘法基本相同，不另作说明

#### 6.1.6.1.6 Number::remainder(n，d)

抽象操作Number::remainder接受参数n(一个Number)和d(一个Number)。 它从一个隐式除法得到其余的操作数； n是被除数，d是除数。
注意
在C和C ++中，余数运算符仅接受整数操作数。 在ECMAScript中，它也接受浮点操作数。
％运算符计算的浮点余数运算的结果与IEEE 754-2019定义的"余数"运算不同。 IEEE 754-2019"余数"运算根据舍入除法而不是截断除法计算余数，因此其行为与通常的整数余数运算符的行为不相似。 相反，ECMAScript语言在浮点运算上定义了％，其行为类似于Java整数余数运算符； 可以将其与C库函数fmod进行比较。

ECMAScript浮点余数运算的结果由IEEE算术规则确定：

1. If n is NaN or d is NaN, return NaN.
2. If n is +∞𝔽 or n is -∞𝔽, return NaN.
3. If d is +∞𝔽 or d is -∞𝔽, return n.
4. If d is +0𝔽 or d is -0𝔽, return NaN.
5. If n is +0𝔽 or n is -0𝔽, return n.
6. Assert: n and d are finite and non-zero.
7. Let quotient be ℝ(n) / ℝ(d).
8. Let q be the mathematical value whose sign is the sign of quotient and whose magnitude is floor(abs(quotient)).
9. Let r be ℝ(n) - (ℝ(d) × q).
10. If r is 0 and n < -0𝔽, return -0𝔽.
11. Return 𝔽(r).

>NOTE1 在C和C++中，余数运算符只接受整数操作数；在ECMAScript中，它也接受浮点操作数。

>NOTE2 由%运算符计算的浮点余数运算的结果与IEEE 754-2019定义的“余数”运算不同。IEEE 754-201“余数运算”通过舍入除法而不是截断除法计算余数，因此其行为与通常的整数余数运算符不同。相反，ECMAScript语言定义了浮点运算上的%，其行为方式类似于Java整数余数运算符；这可以与C库函数fmod进行比较。

    说明
    如果任一操作数为NaN，则结果为NaN。
    符号取决于被除数(此处没有按原文翻译)
    如果被除数是无穷大，或者除数是零，或者两者都是，则结果是NaN。
    如果被除数是有限的且除数是无穷大，则结果等于被除数。
    如果被除数是零，而除数是非零且有限的，则结果与被除数相同。
    在其余的情况下，如果不涉及无穷大、零或NaN，则被除数n和除数d的浮点余数r由数学关系r=n-(d×q)定义，其中q是一个整数，仅当n/d为负时为负，仅当n/d为正时为正，在不超过n和d的真实数学商的大小的情况下，计算其大小，并使用IEEE 754-2019 roundTiesToEven模式四舍五入到最接近的可表示值。

    NaN/2       //NaN
    -3%-2       //-1

##### 6.1.6.1.7 Number::add(x，y)

抽象操作Number::add接受参数x(一个数字)和y(一个数字)。 它执行加法运算，生成x和y的和，该和是使用IEEE 754-2019二进制双精度算法的规则确定的：

1. If x is NaN or y is NaN, return NaN.
2. If x is +∞𝔽 and y is -∞𝔽, return NaN.
3. If x is -∞𝔽 and y is +∞𝔽, return NaN.
4. If x is +∞𝔽 or x is -∞𝔽, return x.
5. If y is +∞𝔽 or y is -∞𝔽, return y.
6. Assert: x and y are both finite.
7. If x is -0𝔽 and y is -0𝔽, return -0𝔽.
8. Return 𝔽(ℝ(x) + ℝ(y)).

>NOTE 有限精度加法是可交换的，但并不总是结合的。(同乘法)

#### 6.1.6.1.8 Number ::subtract(x，y)

抽象运算Number::subtract接受参数x(一个数字)和y(一个数字)。 它执行减法，产生其操作数的差； x是被减数，y是减数。 调用时，它将执行以下步骤：

1. Return Number::add(x， Number::unaryMinus(y)).


>NOTE 总是会出现x-y与x +(-y)产生相同结果的情况。


#### 6.1.6.1.9 Number::leftShift(x，y)

抽象操作Number::leftShift接受参数x(一个数字)和y(一个数字)。 调用时，它将执行以下步骤：

1. Let lnum be ! ToInt32(x).
2. Let rnum be ! ToUint32(y).
3. Let shiftCount be ℝ(rnum) modulo 32.
4. Return the result of left shifting lnum by shiftCount bits. The mathematical value of the result is exactly representable as a 32-bit two's complement bit string.

    说明： 
    <<"运算符执行左移位运算。在移位运算过程中，符号位始终保持不变。如果右侧空出位置，则自动填充为 0；超出 32 位的值，则自动丢弃。
    5 << 2;     //20

#### 6.1.6.1.10 Number::signedRightShift(x，y)
抽象操作Number::signedRightShift带有参数x(一个Number)和y(一个Number)。 调用时，它将执行以下步骤：

1. Let lnum be ! ToInt32(x).
2. Let rnum be ! ToUint32(y).
3. Let shiftCount be ℝ(rnum) modulo 32.
4. Return the result of performing a sign-extending right shift of lnum by shiftCount bits. The most significant bit is propagated. The mathematical value of the result is exactly representable as a 32-bit two's complement bit string.

    说明：
    按shiftCount位返回左移lnum的结果的数值。结果的数学值可以精确地表示为32位2的补码位字符串。
    1000 >> 8;  //3

#### 6.1.6.1.11 Number::unsignedRightShift(x，y)
抽象操作Number::unsignedRightShift带有参数x(一个数字)和y(一个数字)。 调用时，它将执行以下步骤：

1. Let lnum be ! ToUint32(x).
2. Let rnum be ! ToUint32(y).
3. Let shiftCount be ℝ(rnum) modulo 32.
4. Return the result of performing a zero-filling right shift of lnum by shiftCount bits. Vacated bits are filled with zero. The mathematical value of the result is exactly representable as a 32-bit unsigned bit string.
    
    说明：
    返回按shiftCount位对lnum执行零填充右移位的结果。空出的位用零填充。结果的数学值可以精确表示为32位无符号位字符串。
    1000 >>> 8;  //3
    -1   >>> 8;  //16777215

#### 6.1.6.1.12 Number ::lessThan(x，y)

抽象操作Number::less Than接受参数x(一个Number)和y(一个Number)。 调用时，它将执行以下步骤：

1. If x is NaN, return undefined.
2. If y is NaN, return undefined.
3. If x and y are the same Number value, return false.
4. If x is +0𝔽 and y is -0𝔽, return false.
5. If x is -0𝔽 and y is +0𝔽, return false.
6. If x is +∞𝔽, return false.
7. If y is +∞𝔽, return true.
8. If y is -∞𝔽, return false.
9. If x is -∞𝔽, return true.
10. Assert: x and y are finite and non-zero.
11. If ℝ(x) < ℝ(y), return true. Otherwise, return false.

    说明，这里有个特殊的情况和算法有一些不同
    1 < NaN         //false
    1 > NaN         //false

#### 6.1.6.1.13 Number ::equal(x，y)

抽象操作Number::equal采用参数x(一个数字)和y(一个数字)。 调用时，它将执行以下步骤：

1. If x is NaN, return false.
2. If y is NaN, return false.
3. If x is the same Number value as y, return true.
4. If x is +0𝔽 and y is -0𝔽, return true.
5. If x is -0𝔽 and y is +0𝔽, return true.
6. Return false.

    说明：因为比较涉及类型的隐式转换，具体算法在Relational Operators， Abstract Relational Comparison两节

#### 6.1.6.1.14 Number::sameValue(x，y)

抽象操作Number::sameValue接受参数x(一个Number)和y(一个Number)。 调用时，它将执行以下步骤：

1. If x is NaN and y is NaN, return true.
2. If x is +0𝔽 and y is -0𝔽, return false.
3. If x is -0𝔽 and y is +0𝔽, return false.
4. If x is the same Number value as y, return true.
5. Return false.

    说明:
    同值判断，Object.is()的算法会涉及此过程，但并不完全是。此处简单提及，下面是Object.is()包括的算法步骤，由于typeof NaN会返回'number'，所以此处应用了这个算法
    If Type(x) is Number or BigInt， then
        Return ! Type(x)::sameValue(x， y).
    Return ! SameValueNonNumeric(x， y).

#### 6.1.6.1.15 Number::sameValueZero(x，y)

抽象操作Number::sameValueZero接受参数x(一个Number)和y(一个Number)。 调用时，它将执行以下步骤：

1. If x is NaN and y is NaN, return true.
2. If x is +0𝔽 and y is -0𝔽, return true.
3. If x is -0𝔽 and y is +0𝔽, return true.
4. If x is the same Number value as y, return true.
5. Return false.

    说明: Array.prototype.includes会涉及该算法按升序将要搜索的元素与数组元素进行比较。同样的set，map等结构也会涉及此算法
   
#### 6.1.6.1.16NumberBitwiseOp(op，x，y)

抽象操作NumberBitwiseOp接受参数op(a sequence of Unicode code points)、x和y。调用时它执行以下步骤：

1. Let lnum be ! ToInt32(x).
2. Let rnum be ! ToInt32(y).
3. Let lbits be the 32-bit two's complement bit string representing ℝ(lnum).
4. Let rbits be the 32-bit two's complement bit string representing ℝ(rnum).
5. If op is &, let result be the result of applying the bitwise AND operation to lbits and rbits.
6. Else if op is ^, let result be the result of applying the bitwise exclusive OR (XOR) operation to lbits and rbits.
7. Else, op is |. Let result be the result of applying the bitwise inclusive OR operation to lbits and rbits.
8. Return the Number value for the integer represented by the 32-bit two's complement bit string result.

#### 6.1.6.1.17 Number::bitwiseAND(x，y)

抽象操作Number::bitwiseAND接受参数x(一个数字)和y(一个数字)。 调用时，它将执行以下步骤：

1. Return NumberBitwiseOp(&， x， y).

#### 6.1.6.1.18 Number::bitwiseXOR ( x， y )
The abstract operation Number::bitwiseXOR takes arguments x (a Number) and y (a Number). It performs the following steps when called:

1. Return NumberBitwiseOp(^， x， y).

#### 6.1.6.1.19 Number::bitwiseOR(x，y)
抽象操作Number::bitwiseOR接受参数x(一个Number)和y(一个Number)。 调用时，它将执行以下步骤：

1. Return NumberBitwiseOp(|， x， y).
    
#### 6.1.6.1.20 Number::toString(x)


抽象操作Number:：toString接受参数x（一个数字）和基数（一个介于2到36之间的整数），并返回一个字符串。它使用带基数的位置数字系统将x表示为字符串。使用基数r表示数字时使用的数字按顺序取自“0123456789abdefghijklmnopqrstuvxyz”的第一个r代码单元。大小大于或等于1的数字的表示𝔽 从不包含前导零。调用时执行以下步骤：

1. If x is NaN, return "NaN".
2. If x is +0𝔽 or -0𝔽, return "0".
3. If x < -0𝔽, return the string-concatenation of "-" and Number::toString(-x, radix).
4. If x is +∞𝔽, return "Infinity".
5. Let n, k, and s be integers such that k ≥ 1, radixk - 1 ≤ s < radixk, 𝔽(s × radixn - k) is x, and k is as small as possible. Note that k is the number of digits in the representation of s using radix radix, that s is not divisible by radix, and that the least significant digit of s is not necessarily uniquely determined by these criteria.
6. If radix ≠ 10 or n is in the inclusive interval from -5 to 21, then
    - If n ≥ k, then
        1. Return the string-concatenation of:
            - the code units of the k digits of the representation of s using radix radix
            - n - k occurrences of the code unit 0x0030 (DIGIT ZERO)
    - Else if n > 0, then
        1. Return the string-concatenation of:
            - the code units of the most significant n digits of the representation of s using radix radix
            - the code unit 0x002E (FULL STOP)
            - the code units of the remaining k - n digits of the representation of s using radix radix
    - Else,
        1. Assert: n ≤ 0.
        2. Return the string-concatenation of:
            - the code unit 0x0030 (DIGIT ZERO)
            - the code unit 0x002E (FULL STOP)
            - n occurrences of the code unit 0x0030 (DIGIT ZERO)
            - the code units of the k digits of the representation of s using radix radix
7. NOTE: In this case, the input will be represented using scientific E notation, such as 1.2e+3.
8. Assert: radix is 10.
9. If n < 0, then
    - Let exponentSign be the code unit 0x002D (HYPHEN-MINUS).
10. Else,
    - Let exponentSign be the code unit 0x002B (PLUS SIGN).
11. If k is 1, then
    - Return the string-concatenation of:
        1. the code unit of the single digit of s
        2. the code unit 0x0065 (LATIN SMALL LETTER E)
        3. exponentSign
        4. the code units of the decimal representation of abs(n - 1)
12. Return the string-concatenation of:
    - the code unit of the most significant digit of the decimal representation of s
    - the code unit 0x002E (FULL STOP)
    - the code units of the remaining k - 1 digits of the decimal representation of s
    - the code unit 0x0065 (LATIN SMALL LETTER E)
    exponentSign
    - the code units of the decimal representation of abs(n - 1)


> NOTE1 下列意见可能对实施有帮助，但不是本标准规范要求的一部分：

- 如果x是-0<sub>𝔽</sub>以外的任何Number值，则ToNumber(ToString(x))与x完全相同。
- s的最低有效位数并非总是由步骤5中列出的要求唯一确定。

>NOTE2 对于提供比上述规则所需的转换更为准确的转换的实现，建议将以下第5步的替代版本用作指导：

否则，令n，k和s为整数，使得k≥1，10k-1≤s <10k，s×10n-k为ℝ(x)，并且k尽可能小。如果s有多种可能性，请选择s×10n-k的值最接近ℝ(x)的s的值。如果s有两个这样的可能值，请选​​择一个偶数。请注意，k是s的十进制表示形式中的位数，并且s不能被10整除。

>NOTE3 ECMAScript的实现者可能会发现David M.Gay为浮点数的二进制到十进制转换编写的论文和代码很有用：
