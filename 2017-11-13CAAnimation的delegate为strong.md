##CAAnimation的delegate为strong
由于CAAnimation的delegate使用的strong类型

使用NSProxy来解决

该类可直接引用YYKit中的YYWeakProxy类
使用方法

`animation.delegate = [YYWeakProxy proxyWithTarget:self];`


by the way
当你不需要调用它的代理方法时，为什么还要成为它的代理对象？