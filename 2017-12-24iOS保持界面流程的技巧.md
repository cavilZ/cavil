
iOS保持界面流程的技巧

分析卡顿

卡顿一般从CPU和GPU入手，一般来说，GPU都不是卡顿的瓶颈。

卡顿的三大原因：

 * UI对象的创建，属性修改
 * 布局
 * 渲染
  
iOS设备是每秒60帧，也就是一帧 CPU计算->GPU渲染->显示= 16.7ms。

分析CPU，使用工具Time Profiler

1.图片解码

* 作为一个常见的优化点，当你创建一个UIImage时，默认发生实际解码，只有当图片要被显示到屏幕时，才会发生实际解码。解码是在CPU上进行。
(UIImage imageNamed:””)并不会后台解码。
* 解码的原理很简单，提前将图片绘制到CGContext中，再从Context获取图片，这样就能够强制图片解码。通常三方库（SDWebImage）都自带后台解码。

2.XIB

* 初始化XIB耗时较多。相比直接从代码创建和读取文件创建，肯定是读文件要慢得多。
* 纯代码代替XIB

3.AutoLayout

* 通过添加约束，实现复杂的布局。CPU在布局的时候，需要添加不少计算
* 手动layout代替AutoLayout

4.reload

* 在collectionView中，每次滚动都调用reload是一件愚蠢的事。
* 可以给row添加一个isDirty属性,只有存在脏数据，才需要更新

5.轻量级的View
* collectionView是一种昂贵的视图，当布局比较简单时，改写成简单的UIView，布局代码在layoutSubview实现。

6.预计算Layout

* 使用Time Profiler进行数据分析时，检查LayoutSubview占用时间
* sizeToFit，让Label自适应大小需要额外的CPU计算
* 文本大小计算很损耗性能，如果大量使用富文本（微博），基于CoreText异步绘制引擎是一个不错的优化点，直接使用YYText也是推荐的。

7.预加载&&RunLoop

继续使用Time Proflier分析发现主线程CPU占用几乎在main函数中。Cell显示在屏幕上，CPU都做了什么

* 创建Cell实例
* 对Cell实例和Model进行数据绑定，包括调整UI属性
* 根据模型数据，涉及到重新布局，比如说动态文本数据

这里的任务都必须在主线程上进行，无法调度到后台执行

* 预加载。对模板Cell进行提前创建，并放入内存缓存。在需要显示到屏幕的时候，就不再需要重新创建Cell对象。
* 任务拆分。将大的任务进行拆分，一次执行一个，CPU占用就不会突然很高


1.如何实现预加载，什么时候预加载，一次预加载多少个Cell。

* 这里采用监听RunLoop方式进行加载，监听beforeWaiting和exit两个事件，去执行一些任务
* 核心代码TaskDispatcher类，每个runLoop中TaskDispatcher只执行一个任务，TaskDispatcher有两个模式，枚举，一个滚动时停止预加载，一个用来大任务拆分。在cellForRow中，检查预加载缓存，有则返回缓存数据

2.任务拆分

* 对每行进行reloadData时，针对需要调整的属性，进行任务拆分

总结：

性能优化就是:能放到后台的就放到后台，不能放到后台的，选择预加载，或者拆分。推荐Texture，异步显示框架。

这里仅从UI角度优化，实际开发，主线程读写DB，进行大量计算也会影响性能。