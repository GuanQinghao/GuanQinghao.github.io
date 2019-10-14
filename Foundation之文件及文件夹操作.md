--
> 创建日期：2017年01月29日  
> 修改日期：2018年01月29日  

--
```objc
//字符串数组转路径
NSArray *componentsArray = @[@"var",@"usr",@"doc"];
NSString *pathString = [NSString pathWithComponents:componentsArray];
NSLog(@"%@",pathString);

//查找Documents文件夹路径
NSLog(@"%@",NSHomeDirectory());//Documents上一级目录路径 沙盒内文件路径
//第一种
NSArray *pathArray = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSLog(@"%@",pathArray[0]);
//第二种
NSString *pathOne = [NSHomeDirectory() stringByAppendingString:@"/Documents"];
NSLog(@"%@",pathOne);
//第三种
NSString *pathTwo = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents"];
NSLog(@"%@",pathTwo);

//查找Library文件夹路径
NSArray *pathLibrary = NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES);
NSLog(@"%@",pathLibrary[0]);

//查找Caches文件夹路径 Caches文件夹在Library文件夹内
NSArray *pathCaches = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
NSLog(@"%@",pathCaches[0]);

//查找tmp文件夹路径
NSLog(@"%@",NSTemporaryDirectory());


//字符串编码转NSData
NSData *stringData = [@"nihao" dataUsingEncoding:NSUTF8StringEncoding];
//NSData编码转字符串
NSString *dataString = [[NSString alloc]initWithData:stringData encoding:NSUTF8StringEncoding];

//创建一个文件路径
NSString *path = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/data.txt"];
//按照路径创建文件 写入内容
[[NSFileManager defaultManager]createFileAtPath:path contents:stringData attributes:nil];
//写入已有文件 内容会更新
[@"cocoachina" writeToFile:path atomically:YES encoding:NSUTF8StringEncoding error:nil];

//创建新的文件夹并在文件夹内创建一个文件 写入内容
NSString *directoryPath = [NSHomeDirectory() stringByAppendingString:@"/Documents/media"];//创建文件夹路径
[[NSFileManager defaultManager]createDirectoryAtPath:directoryPath withIntermediateDirectories:YES attributes:nil error:nil];//创建文件夹
NSString *filePath = [directoryPath stringByAppendingString:@"/song.txt"];//创建文件路径
NSData *fileContents = [@"today is sunny day!" dataUsingEncoding:NSUTF8StringEncoding];//创建文件内容
[[NSFileManager defaultManager]createFileAtPath:filePath contents:fileContents attributes:nil];//内容写入文件

/*
 1、获取Info.plist文件内容
 2、创建一个新文件夹
 3、在新文件夹下创建一个文件(mine.plist)
 4、把Info.plist文件内容写入mine.plist
 */

//获取Info.plist文件路径
NSString *plistPath = [[NSBundle mainBundle]pathForResource:@"Info" ofType:@"plist"];
//获取Info.plist文件内容
NSDictionary *dicPlist = [[NSDictionary alloc]initWithContentsOfFile:plistPath];
//获取新文件夹路径
NSString *myDirectoryPath = [NSHomeDirectory() stringByAppendingString:@"/Documents/Plist"];
//创建新文件夹
[[NSFileManager defaultManager]createDirectoryAtPath:myDirectoryPath withIntermediateDirectories:YES attributes:nil error:nil];
//获取新文件路径
NSString *myFilePath = [myDirectoryPath stringByAppendingString:@"/mine.plist"];
//创建新文件
[[NSFileManager defaultManager]createFileAtPath:myFilePath contents:nil attributes:nil];
//字典内容写入新文件
[dicPlist writeToFile:myFilePath atomically:YES];

/*
 将项目中(压缩到.app中)的mp3文件复制到沙盒中并删除
 */

//获取项目中(压缩到.app中)的mp3文件内容
NSString *songPath = [[NSBundle mainBundle]pathForResource:@"song" ofType:@"mp3"];//项目中的文件路径
NSData *song = [NSData dataWithContentsOfFile:songPath];
//在sandbox中创建文件夹及文件
NSString *songDirectoryPath = [NSHomeDirectory() stringByAppendingString:@"/Documents/song"];
[[NSFileManager defaultManager]createDirectoryAtPath:songDirectoryPath withIntermediateDirectories:YES attributes:nil error:nil];
//在sandbox中创建文件
NSString *songFilePath = [songDirectoryPath stringByAppendingString:@"/song.mp3"];
[[NSFileManager defaultManager]createFileAtPath:songFilePath contents:nil attributes:nil];
//将项目中的文件写入sandbox中的新文件
[song writeToFile:songFilePath atomically:YES];

//移除文件或文件夹 移除文件夹会移除文件夹里的文件
[[NSFileManager defaultManager]removeItemAtPath:songDirectoryPath error:nil];
```