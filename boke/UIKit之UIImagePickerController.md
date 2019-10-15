--
> 创建日期：2017年09月19日  
> 修改日期：2018年01月29日  

--
真机包括相机和相册，模拟器只有相册

```objc
-(IBAction)choosePhotoOrCamera:(id)sender{
    UIAlertController *myAlert = [UIAlertController alertControllerWithTitle:nil message:nil preferredStyle:UIAlertControllerStyleActionSheet];
    //真机和模拟器
    if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
        [myAlert addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
            //取消选择操作
            
        }]];
        
        [myAlert addAction:[UIAlertAction actionWithTitle:@"相机" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            //选择相机
            sourceType = 1;
            [self performSelector:@selector(showResult:) withObject:nil afterDelay:0.3];
        }]];
        
        [myAlert addAction:[UIAlertAction actionWithTitle:@"相册" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            //选择相册
            sourceType = 2;
            [self performSelector:@selector(showResult:) withObject:nil afterDelay:0.3];
        }]];
    }else{
        [myAlert addAction:[UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
            //取消选择操作
            
        }]];
        
        [myAlert addAction:[UIAlertAction actionWithTitle:@"相册" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            //选择相册
            sourceType = 2;
            [self performSelector:@selector(showResult:) withObject:nil afterDelay:0.3];
        }]];
    }
    [self presentViewController:myAlert animated:YES completion:nil];
}

//图片选择器
-(IBAction)showResult:(id)sender{
    UIImagePickerController *imagePickerView = [[UIImagePickerController alloc]init];
    imagePickerView.delegate = self;
    imagePickerView.allowsEditing = YES;
    imagePickerView.sourceType = sourceType;
    [self presentViewController:imagePickerView animated:YES completion:nil];
}
```