## runtime的优点
一

* 1.实现多继承 ： 在OC程序中可以借用消息转发机制来实现多继承的功能。一个对象对一个消息做出回应，类似于另一个对象中的方法借过来或是“继承”过来一样。
消息转发提供了许多类似于多继承的特性，但是他们之间有一个很大的不同：
多继承：合并了不同的行为特征在一个单独的对象中，会得到一个重量级多层面的对象。
消息转发：将各个功能分散到不同的对象中，得到的一些轻量级的对象，这些对象通过消息通过消息转发联合起来。
* 2.黑魔法Method Swizzling
它可以通过Runtime的API实现更改任意的方法，理论上可以在运行时通过类名/方法名hook到任何 OC 方法，替换任何类的实现以及新增任意类。
埋点统计用户信息的例子

* 3.Method Swizzling注意点
* 3.1 Swizzling应该总在+load中执行
Objective-C在运行时会自动调用类的两个方法+load和+initialize。
+load会在类初始加载时调用， +initialize方法是以懒加载的方式被调用的，
如果程序一直没有给某个类或它的子类发送消息，那么这个类的 +initialize方法是永远不会被调用的。所以Swizzling要是写在+initialize方法中，是有可能永远都不被执行。
* 3.2 Swizzling应该总是在dispatch_once中执行 二次交换变回原来的方法： 那就是继承中用了Swizzling。如果不写dispatch_once就会导致Swizzling失效！

二 Method Swizzling使用场景

* 4.1.实现AOP
* 4.2.实现埋点统计
* 4.3.实现异常保护

三. Aspect Oriented Programming
AOP作用就是分离横向关注点(Cross-cutting concern)来提高模块复用性，它可以在既有的代码添加一些额外的行为(记录日志、身份验证、缓存)而无需修改代码。

四. Isa Swizzling
前面第二点谈到了黑魔法Method Swizzling，本质上就是对IMP和SEL进行交换。其实接下来要说的Isa Swizzling，和它类似，本质上也是交换，不过交换的是Isa。
在苹果的官方库里面有一个很有名的技术就用到了这个Isa Swizzling，那就是KVO——Key-Value Observing。

KVO是为了监听一个对象的某个属性值是否发生变化。在属性值发生变化的时候，肯定会调用其setter方法。所以KVO的本质就是监听对象有没有调用被监听属性对应的setter方法。具体实现应该是重写其setter方法即可。

- (void)willChangeValueForKey:(NSString *)key
- (void)didChangeValueForKey:(NSString *)key

五. Associated Object 关联对象
通过关联对象来实现在 Category 中添加属性的功能

六.动态的增加方法
在消息发送阶段，如果在父类中也没有找到相应的IMP，就会执行resolveInstanceMethod方法。在这个方法里面，我们可以动态的给类对象或者实例对象动态的增加方法

七.NSCoding的自动归档和自动解档
用runtime实现的思路就比较简单，我们循环依次找到每个成员变量的名称，然后利用KVC读取和赋值就可以完成encodeWithCoder和initWithCoder了

八.字典和模型互相转换
1.调用 class_getProperty 方法获取当前 Model 的所有属性。
2.调用 property_copyAttributeList 获取属性列表。
3.根据属性名称生成 setter 方法。
4.使用 objc_msgSend 调用 setter 方法为 Model 的属性赋值（或者 KVC）


懂得原理再去使用三方成熟框架