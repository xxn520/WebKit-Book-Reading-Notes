## WebKit 架构和模块

### WebKit 架构

#### 架构

![](图片)

和前面给出的架构图不同，这张图更加细致地描述了 WebKit 的架构。首先底下两层保持不变（不变的是需要这些模块，但是不同平台对于这些底层模块的实现是可以有差异的）。

在此之上，将 WebKit 引擎划分为 WebCore 和 WebKit Ports。前者是各个平台共享的部分，用实线包围，包括了加载和渲染网页的核心部分；WebKit Ports 则是非共享的部分，对于不同浏览器使用的 WebKit 来说，移植中的这些模块由于平台差异依赖的第三方库和需求不同等方面的原因，往往按照自己的方式来设计和实现，所以叫做移植部分。它包括了硬件加速、网络栈、视频解码、图片解码等。这部分对于性能和功能的影响很大。

在 WebCore 和 WebKit Ports 之上主要是嵌入式的编程接口，提供给浏览器调用。左右分别是狭义的 WebKit 和 Webkit2 的接口。因为接口具体与移植相关，所以有一个与浏览器相关的绑定层。绑定层上就是对外暴露的接口层。但是实际上接口层也是与移植相关的，所以不存在什么统一的接口。

> 这幅图没有提到的是测试用例，包括布局测试和性能测试两大类。另外每个浏览器所用的 WebKit 移植必须保证能够编译出来一个可执行程序，称为 DumpRenderTree，它被用来运行测试用例并将结果同期望结果对比。

#### 获取源码

我们可以从[官网](www.webkit.org)获取 WebKit 源码，进行编译，并可以选择编译不同版本的移植。比如 Qt 版、Safari版。

#### 源代码结构

WebKit 代码相当多，大概超过 500 万行。幸运地是目录结构非常清晰，通过目录基本可以了解 WebKit 的功能模块。

###### 主要的一级目录

- LayoutTests：各种各样的测试用例和渲染期望结果，包括文本和图片。
- PerformanceTests：各种各样的性能测试基准用例，包括著名的 Sunspider。
- Source：源代码目录。
- Tools：工具。

###### 主要的二级目录

- Source
 - JavaScriptCore：（默认的 Js 引擎）。
 - Platform：平台相关的代码。
 - WebCore：WebCore 包含的模块。
 - WebKit：WebKit 绑定和接口。
 - WebKit2：WebKit2 绑定和接口。
 - WTF：基础类库。
- Tools
 - DumpRenderTree：用于生成 DumpRenderTree。
 - gdb：帮助 gdb 调试 Python 脚本。
 - Scripts：各种脚本，负责编译、语法检查、代码提交等。
 - TestWebKitAPI：测试 WebKit 嵌入式 API 的测试代码。

###### 重要的三级目录

- WebCore
 - css：Css 解释器。
 - dom： DOM 节点基础类及树结构。
 - html：HTML 解释器和 DOM 节点类。
 - inspector：Web Inspector 的实现。
 - loader：资源加载器、缓存等。
 - page：与页面相关的全局对象的实现，包括 window、navigator 等 DOM 对象、事件、动画处理。
 - paltform：各个移植代码。
 - storage：存储的共享代码。
- WebKit2
 - eft：eft 主函数，构建简单的浏览器，还有很多移植代码。
 - NetworkProcess： 网络进程相关代码。
 - UIProcess：UI 进程相关代码。
 - WebProcess：Web 进程相关代码。 

### 基于 Blink 的 Chromium 浏览器结构

### WebKit2