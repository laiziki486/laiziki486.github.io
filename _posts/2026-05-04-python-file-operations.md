---
title: Python 文件操作踩坑记录
date: 2026-05-04 16:45:00 +0800
categories: [编程, Python]
tags: [python, 文件操作]
---

## 为什么要学文件操作

之前写的程序都是运行完就结束了，数据都没保存下来。想着如果能把数据存到文件里，下次打开程序还能继续用，那就方便多了。所以开始学文件操作。

## 读取文件

### 基本读取

最简单的方式：

```python
file = open("test.txt", "r", encoding="utf-8")
content = file.read()
print(content)
file.close()
```

**重要**：一定要记得 `close()`！我刚开始经常忘记关闭文件，虽然程序能跑，但是不规范。

### 更好的方式：with 语句

```python
with open("test.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(content)
```

用 `with` 的好处是不用手动 `close()`，它会自动关闭文件。现在我都用这种方式。

### 按行读取

如果文件很大，一次性读取可能会占用太多内存。可以按行读取：

```python
with open("test.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line.strip())  # strip() 去掉换行符
```

或者用 `readlines()` 读取所有行到列表：

```python
with open("test.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()
    for line in lines:
        print(line.strip())
```

## 写入文件

### 覆盖写入

```python
with open("output.txt", "w", encoding="utf-8") as file:
    file.write("这是第一行\n")
    file.write("这是第二行\n")
```

**注意**：`"w"` 模式会覆盖原文件！我之前就因为这个把一个重要文件给覆盖了，还好有备份...

### 追加写入

```python
with open("output.txt", "a", encoding="utf-8") as file:
    file.write("这是追加的内容\n")
```

`"a"` 模式是追加，不会覆盖原内容。

## 编码问题（重要！）

这个坑我踩得最惨。刚开始我没加 `encoding="utf-8"`，结果：

```python
# 错误示范
with open("test.txt", "r") as file:
    content = file.read()
```

在 Windows 上读取中文文件就报错：`UnicodeDecodeError`。

**解决方法**：始终指定编码格式：

```python
with open("test.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

现在我每次打开文件都会加上 `encoding="utf-8"`，已经成习惯了。

## 文件路径问题

### 相对路径 vs 绝对路径

```python
# 相对路径（相对于当前工作目录）
with open("data.txt", "r", encoding="utf-8") as file:
    pass

# 绝对路径
with open("D:/projects/data.txt", "r", encoding="utf-8") as file:
    pass
```

**踩坑**：Windows 的路径分隔符是反斜杠 `\`，但在 Python 字符串里 `\` 是转义字符，所以要么用双反斜杠 `\\`，要么用正斜杠 `/`：

```python
# 这样会报错
file = open("D:\test\data.txt", "r")  # \t 会被当成制表符

# 正确方式
file = open("D:/test/data.txt", "r")
file = open("D:\\test\\data.txt", "r")
file = open(r"D:\test\data.txt", "r")  # r 表示原始字符串
```

我最喜欢用正斜杠 `/`，简单不容易出错。

## 检查文件是否存在

```python
import os

if os.path.exists("data.txt"):
    print("文件存在")
else:
    print("文件不存在")
```

这个很有用，可以避免程序因为找不到文件而崩溃。

## 实际应用：简单的记事本

我写了个小程序，可以保存和读取笔记：

```python
import os

def save_note():
    note = input("请输入笔记内容：")
    with open("notes.txt", "a", encoding="utf-8") as file:
        file.write(note + "\n")
    print("保存成功！")

def read_notes():
    if not os.path.exists("notes.txt"):
        print("还没有笔记")
        return
    
    with open("notes.txt", "r", encoding="utf-8") as file:
        notes = file.readlines()
        print("\n=== 我的笔记 ===")
        for i, note in enumerate(notes, 1):
            print(f"{i}. {note.strip()}")

while True:
    print("\n1. 添加笔记")
    print("2. 查看笔记")
    print("3. 退出")
    
    choice = input("请选择：")
    
    if choice == "1":
        save_note()
    elif choice == "2":
        read_notes()
    elif choice == "3":
        print("再见！")
        break
    else:
        print("无效选择")
```

虽然功能很简单，但是能把数据保存下来的感觉还是挺有成就感的。

## JSON 文件

后来发现 Python 有个 `json` 模块，可以更方便地保存复杂数据：

```python
import json

# 保存数据
data = {
    "name": "张三",
    "age": 20,
    "scores": [85, 90, 88]
}

with open("data.json", "w", encoding="utf-8") as file:
    json.dump(data, file, ensure_ascii=False, indent=4)

# 读取数据
with open("data.json", "r", encoding="utf-8") as file:
    data = json.load(file)
    print(data)
```

`ensure_ascii=False` 可以保存中文，`indent=4` 让 JSON 文件格式化，更容易阅读。

## 小结

文件操作确实有不少坑，特别是编码和路径问题。不过掌握了之后就很方便了，可以做很多实用的小工具。

下一步打算学习网络请求，试着写个爬虫玩玩。
