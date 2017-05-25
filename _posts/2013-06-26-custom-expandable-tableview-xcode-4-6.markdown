---
layout: post
title:  "Custom Expandable TableView with Xcode 4.6"
redirect_from:
   - /custom-expandable-tableview-xcode-4-6
date:   2013-06-26 23:30:39 +0100
categories: best domain registrar
description: While developing an app called SFSFUM I needed a custom expandable UITableView for the schedule function. This was accomplished by following
---

While developing an app called SFSFUM I needed a custom expandable UITableView for the schedule function. This was accomplished by following [this guide](http://www.appcoda.com/customize-table-view-cells-for-uitableview/ "Customize table view cells for UITableView") which shows you how to make custom table view cells for a UITableView. In order to achieve the expandable effect for the UITableView we simply change the height of the cell when the cell is pressed like so.

```
<pre lang="html">-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
self.selectedRow = indexPath;
SchemaCell *cell = (SchemaCell *)[tableView cellForRowAtIndexPath:indexPath];
[tableView beginUpdates];
[tableView endUpdates];
cell.location.hidden = NO;
}

-(void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath {
SchemaCell *cell = (SchemaCell *)[tableView cellForRowAtIndexPath:indexPath];
[tableView beginUpdates];
[tableView endUpdates];
cell.location.hidden = YES;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
if(_selectedRow && indexPath.row == _selectedRow.row) {
return 100;
}
return 44;
}
``` The above code is taken right out of my app and will toggle between a height of 100 and 44 when the row is selected or deselected. This achieves the effect and function of a expandable UITableView.