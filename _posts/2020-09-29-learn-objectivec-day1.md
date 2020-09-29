---
title: Learn Objective-C
tags: Objective-C
categories: Objective-C
---


2020.10.12就要去字节实习了，重没接触过IOS开发，但从本科学习Android开发的点点经验看，入门应该不是很难，
现在很多数据和视频都讲解的是Swift和SwiftUI，但问了mentor后说字节采用的目前主要还是Objective-C和UIKIT,
所以就从最古老的书籍开始看起吧

看了一圈知乎和Quora，最终选定了《Programming in Objective-C》这本书，有C++的基础，不知道看完这500页需要多久，
整个Checkbox玩玩吧~

- [ ] Chapter 1. The Objective-C Language Page:0-304
- [ ] Chapter 2. The Foundation Framework Page:307-449
- [ ] Chapter 3. Cocoa, Cocoa Touch, and the iOS SDK Page:449-484

# Chapter 1
## Programming in Objective-C

> Any program statements between the { and the matching closing } are executed within a context known an autorelease pool. The autorelease pool is a mechanism 
that allows the system to efficiently manage the memory your application uses as it creates new objects.

这是把C++自动回收栈上内存的过程显式化了吗？

> Without that leading @ character, you are writing a constant C-style string; with it, you are writing an NSString string object.

有C和Objective-C混用内味了

## Classes, Objects, and Methods

``` 
// Program to work with fractions – class version
#import <Foundation/Foundation.h>

//---- @interface section ----
@interface Fraction: NSObject
-(void) print;
-(void) setNumerator: (int) n;
-(void) setDenominator: (int) d;
@end

//---- @implementation section ----
@implementation Fraction
{
    int numerator;
    int denominator;
}
-(void) print
{
    NSLog (@"%i/%i", numerator, denominator);
}
-(void) setNumerator: (int) n
{
    numerator = n;
}
-(void) setDenominator: (int) d
{
    denominator = d;
}
@end

//---- program section ----
int main (int argc, char * argv[])
{
    @autoreleasepool {
        Fraction *myFraction;

        // Create an instance of a Fraction
        myFraction = [Fraction alloc];
        myFraction = [myFraction init];

        // Set fraction to 1/3
        [myFraction setNumerator: 1];
        [myFraction setDenominator: 3];

        // Display the fraction using the print method
        NSLog (@"The value of myFraction is:");
        [myFraction print];
    }
    return 0;
}
```


它来了它来了，这迷惑的语法，还能再复杂一点吗？`@interface` ... `@end` 为什么不和`autoreleasepool`一样用花括号括起来，成员函数的返回值要加括号，前面还要加 `-`！


> The leading minus sign ( - ) tells the Objective-C compiler that the method is an instancemethod. The only other option is a plus sign ( + ), which indicates a class method. A class method is one that performs some operation on the class itself, such as creating a new instance of the class.

看起来有点像成员函数和静态成员函数？

> The alloc method is guaranteed to zero out all of an object’s instance variables.

和C++不一样，C++类内成员变量在创建实例后默认是未初始化的，只有全局变量和静态变量会初始化为0（可能不准确）

> the idea that you can’t directly set or get the value of an instance variable from outside of the methods written for the class, but instead have to write setter and getter methods to do so is the principle of data encapsulation.

emmm... 就是麻烦了点，不过有`@synthesize`，只是还没看到这章

> We should also point out that a method called new combines the actions of an alloc and init . So, this line could be used to allocate and initialize a new Fraction :  
> `Fraction *myFraction = [Fraction new];`

很好的语法糖，先alloc再init，太反人类了

## Data Types and Expressions

> The id data type is used to store an object of any type. In a sense, it is a generic object type.

有点像C++的(void*)？

> The argument to the method is an integer, yet the method expects a double . No problem arises here because numeric arguments to methods are automatically converted to match the type expected. A double is expected by multiply :, so the integer value 5 automatically is converted to a double precision floating value when the function is called.

在C++中应该报错了吧，这种隐式转化看起来很容易出bug？

## Program Looping

> The characters %2i tell the NSLog routine not only that you want to display the value of an integer at that particular point, but also that the size of the integer to be displayed should
take up at least two columns in the display.

格式化输出

## Making Decisions

> if the conditions specified inside the if statement are `satisfied`, the program statement that immediately followed is executed.But what exactly does `satisfied` mean? In the Objective-C language, `satisfied` means `nonzero`

只要if判断的条件不是`nonzero`， 就隐式转换成`true`，和C++一样

> A couple of built-in features in Objective-C make working with Boolean variables a little easier.
One is the special type BOOL , which can be used to declare variables that will contain either
a true or a false value. The other is the built-in values YES and NO .

为什么BOOL要大写而不和C保持一直，为什么多了YES和NO而不用True，False？ 迷惑

> `condition ?: expression`condition is evaluated, and if true, then the value of the operation iscondition. If condition is false, then the value of the operation is expression.Logically, this syntax is identical to the following: condition ? condition : expression

可以有，但没必要，感觉三元表达式已经很方便了

> sign = ( number < 0 ) ? -1 : (( number == 0 ) ? 0 : 1);

三元表达式嵌套...还是if清晰点吧，但写起来好看，逻辑简单的可以用一用

## More on Classes

> You don’t need to use the `@synthesize` directive. Using the `@property` directive suffices. The
compiler automatically generates the setter and getter for you. However, be advised that if you
leave out `@synthesize`, then the compiler will generate the corresponding instance variables
with an underscore (_) character as the first character of its name.

封装后其实我们根本不关心具体的成员变量名是什么，只需要知道getter和setter就行，所以只需要定义`@property`就行

```
// Interface
@interface Fraction : NSObject
@property int numerator, denominator;
-(void) print;
-(double) convertToNum;
@end

// Implementation
@implementation Fraction
@synthesize numerator, denominator;
@end
```

> 135 页