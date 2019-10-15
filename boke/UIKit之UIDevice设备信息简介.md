--
> 创建日期：2017年07月27日  
> 修改日期：2018年01月29日  

--

```objc
 //获取当前设备
 UIDevice *device = [UIDevice currentDevice];
 
 //设备和系统基本信息
 NSLog(@"设备名称：%@", device.name);
 NSLog(@"设备类型：%@", device.model);
 NSLog(@"本地化模式：%@", device.localizedModel);
 NSLog(@"系统名称:%@", device.systemName);
 NSLog(@"系统版本：%@", device.systemVersion);
 NSLog(@"设备朝向：%ld", device.orientation);
 NSLog(@"UUID:%@", device.identifierForVendor.UUIDString);
 
 //判断设备种类
 if (device.userInterfaceIdiom == UIUserInterfaceIdiomPhone) {
     NSLog(@"iPhone 设备");
 }
 if (device.userInterfaceIdiom == UIUserInterfaceIdiomPad) {
     NSLog(@"iPad 设备");
 }
 if (device.userInterfaceIdiom == UIUserInterfaceIdiomTV) {
     NSLog(@"Apple TV设备");
 }
 if (device.userInterfaceIdiom == UIUserInterfaceIdiomCarPlay){
     NSLog(@"车载设备");
 }
 
 //电池相关信息
 //设置电池是否被监视
 device.batteryMonitoringEnabled = YES;
 //判断当前电池状态
 if (device.batteryState == UIDeviceBatteryStateUnknown) {
     NSLog(@"UnKnow");
 }
 if (device.batteryState == UIDeviceBatteryStateUnplugged){
     NSLog(@"未充电");
 }
 if (device.batteryState == UIDeviceBatteryStateCharging){
     NSLog(@"正在充电，电量未满");
 }
 if (device.batteryState == UIDeviceBatteryStateFull){
     NSLog(@"正在充电，电量已满");
 }
 
 //当前电量等级 [0.0, 1.0]
 NSLog(@"%f",device.batteryLevel);
 
 //红外线感应
 //开启红外感应-- 用于检测手机是否靠近面部
 device.proximityMonitoringEnabled = YES;
 if (device.proximityState == YES) {
     NSLog(@"靠近面部");
 } else {
     NSLog(@"没有靠近");
 }
 
 //多任务环境监测
 //判断当前系统是否支持多任务
 if (device.isMultitaskingSupported == YES) {
     NSLog(@"支持多任务!!!");
 } else {
     NSLog(@"不支持多任务！！！");
 }
```