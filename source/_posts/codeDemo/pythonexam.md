---
title: Python期末复习
date: 2024-06-18 22:58:25
tags: "Python"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/bokymo.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/bokymo.jpg
---
# Python

### 简答题

Python中断Lambda表达式

- Python中的Lambda表达式是用于创建匿名函数的简单方式。
- Lambada表达式也成为匿名函数，因为它们不需要显示命名。Lambada表达式可以有任意数量的参数，但是只能有一个表达式
- 表达式会被计算并返回结果

集合的特点

- 无序性：集合的元素没有特定的顺序，因此无法通过索引访问元素。
- 唯一性：集合的每一个元素都是唯一的，重复的元素在集合中只会存在一个。
- 可变性：集合本身是可变的，可以添加或删除元素，但集合中的元素必须是不可变的（例如，数字，字符串，元组）。
- 支持集合操作

```python
numbers_set = {2, 4, 6, 8, 10}
print(numbers_set) #输出结果：{2, 4, 6, 8, 10}
numbers_set.add(3)
print(numbers_set) #输出结果：{2, 3, 4, 6, 8, 10} 
numbers_set.update({2, 6, 7})
print(numbers_set) #输出结果：{2, 3, 4, 6, 7, 8, 10}
numbers_set.remove(2)
print(numbers_set) #输出结果：{3, 4, 6, 7, 8, 10}
```

字典的特点：

- 键值对存储：字典以键值对的形式储存数据，每个键值对由唯一的键（key）和与之相关联的值value组成。
- 键的唯一性：字典中的每一个键都是唯一的，不能有重复的键，如果向字典添加一个已经存在的键，新的值会覆盖旧的值。
- 可变性：字典是可变的数据类型，可以动态添加，修改和删除键值对。
- 无序性：字典中的键值对是无序的。

thinter中的布局管理器：

- pack()：适用于简单的布局需求，简单一次排列，快捷方便。
- grid()：适用于精确控制控件位置的情况，可以轻松实现复杂的布局。
- place()：适用于需要精确指定空间位置和大小的情况，通常在其他布局管理器无法满足需求时的使用。

Tkinter中的主事件循环

- 在Tkinker中，主事件循环是应用程序的核心，它使得应用程序能够响应用户的输入和其他事件。
- 主事件循环不断地运行，等待并处理来自用户的事件（鼠标点击，按键等）以及系统事件（窗口关闭，重绘等）

组件（小部件）概念，举出三个例子

- 在图形用户界面（GUI中，组件（Weiget）是构成界面的基本元素）。
- 每个组件通常代表一个独立的交互单元，例如标签，按钮，文本框，输入框等。
- Label：用于显示文本或图像。
- Button：用于创建按钮，用户可以点击它来触发事件。
- Entry：用于创建单行文本输入框。
- Text：用于创建多行文本输入框。

### 编程结果

1. 递归求4的阶乘
    
    ```python
    def factorial(n):
        if n == 0:
            return 1
        else:
            return n * factorial(n - 1)
    
    result = factorial(4)
    print(result)
    #结果
    24
    ```
    
2. 字典账单
    
    ```python
    my_dict = {"apple": 3, "banana": 3, "orange": 1}
    total_fruits = sum(my_dict.values())
    print(total_fruits) #（1）输出结果：7
    del my_dict["orange"]
    total_fruits = sum(my_dict.values())
    print(total_fruits) #（2）输出结果：6 
    ```
    
3. 计算列表的函数
    
    ```python
    def calculate_square(number):
        return number ** 2
    
    numbers = [1, 2, 3, 4, 5]
    squared_numbers = [calculate_square(num) for num in numbers]
    print(squared_numbers) #（1）输出结果：[1, 4, 9, 16, 25]
    
    numbers = [-1, 3, -4, 5, -2]
    squared_numbers = [calculate_square(num) for num in numbers]
    print(squared_numbers) #（2）输出结果：[1, 9, 16, 25, 4]
    ```
    
4. 圆的周长面积计算类
    
    ```python
    class Circle:
        def __init__(self, radius):
            self.radius = radius
    
        def area(self):
            return 3.14 * self.radius ** 2
        
        def perimeter(self):
            return 2 * 3.14 * self.radius
        
    circle1 = Circle(4)
    circle2 = Circle(5)
    
    print(circle1.radius) #（1）输出结果 4
    print(circle1.area()) #（2）输出结果 50.24
    
    print(circle2.radius) #（3）输出结果 25
    print(circle2.area()) #（4）输出结果 78.5
    ```