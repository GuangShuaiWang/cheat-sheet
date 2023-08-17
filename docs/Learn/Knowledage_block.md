## 知识块

这里就不是那些可以一句话就能说清楚的知识了。

### vscode无密链接服务器
整体流程是很简答的，通过ssh-keygen生成公钥和私钥，然后将公钥放到服务起的~/.ssh/authorized_keys中，并本地配置好自己的私钥路径，但是，我这里就一直有问题。

### slumr调度系统

交互模式提交任务：使用的是salloc命令，

### 函数装饰器的理解

functools 模块提供了一些常用的函数工具，包括函数装饰器、偏函数等。
当我们需要在多个函数中添加相同的功能代码时，可以考虑使用函数装饰器来避免代码的重复编写。下面是一个使用函数装饰器的实际例子，用于记录函数的运行时间：
```python
import time
import functools

def timeit(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.5f} seconds to run.")
        return result
    return wrapper

@timeit
def my_function(x, y):
    time.sleep(1)
    return x + y

result = my_function(1, 2)
print(result)  
#my_function took 1.00505 seconds to run.
#3
```
在这个例子中，我们定义了一个函数装饰器 timeit，用于记录函数的运行时间。在使用 @timeit 装饰器来装饰 my_function 函数时，实际上是将 my_function 函数作为参数传递给 timeit 函数，并将 timeit 函数的返回值作为新的函数重新绑定到 my_function 上。当调用 my_function 函数时，实际上是调用了经过装饰器修改后的函数 wrapper，从而实现了记录函数运行时间的功能。
在 timeit 装饰器中，我们使用了 functools.wraps 来保留原始函数的属性和文档字符串等信息，以避免在调用 help 函数时出现问题。同时，我们还使用了 *args 和 **kwargs 来接收任意数量和类型的参数，以便于装饰器可以适用于任何函数。

@property 是 Python 中的一个装饰器，用于将一个类方法转换为属性。使用 @property 装饰器可以使得该方法可以像普通属性一样被访问，而无需显式调用该方法，说人话就是：通过 @property 装饰器，可以直接通过方法名来访问方法，不需要在方法名后添加一对“（）”小括号。
@property 装饰器通常用于将一个方法封装为只读属性，以提供更加简洁的访问方式。例如，可以使用 @property 装饰器来封装一个类的属性，并提供只读访问方式：
```python
class MyClass:
    def __init__(self, x):
        self._x = x

    @property
    def x(self):
        return self._x

obj = MyClass(10)
print(obj.x)   # 输出：10
```
在这个例子中，我们定义了一个类 MyClass，其中包含一个私有属性 _x，以及一个使用 @property 装饰器封装的只读属性 x。当我们访问 obj.x 时，实际上是调用了 x 方法，并返回了 _x 属性的值。
需要注意的是，在使用 @property 装饰器时，我们需要将方法名和属性名保持一致，以便于使用者可以方便地使用点号语法来访问属性。同时，我们还可以定义一个 @<property_name>.setter 装饰器来实现属性的写入功能：
```python
class MyClass:
    def __init__(self, x):
        self._x = x

    @property
    def x(self):
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

obj = MyClass(10)
print(obj.x)   # 输出：10 不加括号即可调用

obj.x = 20
print(obj.x)   # 输出：20 不需要再重新实例化，即可重新赋值
```
在这个例子中，我们定义了一个 @x.setter 装饰器，用于实现属性的写入功能。当我们使用 obj.x = 20 来修改属性值时，实际上是调用了 x 方法的 setter 方法，并将 20 作为参数传递给了该方法。

### __init__.py

在 Python 中，`__init__.py` 是一个特殊的文件，用于标识一个目录是一个 Python 包。当 Python 解释器在导入一个包时，它会首先查找该包下的 `__init__.py` 文件，并执行其中的代码。因此，可以在 `__init__.py` 中定义一些变量、函数或类等，以便于在导入该包时执行一些初始化操作。

下面是一个示例，展示了如何使用 `__init__.py` 文件来定义一个 Python 包：

```
my_package/
    __init__.py
    module1.py
    module2.py
```

在这个示例中，我们创建了一个名为 `my_package` 的 Python 包，其中包含了一个 `__init__.py` 文件和两个模块文件 `module1.py` 和 `module2.py`。当我们导入 `my_package` 包时，Python 解释器会首先执行 `my_package/__init__.py` 文件中的代码，然后再导入 `module1` 和 `module2` 模块。

需要注意的是，`__init__.py` 文件可以为空，或者包含任意的 Python 代码。在一些情况下，我们可以在 `__init__.py` 文件中定义一些变量、函数或类等，以便于在导入包时执行一些初始化操作。例如，我们可以在 `__init__.py` 文件中定义一个名为 `VERSION` 的变量，用于表示包的版本号：

```
my_package/
    __init__.py
    module1.py
    module2.py
```

```python
# my_package/__init__.py

VERSION = '1.0.0'
```

在这个示例中，我们在 `__init__.py` 文件中定义了一个名为 `VERSION` 的变量，用于表示 `my_package` 包的版本号。当我们导入 `my_package` 包时，可以通过 `my_package.VERSION` 来访问该变量。
