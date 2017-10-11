## ASyncDisPlayKit	YYKit
阻塞主线程的任务：

* Layout文本动态宽高，视图布局计算        Rendering文本渲染，图形解码，图形绘制         UIKit（键盘Button） Core Animation等必须主线程的操作   

* UIView 与 CALayer关系: UIView持有Layer显示，View显示属性由Layer映射。只能在主线程创建，访问，销毁。

* 当不需要响应触摸事件时，节省对应的资源  。封装成对应的ASDK控件，避免直接使用UIKit控件

微博Demo性能优化：

* 预排版：提前在后台计算好布局结果
* 预渲染：避免离屏渲染，屏幕平均帧数，应当尽量避免使用layer的corner，shadow等技术，而尽量在后台线程预先绘制
* 异步绘制：显示文本控件

评测界面的流畅度
FPS 指示器



##多线程编程
同步（sync）   阻塞当前线程并等待block任务执行完毕，当前线程才会继续运行
异步（async） 等回调

队列

* 串行队列任务：FIFO，一个取出，执行一个，取下一个
* 并行队列任务:  FIFO，取出放到一个线程，取出放到另一个线程，取出速度很快。看起来一起执行实际GCD会根据系统资源控制并行数量

* 队列组：当几个小任务完成时，队列组会有一个方法通知我们

NSOperation和NSOperationQueue
将要执行的任务封装到一个 NSOperation 对象中
将此任务添加到一个 NSOperationQueue 对象中

NSOperation 有一个非常实用的功能，那就是添加依赖




