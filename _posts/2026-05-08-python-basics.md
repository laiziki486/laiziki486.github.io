---
title: Python 基础学习笔记
date: 2026-05-08 14:30:00 +0800
categories: [编程, Python]
tags: [python, 学习笔记]
---

## 开始学 Python 了

最近决定学 Python，主要是看到身边好多人都在用，而且听说比较好上手。之前也试过学 C++，但是指针那块真的搞不懂就放弃了。这次希望能坚持下来。

## 安装和环境

我用的是 Windows 系统，直接从官网下载的 Python 3.11。安装的时候记得勾选"Add Python to PATH"，不然后面用命令行会很麻烦（我第一次就忘了勾，后来又重装了一遍）。

编辑器我选的是 VS Code，装了个 Python 插件，感觉还挺好用的。

## 变量和数据类型

Python 的变量不用声明类型，直接赋值就行：

```python
name = "张三"
age = 20
height = 1.75
is_student = True
```

这个确实比 C++ 方便多了，不用写一堆 `int`、`float` 什么的。

有个地方我刚开始搞混了，就是字符串可以用单引号也可以用双引号，但是不能混用：

```python
# 这样是对的
text1 = 'hello'
text2 = "world"

# 这样会报错
text3 = 'hello"  # 错误！
```

## 打印输出

最基础的就是 `print()` 函数：

```python
print("Hello World")
print("我的年龄是", age)
```

我发现 `print()` 可以直接打印多个东西，用逗号隔开就行，它会自动加空格。如果不想要空格，可以用字符串拼接：

```python
print("我的年龄是" + str(age))  # 注意要把数字转成字符串
```

还有一种更方便的方法是 f-string（Python 3.6 以后才有）：

```python
print(f"我的年龄是{age}")
```

这个我觉得最好用，不用管类型转换的问题。

## 输入

用 `input()` 函数可以获取用户输入：

```python
name = input("请输入你的名字：")
print(f"你好，{name}！")
```

**注意**：`input()` 返回的永远是字符串！如果要输入数字，需要转换：

```python
age = int(input("请输入你的年龄："))
```

我刚开始就在这里踩坑了，写了个简单的加法程序：

```python
a = input("输入第一个数：")
b = input("输入第二个数：")
print(a + b)
```

结果输入 3 和 5，输出的是 35 而不是 8！后来才知道字符串的 `+` 是拼接，不是相加。改成这样就对了：

```python
a = int(input("输入第一个数："))
b = int(input("输入第二个数："))
print(a + b)
```

## 条件判断

Python 的 if 语句不用写括号，但是要注意缩进：

```python
age = 18
if age >= 18:
    print("成年了")
else:
    print("未成年")
```

**缩进很重要！** Python 是靠缩进来判断代码块的，不像其他语言用大括号。我刚开始经常忘记缩进，然后就报 `IndentationError`。

多条件判断用 `elif`：

```python
score = 85
if score >= 90:
    print("优秀")
elif score >= 80:
    print("良好")
elif score >= 60:
    print("及格")
else:
    print("不及格")
```

## 循环

for 循环一般配合 `range()` 使用：

```python
for i in range(5):
    print(i)  # 输出 0 1 2 3 4
```

注意 `range(5)` 是从 0 到 4，不包括 5。如果要从 1 开始：

```python
for i in range(1, 6):
    print(i)  # 输出 1 2 3 4 5
```

while 循环：

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

我写 while 循环的时候经常忘记更新计数器，结果变成死循环，只能强制关闭程序...

## 小结

目前学了这些基础内容，感觉 Python 确实比较容易上手。接下来打算学习列表、字典这些数据结构，还有函数的使用。

希望能坚持学下去！
