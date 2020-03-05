## 一 Stack层叠布局

层叠布局使用场景，比如：图片上书写一些文字，或者图片内部也有一些容器，row和column很难满足。  

```dart
import 'package:flutter/material.dart';
void main () => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){

    var stack = new Stack(
      alignment: const FractionalOffset(0.5, 0.8),    // 该属性用于控制层叠位置
      children: <Widget>[
        new CircleAvatar(     // 该组件经常可用作头像
          backgroundImage: new NetworkImage('https://pic1.zhimg.com/50/v2-a1d7ae086fa04ff53a201f02ed3a245a_hd.jpg'),
          radius: 100.0,
        ),
        new Container(
          decoration: new BoxDecoration(
            color: Colors.lightBlue,
          ),
          padding: EdgeInsets.all(5.0),
          child: new Text('stack布局'),
        )
      ],
    );


    return MaterialApp(
      title:'ListView widget',

      home:Scaffold(
        appBar:new AppBar(
          title:new Text('垂直方向布局'),
        ),
        body:Center(child:stack),
      ),
    );
  }
}
```

alignment属性是控制层叠的位置的，建议在两个内容进行层叠时使用。它有两个值X轴距离和Y轴距离，值是从0到1的，都是从上层容器的左上角开始算起的。  

## 二 层叠的定位 

Positioned组件的属性
- bottom: 距离层叠组件下边的距离
- left：距离层叠组件左边的距离
- top：距离层叠组件上边的距离
- right：距离层叠组件右边的距离
- width: 层叠定位组件的宽度
- height: 层叠定位组件的高度


如下示例中的两个`hello world`会重叠在一起：
```dart
    var stack = new Stack(
      alignment: const FractionalOffset(0.5, 0.8),    // 该属性用于控制层叠位置
      children: <Widget>[
        new CircleAvatar(     // 该组件经常可用作头像
          backgroundImage: new NetworkImage('https://pic1.zhimg.com/50/v2-a1d7ae086fa04ff53a201f02ed3a245a_hd.jpg'),
          radius: 100.0,
        ),
        new Positioned(
          top: 10.0,
          left: 10.0,
          child: new Text("hello world1"),
        ),
        new Positioned(
          top: 10.0,
          left: 10.0,
          child: new Text("hello world2"),
        )
      ],
    );
```