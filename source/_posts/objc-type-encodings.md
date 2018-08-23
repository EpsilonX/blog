---
title: Type-Encoding研究
date: 2018-08-23 10:56:15
tags:
- objc
- type-encoding
- runtime
category: Objective-C
---

# Type Encoding

最近在看runtime，对于消息发送机制中的类型编码（Type-Encoding)比较感兴趣，我们知道，当我们调用`[receiver sendWithArg:arg1 another:arg2]`时，编译器会转换成`objc_msgSend(receiver,@selector(sendWithArg:another), arg1, arg2)`, 假如我们有个方法：

```objc
- (void)sendMsgWithParams:(NSDictionary *)params;
```

我们要如何获得这个方法的Type-Encoding呢，很简单:

```objc
SEL aSel = @selector(sendMsgWithParams:);
Method aMethod = class_getInstanceMethod(self.class, aSel);
const char *typeEncoding = method_getTypeEncoding(aMethod);
NSLog(@"sel:%s, %s",aSel,typeEncoding);
```

我们会得到这么一串儿: `v24@0:8@16`, 这就是这个方法的type-encoding，我们来解释一下这串编码：

1. @encode(void) =" v"，标识返回类型
2. @encode(id)="@", 标识receiver(self)
3. @encode(SEL)=":"，标识selector（_cmd)
4. @encode(NSDictionary *)="@", 标识参数params的类型

完整的类型编码表我会在文末放出，那么编码中的数字是什么意思呢？其实是runtime方法调用参数的实际顺序:

```objc
0: self, 8: @selector(sendMsgWithParams:), 16: params(NSDictionary)， 24：void
```

其实就和`objc_msgSend(self, _cmd, arg1, arg2, ...)`对应起来了。



# @的问题

在type-encoding中，所有NSObject及其子类的类型type-encoding都是@，那么这是不是意味着runtime其实并不会检查具体的NSObject类型，我们来做个试验吧。

还是拿sendMsgWithParams这个方法做例子，这个方法的参数要求是NSDictionary，那么我们传一个NSNumber试试？

```objc
[self sendMsgWithParams:@(11)];
```

这个时候编译器会提示一个warning：

> Incompatible pointer types sending 'NSNumber *' to parameter of type 'NSDictionary *'

但是编译过程不会报错，程序也能够跑起来。

如果我们用id呢？

```objc
id aNum = @(11);
[self sendMsgWithParams:aNum];
```

这时连warning都没有了，因为编译器并不知道id类型是否是NSDictionary，对于type-encoding来说都是@。

如果直接传数字呢？

```objc
[self sendMsgWithParams:11];
```

这时候会出现编译错误:

> Implicit conversion of 'int' to 'NSDictionary *' is disallowed with ARC

说明根据type-encoding是能够监测出int(i)和NSObject(@)，比较特殊的是：

```objc
[self sendMsgWithParams:0];
```

不会有任何报错，其实这里0被当成了(NSObject *)0，也就是nil了。

# 小结

Objective-C的类型系统，只能检查基本的数据类型，对于NSObject及其子类，类型系统都是统一当成一个类型处理的，这一特点可以使Objective-C足够动态化，但是带来的弊端也比较明显，就是想依赖方法定义的参数类型来约束传参类型，是比较困难的。必要的时候，应该要对传入的参数做类型检查，避免类型不一致引发的crash。



附上Type-Encoding的编码表:

| Code           | 含义                            |
| -------------- | ------------------------------- |
| c              | char                            |
| i              | int                             |
| s              | short                           |
| l              | long                            |
| q              | long long                       |
| C              | unsigned char                   |
| I              | unsigned int                    |
| S              | unsigned short                  |
| L              | unsigned long                   |
| Q              | unsigned long long              |
| f              | float                           |
| d              | double                          |
| B              | bool                            |
| v              | void                            |
| *              | char *                          |
| @              | id, NSObject                    |
| #              | Class                           |
| :              | SEL, selector                   |
| [array type]   | array                           |
| {name=type...} | structure                       |
| (name=type...) | union                           |
| b              | bit field of num bits           |
| ^              | pointer to type                 |
| ?              | unknown type(function pointers) |

