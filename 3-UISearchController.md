##UISearchController
###1.cancel 改为中文
```objective-c
    // 修改 Cancle 按钮,这样修改,按钮一开始就直接出现,而不是搜索的时候再出现
    searchController.searchBar.showsCancelButton = YES;
    for(id sousuo in [searchController.searchBar subviews]) {
        for (id view in [sousuo subviews]) {
            if([view isKindOfClass:[UIButton class]]){
                UIButton *btn = (UIButton *)view;
                [btn setTitle:@"取消" forState:UIControlStateNormal];
                [btn setTitleColor:[UIColor blueColor] forState:UIControlStateNormal];
                break;
            }
        }
    }
```
###2.搜索栏输入的中文是否包含在数据源中
```objective-c
    NSString *searchString = searchController.searchBar.text;
    NSPredicate *preicate = [NSPredicate predicateWithFormat:@"SELF CONTAINS %@",searchString];
    if (self.resultArray) {
        [self.resultArray removeAllObjects];
    }
    self.resultArray = [NSMutableArray arrayWithArray:[_dataSource filteredArrayUsingPredicate:preicate]];
    // 数组不为空,刷新
    if (self.resultArray) {
        [self.tableView reloadData];
    }
``` 
###3.搜索框去掉灰色背景
```objective-c
    for (UIView *cc in _searchController.searchBar.subviews) {
        if ([cc isKindOfClass:NSClassFromString(@"UIView")] && self.view.subviews.count > 0) {
            [[cc.subviews objectAtIndex:0]removeFromSuperview];
        }
        break;
    }
```
###4.Tips
```objective-c
    // 传入 nil 默认在原视图展示
    _searchController = [[UISearchController alloc]initWithSearchResultsController:nil];
    // 成为代理
    _searchController.searchResultsUpdater = self;

    // 搜索结果通常写在代理方法
	- (void)updateSearchResultsForSearchController:(UISearchController *)searchController;

```