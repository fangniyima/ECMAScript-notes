#### 6.1.6.2 The BigInt Type


    说明:BigInt和Number类型的方法基本相同，算法稍有不同，此篇不再做过多翻译。
    例子:

    const max = Number.MAX_SAFE_INTEGER

    max + 1                         //9007199254740992
    max + 2                         //9007199254740992

    BigInt类型是对超出安全整数范围的一个补充，我们先来简单了解一下:

    const max = BigInt(Number.MAX_SAFE_INTEGER)

    max + 1n                         //9007199254740992n
    max + 2n                         //9007199254740993n
    max + 1.5n                       //Uncaught SyntaxError
    let a = BigInt(1), b = 1n
    a== b                           //true

    这里需要主意:
    
    1. Number不能和BigInt进行运算
    2. 不支持一元+运算符

BigInt类型表示一个整数值。 该值可以是任何大小，并且不限于特定的位宽。 通常，除非另有说明，否则将操作设计为返回精确的基于数学的答案。 对于二进制运算，BigInts用作二进制补码二进制字符串，负数被视为将位无限地设置在左侧。

The BigInt::unit value is 1ℤ.

##### 6.1.6.2.1 BigInt::unaryMinus(x)

抽象操作BigInt::unaryMinus采用参数x(一个BigInt)。 调用时，它将执行以下步骤：

    1.If x is 0ℤ, return 0ℤ.
    2.Return the BigInt value that represents the negation of ℝ(x).

##### 6.1.6.2.2 BigInt::bitwiseNOT(x)

抽象操作BigInt::bitwiseNOT接受参数x(一个BigInt)。 返回x的补码； 即-x-1ℤ。

##### 6.1.6.2.3 BigInt::exponentiate(base，exponent)

抽象操作BigInt::exponentiate接受参数base(BigInt)和指数(BigInt)。 调用时，它将执行以下步骤：

    1.If exponent < 0ℤ, throw a RangeError exception.
    2.If base is 0ℤ and exponent is 0ℤ, return 1ℤ.
    3.Return the BigInt value that represents ℝ(base) raised to the power ℝ(exponent).

##### 6.1.6.2.4 BigInt::multiply(x，y)

抽象操作BigInt::multiply接受参数x(一个BigInt)和y(一个BigInt)。 它返回BigInt值，该值表示x和y相乘的结果。

注意
即使结果的位宽比输入的宽得多，也会给出确切的数学答案。

##### 6.1.6.2.5 BigInt::divide(x，y)

抽象操作BigInt::divide接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.If y is 0ℤ, throw a RangeError exception.
    2.Let quotient be ℝ(x) / ℝ(y).
    3.Return the BigInt value that represents quotient rounded towards 0 to the next integer value.

##### 6.1.6.2.6 BigInt::remainder(n，d)

抽象操作BigInt::remainder接受参数n(BigInt)和d(BigInt)。 调用时，它将执行以下步骤：

    1.If d is 0ℤ, throw a RangeError exception.
    2.If n is 0ℤ, return 0ℤ.
    3.Let r be the BigInt defined by the mathematical relation r = n - (d × q) where q is a BigInt that is negative only if n/d is negative and positive only if n/d is positive, and whose magnitude is as large as possible without exceeding the magnitude of the true mathematical quotient of n and d.
    4.Return r.

注意

结果的符号等于n的符号。

##### 6.1.6.2.7 BigInt::add(x，y)

抽象操作BigInt::add接受参数x(一个BigInt)和y(一个BigInt)。 它返回代表x和y之和的BigInt值。

##### 6.1.6.2.8 BigInt::subtract(x，y)

抽象操作BigInt::subtract接受参数x(一个BigInt)和y(一个BigInt)。 它返回代表差x减y的BigInt值。

##### 6.1.6.2.9 BigInt::leftShift(x，y)

抽象操作BigInt::leftShift接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.If y < 0ℤ, then
    Return the BigInt value that represents ℝ(x) ÷ 2-y, rounding down to the nearest integer, including for negative numbers.
    2.Return the BigInt value that represents ℝ(x) × 2y.

注意
此处的语义应等效于按位移位，将BigInt视为无限长度的二进制二进制补码数字。

##### 6.1.6.2.10 BigInt::signedRightShift(x，y)

抽象操作BigInt::signedRightShift采用参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Return BigInt::leftShift(x, -y).

##### 6.1.6.2.11 BigInt::unsignedRightShift(x，y)

抽象操作BigInt::unsignedRightShift采用参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Throw a TypeError exception.

##### 6.1.6.2.12 BigInt::lessThan(x，y)

抽象操作BigInt::lessThan接受参数x(一个BigInt)和y(一个BigInt)。 如果ℝ(x)<ℝ(y)，则返回true，否则返回false。

##### 6.1.6.2.13 BigInt::equal(x，y)

抽象操作BigInt::equal接受参数x(一个BigInt)和y(一个BigInt)。 如果ℝ(x)=ℝ(y)，则返回true，否则返回false。

##### 6.1.6.2.14 BigInt::sameValue(x，y)

抽象操作BigInt::sameValue接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Return BigInt::equal(x, y).

##### 6.1.6.2.15 BigInt::sameValueZero(x，y)

抽象操作BigInt::sameValueZero接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Return BigInt::equal(x, y).

##### 6.1.6.2.16 BinaryAnd(x，y)

抽象操作BinaryAnd接受参数x和y。 调用时，它将执行以下步骤：

    1..Assert: x is 0 or 1.
    2.Assert: y is 0 or 1.
    3.If x is 1 and y is 1, return 1.
    4.Else, return 0.

##### 6.1.6.2.17 BinaryOr(x，y)

抽象操作BinaryOr带有参数x和y。 调用时，它将执行以下步骤：

    1.Assert: x is 0 or 1.
    2.Assert: y is 0 or 1.
    3.If x is 1 or y is 1, return 1.
    4.Else, return 0.

##### 6.1.6.2.18 BinaryXor(x，y)

抽象操作BinaryXor带有参数x和y。 调用时，它将执行以下步骤：

    1.Assert: x is 0 or 1.
    2.Assert: y is 0 or 1.
    3.If x is 1 and y is 0, return 1.
    4.Else if x is 0 and y is 1, return 1.
    5.Else, return 0.

##### 6.1.6.2.19 BigIntBitwiseOp(op，x，y)

抽象操作BigIntBitwiseOp接受参数op(一系列Unicode代码点)，x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Assert: op is &, ^, or |.
    2.Set x to ℝ(x).
    3.Set y to ℝ(y).
    4.Let result be 0.
    5.Let shift be 0.
    6.Repeat, until (x = 0 or x = -1) and (y = 0 or y = -1),
        Let xDigit be x modulo 2.
        Let yDigit be y modulo 2.
        If op is &, set result to result + 2shift × BinaryAnd(xDigit, yDigit).
        Else if op is |, set result to result + 2shift × BinaryOr(xDigit, yDigit).
        Else,
            Assert: op is ^.
            Set result to result + 2shift × BinaryXor(xDigit, yDigit).
        Set shift to shift + 1.
        Set x to (x - xDigit) / 2.
        Set y to (y - yDigit) / 2.
    7.If op is &, let tmp be BinaryAnd(x modulo 2, y modulo 2).
    8.Else if op is |, let tmp be BinaryOr(x modulo 2, y modulo 2).
    9.Else,
        a.Assert: op is ^.
        b.Let tmp be BinaryXor(x modulo 2, y modulo 2).
    10.If tmp ≠ 0, then
        Set result to result - 2shift.
        NOTE: This extends the sign.
    11.Return the BigInt value for result.

##### 6.1.6.2.20 BigInt::bitwiseAND(x，y)

抽象操作BigInt::bitwiseAND接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：

    1.Return BigIntBitwiseOp(&, x, y).

##### 6.1.6.2.21 BigInt::bitwiseXOR(x，y)

抽象操作BigInt::bitwiseXOR接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：
    1.Return BigIntBitwiseOp(^, x, y).

##### 6.1.6.2.22 BigInt::bitwiseOR(x，y)

抽象操作BigInt::bitwiseOR接受参数x(一个BigInt)和y(一个BigInt)。 调用时，它将执行以下步骤：
    1.Return BigIntBitwiseOp(|, x, y).

##### 6.1.6.2.23 BigInt::toString(x)

抽象操作BigInt::toString接受参数x(一个BigInt)。 它将x转换为String格式。 调用时，它将执行以下步骤：

    1.If x < 0ℤ, return the string-concatenation of the String "-" and ! BigInt::toString(-x).
    2.Return the String value consisting of the code units of the digits of the decimal representation of x.