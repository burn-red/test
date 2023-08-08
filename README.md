# test
### VBox

子元素单个垂直列排放. If the vbox has a border and/or padding set, then the contents will be laid out within those insets.

VBox example:

```
     VBox vbox = new VBox(8); // spacing = 8
     vbox.getChildren().addAll(new Button("Cut"), new Button("Copy"), new Button("Paste"));
 
```

VBox will resize children (if resizable) to their preferred heights and uses its [`fillWidth`](https://openjfx.io/javadoc/20/javafx.graphics/javafx/scene/layout/VBox.html#fillWidthProperty()) property to determine whether to resize their widths to fill its own width or keep their widths to their preferred (fillWidth defaults to true). 内容对齐 by the [`alignment`](https://openjfx.io/javadoc/20/javafx.graphics/javafx/scene/layout/VBox.html#alignmentProperty()) property, which defaults to Pos.TOP_LEFT.

If a vbox is resized larger than its preferred height, by default it will keep children to their preferred heights, leaving the extra space unused. If an application wishes to have one or more children be allocated that extra space it may optionally set a vgrow constraint on the child. See "Optional Layout Constraints" for details.

VBox lays out each managed child regardless of the child's visible property value; unmanaged children are ignored.

不可见也会有布局

## Resizable Range

A vbox's parent will resize the vbox within the vbox's resizable range during layout. By default the vbox computes this range based on its content as outlined in the table below.

|           | width                                                        | height                                                       |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| minimum   | left/right insets plus the largest of the children's min widths. | top/bottom insets plus the sum of each child's min height plus spacing between each child. |
| preferred | left/right insets plus the largest of the children's pref widths. | top/bottom insets plus the sum of each child's pref height plus spacing between each child. |
| maximum   | Double.MAX_VALUE                                             | Double.MAX_VALUE                                             |

A vbox's unbounded maximum width and height are an indication to the parent that it may be resized beyond its preferred size to fill whatever space is assigned to it.

VBox provides properties for setting the size range directly. These properties default to the sentinel value USE_COMPUTED_SIZE, however the application may set them to other values as needed:

```
     vbox.setPrefWidth(400);
 
```

Applications may restore the computed values by setting these properties back to USE_COMPUTED_SIZE.

VBox does not clip its content by default, so it is possible that children's bounds may extend outside its own bounds if a child's min size prevents it from being fit within the vbox.

## Optional Layout Constraints

An application may set constraints on individual children to customize VBox's layout. For each constraint, VBox provides a static method for setting it on the child.

| Constraint | Type                         | Description                                   |
| ---------- | ---------------------------- | --------------------------------------------- |
| vgrow      | javafx.scene.layout.Priority | The vertical grow priority for the child.     |
| margin     | javafx.geometry.Insets       | Margin space around the outside of the child. |

For example, if a vbox needs the ListView to be allocated all extra space:

```
     VBox vbox = new VBox();
     ListView list = new ListView();
     VBox.setVgrow(list, Priority.ALWAYS);
     vbox.getChildren().addAll(new Label("Names:"), list);
 
```

If more than one child has the same grow priority set, then the vbox will allocate equal amounts of space to each. VBox will only grow a child up to its maximum height, so if the child has a max height other than Double.MAX_VALUE, the application may need to override the max to allow it to grow.
