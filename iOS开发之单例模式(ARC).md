--
> 创建日期：2017年10月12日  
> 修改日期：2018年01月29日  

--
### 单例模式基本概念

单例模式是一种常用的软件设计模式，单例的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例对象而且该实例易于外界访问。

### 单例模式使用情况

* 只需要一个实例，节省资源

* 多个模块使用同一个变量

### ARC环境创建单例模式基本步骤

1、实现NSCopying协议

```objc
@interface Person : NSObject<NSCopying>//遵守NSCoding协议

//单例方法一般以default  share  stand单词开头
+ (instancetype)sharePerson;

@end
```

2、声明一个静态单例实例对象，并初始化为nil

```objc
static Person *person = nil;
```

3、声明一个类的工厂方法，生成且只生成一个该类的实例

```objc
+ (instancetype)sharePerson {
    //多线程防死锁
    @synchronized (self) {
        if (person == nil) {
            person = [[self alloc]init];
        }
    }
    return person;
}
```

4、覆盖allcoWithZone和copyWithZone方法，确保用户在alloc和copy时不会产生多余对象

```objc
+ (instancetype)allocWithZone:(struct _NSZone *)zone {
    @synchronized (self) {
        if (person == nil) {
            person = [super allocWithZone:zone];
        }
        return person;
    }
    return nil;
}

//实现NSCopying协议
- (id)copyWithZone:(NSZone *)zone {
    return self;
}
```

5、使用单例

```objc
Person *person0 = [Person sharePerson];//类名调用单例类方法
Person *person1 = [[Person alloc]init];//alloc调用单例allocWithZone方法
Person *person2 = [person0 copy];
```
