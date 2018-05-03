---
title: "iOS 数据精度及大数的处理"
date: 2018-05-03T14:50:02+08:00
draft: true
slug: "iOS-Shu-Ju-Jing-Du-Ji-Da-Shu-De-Chu-Li"
tags: ["iOS"]
category: ["iOS"]
---

在 iOS 开发中，我们很容易遇到使用 CGFloat 来标示浮点数，但这样的表示会造成精度失真。这时我们可以使用`NSDecimalNumber`来处理这个问题。

`NSDecimalNumber`是`NSNumber`的子类，可以处理大数运算及数据的精度问题。

### 大数相乘可能导致的问题

我们先上一段代码：

```
NSString *priceStr = @"";
NSDecimalNumber *number = [NSDecimalNumber decimalNumberWithString:priceStr];
NSDecimalNumber *countNum = [NSDecimalNumber decimalNumberWithString:stringWithNSInteger(NSIntegerMax)];
number = [number decimalNumberByMultiplyingBy:countNum];
```

在这段代码中，number的值为：NaN，即：not a number ，非数值；
而countNum 是一个最大的整数，
最后，将NaN和最大的整数相乘，导致了overflow的crash。

解决方案：

```
//定义数值处理的行为
NSDecimalNumberHandler *roundUp = [NSDecimalNumberHandler
                                      decimalNumberHandlerWithRoundingMode:NSRoundBankers
                                      scale:2
                                      raiseOnExactness:NO
                                      raiseOnOverflow:NO
                                      raiseOnUnderflow:NO
                                      raiseOnDivideByZero:NO];
    
NSString *priceStr = @"";
NSDecimalNumber *number = [NSDecimalNumber decimalNumberWithString:priceStr];
NSDecimalNumber *countNum = [NSDecimalNumber decimalNumberWithString:stringWithNSInteger(NSIntegerMax)];

//使用数据处理行为的约定来进行运算，防止crash
number = [number decimalNumberByMultiplyingBy:countNum withBehavior:roundUp];

```

上面这个例子不会crash了，但是最终number的值为NaN，需要后续的业务逻辑进行判断处理；

NSDecimalNumberHandler 用到的参数，其中：

#### NSRoundBankers

枚举，截断的方式；完整的定义如下：

```
// Rounding policies :
// Original
//    value 1.2  1.21  1.25  1.35  1.27
// Plain    1.2  1.2   1.3   1.4   1.3
// Down     1.2  1.2   1.2   1.3   1.2
// Up       1.2  1.3   1.3   1.4   1.3
// Bankers  1.2  1.2   1.2   1.4   1.3

typedef NS_ENUM(NSUInteger, NSRoundingMode) {
    NSRoundPlain,   // Round up on a tie
    NSRoundDown,    // Always down == truncate
    NSRoundUp,      // Always up
    NSRoundBankers  // on a tie round so last digit is even
};
```

**scale**

小数点后面的位数（精度）

**raiseOnExactness**

> The exception raised if there is an exactness error.

**raiseOnOverflow**

是否抛出溢出错误，如果为YES，则APP会捕获溢出错误，这会导致APPcrash；

> The exception raised on overflow.

**raiseOnUnderflow**

> The exception raised on underflow.

**raiseOnDivideByZero**

> The exception raised on divide by zero.

#### 补充知识：

* 判断一个数值是否为NaN可以使用系统方法：isnan(x)；注：x为数值类型，不是NSDecimalNumber，更不是NSNumber;
* 如果要判断一个NSDecimalNumber 是否为 NAN ，则使用下面的方法：

```
if([number isEqualToNumber:NSDecimalNumber.notANumber]){
    NSLog(@"number is nan");
}else{
    NSLog(@"number:%@",number);
}
```

参考链接：[iOS 两个数相乘导致 NSDecimalNumber overflow exception 错误的分析及解决](https://www.jianshu.com/p/c8feb12ecba5)