--
> 创建日期：2017年07月16日  
> 修改日期：2018年01月29日  

--
Alert视图简单用法

```objc
//添加Alert视图
- (IBAction)newAlert:(id)sender {
    UIAlertController *newAlert = [UIAlertController alertControllerWithTitle:@"警告框" message:@"这是新的警告框信息栏" preferredStyle:UIAlertControllerStyleAlert];
    //添加用户输入框
    [newAlert addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
        textField.keyboardType = UIKeyboardTypeDefault;
        textField.backgroundColor = [UIColor redColor];
    }];
    //添加密码输入框
    [newAlert addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
        textField.keyboardType = UIKeyboardTypeNumberPad;
        textField.secureTextEntry = YES;
        textField.backgroundColor = [UIColor blueColor];
    }];
    //添加点击按钮操作
    [newAlert addAction:[UIAlertAction actionWithTitle:@"默认" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击默认按钮");//点击事件代码
        //用户登录
        NSString *name = [newAlert textFields][0].text;
        NSString *password = [newAlert textFields][1].text;
        
        if ([name isEqualToString:@"username"] && [password isEqualToString:@"123"]) {
            NSLog(@"登陆成功");
        }else {
            NSLog(@"登录失败");
        }
    }]];
    [newAlert addAction:[UIAlertAction actionWithTitle:@"警告" style:UIAlertActionStyleDestructive handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击警告按钮");//点击事件代码
    }]];
    [newAlert addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击取消按钮");//点击事件代码
    }]];
    //控制器添加到视图上
    [self presentViewController:newAlert animated:YES completion:nil];
}
```

ActionSheet视图简单用法

```objc
//添加ActionSheet视图
- (IBAction)newActionSheet:(id)sender {
    UIAlertController *newAlert = [UIAlertController alertControllerWithTitle:@"警告框" message:@"这是新的警告框信息栏" preferredStyle:UIAlertControllerStyleActionSheet];
    //添加点击按钮操作
    [newAlert addAction:[UIAlertAction actionWithTitle:@"默认" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击默认按钮");//点击事件代码
    }]];
    [newAlert addAction:[UIAlertAction actionWithTitle:@"警告" style:UIAlertActionStyleDestructive handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击警告按钮");//点击事件代码
    }]];
    [newAlert addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        NSLog(@"点击取消按钮");//点击事件代码
    }]];
    //控制器添加到视图上
    [self presentViewController:newAlert animated:YES completion:nil];
}
```