--
> 创建日期：2017年01月29日  
> 修改日期：2018年01月29日  

--

集合

```objc
NSArray *array0 = [NSArray arrayWithObjects:@"1",@"2",@"3",@"4",@"5", nil];

//类方法创建集合
NSSet *set0 = [NSSet setWithObject:@"hello"];//单元素集合
NSSet *set1 = [NSSet setWithObjects:@"one",@"two",@"three",@"four",@"five", nil];//罗列元素定义集合
NSSet *set2 = [NSSet setWithArray:array0];//根据数组定义集合

//实例方法创建集合
NSSet *set3 = [[NSSet alloc]initWithObjects:@"zero",@"one",@"two",@"three",@"four",@"five", nil];
NSSet *set4 = [[NSSet alloc]initWithArray:@[@"one",@"two",@"three",@"four"]];

[set4 count];//返回集合元素个数
[set4 anyObject];//返回集合任一元素
[set4 allObjects];//返回集合所有元素

//集合运算 相同 子集 交集 特定值
[set0 isEqualToSet:set1];//集合是否相同 返回值为BOOL
[set1 isSubsetOfSet:set2];//是否为子集 返回值为BOOL
[set2 intersectsSet:set3];//集合是否相交 返回值为BOOL
[set3 containsObject:@"zero"];//集合含有特定值 返回值为BOOL

//判断数组元素是否在集合中 只要一个在就返回yes
NSSet *set5 = [NSSet setWithObjects:@"one",@"two",@"three",@"four",@"five", nil];
NSArray *array1 = [NSArray arrayWithObjects:@"four",@"five", nil];
//第一种遍历方法
for (int x = 0; x < array1.count; x++) {
    BOOL yesOrNot = [set5 containsObject:[array1 objectAtIndex:x]];
    if (yesOrNot) {
    NSLog(@"集合包含数组元素");
    break;
    }
}
//第二种遍历方法
for (id x in array1) {
    BOOL yesOrNot = [set5 containsObject:x];
    if (yesOrNot) {
    NSLog(@"集合包含数组元素");
    break;
    }
}

//可变集合
//定义可变集合
NSMutableSet *mutableSet0 = [[NSMutableSet alloc]initWithObjects:@"one",@"two",@"three",@"four",@"five",nil];
NSMutableSet *mutableSet1 = [[NSMutableSet alloc]initWithObjects:@"one",@"two",@"3",@"4",@"5",nil];
//从第一个集合中删除两者相同的元素 第二个内元素不变
[mutableSet0 minusSet:mutableSet1];
//保留两者的交集
[mutableSet0 intersectSet:mutableSet1];
//保留并集
[mutableSet0 unionSet:mutableSet1];
//添加指定元素
[mutableSet0 addObject:@"six"];
//数组元素添加到集合
[mutableSet0 addObjectsFromArray:@[@"seven",@"eight"]];
//移除指定元素
[mutableSet0 removeObject:@"one"];
```

分类-去除字符串中的空格

```objc
-(NSString*)removeSpace{
    NSString *string = [self stringByTrimmingCharactersInSet: [NSCharacterSet whitespaceAndNewlineCharacterSet]];
    return string;
}
```

分类-	移除数组中包含的元素

```objc
-(void)removeObjectsWithArray:(NSArray*)array{
    for (id x in array) {
        [self removeObject:x];
    }
}
```

分类-移除集合中数组包含的元素

```objc
-(void)RemoveSet:(NSMutableSet*)mutableSet WithArray:(NSArray*)array{
    for (id x in array) {
        [mutableSet removeObject:x];
    }
}
```

分类-集合中是否包含数组中的元素

```objc
-(BOOL)Contains:(NSArray*)array OrNot:(NSSet*)set{
    for (int x = 0; x < array.count; x++) {
        BOOL yesOrNot = [set containsObject:[array objectAtIndex:x]];
        if (yesOrNot) {
            return yesOrNot;
            break;
        }
    }
    return NO;
}
```