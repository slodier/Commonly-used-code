##网络相关
###1.获取 MIMEType
```Objective-C
- (void)readMIMEType:(NSString *)urlStr {
    
    dispatch_queue_t queue = dispatch_queue_create("com.test.mimetype", NULL);
    dispatch_async(queue, ^{
        
        NSURL *url = [NSURL URLWithString:urlStr];
        NSURLRequest *request = [NSURLRequest requestWithURL:url];
        NSURLSession *session = [NSURLSession sharedSession];
        
        NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
            if (!error) {
                
                // 1.获取 MIMEType
                NSLog(@"response:%@",response.MIMEType);
                
                // 2.获取 allHeaderFields
                NSHTTPURLResponse *httpURLResponse = (NSHTTPURLResponse *)response;
                NSLog(@"allHeaderFields:%@",httpURLResponse.allHeaderFields);
                
            }else{
                
                NSLog(@"error:%@",error);
            }
        }];
        [dataTask resume];
    });
}
```

###2.下载
```Objective-C
- (void)prepareDownload:(NSString *)urlStr {
    
    __weak typeof(self) weakSelf = self;
    
    dispatch_queue_t queue = dispatch_queue_create("com.test.download", NULL);
    dispatch_async(queue, ^{
        
        __strong typeof(weakSelf) strongSelf = weakSelf;
        
        //根据 url, 获取文件名及后缀
        NSURL *url = [NSURL URLWithString:ImageUrl];
        
        NSString *suffix = [NSString stringWithFormat:@"%@",url.lastPathComponent];
        
        // 本地是否存在已下载文件
        if ([strongSelf isFileExist:suffix]) {
            
            // your code ...
        }else{
            
            [strongSelf download:ImageUrl filename:suffix];
        }
    });
}

#pragma mark - 下载及检测本地有无文件
- (BOOL)isFileExist:(NSString *)filename {
    // 文件路径
    NSString *cachePath = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)lastObject];
    NSString *filePath = [cachePath stringByAppendingPathComponent:filename];
    // 检测本地有无已下载过的文件
    NSFileManager *manager = [NSFileManager defaultManager];
    if ([manager fileExistsAtPath:filePath]) {
        return NO;
    }else{
        NSLog(@"file don't exist");
        return YES;
    }
    return NO;
}

#pragma mark - 下载
- (void)download:(NSString *)urlStr filename:(NSString *)filename {
    NSURL *downloadUrl = [NSURL URLWithString:urlStr];
    NSURLRequest *request = [NSURLRequest requestWithURL:downloadUrl];
    NSURLSession *session = [NSURLSession sharedSession];
    NSURLSessionDownloadTask *downloadTask = [session downloadTaskWithRequest:request completionHandler:^(NSURL * _Nullable location, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        if(!error){
            // location 是下载后的临时保存路径，需要将它移动到需要保存的位置
            NSError *saveError = nil;
            NSString *cachePath = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)lastObject];
            NSString *savePath = [cachePath stringByAppendingString:filename];
            NSLog(@"savePath:%@",savePath);
            NSURL *url = [NSURL fileURLWithPath:savePath];
            
            [[NSFileManager defaultManager]copyItemAtURL:location toURL:url error:&saveError];
            if (!saveError) {
                NSLog(@"save success");
            }else{
                NSLog(@"save error:%@",saveError.localizedDescription);
            }
        }else{
            NSLog(@"download error:%@",error.localizedDescription);
        }
    }];
    [downloadTask resume];
}

```