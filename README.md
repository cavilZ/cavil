# 记录

###关于SDWeb存在黑名单blackList机制 
####问题：存在多次刷新仍没有图片的现象 
* 当SDWebImage在加载图片的时候 - (void)sd_setImageWithURL:(NSURL *)url placeholderImage:(UIImage *)placeholder；这个方法。在加载过程中因为网络或别的原因造成加载失败！SDWeb把当前的图片url加入到blacklist,第二次加载这个url时 跳过不再去请求网络数据。 
* 解决方案：使用- (void)sd_setImageWithURL:(NSURL *)url placeholderImage:(UIImage *)placeholder options:(SDWebImageOptions)options;这个方法 options 传SDWebImageRetryFailed。默认是0未定义。
