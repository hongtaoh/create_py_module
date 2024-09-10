# 如何开发一个 Python 包 (保姆级教程)

很多时候我们只是用别人的包，但有时候我们需要自己制作一个 python 包。此教程用最简单的例子解释如何制作一个 python package 并放在 https://pypi.org/ 上。

此教程参考 Jason Dsouza 的教程: [How to Build Your Very First Python Package](https://www.freecodecamp.org/news/build-your-first-python-package/)。

此教程也参考了 ChatGPT 的回答。

此教程的成品：
- https://github.com/hongtaoh/htpymodule
- https://pypi.org/project/htpymodule/

首先，你要给你的包起一个名字，要不同于我的例子：`htpymodule`。

## 项目结构

你的包应该有类似这样的结构：

```
mypypackage/
│
├── htpymodule/
│   ├── __init__.py
│   ├── add.py
│   ├── subtract.py
│   └── extras/
│       ├── multiply.py
│       └── divide.py
│
├── setup.py
├── README.md
└── LICENSE (optional)
```

其中

`add.py`:

```py
def add(x, y):
    return x + y
```

`subtract.py`:

```py
def subtract(x, y):
    return x - y
```

`extras/multiply.py`:

```py
def multiply(x, y):
    return x * y
```

`extras/divide.py`:

```py
def divide(x, y):
    if y == 0:
        raise ValueError("Cannot divide by zero.")
    return x / y
```

`htpymodule/__init__.py`:

```py
from .add import add
from .subtract import subtract
from .extras.multiply import multiply
from .extras.divide import divide
```

## Build & Publish

在项目根目录：

```bash
python setup.py sdist bdist_wheel
pip install .
```

`python setup.py sdist bdist_wheel` 可以把 python 包弄好。

`pip install .` 可以在本地直接安装此包。

为了验证，你可以在根目录创建一个 `test.py`，检测该包是否正常运行：

```py
from htpymodule import add, subtract, multiply, divide

print(add(1, 2))         # Output: 3
print(subtract(5, 3))    # Output: 2
print(multiply(4, 2))    # Output: 8
print(divide(10, 2))     # Output: 5.0
```

如果一切正常，我们要把包上传到 https://pypi.org/。

如果你没有 PyPi 账号，先注册一个：https://pypi.org/account/register/

你大概率需要设置双重验证 (Two factor authentication (2FA))。

弄好之后，在 https://pypi.org/manage/account/，找到 API tokens，选择 Add API token，随便填一个 token name，选择 Entire account，然后点击 Add token。把 token 保存起来。

然后，在根目录运行

```sh
pip install twine
twine upload dist/*
```

然后粘贴你刚才保存的 token。然后你就可以看到你的包在 pypi 上线了：https://pypi.org/project/htpymodule/。所有人都可以通过运行

```sh
pip install htpymodule
```

来安装你的包。


## 细节

包的名字如果包含 `_`，上传之后 pypi 会自动换成 `-`。
