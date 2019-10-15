--
> 创建日期：2017年06月01日  
> 修改日期：2018年01月29日  

--
Block在ObjectC中是比较重要的一个知识点。对于新手来说是很难理解的(比如说我^_^)。这篇博文放了四天，在网上查找了一些资料，发现对我来说只有两种情况：看不懂和太简单！

Block简介：

Block是一个对象，是封装起来的代码，有点像函数，可以在任何时候执行。Block和函数的相似性：

* 可以保存代码
* 有返回值
* 有形参
* 调用方式一样

Block和函数的区别是：函数指针是对一个函数地址的引用，这个函数在编译的时候就已经确定了，而block是一个函数对象，是在程序运行过程中产生的。

参考Dash上的资料，Block的常见四种用法：

Block as a local variable (Block变量)

```
returnType(^blockName)(parameterType)=^returnType(parameters){...};
```

Block as a property(Block属性)

```
@property (nonatomic, copy)returnType(^blockName)(parameterType);
```

Block as a method parameter(Block方法)

```
- (void)someMethodThatTakesABlock:(returnType(^)(parameterType))blockName;
[someObject someMethodThatTakesABlock:^returnType(parameterType)){...}];
```

Block as typedef(Block重定义)

```
typedef returnType(^TypeName)(parameterTypes);
TypeName blockName = ^returnType(parameters){...};
```

Block使用注意事项：

* Block内部可以访问外部变量；

* 默认情况下，Block内部不能修改外部的局部变量

* 给局部变量加上 **__block** 关键字，则这个局部变量可以在Block内部进行修改。