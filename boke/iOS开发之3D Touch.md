--
> 创建日期：2017年01月29日  
> 修改日期：2018年01月29日  

--
### 3D Touch功能环境

* 系统：iOS 9及以上
* 机型：iPhone6s/6sp及以后

### 检测是否支持3DTouch

系统中检测设备是否支持3D Touch的属性：

```objc
@property(nonatomic, readonly) UIForceTouchCapability forceTouchCapability;
```

UIForceTouchCapability 是一个枚举类型：

```objc
UIForceTouchCapabilityUnknown //3D Touch检测失败
UIForceTouchCapabilityUnavailable //3D Touch不可用
UIForceTouchCapabilityAvailable //3D Touch可用
```

判断设备是否开启3D Touch功能的方法：

```objc
if (self.traitCollection.forceTouchCapability == UIForceTouchCapabilityAvailable) {
    NSLog(@"3D Touch功能可用!");
    //注册3DTouch的peek（预览）和pop功能
    [self registerForPreviewingWithDelegate:self sourceView:view];
} else if (self.traitCollection.forceTouchCapability == UIForceTouchCapabilityUnavailable) {
    NSLog(@"3D Touch功能不可用!");
} else {
    NSLog(@"3D Touch功能状态未知!");//UIForceTouchCapabilityUnknown
}
```

如果用户修改了设备的3D Touch功能，可以重新检测：

```objc
- (void)traitCollectionDidChange:(UITraitCollection *)previousTraitCollection {
    //check 3D Touch
}
```

### 3D Touch--Home Screen Quick Action

用力按压手机主屏上的App图标，会调出App快捷菜单，这就是主屏上的3D Touch功能。

每一个App的Quick Action最多添加四个(App上架后商店会自动添加一个Share)，每个Action是使用UIApplicationShortcutItem对象进行描述。UIApplicationShortcutItem对象中包含的信息：

|名称|说明|是否必须|
|:---|---|:---:|
|UIApplicationShortcutItemType|唯一标识|是|
|UIApplicationShortcutItemTitle|标题|是|
|UIApplicationShortcutItemSubtitle|子标题|否|
|UIApplicationShortcutItemIconType|系统图标类型|否|
|UIApplicationShortcutItemIconFile|自定义图标|否|
|UIApplicationShortcutItemUserInfo|用户信息|否|

**注意事项**

* UIApplicationShortcutItemTitle属性在没有子标题的情况下标题过长会自动换行
* UIApplicationShortcutItemIconType属性和UIApplicationShortcutItemIconFile属性不能同时起作用
* UIApplicationShortcutItemIconFile属性自定义图标是单一颜色，大小为35x35

#### 创建方式

创建Quick Action有两种方式：静态和动态。

##### 静态方式创建

静态创建是在项目的Info.plist文件中添加UIApplicationShortcutItem数组对象，对象中添加属性及属性值，需手动输入。

##### 动态方式创建

iOS 9中UIApplication对象新增shortcutItems属性用来管理3D Touch Quick Action，在程序初始化是通过代码修改shortcutItems属性可以动态创建、更改Quick Action。

```objc
//创建应用程序主屏3D Touch快捷选项 -- Home Screen Quick Actions
- (void)creatAppShortcutItems {
    //创建系统风格的icon
    UIApplicationShortcutIcon *systemAddIcon = [UIApplicationShortcutIcon iconWithType:UIApplicationShortcutIconTypeAdd];
    //创建应用程序主屏3D Touch快捷选项
    UIApplicationShortcutItem *addShortcut = [[UIApplicationShortcutItem alloc]initWithType:@"identifier.add" localizedTitle:@"Add" localizedSubtitle:@"" icon:systemAddIcon userInfo:nil];
    
    //创建系统风格的icon
    UIApplicationShortcutIcon *composeIcon = [UIApplicationShortcutIcon iconWithType:UIApplicationShortcutIconTypeCompose];
    //创建应用程序主屏3D Touch快捷选项
    UIApplicationShortcutItem *composeShortcut = [[UIApplicationShortcutItem alloc]initWithType:@"identifier.compose" localizedTitle:@"Compose" localizedSubtitle:@"" icon:composeIcon userInfo:nil];
    
    //创建系统风格的icon
    UIApplicationShortcutIcon *systemSearchIcon = [UIApplicationShortcutIcon iconWithType:UIApplicationShortcutIconTypeSearch];
    //创建应用程序主屏3D Touch快捷选项
    UIApplicationShortcutItem *searchShortcut = [[UIApplicationShortcutItem alloc]initWithType:@"identifier.search" localizedTitle:@"Search" localizedSubtitle:@"" icon:systemSearchIcon userInfo:nil];
    
    //创建自定义风格的icon 图片大小35×35
    UIApplicationShortcutIcon *customPraiseIcon = [UIApplicationShortcutIcon iconWithTemplateImageName:@"icon.png"];
    //创建应用程序主屏3D Touch快捷选项
    UIApplicationShortcutItem *praiseShortcut = [[UIApplicationShortcutItem alloc]initWithType:@"identifier.praise" localizedTitle:@"Praise" localizedSubtitle:@"" icon:customPraiseIcon userInfo:nil];
    
    //添加到快捷选项数组  启用3D Touch功能后应用上架自动添加一个分享快捷选项
    [UIApplication sharedApplication].shortcutItems = @[addShortcut,composeShortcut,searchShortcut,praiseShortcut];
    
}
```

#### Quick Action跳转App内的页面

3D Touch主屏App快捷菜单跳转App内的页面有两种方式，**present**和**push**。这两种方式之间是有区别的，present的子页面不能返回到它的父页面，而push的子页面可以pop回它的父页面。

```objc
-(void)application:(UIApplication *)application performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem completionHandler:(void (^)(BOOL))completionHandler {
    
    //push跳转子页面
    if ([shortcutItem.type isEqualToString:@"identifier.add"] ) {
        //创建跳转到子页面
        AddViewController *addViewController = [[AddViewController alloc]init];
        
        //获取主窗口的导航视图，push到子页面
        UIWindow *mainWindow = [[UIApplication sharedApplication] keyWindow];
        UINavigationController *navigationController =(UINavigationController *)mainWindow.rootViewController;
        UIViewController *viewController = [navigationController.viewControllers objectAtIndex:0];
        [viewController.navigationController pushViewController:addViewController animated:YES];
    }
    //present跳转子页面
    if ([shortcutItem.type isEqualToString:@"identifier.search"] ) {
        //创建跳转到子页面
        SearchViewController *searchViewController = [[SearchViewController alloc]init];
        
        //任意UIViewController，present子页面
        [self.window.rootViewController presentViewController:searchViewController animated:YES completion:nil];
    }
}
```

### App内部的3DTouch

待续

