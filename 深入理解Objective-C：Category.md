## 深入理解Objective-C：Category

1.category  ： 为已存在的类添加方法 （模拟多继承）    

JS可以随时添加（动态语言）    OC（不那么动态）   编译与运行的区别

2.category和extension
 extension 是一个匿名的 category，两者区别很大。

extension在编译时期就决定，伴随着类的存在以及消失，
必须有类的源码才能添加，因此无法为系统的类添加extension。

category则完全不一样，它是在运行期决议的。
就category和extension的区别来看，我们可以推导出一个明显的事实，
extension可以添加实例变量，而category是无法添加实例变量的
（因为在运行期，对象的内存布局已经确定，如果添加实例变量就会破坏类的内部布局，这对编译型语言来说是灾难性的）

3、挑灯细览-category真面目
编译时期生成用于运行期category的加载

4、追本溯源-category如何加载

1)、category的方法没有“完全替换掉”原来类已经有的方法，也就是说如果category和原来类都有methodA，那么category附加完成之后，类的方法列表里会有两个methodA
2)、category的方法被放到了新方法列表的前面，而原来类的方法被放到了新方法列表的后面，这也就是我们平常所说的category的方法会“覆盖”掉原来类的同名方法，这是因为运行时在查找方法的时候是顺着方法列表的顺序查找的，它只要一找到对应名字的方法，就会罢休^_^，殊不知后面可能还有一样名字的方法

5 在类和category中都可以有+load方法
1)、可以调用，因为附加category到类的工作会先于+load方法的执行
2)、+load的执行顺序是先类，后category，而category的+load执行顺序是根据编译顺序决定的。

6、触类旁通-category和方法覆盖

7、更上一层-category和关联对象
我们知道在category里面是无法为category添加实例变量的。但是我们很多时候需要在category中添加和对象关联的值，这个时候可以求助关联对象来实现。