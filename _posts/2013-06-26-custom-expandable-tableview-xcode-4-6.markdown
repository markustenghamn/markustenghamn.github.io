---
layout: post
title:  "Custom Expandable TableView with Xcode 4.6"
redirect_from:
   - /custom-expandable-tableview-xcode-4-6
date:   2013-06-26 23:30:39 +0100
description: While developing an app called SFSFUM I needed a custom expandable UITableView for the schedule function. This was accomplished by following...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

While developing an app called SFSFUM I needed a custom expandable UITableView for the schedule function. This was accomplished by following [this guide](http://www.appcoda.com/customize-table-view-cells-for-uitableview/ "Customize table view cells for UITableView") which shows you how to make custom table view cells for a UITableView. In order to achieve the expandable effect for the UITableView we simply change the height of the cell when the cell is pressed like so.

```
<pre lang="html">-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {<br></br>
self.selectedRow = indexPath;<br></br>
SchemaCell *cell = (SchemaCell *)[tableView cellForRowAtIndexPath:indexPath];<br></br>
[tableView beginUpdates];<br></br>
[tableView endUpdates];<br></br>
cell.location.hidden = NO;<br></br>
}<br></br><br></br>
-(void)tableView:(UITableView *)tableView didDeselectRowAtIndexPath:(NSIndexPath *)indexPath {<br></br>
SchemaCell *cell = (SchemaCell *)[tableView cellForRowAtIndexPath:indexPath];<br></br>
[tableView beginUpdates];<br></br>
[tableView endUpdates];<br></br>
cell.location.hidden = YES;<br></br>
}<br></br><br></br>
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {<br></br>
if(_selectedRow && indexPath.row == _selectedRow.row) {<br></br>
return 100;<br></br>
}<br></br>
return 44;<br></br>
}
```  
 The above code is taken right out of my app and will toggle between a height of 100 and 44 when the row is selected or deselected. This achieves the effect and function of a expandable UITableView.