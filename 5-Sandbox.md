##沙盒
###1.计算文件大小 
```objective-c
- (NSString *)changeMega:(CGFloat)length {
    //大于等于 1G
    if (length >= 1024 *1024 *1024) {
        int value = (int)length % (1024 *1024 *1024);
        //能整除
        if (value == 0) {
            return [NSString stringWithFormat:@"%dG",(int)length/(1024 *1024 *1024)];
        //带小数的的 G
        }else{
            return [NSString stringWithFormat:@"%.2fG",length/(1024 *1024 *1024)];
        }
    //小于 1G,大于等于 1M
    }else if (length >= 1024 *1024){
        //能整除 M
        if ((int)length %(1024 *1024) == 0) {
            return [NSString stringWithFormat:@"%dM",(int)length / (1024 *1024)];
        //带小数点的 M
        }else{
            return [NSString stringWithFormat:@"%.2fM",length/(1024 *1024)];
        }
    //小于 1G,小于 1M, 大于 1KB
    }else if (length >= 0){
        //能整除 KB
        if ((int)length %1024 == 0) {
            return [NSString stringWithFormat:@"%dKB",(int)length /1024];
        //带小数点的 KB
        }else{
            return [NSString stringWithFormat:@"%.2fKB",length /1024];
        }
    }
    return nil;
}
```
###2.管理文件 
如果 `error` 指定为 `nil`，就会采用默认的行为

```objective-c
// 从一个文件中读取数据
- (NSData *)contentsAtPath:path;
```

```objective-c
// 向一个文件写入数据
- (BOOL)createFileAtPath:path contents:(NSData *)data attributes:attr;
```

```objective-c
// 删除一个文件
- (BOOL)removeItemAtPath:path error:error;
```

```objective-c
// 重命名或者移动一个文件（to 是不能已经存在的）
- (BOOL)moveItemAtPath:from toPath:to error:err;
```

```objective-c
// 复制文件（to 是不能已经存在的）
-(BOOL)copyItemAtPath:from toPath:to error:err;
```

```objective-c
// 比较这两个文件的内容
- (BOOL)contentsEqualAtPath:path1 andPath: path2;
```

```objective-c
// 测试文件是否存在
- (BOOL)fileExistsAtPath:path;
```

```objective-c
// 测试文件是否存在, 并且是否能执行读操作
- (BOOL)isReadableFileAtPath:path;
```

```objective-c
// 测试文件是否存在, 并且是否能执行写操作
- (BOOL)isWritableFileAtPath:path;
```

```objective-c
// 获取文件的属性：文件大小、文件创建日期等
- (NSDictionary *)attributesOfItemAtPath:path error:err;
```

```objective-c
// 更改文件的属性
- (BOOL)setAttributesOfItemAtPath:path error:err;
```

1. `moveItemAtPath`
可以用来将文件从一个目录移到另一个目录中（也可以用来移动整个目录），如果两个路径引用同一目录中的文件，其结果仅仅是重新命名这个文件。
2. `contentsAtPath`
       仅仅接受一个路径名，并将指定文件的内容读入该方法创建的存储区，如果读取成功，这个方法将返回存储区对象作为结果，否则将返回 `nil`，（例如，文件不存在或者你不能读取）。
3. `createFileAtPath:path contents:(NSData *)data attributes:attr` 创建了一个具有特定属性的文件（或者如果 `attributes` 参数提供为 `nil`，则采用默认的属性值）。然后，将指定的 `NSData` 对象内容写入这个文件中。
###3.使用目录
```objective-c
// 获取当前目录
- (NSString *)currentDirectoryPath;
```

```objective-c
// 更改当前目录
- (BOOL)changeCurrentDirectoryPath:path;
```

```objective-c
// 列出目录内容
- (NSArray *)contentsOfDirectoryAtPath:path error:err;
```

```objective-c
// 枚举目录的内容
- (NSDirectoryEnumerator *)enumeratorAtPath:path;
```




