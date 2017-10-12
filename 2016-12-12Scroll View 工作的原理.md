## Scroll View 工作的原理

* contentOffset :实时改变它的位移
当你设置它的 contentOffset 属性时它改变 scroll view.bounds 的 origin.事实上，contentOffset 甚至不是实际存在的。
* 代码看起来像这样：
` -(void)setContentOffset:(CGPoint)offset
{
    CGRect bounds = [self bounds];
    bounds.origin = offset;
    [self setBounds:bounds];
} `

Content Size:可滚动范围

* content size 定义了可滚动区域。scroll view 的默认 content size 为 {w:0, h:0}。既然没有可滚动区域，用户是不可以滚动的，但是 scroll view 仍然会显示其 bounds 范围内所有的子视图。 当 content size 设置为比 bounds 大的时候，用户就可以滚动视图了。

* content offset 的最大值是 content size 和 scroll view size 的差

` contentOffset.x = contentSize.width - bounds.size.width; `
` contentOffset.y = contentSize.height - bounds.size.height; `