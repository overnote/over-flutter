## 一 垂直布局

Column组件可以实现垂直布局，其子组件会垂直排列。  

```dart
import 'package:flutter/material.dart';
void main () => runApp(MyApp());

class MyApp extends StatelessWidget{
  @override
  Widget build(BuildContext context ){
    return MaterialApp(
      title:'ListView widget',

      home:Scaffold(
          appBar:new AppBar(
            title:new Text('垂直方向布局'),
          ),
          body:Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: <Widget>[
              Text('hello world!'),
              Text('hello world! hello world!'),
              Text('hello')
            ],
          )
      ),
    );
  }
}
```

CrossAxisAlignment负责水平方向的对齐，比如示例中文字对齐，主要有：start、end、center三个选项。  

mainAxisAlignment负责垂直方向上的对齐，比如示例中的内容被垂直居中。  

Expanded使用：  
```dart
          body:Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Center(child: Text('hello world')),
              Expanded(child:Center(child:Text('hello world! hello world!'))),
              Center(child: Text('hello')),
            ],
```
