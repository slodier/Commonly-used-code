##颜色相关
###1.宏定义 
```Objective-C
// 透明度为1
#define KColorWithRGB(r, g, b) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:1]
// 自定义透明度
#define KColorWithRGBAlpha(r, g, b, a) [UIColor colorWithRed:(r)/255.0 green:(g)/255.0 blue:(b)/255.0 alpha:a]
```

###2.16进制颜色转为 UIColor
```Objective-C
+ (UIColor *)getColorFromHEX:(NSString *)hex {
    hex = [[hex uppercaseString] substringFromIndex:1];
    CGFloat valueArray[3];
    NSArray *strArray = [NSArray arrayWithObjects:[hex substringWithRange:NSMakeRange(0, 2)], [hex substringWithRange:NSMakeRange(2, 2)], [hex substringWithRange:NSMakeRange(4, 2)], nil];
    for(int i = 0; i<strArray.count; i++){
        hex = strArray[i];
        CGFloat value = ([hex characterAtIndex:0] - 'A' + 10) *16.0f + [hex characterAtIndex:1] - 'A' + 10;
        valueArray[i] = value;
    }
    return [UIColor colorWithRed:valueArray[0] /255 green:valueArray[1] /255 blue:valueArray[2] /255 alpha:1];
}
```