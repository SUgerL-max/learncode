# Python PyInstaller安装和使用教程

## 1 PyInstaller 概述

PyInstaller 将 Python 应用程序及其所有依赖项捆绑到一个包中。用户无需安装 Python 解释器或任何模块即可运行打包的应用程序。

## 2 安装

### 2.1 从 PyPI 安装 PyInstaller：

```
pip install pyinstaller
```

强烈建议使用 pip 在线安装的方式来安装 PyInstaller 模块，不要使用离线包的方式来安装，因为 PyInstaller 模块还依赖其他模块，pip 在安装 PyInstaller 模块时会先安装它的依赖模块。

运行上面命令，应该看到如下输出结果：

```
Successfully installed pyinstaller-x.x.x
```

其中的 x.x.x 代表 PyInstaller 的版本。

在 PyInstaller 模块安装成功之后，在 Python 的安装目录下的 `Scripts(D:\Python\Python36\Scripts)` 目录下会增加一个 pyinstaller.exe 程序，接下来就可以使用该工具将 Python 程序生成 EXE 程序了。



### 2.2 下载安装包

进入**PyInstaller**的官网[(PyInstaller Host Page)](https://link.jianshu.com?t=http://www.pyinstaller.org/),在Download一栏中下载最新的安装包，因为是在Winodws中运行，所以我们下载**ZIP**格式的，如果是在Linux中安装可以下载上面的，如下图：

![img](https://upload-images.jianshu.io/upload_images/2162869-6bc9a8a895a54f53.png?imageMogr2/auto-orient/strip|imageView2/2/w/842/format/webp)

安装Pyinstaller首先需要Python2.7或者3.3-3.5的环境，不过大多数人在安装前肯定都早就安装了Python,所以具体如何安装Python这里就不多赘述了。

进入解压好的PyInstaller目录，使用以下命令：

```
cd C:\xxx\pyInstaller-3.2  #你的解压好的PyInstaller文件夹的位置
python setup.py install
```

安装完成后将PyInstaller的目录加入到系统的环境变量中后，输入如下命令：

```undefined
pyinstaller
```

如果出现下图的界面，表示安装成功。

![img](https://upload-images.jianshu.io/upload_images/2162869-bdbb59109d2da47a.png?imageMogr2/auto-orient/strip|imageView2/2/w/641/format/webp)



### 2.3 配置系统变量

安装需要用到pip工具，该工具在3.5版本的[python](https://so.csdn.net/so/search?from=pc_blog_highlight&q=python)中已经自带不用另行安装，但是需要在系统变量中添加python下的Scripts文件夹，如下图：

![img](https://img.jbzj.com/file_images/article/202006/202006020923028.png)
图（https://img.jbzj.com/file_images/article/202006/202006020923028.png）

在Path变量值中添加【;(python的安装目录)\Scripts】

*注意不要漏了最前面的分号





## 3 用法

### 3.1 PyInstaller生成可执行程序

在需要封装的`.py`文件目录下打开`cmd`，运行命令行：

```
pyinstaller [option] /path/to/yourscript.py
```

不管这个 Python 应用是单文件的应用，还是多文件的应用，只要在使用 pyinstaller 命令时编译作为程序入口的 Python 程序即可。

> PyInstaller工具是跨平台的，它既可以在 Windows平台上使用，也可以在 Mac OS X 平台上运行。在不同的平台上使用 PyInstaller 工具的方法是一样的，它们支持的选项也是一样的。

​																表 1 PyInstaller 支持的常用选项[option]

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/zZTbkic2pYRrNzvyBLNJ4mtA9dZXWBnUTEe5NZ5kfJEytpWc30fk7dPsjNs8pswHUvJUibQnYRJ9GWqj8TMMLialQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> 在表 1 中列出的只是 PyInstaller 模块所支持的常用选项，如果需要了解 PyInstaller 选项的详细信息，则可通过 pyinstaller -h 来查看。

执行完毕之后，会生成几个文件夹，如下图所示。

![img](https://img2020.cnblogs.com/blog/1364361/202009/1364361-20200905114853579-1237446053.png)
图 https://img2020.cnblogs.com/blog/1364361/202009/1364361-20200905114853579-1237446053.png

在dist里面呢，就有了一个exe程序，这个就是可执行的exe程序，如下图所示。

![img](https://img2020.cnblogs.com/blog/1364361/202009/1364361-20200905114917749-99285496.png)



### 3.2 特殊用法：配置文件

`.spec`，这个文件非常重要，可以通过编辑这个文件来打包我们的项目，类似DockerFile。

简单来说它的作用就是告诉 Pyinstaller 如何打包你的 py文件。当你在终端使用命令自动打包 py文件时，py 会首先自动创建一个规范文件。于：

①需要打包资源文件；

② 为可执行文件添加运行时选项，或需要包含一些 Pyinstaller 不知道的运行时库。

一份为xxx.pyspec文件，简单在终端生成如下命令：

```python
pyi-makespec xxx.py

#命令可选项同pyinstaller。
```

一个简单的规范文件实例：

```python
# -*- mode: python -*-

block_cipher = None

a = Analysis(['C:\\app\\main.py'],
            pathex=['C:\\'],
            binaries=[],
            datas=[
                ('C:\\data\\input\\builtin\\*.xlsx', '.\\data\\input\\builtin\\'),
                ('C:\\data\\input\\*.xlsx', '.\\data\\input\\'),
                ('C:\\data\\output\\', '.\\data\\output\\'),
                ('C:\\log\\', '.\\log\\'),
                ('C:\\app\\db\\', '.\\app\\db\\')
            ],
            hiddenimports=['numpy', 'pandas'],
            ...
            )

pyz = PYZ(a.pure, a.zipped_data,
            cipher=block_cipher)

exe = EXE(pyz,
         ...
         )
```

这其实就是一个python文件，只不过后缀是spec罢了。

`.spec`一共会有4个对象，分别是：Analysis、PYZ、EXE、COLLECT。

**Analysis：**

- 第一个参数【】，是指定我们整个项目的主程序，也就是我们的入口文件。

- scripts：在Analysis中定义的源文件

- pure：python模块

- binaries：动态库

- datas：数据文件，可以是任意文件类型，例如ini配置文件、字体文件、图片等（元组形式），

  `datas = [('data要打包文件夹名的绝对路径','data打包后的文件夹名')]`。

- zipfiles：zip格式的依赖文件，一般是egg格式的库文件

- pathex，就是我们的工作目录。

- hiddenimports ，PyInstaller有时候无法侦察到全部的依赖包，怎么办？我们可以在这个后面加，把PyInstaller编译出来的exe在运行的时候报的缺少模块给写里面。

**PYZ：**

将python文件压缩打包，包含程序运行需要的所有依赖，输入一般是Analysis.pure。

**EXE：**

打包生成exe文件，根据上面两项生成。EXE子任务包括Analysis的所有5个输出项以及程序运行所需的一些配置文件和动态库。

配置文件和动态库通过TOC格式来配置，格式为(name, path, typecode)，例如：

![图片](https://mmbiz.qpic.cn/mmbiz_png/zZTbkic2pYRrNzvyBLNJ4mtA9dZXWBnUTZgYSqW1icX9tqsOiaaq963PH4HUlF5366ANsveMQZbfmHGx9fB00BTgw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

`typecode`包括：

① EXTENSION：python扩展库；

② PYSOURCE：python脚本；

③ PYMODULE；

④ PYZ；

⑤ PKG；

⑥ BINARY：动态库；

⑥ DATA：数据文件；

⑦ OPTION。

**COLLECT：**

用来构建最终的生成目录，可以复制其他子任务生成的结果，并拷贝到指定目录，形成最终的打包结果，COLLECT也可以没有。



### 3.3 Pyinstaller的工作流程

当我们双击编译好的exe后，他是会创建一个临时目录，把所有需要用的包都解压到那里，然后执行。执行完毕后，临时文件夹就消失了。

这和我们有什么关系呢？想一下，如果你的项目中需要去读取某些文件，甚至是用户的输入参数，怎么办？打包出来的exe 是没有办法通过直接指定参数，类似：`python [main.py](http://main.py/) --input=*.xlsx` 来读取文件的（在执行的时候会把项目解压到一个临时目录，所以原来项目中写好的相对路径也不管用）。

为此，我们需要把host上的实际文件给copy到那个临时目录下，所以这个datas的作用就是这个，上述文件中，把`host`下的 `C:\data\input\builtin*.xlsx`文件都`copy`到临时目录的 `data\input\builtin` 下面。



- **注意了**： pandas 和 numpy 有个很奇怪的地方，就是引用了 pandas 的地方，如果没有引用 numpy ，就会报错。所以你可以在主入口上面加一个 import numpy 。
- **注意了**：直接 import numpy 还是会报错。怎么办？在 import numpy 下面加 import numpy.core._dtype_ctypes



### 3.4 临时目录

临时目录在代码里怎么处理呢，如果代码中还是老样子处理相对路径，一定是找不到的。

官方文档中给出了这么一段：

> Your app should run in a bundle exactly as it does when run from source. However, you may need to learn at run-time whether the app is running from source, or is “frozen” (bundled).

```python
import sys
if getattr( sys, 'frozen', False ) :
      # running in a bundle
      basedir = sys._MEIPASS
else :
      # running live
```

所以在项目中，如果有配置文件的话，就在那里加上这一段，然后在`bundle`中添加新路径，`else`还是老代码。

这个 `sys._MEIPASS` 是个特殊的值，是在Pyinstaller打包的时候才会添加的临时变量，通过这个变量可以获取到在执行exe时候的临时目录。

这对代码的改动是最小的。



### 3.5 编译打包

最后，执行 `python xxx.spec` 。

**命令可选项包括：**

**–upx-dir，**

**–distpath，**

**–noconfirm，**

**–ascii。**

打包的可执行文件会在`dist`里，`build`中是一些打包时候需要的文件。

输出中最后有 successfully 字样，就算成功了。cmd会显示exe出现在哪个位置。




## 4 例子

### 4.1 例1

下面先创建一个 app 目录，在该目录下创建一个 app.py 文件，文件中包含如下代码：

```python
from say_hello import *

def main():
    print('程序开始执行')
    print(say_hello('孙悟空'))
# 增加调用main()函数
if __name__ == '__main__':
    main()
```

接下来使用命令行工具进入到此 app 目录下，执行如下命令：

```python
pyinstaller -F app.py
```

执行上面命令，将看到详细的生成过程。当生成完成后，将会在此 app 目录下看到多了一个 dist 目录，并在该目录下看到有一个 app.exe 文件，这就是使用 PyInstaller 工具生成的 EXE 程序。

在命令行窗口中进入 dist 目录下，在该目录执行 app.exe ，将会看到该程序生成如下输出结果：

```python
程序开始执行：
孙悟空，您好！
```

由于该程序没有图形用户界面，因此如果读者试图通过双击来运行该程序，则只能看到程序窗口一闪就消失了，这样将无法看到该程序的输出结果。

在上面命令中使用了-F 选项，该选项指定生成单独的 EXE 文件，因此，在 dist 目录下生成了一个单独的大约为 6MB 的 app.exe 文件（在 Mac OS X 平台上生成的文件就叫 app，没有后缀）；与 -F 选项对应的是 -D 选项（默认选项），该选项指定生成一个目录（包含多个文件）来作为程序。

下面先将 PyInstaller 工具在 app 目录下生成的 build、dist 目录删除，并将 app.spec 文件也删除，然后使用如下命令来生成 EXE 文件。

```python
pyinstaller -D app.py
```

执行上面命令，将看到详细的生成过程。当生成完成后，将会在 app 目录下看到多了一个 dist 目录，并在该目录下看到有一个 app 子目录，在该子目录下包含了大量 .dll 文件和 .pyz 文件，它们都是 app.exe 程序的支撑文件。在命令行窗口中运行该 app.exe 程序，同样可以看到与前一个 app.exe 程序相同的输出结果。

PyInstaller 不仅支持 -F、-D 选项，而且也支持如表 1 所示的常用选项。



### 4.2 例2

下面再创建一个带图形用户界面，可以访问 [MySQL](http://c.biancheng.net/mysql/) 数据库的应用程序。

在 app 当前所在目录再创建一个 dbapp 目录，并在该目录下创建 Python 程序，其中 exec_select.py 程序负责查询数据，main.py 程序负责创建图形用户界面来显示查询结果。

exec_select.py 文件包含的代码如下：

```python
# 导入访问MySQL的模块
import mysql.connector

def query_db():
    # 1、连接数据库
    conn = conn = mysql.connector.connect(user='root', password='32147',
        host='localhost', port='3306',
        database='python', use_unicode=True)
    # 2、获取游标
    c = conn.cursor()
    # 3、调用执行select语句查询数据
    c.execute('select * from user_tb where user_id > %s', (2,))
    # 通过游标的description属性获取列信息
    description = c.description
    # 使用fetchall获取游标中的所有结果集
    rows = c.fetchall()
    # 4、关闭游标
    c.close()
    # 5、关闭连接
    conn.close()
    return description, rows
```

mian.py 文件包含的代码如下：

```python
from exec_select import *
from tkinter import *

def main():
    description, rows = query_db()
    # 创建窗口
    win = Tk()
    win.title('数据库查询')
    # 通过description获取列信息
    for i, col in enumerate(description):
        lb = Button(win, text=col[0], padx=50, pady=6)
        lb.grid(row=0, column=i)
    # 直接使用for循环查询得到的结果集
    for i, row in enumerate(rows):
        for j in range(len(row)):
            en = Label(win, text=row[j])
            en.grid(row=i+1, column=j)
    win.mainloop()
if __name__ == '__main__':
    main()
```

通过命令行工具进入 dbapp 目录下，在该目录下执行如下命令：

```
Pyinstaller -F -w main.py
```

上面命令中的 -F 选项指定生成单个的可执行程序，-w 选项指定生成图形用户界面程序（不需要命令行界面）。运行上面命令，该工具同样在 dbapp 目录下生成了一个 dist 子目录，并在该子目录下生成了一个 main.exe 文件。

```
直接双击运行 main.exe 程序（该程序有图形用户界面，因此可以双击运行），读者可自行查看运行结果。
```



### 4.3 EXE执行时获取控制台传入的参数

编写的python脚本需要获取屏幕输入，入口函数如下：

```
if len(sys.argv) < 2:
  print 'argv Error'
else:
  para = sys.argv[1]
  transformTxt2Xls(para)
```

在cmd窗口下直接运行脚本： `.\testExcel.py`   脚本程序能正常执行，

但是将脚本打包成exe可执行程序后，

在cmd窗口下运行exe程序： `.\testExcel.exe`  结果会提示输入参数错误，似乎传入的空字符串无法识别，

然后换一种写法：`.\testExcel.exe "11"` 脚本程序同样能正常执行。





## 5 存在问题

### 5.1 python打包Failed to execute script **.exe问题

#### 法一:

命令执行完毕之后 `build\yourscript\warnpachonggui.txt`,上面会**记载着错误**

#### 法二:

```
# 使用完下面这条指令之后,打开exe,提示failed to execute script
pyinstaller -Fw yourscript.py
# 然后执行下面这条执行,会在list下生成一个目录,进入该目录,用**命令行**执行该exe,就会看到错误了
pyinstaller -D yourscript.py
```

![img](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1533801281.jpg)

在这种情况下,手动在代码里面加入它,然后再执行一次打包命令

![img](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1533801753.jpg)

![img](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1533801883.jpg)

提示`sip not found`还在,但是,这时候,exe已经可以运行,没有bug了



### 5.2 RecursionError: maximum recursion depth exceeded

解决办法：

pyinstaller 之后会生成一个和xxx.py文件对一个的 xxx.spec 文件

![img](https://img-blog.csdnimg.cn/20200107154911889.png)

打开xxx.spec文件，在行首导入sys包，然后设置一下递归调用的限制次数，可以尽量大一点，这里设置100万次后就没有报错了，具体如下图所示

![img](https://img-blog.csdnimg.cn/20200107163432342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpcWk0MTQ1,size_16,color_FFFFFF,t_70)

修改之后，然后 **pyinstall  -F xxx.spec** （刚才修改过的文件）就行了, --add-data 参数就不需要了，spec文件里面已经有了。

