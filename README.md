# drag\_and\_drop\_lists
Two-level drag and drop reorderable lists.

## Features
- Reorder elements between multiple lists
- Reorder lists
- Drag and drop new elements from outside of the lists
- Vertical or horizontal layout
- Use with drag handles, long presses, or short presses
- Expandable lists
- Can be used in slivers
- Prevent individual lists/elements from being able to be dragged
- Easy to extend with custom layouts

<p>
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/basic.gif" width="180" title="Basic">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/expansion_tiles.gif" width="180" title="Expansion Tiles">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/drag_handle.gif" width="180" title="Drag Handle">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/drag_into_list.gif" width="180" title="Drag Into Lists">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/horizontal.gif" width="180" title="Horizontal">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/list_tiles.gif" width="180" title="List Tiles">
<img src="https://raw.githubusercontent.com/philip-brink/DragAndDropLists/master/readme_images/slivers.gif" width="180" title="Slivers">
</p>

## Known Issues
There is currently (as of flutter v. 1.24.0-1.0.pre) an issue only on web where dragging an item with some descendant that includes an InkWell widget with an onTap method will throw an exception. This includes having a ListTile with an onTap method defined.

This seems to be resolved by using a GestureDetector and its onTap method instead of the InkWell.

See the following issues:
* [#14](https://github.com/philip-brink/DragAndDropLists/issues/14)
* [Flutter #69774](https://github.com/flutter/flutter/issues/69774)
* [Flutter #67044](https://github.com/flutter/flutter/issues/67044)
* [Flutter #66887](https://github.com/flutter/flutter/issues/66887)

## Usage
To use this plugin, add `drag_and_drop_lists` as a [dependency in your pubspec.yaml file.](https://flutter.dev/docs/development/packages-and-plugins/using-packages)
For example:

```
dependencies:
  drag_and_drop_lists: ^0.2.1
``` 

Now in your Dart code, you can use: `import 'package:drag_and_drop_lists/drag_and_drop_lists.dart';`

To add the lists, add a `DragAndDropLists` widget. Set its children to a list of `DragAndDropList`. Likewise, set the children of `DragAndDropList` to a list of `DragAndDropItem`.
For example:

```
  // Outer list
  List<DragAndDropList> _contents;

  @override
  void initState() {
    super.initState();

    // Generate a list
    _contents = List.generate(10, (index) {
      return DragAndDropList(
        header: Text('Header $index'),
        children: <DragAndDropItem>[
          DragAndDropItem(
            child: Text('$index.1'),
          ),
          DragAndDropItem(
            child: Text('$index.2'),
          ),
          DragAndDropItem(
            child: Text('$index.3'),
          ),
        ],
      );
    });
  }

  @override
  Widget build(BuildContext context) {
    // Add a DragAndDropLists. The only required parameters are children,
    // onItemReorder, and onListReorder. All other parameters are used for
    // styling the lists and changing its behaviour. See the samples in the
    // example app for many more ways to configure this.
    return DragAndDropLists(
      children: _contents,
      onItemReorder: _onItemReorder,
      onListReorder: _onListReorder,
    );
  }

  _onItemReorder(int oldItemIndex, int oldListIndex, int newItemIndex, int newListIndex, DragAndDropItem droppedItem) {
    setState(() {
      var movedItem = _contents[oldListIndex].children.removeAt(oldItemIndex);
      _contents[newListIndex].children.insert(newItemIndex, movedItem);
    });
  }

  _onListReorder(int oldListIndex, int newListIndex) {
    setState(() {
      var movedList = _contents.removeAt(oldListIndex);
      _contents.insert(newListIndex, movedList);
    });
  }

```

For further examples, see the example app or [view the example code](https://github.com/philip-brink/DragAndDropLists/tree/master/example/lib) directly.
