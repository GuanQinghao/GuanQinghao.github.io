--
> 创建日期：2017年04月19日  
> 修改日期：2018年01月29日  

--
iOS中的UITableViewDelegate提供Cell左滑自定义事件方法，简单介绍一下。 

其中dataArray为Cell的数据内容。

```objc
//自定义左滑操作置顶标记更多等
- (nullable NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    UITableViewRowAction *top = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"置顶"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //数据源置顶
        NSString *data = [dataArray objectAtIndex:indexPath.row];
        [dataArray removeObject:[dataArray objectAtIndex:indexPath.row]];
        [dataArray insertObject:data atIndex:0];
        //        [dataArray exchangeObjectAtIndex:indexPath.row withObjectAtIndex:0];//第二种写法
        
        //表单移动
        //获取顶部的IndexPath
        NSIndexPath *topIndexPath = [NSIndexPath indexPathForRow:0 inSection:indexPath.section];
        
        [tableView moveRowAtIndexPath:indexPath toIndexPath:topIndexPath];
        [tableView reloadRowsAtIndexPaths:@[topIndexPath] withRowAnimation:UITableViewRowAnimationRight];
        [tableView reloadData];
    }];
    top.backgroundColor = [UIColor redColor];
    
    UITableViewRowAction *bottom = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"置后"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //数据源置后
        NSString *data = [dataArray objectAtIndex:indexPath.row];
        [dataArray removeObject:[dataArray objectAtIndex:indexPath.row]];
        [dataArray insertObject:data atIndex:dataArray.count];
        //表单移动
        //获取底部的IndexPath
        NSIndexPath *bottomIndexPath = [NSIndexPath indexPathForRow:dataArray.count-1 inSection:indexPath.section];
        
        [tableView moveRowAtIndexPath:indexPath toIndexPath:bottomIndexPath];
        [tableView reloadRowsAtIndexPaths:@[bottomIndexPath] withRowAnimation:UITableViewRowAnimationRight];
        [tableView reloadData];
    }];
    bottom.backgroundColor = [UIColor yellowColor];
    
    UITableViewRowAction *moveUp = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"上移"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //数据源交换
        [dataArray exchangeObjectAtIndex:indexPath.row withObjectAtIndex:indexPath.row-1];
        //表单移动
        //获取上一个的IndexPath
        NSIndexPath *upIndexPath = [NSIndexPath indexPathForRow:indexPath.row-1 inSection:indexPath.section];
        
        [tableView moveRowAtIndexPath:indexPath toIndexPath:upIndexPath];
        [tableView reloadRowsAtIndexPaths:@[upIndexPath] withRowAnimation:UITableViewRowAnimationRight];
        [tableView reloadData];
    }];
    moveUp.backgroundColor = [UIColor greenColor];
    
    UITableViewRowAction *moveDown = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"下移"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //数据源交换
        [dataArray exchangeObjectAtIndex:indexPath.row withObjectAtIndex:indexPath.row+1];
        //表单移动
        //获取下一个的IndexPath
        NSIndexPath *downIndexPath = [NSIndexPath indexPathForRow:indexPath.row+1 inSection:indexPath.section];
        
        [tableView moveRowAtIndexPath:indexPath toIndexPath:downIndexPath];
        [tableView reloadRowsAtIndexPaths:@[downIndexPath] withRowAnimation:UITableViewRowAnimationRight];
        [tableView reloadData];
    }];
    moveDown.backgroundColor = [UIColor blueColor];
    
    UITableViewRowAction *delete = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"删除"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //数据源删除
        [dataArray removeObjectAtIndex:indexPath.row];
        //表单删除动画
        [tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationRight];
    }];
    delete.backgroundColor = [UIColor purpleColor];
    
    UITableViewRowAction *mark = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"标记"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //标记点击事件方法
        
    }];
    mark.backgroundColor = [UIColor blueColor];
    
    
    UITableViewRowAction *more = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"更多"handler:^(UITableViewRowAction *_Nonnull action,NSIndexPath *_Nonnull indexPath) {
        //更多点击事件方法
        
    }];
    more.backgroundColor = [UIColor blueColor];
    
    return @[top,bottom,moveUp,moveDown,delete,mark,more];
}
```
