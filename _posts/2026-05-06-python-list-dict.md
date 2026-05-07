---
title: Python 列表和字典使用心得
date: 2026-05-06 20:15:00 +0800
categories: [编程, Python]
tags: [python, 数据结构]
---

## 列表真的很好用

学完基础语法后，开始接触列表（list）了。列表可以存储多个数据，而且可以是不同类型的：

```python
my_list = [1, 2, 3, "hello", True]
print(my_list)
```

## 列表的常用操作

### 访问元素

用索引访问，从 0 开始：

```python
fruits = ["苹果", "香蕉", "橙子"]
print(fruits[0])  # 苹果
print(fruits[1])  # 香蕉
```

还可以用负数索引，从后往前数：

```python
print(fruits[-1])  # 橙子（最后一个）
print(fruits[-2])  # 香蕉（倒数第二个）
```

这个负数索引我觉得挺方便的，不用算列表长度了。

### 添加元素

```python
fruits.append("葡萄")  # 在末尾添加
print(fruits)  # ['苹果', '香蕉', '橙子', '葡萄']

fruits.insert(1, "西瓜")  # 在索引 1 的位置插入
print(fruits)  # ['苹果', '西瓜', '香蕉', '橙子', '葡萄']
```

### 删除元素

```python
fruits.remove("香蕉")  # 删除指定元素
print(fruits)

del fruits[0]  # 删除指定索引的元素
print(fruits)

last = fruits.pop()  # 删除并返回最后一个元素
print(last)
print(fruits)
```

我之前搞混了 `remove()` 和 `pop()`，`remove()` 是根据值删除，`pop()` 是根据索引删除（默认最后一个）。

### 切片操作

这个功能真的很强大：

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(numbers[2:5])   # [2, 3, 4]，从索引 2 到 4
print(numbers[:3])    # [0, 1, 2]，从开头到索引 2
print(numbers[5:])    # [5, 6, 7, 8, 9]，从索引 5 到末尾
print(numbers[::2])   # [0, 2, 4, 6, 8]，每隔一个取一个
print(numbers[::-1])  # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]，反转列表
```

最后那个反转列表的操作我觉得很酷，`[::-1]` 就能反转，比写循环方便多了。

## 字典（Dictionary）

字典是键值对的集合，用大括号 `{}` 表示：

```python
student = {
    "name": "张三",
    "age": 20,
    "grade": "大二"
}
```

### 访问和修改

```python
print(student["name"])  # 张三

student["age"] = 21  # 修改
print(student["age"])  # 21

student["major"] = "计算机"  # 添加新键值对
print(student)
```

**踩坑记录**：如果访问不存在的键会报错：

```python
print(student["score"])  # KeyError: 'score'
```

安全的做法是用 `get()` 方法：

```python
print(student.get("score"))  # None
print(student.get("score", 0))  # 0（设置默认值）
```

我之前写了个程序，因为没用 `get()` 直接访问键，结果程序跑到一半就崩了，找了半天才发现是这个问题。

### 遍历字典

```python
# 遍历键
for key in student:
    print(key)

# 遍历值
for value in student.values():
    print(value)

# 同时遍历键和值
for key, value in student.items():
    print(f"{key}: {value}")
```

我最常用的是 `items()`，可以同时拿到键和值。

## 实际应用：学生成绩管理

我试着写了个小程序，用字典存储学生信息：

```python
students = {
    "张三": {"语文": 85, "数学": 90, "英语": 88},
    "李四": {"语文": 78, "数学": 92, "英语": 85},
    "王五": {"语文": 90, "数学": 88, "英语": 92}
}

# 查询张三的数学成绩
print(students["张三"]["数学"])  # 90

# 计算张三的总分
total = sum(students["张三"].values())
print(f"张三的总分：{total}")

# 计算平均分
average = total / len(students["张三"])
print(f"张三的平均分：{average:.2f}")
```

这里用到了嵌套字典（字典里面还有字典），刚开始有点绕，多写几遍就习惯了。

## 列表推导式

这个是我最近学到的，感觉很酷：

```python
# 传统方法
squares = []
for i in range(10):
    squares.append(i ** 2)

# 列表推导式
squares = [i ** 2 for i in range(10)]
```

一行代码就搞定了！还可以加条件：

```python
# 只要偶数的平方
even_squares = [i ** 2 for i in range(10) if i % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]
```

不过太复杂的逻辑还是用传统循环比较清楚，列表推导式适合简单的场景。

## 小结

列表和字典真的是 Python 里最常用的数据结构了，基本上每个程序都会用到。多练习就能熟练掌握，我现在写代码的时候已经能很自然地选择用列表还是字典了。

下次打算学习文件操作，把数据保存到文件里。
