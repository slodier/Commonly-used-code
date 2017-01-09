##Quartz-2D
###1.图片顺时针旋转 90° 
```objective-c
- (UIImage *)rotateImage:(UIImage *)image
{
    long double rotate = 0.0;
    CGRect rect;
    float translateX = 0;
    float translateY = 0;
    float scaleX = 1.0;
    float scaleY = 1.0;
    
    rotate = 3 * M_PI_2;
    rect = CGRectMake(0, 0, image.size.height, image.size.width);
    translateX = -rect.size.height;
    translateY = 0;
    scaleY = rect.size.width/rect.size.height;
    scaleX = rect.size.height/rect.size.width;
    
    UIGraphicsBeginImageContext(rect.size);
    CGContextRef context = UIGraphicsGetCurrentContext();
    // 做CTM变换
    CGContextTranslateCTM(context, 0.0, rect.size.height);
    CGContextScaleCTM(context, 1.0, -1.0);
    CGContextRotateCTM(context, rotate);
    CGContextTranslateCTM(context, translateX, translateY);
    
    CGContextScaleCTM(context, scaleX, scaleY);
    // 绘制图片
    CGContextDrawImage(context, CGRectMake(0, 0, rect.size.width, rect.size.height), image.CGImage);
    // 从上下文获得新的图片
    UIImage *newPic = UIGraphicsGetImageFromCurrentImageContext();
    return newPic;
}
```
###2.图片上绘制虚线(存在小 bug, 绘制虚线的时候,图片有轻微变动)
```objective-c
- (UIImage *)dotted:(UIImageView *)imageView
{
    //创建一个基于位图的上下文，大小为 imageView 的大小
    UIGraphicsBeginImageContext(imageView.frame.size);
    //绘图位置，相对画布顶点而言
    [imageView.image drawInRect:imageView.bounds];
    //设置端点的格式
    CGContextSetLineCap(UIGraphicsGetCurrentContext(), kCGLineCapSquare);//绘制方形端点
    //{10，10} 表示先绘制 10 个点，再跳过 10 个点，重复。。。
    //{10，20，10} 表示先绘制 10 个，跳过 20 个，绘制 10 个点，跳过 10 个，绘制 20 个，重复。。相应的 count 要等于lengths 的个数
    CGFloat lengths[] = {10,10};
    CGContextRef lineRef = UIGraphicsGetCurrentContext();
    //虚线的颜色
    CGContextSetStrokeColorWithColor(lineRef, [UIColor redColor].CGColor);
    //phase 表示绘制的时候跳过多少个点
    CGContextSetLineDash(lineRef, 0, lengths, 2);
    //绘制的起点
    CGContextMoveToPoint(lineRef, 0.0, 20);
    //终点
    CGContextAddLineToPoint(lineRef, imageView.frame.size.width, 60);
    //画线
    CGContextStrokePath(lineRef);
    
    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
    return newImage;
}
```