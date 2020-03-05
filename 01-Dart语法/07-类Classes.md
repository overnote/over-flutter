## 一 Classes

#### 1.1 Dart对象说明

Dart 是一种面向对象的语言，并且支持基于mixin的继承方式，即一个类可以继承自多个父类。  

使用new语句来构造一个类，构造函数的名字可能是ClassName，也可以是ClassName.identifier：
```dart

  var jsonData = JSON.decode('{"x":1, "y":2}');

  // Create a Point using Point().
  var p1 = new Point(2, 2);

  // Create a Point using Point.fromJson().
  var p2 = new Point.fromJson(jsonData);

```

使用.（dot）来调用实例的变量或者方法
```dart
  var p = new Point(2, 2);

  // Set the value of the instance variable y.
  p.y = 3;

  // Get the value of y.
  assert(p.y == 3);

  // Invoke distanceTo() on p.
  num distance = p.distanceTo(new Point(4, 4));

```

使用?.来确认前操作数不为空, 常用来替代. , 避免左边操作数为null引发异常。
```dart
 // If p is non-null, set its y value to 4.
 p?.y = 4;
```

使用const替代new来创建编译时的常量构造函数:
```dart
var p = const ImmutablePoint(2, 2);

```

使用runtimeType方法，在运行中获取对象的类型。该方法将返回Type 类型的变
```dart
print('The type of a is ${a.runtimeType}');
```

#### 1.2 实例化

在类定义中，所有没有初始化的变量都会被初始化为null。
```dart
 class Point {
    num x; // Declare instance variable x, initially null.
    num y; // Declare y, initially null.
    num z = 0; // Declare z, initially 0.
 }
```

类定义中所有的变量, Dart语言都会隐式的定义 setter 方法，针对非空的变量会额外增加 getter 方法。
```dart
 class Point {
    num x;
    num y;
 }

 main() {
    var point = new Point();
    point.x = 4;          // Use the setter method for x.
    assert(point.x == 4); // Use the getter method for x.
    assert(point.y == null); // Values default to null.
 }
```

## 二 构造函数

#### 2.1 构造函数初识
声明一个和类名相同的函数，来作为类的构造函数。
```dart
  class Point {
     num x;
     num y;

     Point(num x, num y) {
        // There's a better way to do this, stay tuned.
        this.x = x;
        this.y = y;
     }
  }
```
this关键字指向了当前类的实例, 上面的代码可以简化为：
```dart
 class Point {
    num x;
    num y;

    // Syntactic sugar for setting x and y
    // before the constructor body runs.
    Point(this.x, this.y);
 }

```
贴士：构造函数不能继承(Constructors aren’t inherited)，子类不会继承父类的命名构造函数。如果不显式提供子类的构造函数，系统就提供默认的构造函数。

使用命名构造函数从另一类或现有的数据中快速实现构造函数：
```dart
class Point {
   num x;
   num y;

   Point(this.x, this.y);

   // 命名构造函数Named constructor
   Point.fromJson(Map json) {
     x = json['x'];
     y = json['y'];
   }
}
```
构造函数不能被继承，父类中的命名构造函数不能被子类继承。如果想要子类也拥有一个父类一样名字的构造函数，必须在子类是实现这个构造函数。  

### 2.2 调用父类的非默认构造函数

默认情况下，子类只能调用父类的无名，无参数的构造函数; 父类的无名构造函数会在子类的构造函数前调用; 如果initializer list 也同时定义了，则会先执行initializer list 中的内容，然后在执行父类的无名无参数构造函数，最后调用子类自己的无名无参数构造函数。即下面的顺序：
- initializer list（初始化列表）
- super class’s no-arg constructor（父类无参数构造函数）
- main class’s no-arg constructor （主类无参数构造函数）

如果父类不显示提供无名无参数构造函数的构造函数，在子类中必须手打调用父类的一个构造函数。这种情况下，调用父类的构造函数的代码放在子类构造函数名后，子类构造函数体前，中间使用:(colon) 分割。  

```dart
class Person {
   String firstName;

   Person.fromJson(Map data) {
       print('in Person');
   }
}

class Employee extends Person {
   // 父类没有无参数的非命名构造函数，必须手动调用一个构造函数     
   super.fromJson(data)
   Employee.fromJson(Map data) : super.fromJson(data) {
      print('in Employee');
   }
}

main() {
   var emp = new Employee.fromJson({});

   // Prints:
   // in Person
   // in Employee
   if (emp is Person) {
     // Type check
     emp.firstName = 'Bob';
   }
   (emp as Person).firstName = 'Bob';
}
```

#### 2.3 初始化列表
除了调用父类的构造函数，也可以通过初始化列表在子类的构造函数体前（大括号前）来初始化实例的变量值，使用逗号,分隔。如下所示：
```dart
class Point {
   num x;
   num y;

   Point(this.x, this.y);

   // 初始化列表在构造函数运行前设置实例变量。
   Point.fromJson(Map jsonMap)
   : x = jsonMap['x'],
     y = jsonMap['y'] {
      print('In Point.fromJson(): ($x, $y)');
   }
 }

```
注意：上述代码，初始化程序无法访问 this 关键字。

#### 2.4 静态构造函数

如果你的类产生的对象永远不会改变，你可以让这些对象成为编译时常量。为此，需要定义一个 const 构造函数并确保所有的实例变量都是 final 的。
```dart
class ImmutablePoint {
    final num x;
    final num y;
    const ImmutablePoint(this.x, this.y);
    static final ImmutablePoint origin = const ImmutablePoint(0, 0);
}

```

#### 2.5 重定向构造函数
有时候构造函数的目的只是重定向到该类的另一个构造函数。重定向构造函数没有函数体，使用冒号:分隔。
```dart
class Point {
    num x;
    num y;

    // 主构造函数
    Point(this.x, this.y) {
        print("Point($x, $y)");
    }

    // 重定向构造函数，指向主构造函数，函数体为空
    Point.alongXAxis(num x) : this(x, 0);
}

void main() {
    var p1 = new Point(1, 2);
    var p2 = new Point.alongXAxis(4);
}
```

#### 2.6 常量构造函数

有时候构造函数的目的只是重定向到该类的另一个构造函数。重定向构造函数没有函数体，使用冒号:分隔。
```dart
class Point {
    num x;
    num y;

    // 主构造函数
    Point(this.x, this.y) {
        print("Point($x, $y)");
    }

    // 重定向构造函数，指向主构造函数，函数体为空
    Point.alongXAxis(num x) : this(x, 0);
}

void main() {
    var p1 = new Point(1, 2);
    var p2 = new Point.alongXAxis(4);
}
```

#### 2.7 工厂构造函数

当实现一个使用 factory 关键词修饰的构造函数时，这个构造函数不必创建类的新实例。例如，工厂构造函数可能从缓存返回实例，或者它可能返回子类型的实例。 下面的示例演示一个工厂构造函数从缓存返回的对象：
```dart
class Logger {
   final String name;
   bool mute = false;

   // _cache 是一个私有库,幸好名字前有个 _ 。 
   static final Map<String, Logger> _cache = <String, Logger>{};

   factory Logger(String name) {
       if (_cache.containsKey(name)) {
          return _cache[name];
       } else {
          final logger = new Logger._internal(name);
          _cache[name] = logger;
          return logger;
       }
    }

    Logger._internal(this.name);

    void log(String msg) {
       if (!mute) {
          print(msg);
       }
    }
    
 }

```

注意：工厂构造函数不能用 this。

