##时间转换/比较
###1.时间戳转换为字符串 
```objective-c
- (NSString *)transformTimestamp:(NSTimeInterval)timeInterval {
    NSDate *date = [NSDate dateWithTimeIntervalSince1970:timeInterval];
    NSDateFormatter *formatter = [[NSDateFormatter alloc]init];
    [formatter setDateFormat:@"yyyy-MM-dd HH:MM:SS"];
    NSString *str = [formatter stringFromDate:date];
    return str;
}
```
###2.字符串转换为时间
```objective-c
- (void)changeTime:(NSString *)timeStr {
    // 1.创建一个时间格式化对象
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    
    // 2.格式化对象的样式/z大小写都行/格式必须严格和字符串时间一样
    formatter.dateFormat = @"yyyy-MM-dd HH:mm:ss Z";
    
    // 3.利用时间格式化对象让字符串转换成时间 (自动转换0时区/东加西减)
    NSDate *date = [formatter dateFromString:timeStr];
    
    NSLog(@"%@",date);
}
```
###3.计算两个时间戳之间的差值 
####Method 1
```objective-c
- (NSString *)calculate:(NSTimeInterval)date1 date2:(NSTimeInterval)date2 {
    int time = date2 - date1;
    int days = time /(3600 *24);
    int hours = time %(3600 *24) /3600;
    int minutes = time %(3600 *24) %3600 /60;
    int seconds = time %(3600 *24) %3600 %60;
    if (days > 0) {
        return [NSString stringWithFormat:@"还剩%d天%d小时%d分钟%d秒",days,hours,minutes,seconds];
    }else if (days == 0 && hours > 0 && minutes > 0 && seconds > 0){
        return [NSString stringWithFormat:@"还剩%d小时%d分钟%d秒",hours,minutes,seconds];
    }else if (days == 0 && hours > 0 && minutes > 0 && seconds == 0){
        return [NSString stringWithFormat:@"还剩%d小时%d分钟",hours,minutes];
    }else if (days == 0 && hours > 0 && minutes == 0 && seconds > 0){
        return [NSString stringWithFormat:@"还剩%d小时%d秒",hours,seconds];
    }else if (days == 0 && hours == 0 && minutes > 0 && seconds > 0){
        return [NSString stringWithFormat:@"还剩%d分钟%d秒",minutes,seconds];
    }else if (days == 0 && hours == 0 && minutes > 0 && seconds == 0){
        return [NSString stringWithFormat:@"还剩%d分钟",minutes];
    }else if (days == 0 && hours == 0 && minutes == 0 && seconds > 0){
        return [NSString stringWithFormat:@"还剩%d秒",seconds];
    }
    return 0;
}
```
####Method 2
```objective-c
- (void)compare:(NSString *)timeStr {
    // 1.创建一个时间格式化对象
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
    
    // 2.格式化对象的样式/z大小写都行/格式必须严格和字符串时间一样
    formatter.dateFormat = @"yyyy-MM-dd HH:mm:ss Z";
    
    // 3.字符串转换成时间/自动转换0时区/东加西减
    NSDate *date = [formatter dateFromString:timeStr];
    NSDate *now = [NSDate date];
    
    // 注意获取calendar,应该根据系统版本判断
    NSCalendar *calendar = [NSCalendar currentCalendar];
    
    NSCalendarUnit type = NSCalendarUnitYear   |
                          NSCalendarUnitMonth  |
                          NSCalendarUnitDay    |
                          NSCalendarUnitHour   |
                          NSCalendarUnitMinute |
                          NSCalendarUnitSecond;
    
    // 4.获取了时间元素
    NSDateComponents *cmps = [calendar components:type fromDate:date toDate:now options:0];
    
    NSLog(@"%ld年%ld月%ld日%ld小时%ld分钟%ld秒钟", cmps.year, cmps.month, cmps.day, cmps.hour, cmps.minute, cmps.second);
}
```