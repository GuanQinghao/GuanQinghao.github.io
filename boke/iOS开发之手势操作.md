--
> 创建日期：2017年05月31日  
> 修改日期：2018年01月29日  

--
手势操作

```objc
//单击
UITapGestureRecognizer *click = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(Click:)];
[self.view addGestureRecognizer:click];
//双击
UITapGestureRecognizer *doubleClick = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(DoubleClick:)];
doubleClick.numberOfTapsRequired = 2;
[click requireGestureRecognizerToFail:doubleClick];//取消单击方法
[self.view addGestureRecognizer:doubleClick];
//滑动
UISwipeGestureRecognizer *swipe = [[UISwipeGestureRecognizer alloc]initWithTarget:self action:@selector(Swipe:)];
swipe.direction = UISwipeGestureRecognizerDirectionRight;
[self.view addGestureRecognizer:swipe];
//长按
UIButton *button = [[UIButton alloc]initWithFrame:CGRectMake(100, 100, 50, 50)];
button.backgroundColor = [UIColor redColor];
[button addTarget:self action:@selector(ClickButton:) forControlEvents:UIControlEventTouchUpInside];
[self.view addSubview:button];
UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc]initWithTarget:self action:@selector(LongPress:)];
longPress.minimumPressDuration = 2.0;
[button addGestureRecognizer:longPress];
//平移
UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc]initWithTarget:self action:@selector(Pan:)];
[self.view addGestureRecognizer:pan];
//旋转
UIRotationGestureRecognizer *rotation = [[UIRotationGestureRecognizer alloc]initWithTarget:self action:@selector(Rotation:)];
[self.view addGestureRecognizer:rotation];
```

手势操作触发的方法

```objc
-(IBAction)Click:(id)sender{
    NSLog(@"Click.");
}
-(IBAction)DoubleClick:(id)sender{
    NSLog(@"DoubleClick.");
}
-(IBAction)Swipe:(id)sender{
    NSLog(@"Swipe.");
}
-(IBAction)ClickButton:(id)sender{
    NSLog(@"ClickButton.");
}
//不同的操作逻辑会出现不同的结果
-(IBAction)LongPress:(UILongPressGestureRecognizer*)sender{
    sender.allowableMovement = 25.0;
    if (sender.state==UIGestureRecognizerStateChanged) {
        NSLog(@"StateChanged.");
    }else if (sender.state==UIGestureRecognizerStateEnded){
        NSLog(@"LongPress.");
    }
}
-(IBAction)Pan:(UIPanGestureRecognizer*)sender{
    NSLog(@"Pan.");
}
-(IBAction)Rotation:(UIRotationGestureRecognizer*)sender{
    NSLog(@"Rotation.");
    NSLog(@"%f",sender.rotation*(180/M_PI));
}
```

