--
> 创建日期：2017年07月15日  
> 修改日期：2018年01月29日  

--
根据字符串生成二维码图片，图片格式为CIImage

```objc
#pragma mark - 生成二维码
- (CIImage *)createQRFromString:(NSString *)QRString {
    NSData *stringData = [QRString dataUsingEncoding:NSUTF8StringEncoding];
    CIFilter *QRFilter = [CIFilter filterWithName:@"CIQRCodeGenerator"];
    [QRFilter setValue:stringData forKey:@"inputMessage"];
    [QRFilter setValue:@"M" forKey:@"inputCorrectionLevel"];
    return QRFilter.outputImage;
}
```

生成的CIImage格式二维码图片的大小不好控制，转换成可以控制大小的UIImage

```objc
#pragma mark - 将CIImage转换成可以控制大小的UIImage
- (UIImage *)createNonInterpolatedUIImageFormCIImage:(CIImage *)image withSize:(CGFloat) size {
    CGRect extent = CGRectIntegral(image.extent);
    CGFloat scale = MIN(size/CGRectGetWidth(extent), size/CGRectGetHeight(extent));
    size_t width = CGRectGetWidth(extent) * scale;
    size_t height = CGRectGetHeight(extent) * scale;
    CGColorSpaceRef cs = CGColorSpaceCreateDeviceGray();
    CGContextRef bitmapRef = CGBitmapContextCreate(nil, width, height, 8, 0, cs, (CGBitmapInfo)kCGImageAlphaNone);
    CIContext *context = [CIContext contextWithOptions:nil];
    CGImageRef bitmapImage = [context createCGImage:image fromRect:extent];
    CGContextSetInterpolationQuality(bitmapRef, kCGInterpolationNone);
    CGContextScaleCTM(bitmapRef, scale, scale);
    CGContextDrawImage(bitmapRef, extent, bitmapImage);
    CGImageRef scaledImage = CGBitmapContextCreateImage(bitmapRef);
    CGContextRelease(bitmapRef);
    CGImageRelease(bitmapImage);
    return [UIImage imageWithCGImage:scaledImage];
}
```