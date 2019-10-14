--
> 创建日期：2017年04月19日  
> 修改日期：2018年01月29日  

--
在工作中遇到一个问题，应用程序向后台发送评论时，假如评论中含有特殊符号，比如空格、回车等特殊符号，程序会出现错误以致闪退。出现这种问题是因为网络请求发送评论内容的URL参数含有特殊符号。对于这个问题通过转码的方式就可以解决，现提供两种解决方法。

### 第一种方法：

将内容直接进行UTF8转码。

需要通过URL传送的内容

```objc
//需要通过URL传送的内容
NSString *preContent = @"你好，世界！H e l l o ,>>;\n\t@&^~ W o r l d !";
```
将原文进行UTF8转码，iOS9后已经弃用

```objc
//将原文进行UTF8转码，iOS9后已经弃用
NSString *postContent = [preContent stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];//原文转码文
NSString *originalContent = [preContent stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];//码文转原文
```
将原文进行UTF8转码，iOS9后新的方法

```objc
//将原文进行UTF8转码，iOS9后新的方法
NSCharacterSet *letterCharacterSet = [NSCharacterSet URLPathAllowedCharacterSet];
NSString *postContent = [preContent stringByAddingPercentEncodingWithAllowedCharacters:letterCharacterSet];//原文转码文
NSString *originalContent = [postContent stringByRemovingPercentEncoding];//码文转原文
```

### 第二种方法：

Foundation框架中的NSSring类型和CoreFoundation框架中的CFStringRef类型是可以互相直接转换，因此将NSString类型转换成CFStringRef类型后进行转码，然后再转换成NSString类型。

```objc
NSString *postContent = (NSString *)CFBridgingRelease(CFURLCreateStringByAddingPercentEscapes(kCFAllocatorDefault,(CFStringRef) preContent,NULL,NULL,kCFStringEncodingUTF8));//原文转码文
NSString *originalContent = (NSString *)CFBridgingRelease(CFURLCreateStringByReplacingPercentEscapesUsingEncoding(kCFAllocatorDefault, (CFStringRef) preContent,NULL,kCFStringEncodingUTF8));//码文转原文
```
但是这种方法在iOS9后都被弃用了，在Xcode8中原文转码文的方法由上述第一种方法代替，但是新增了一个码文转原文的方法。

```objc
NSString *originalContent = (NSString *)CFBridgingRelease(CFURLCreateStringByReplacingPercentEscapes(kCFAllocatorDefault, (CFStringRef) preContent,NULL));//码文转原文
```