# 🌏 安装

## ✅️️ 运行环境

操作系统：Windows、Linux 和 Mac。

python 版本：3.6 及以上

支持浏览器：Chromium 内核（如 Chrome 和 Edge）


## ✅️️ 安装

请使用 pip 安装 DrissionPage：

```shell
pip install DrissionPage
```

## ✅️️ 升级

### 📌 升级最新稳定版

```shell
pip install DrissionPage --upgrade
```

### 📌 指定版本升级

```shell
pip install DrissionPage==4.0.0b17
```

# 🌏 导入

DrissionPage 提供的功能放在以下几个路径：

- `from DrissionPage import ****`：浏览器类、配置类、页面类
- `from DrissionPage.errors import ****`：异常
- `from DrissionPage.common import ****`：辅助工具
- `from DrissionPage.items import ****`：衍生对象，用于类型判断

-----

## ✅️ 浏览器类

### 📌 `Chromium`

浏览器类用于连接浏览器、管理标签页及其它和浏览器总体有关的操作。

浏览器类相当于总管，它可以作为浏览器入口，使用它产生 Tab 对象去操控每个标签页。

```python
from DrissionPage import Chromium
```

------

## ✅️ 页面类

### 📌 `ChromiumPage`

`ChromiumPage`是将浏览器对象和第一个标签页对象封装在一起，用于控制浏览器。

`ChromiumPage`只是简化了操作，使用效果和直接使用`Chromium`对象基本一致。

唯一区别是，`ChromiumPage`生成的标签页对象是`ChromiumTab`，不能切换模式。

```python
from DrissionPage import ChromiumPage
```

------

### 📌 `WebPage`

`WebPage`与`ChromiumPage`相似，不过其自身及其产生的 Tab 对象可切换模式，既可控制浏览器，也可收发数据包。

```python
from DrissionPage import WebPage
```

------

### 📌 `SessionPage`

`SessionPage`用于收发数据包，是对 requests 和 lxml 进行封装实现的。

它把网络连接和结果解析封装成页面。操作逻辑和其它页面一致。

```python
from DrissionPage import SessionPage
```

------

## ✅️ 配置工具

### 📌 `ChromiumOptions`

`ChromiumOptions`类用于设置浏览器启动参数。

这些参数只有在启动浏览器时有用，接管已存在的浏览器时是不生效的。

```python
from DrissionPage import ChromiumOptions
```

------

### 📌 `SessionOptions`

`SessionOptions`类用于设置`Session`对象启动参数。

用于配置`SessionPage`或`WebPage`的 s 模式的连接参数。

```python
from DrissionPage import SessionOptions
```



------

### 📌 `Settings`

`Settings`用于设置全局运行配置，如找不到元素时是否抛出异常等。

```python
from DrissionPage.common import Settings
```

------

## ✅️ 辅助工具

### 📌 `Keys`

键盘按键类，用于键入 ctrl、alt 等按键。

```python
from DrissionPage.common import Keys
```

------

### 📌 `By`

与 selenium 一致的`By`类，便于项目迁移。

```python
from DrissionPage.common import By
```

### 📌 其它工具

这些工具都在`DrissionPage.common`路径中。

- `wait_until`：可等待传入的方法结果为真
- `make_session_ele`：从 html 文本生成`ChromiumElement`对象
- `configs_to_here`：把配置文件复制到当前路径
- `get_blob`：获取指定的 blob 资源
- `tree`：用于打印页面对象或元素对象结构
- `from_selenium`：用于对接 selenium 代码
- `from_playwright`：用于对接 playwright 代码

```python
from DrissionPage.common import wait_until
from DrissionPage.common import make_session_ele
from DrissionPage.common import configs_to_here
```

## ✅️ 异常

异常放在`DrissionPage.errors`路径。

全部异常详见进阶使用章节。

```python
from DrissionPage.errors import ElementNotFoundError
```

## ✅️ 衍生对象类型

Tab、Element 等被其它对象生成的对象，开发过程中需要类型判断时需要导入这些类型。

可在`DrissionPage.items`路径导入。

```python
from DrissionPage.items import SessionElement
from DrissionPage.items import ChromiumElement
from DrissionPage.items import ShadowRoot
from DrissionPage.items import NoneElement
from DrissionPage.items import ChromiumTab
from DrissionPage.items import MixTab
from DrissionPage.items import ChromiumFrame
```

# 🌏 准备工作

开始前，我们先设置一下浏览器路径。

如果只使用收发数据包功能，无需任何准备工作。

程序默认控制 Chrome，所以下面用 Chrome 演示。 如果要使用 Edge 或其它 Chromium 内核浏览器，设置方法是一样的。

注意

尽量使用版本号在 100 以上的浏览器，旧版有些功能不支持。

## 1️⃣ 尝试启动浏览器

默认状态下，程序会自动在系统内查找 Chrome 路径。

执行以下代码，浏览器启动并且访问了项目官网，说明可直接使用，跳过后面的步骤即可。

```python
from DrissionPage import Chromium

tab = Chromium().latest_tab
tab.get('https://DrissionPage.cn')
```

------

## 2️⃣ 设置路径

如果上面的步骤提示出错，说明程序没在系统里找到 Chrome 浏览器。

可用以下其中一种方法设置，设置会持久化记录到默认配置文件，之后程序会使用该设置启动。

获取浏览器路径的方法

- 这里的浏览器路径不一定是 Chrome，Edge 等 Chromium 内核的浏览器都可以。
- 打开浏览器，在地址栏输入`chrome://version`（Edge 输入`edge://version`），回车。 ![img](https://drissionpage.cn/assets/images/find_browser_path-1b46b5e4ba053115091a598d3b4211ac.png)
  如图所示，红框中就是要获取的路径。
  此法不限于 Windows，有界面的 Linux 也可使用。

## 📌 方法一

新建一个临时 py 文件，并输入以下代码，填入您电脑里的 Chrome 浏览器可执行文件路径，然后运行。

```python
from DrissionPage import ChromiumOptions

path = r'D:\Chrome\Chrome.exe'  # 请改为你电脑内Chrome可执行文件路径
ChromiumOptions().set_browser_path(path).save()
```

这段代码会把浏览器路径记录到配置文件，今后启动浏览器皆使用该路径。

如果是想临时切换浏览器路径以尝试运行和操作是否正常，可以去掉`.save()`，以如下方式结合第1️⃣步的代码。

```python
from DrissionPage import Chromium, ChromiumOptions

path = r'D:\Chrome\Chrome.exe'  # 请改为你电脑内Chrome可执行文件路径
co = ChromiumOptions().set_browser_path(path)
tab = Chromium(co).latest_tab
tab.get('https://DrissionPage.cn')
```

## 📌 方法二

在命令行输入以下命令（路径改成自己电脑里的）：

```shell
dp -p "D:\Chrome\chrome.exe"
```

注意

- 注意命令行的 python 环境与项目应是同一个
- 注意要先使用 cd 命令定位到项目路径

------

## 3️⃣ 重试控制浏览器

现在，请重新执行第1️⃣步的代码，如果正确访问了项目官网，说明已经设置完成。

```python
from DrissionPage import Chromium

tab = Chromium().latest_tab
tab.get('https://DrissionPage.cn')
```

------

## ✅️️ 说明

当您完成准备工作后，无需关闭浏览器，后面的上手示例可继续接管当前浏览器。

# 🌏上手示例

- [01-操控浏览器](./上手示例/01-操控浏览器.md)

- [02-收发数据包](./上手示例/02-收发数据包.md)

- [03-模式切换](./上手示例/03-模式切换.md)

# 🚀 控制浏览器

- [01-概述](./控制浏览器/01-概述.md) 

-  [02-连接浏览器](./控制浏览器/02-连接浏览器.md) 

-  [03-浏览器启动设置](./控制浏览器/03-浏览器启动设置.md) 

-  [04-浏览器对象](./控制浏览器./04-浏览器对象.md) 

-  [05-访问网页](./控制浏览器/05-访问网页.md) 

-  [06-页面交互](./控制浏览器/06-页面交互.md) 

-  [07-获取网页信息](./控制浏览器/07-获取网页信息.md) 

-  [08-查找元素](./控制浏览器/08-查找元素) 

      -  [08-01-概述](./控制浏览器/08-查找元素\08-01-概述.md) 
      -  [08-02-定位语法](./控制浏览器/08-查找元素\08-02-定位语法.md) 
      -  [08-03-页面或元素内查找](./控制浏览器/08-查找元素\08-03-页面或元素内查找.md) 
      -  [08-04-相对定位](./控制浏览器/08-查找元素\08-04-相对定位.md) 
      -  [08-05-行为模式](./控制浏览器/08-查找元素\08-05-行为模式.md) 
      -  [08-06-在结果列表中筛选](./控制浏览器/08-查找元素\08-06-在结果列表中筛选.md) 
      -  [08-07-简化写法](./控制浏览器/08-查找元素\08-07-简化写法.md) 
      -  [08-08-语法速查表](./控制浏览器/08-查找元素\08-08-语法速查表.md) 

-  [09-元素交互](./控制浏览器/09-元素交互.md) 

-  [10-获取元素信息](./控制浏览器/10-获取元素信息.md) 

-  [11-iframe操作](./控制浏览器/11-iframe操作.md) 

-  [12-动作链](./控制浏览器/12-动作链.md) 

-  [13-模式切换](./控制浏览器/13-模式切换.md) 

-  [14-等待](./控制浏览器/14-等待.md) 

-  [15-监听网络数据](./控制浏览器/15-监听网络数据.md) 

-  [16-获取控制台信息](./控制浏览器/16-获取控制台信息.md) 

-  [17-截图和录像](./控制浏览器/17-截图和录像.md) 

-  [18-上传文件](./控制浏览器/18-上传文件.md) 

-  [19-Page对象](./控制浏览器/19-Page对象.md) 

  
