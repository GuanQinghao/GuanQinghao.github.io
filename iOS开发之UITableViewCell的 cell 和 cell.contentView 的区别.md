--
> 创建日期：2018年01月29日  
> 修改日期：2018年01月29日  

--
#### 添加子视图 (addSubview)

在UITableViewCell实例上添加子视图有两种方式

```objc
// 第一种方式
[cell addSubview:subView];

// 第二种方式
[cell.contentView addSubview:subView];
```

一般情况下，两种添加子视图的方式没有区别。但是在UITableViewCell多选编辑状态下，直接添加到cell上的子视图不会移动；添加在contentView上的子视图会随着整体移动。推荐使用contentView添加子视图的方式。

#### 设置背景色 (backgroundColor)

设置UITableViewCell的背景色有两种方式

```objc
// 第一种方式
[cell setBackgroundColor:[UIColor whiteColor]];

// 第二种方式
[cell.contentView setBackgroundColor:[UIColor whiteColor]];
```

一般情况下，两种设置背景色的方式效果是一样的。但是在UITableViewCell多选编辑状态下，直接设置cell的背景色可以保证左侧多选框部分的背景色与cell背景色一致；设置contentView背景色，左侧多选框的背景色会是UITableView的背景色或UITableView父视图的背景色。如果需要保证UITableViewCell的背景色一致，必须设置cell的背景色。