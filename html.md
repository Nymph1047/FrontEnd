## 你对语义化的理解

用正确的标签做正确的事情

- 利于开发：使代码**结构清晰**，**可读性高**，**方便维护**
- 利于SEO：方便爬虫根据语义标签确定**页面结构**和**关键字**的权重

## h5的新特性
- 画布（Canvas）API
- 地理（Geolacation）API 音频、视频API(audio,video)
- localStorage和sessionStorage
- webworker，websocket
- 新的一套标签header,nav,footer,aside,article,section
- web worker是运行在浏览器后台的js程序，他不影响主程序的运行，是另开的一个js线程，可以用这个线程执行复杂的数据操作，然后把操作结果通过postMessage传递给主线程，这样在进行复杂且耗时的操作时就不会阻塞主线程了。
- HTML5 History两个新增的API：history.pushState 和 history.replaceState，两个 API 都会操作浏览器的历史记录，而不会引起页面的刷新。

## img的title和alt有什么区别
- 通常当鼠标滑动到元素上的时候显示
- alt是<img>的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。

## script标签中defer和async的区别
- defer:浏览器指示脚本在文档被解析后执行，script被异步加载后并不会立刻执行，而是等待文档被解析完毕后执行。
- async:同样是异步加载脚本，区别是脚本加载完毕后立即执行，这导致async属性下的脚本是乱序的，对于 script 有先后依赖关系的情况，并不适用