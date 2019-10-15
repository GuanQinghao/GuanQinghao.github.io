--
> 创建日期：2017年08月03日  
> 修改日期：2018年01月29日  

--
iOS中的状态栏样式有两种：

一种是浅色背景下状态栏文字颜色为**黑色(UIStatusBarStyleDefault)**  
一种是深色背景下状态栏文字颜色为**白色(UIStatusBarStyleLightContent)**

### 全局设置状态栏样式

在 Info.plist 文件中添加字段 ***View controller-based status bar appearance*** 设置为 Boolean 类型值为 **NO**，继续添加字段 ***Status bar style*** 设置为 String 类型值为 **UIStatusBarStyleDefault** 或 **UIStatusBarStyleLightContent** 。

### 局部设置状态栏样式

在 Info.plist 文件中的字段 ***View controller-based status bar appearance*** 值设置为 **YES**，然后在控制器( UIViewController )中添加方法：

```objc
- (UIStatusBarStyle)preferredStatusBarStyle {
    
    return UIStatusBarStyleLightContent;
}

- (BOOL)prefersStatusBarHidden {
    
    return NO;
}
```

但是当应用中存在导航控制器( UINavigationController )时，上述方法无效，此时可以设置导航条( navigationBar )的 barStyle：

```objc
//状态栏文字颜色为黑色 背景色为白色
self.navigationController.navigationBar.barStyle = UIBarStyleDefault;
    
//状态栏文字颜色为白色 背景色为黑色
self.navigationController.navigationBar.barStyle = UIBarStyleBlack;
```