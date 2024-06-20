---
title: Python期末复习
date: 2024-06-18 22:58:25
tags: "Python"
categories: "Note"
top_img: https://raw.githubusercontent.com/hmwm/vblog/main/pic/bokymo.jpg
cover: https://raw.githubusercontent.com/hmwm/vblog/main/pic/bokymo.jpg
---
# Python

## 基础概念（包括选择简答）

### Python中的Lambda表达式

- Python中的Lambda表达式是用于创建匿名函数的简单方式。
- Lambada表达式也成为匿名函数，因为它们不需要显示命名。Lambada表达式可以有任意数量的参数，但是只能有一个表达式
- 表达式会被计算并返回结果

### 集合的特点

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

### 字典的特点：

- 键值对存储：字典以键值对的形式储存数据，每个键值对由唯一的键（key）和与之相关联的值value组成。
- 键的唯一性：字典中的每一个键都是唯一的，不能有重复的键，如果向字典添加一个已经存在的键，新的值会覆盖旧的值。
- 可变性：字典是可变的数据类型，可以动态添加，修改和删除键值对。
- 无序性：字典中的键值对是无序的。

### thinter中的布局管理器：

- pack()：适用于简单的布局需求，简单一次排列，快捷方便。
- grid()：适用于精确控制控件位置的情况，可以轻松实现复杂的布局。
- place()：适用于需要精确指定空间位置和大小的情况，通常在其他布局管理器无法满足需求时的使用。

### Tkinter中的主事件循环

- 在Tkinker中，**主事件循环**是应用程序的核心，它使得应用程序能够响应用户的输入和其他事件。
- 主事件循环不断地运行，等待并处理来自用户的事件（鼠标点击，按键等）以及系统事件（窗口关闭，重绘等）

组件（小部件）概念，举出三个例子

- 在图形用户界面（GUI中，组件（Weiget）是构成界面的基本元素）。
- 每个组件通常代表一个独立的交互单元，例如标签，按钮，文本框，输入框等。
- Label：用于显示文本或图像。
- Button：用于创建按钮，用户可以点击它来触发事件。
- Entry：用于创建单行文本输入框。
- Text：用于创建多行文本输入框。

## 程序阅读题

1. 阅读以下代码，回答以下问题。
    
    ```python
    class Book:
        def __init__(self, title, author, year_published):
            self.title = title
            self.author = author
            self.year_published = year_published
    
        def display_info(self):
            print(f"Title:{self.title}")
            print(f"Author:{self.author}")
            print(f"Year Published:{self.year_published}")
    
        
    book1 = Book("Python Programming", "John Doe", 1990)
    book2 = Book("Data Science Essentials", "Jane Smith", 2021)
    
    book1.display_info()
    print()
    book2.display_info()
    ```
    
    1. 程序中定义了哪个类？创建了哪几个实例？
        - 程序中定义了book类。
        - 创建了两个实例：book1和book2。
    2. display_info方法的作用是什么？
        - display_info方法的作用是显示书籍的标题、作者和出版年份信息。
    3. 请修改display_info方法，如果年份早于2000年，则额外显示一条警告信息：”注意：本书是20世纪出版的“
        
        ```python
        def display_info(self):
                print(f"Title:{self.title}")
                print(f"Author:{self.author}")
                print(f"Year Published:{self.year_published}")
                if self.year_published < 2000:
                    print("注意:本书是20世纪出版的！")
        ```
        

## 分析结果题

1. （函数调用）分析以下程序，给出输出结果，并说明原因
    
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

    调用factorial(4)进行以下计算：
    
    factorial(4)返回4*factorial(3)
    
    factorial(3)返回3*factorial(2)
    
    factorial(2)返回2*factorial(1)
    
    factorial(1)返回1*factorial(0)
    
    factorial(0)返回1（基准情况）
    
    计算过程：
    
    factorial(1)返回1*1 = 1
    
    factorial(2)返回2*1 = 2
    
    factorial(3)返回3*2 = 6
    
    factorial(4)返回4*6 = 24
    
    故result值为24
    ```
    
2. （字典的特点和运用）给出下面的程序执行后的打印结果。
    
    ```python
    my_dict = {"apple": 3, "banana": 3, "orange": 1}
    total_fruits = sum(my_dict.values())
    print(total_fruits) #（1）输出结果：7
    del my_dict["orange"]
    total_fruits = sum(my_dict.values())
    print(total_fruits) #（2）输出结果：6 
    ```
    
3. （列表表达式）分析以下程序，给出输出结果。
    
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
    
4. （类与实例）圆的周长面积计算类
    
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
    
5. 分析以下程序，给出输出结果。
    
    ```python
     def divide_numbers(a, b):
        try:
            result = a / b
            print(f"Result of {a} divided by {b} is : {result}")
        except ZeroDivisionError:
            print("Error: Division by zero!")
    
    divide_numbers(6, 3) # (1) 输出结果 Result of 6 divided by 3 is : 2.0
    divide_numbers(5, 0) # (2) 输出结果 Error: Division by zero!
    ```
    
6. 分析以下程序，给出输出结果
    
    ```python
    def foo(x):
        x.append(2)
    
    x = []
    foo(x)
    print(x) # (1) 输出结果 [2]
    foo(x)
    print(x) # (2) 输出结果 [2, 2]
    foo(x)
    print(x) #（3）输出结果 [2, 2, 2]
    ```
    
7. 分析以下程序，给出输出结果
    
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
    

### 编程题

1. 编写一个 Python 程序，要求定义一个函数，用户输入一个正整数 n，然后输出从 1 到 n 的所有正整数的平方值。
    
    ```python
    def print_squares(n):
        for i in range(1, n + 1):
            print(f"{i} 的平方是 {i ** 2}")
    
    def main():
        try:
            n = int(input("请输入一个正整数："))
            if n > 0:
                print_squares(n)
            else:
                print("请输入一个大于0的正整数。")
        except ValueError:
            print("请输入有效的正整数。")
    
    if __name__ == "__main__":
        main()
    ```
    
2. 编写一个Python程序，要求实现以下功能
    1. 定义一个函数 calculate_area，接收两个参数 width 和 height,分别表示矩形的宽和高。
    2. 函数内部计算并返回矩形的面积。
    3. 在主程序中，提示用户输入矩形的宽和高，调用 calculate_area()函数计算并输出矩形的面积。
    
    ```python
    def calculate_area(width, height):
        return width * height
    
    def main():
        try:
            width = float(input("请输入矩形的宽度："))
            height = float(input("请输入矩形的高度："))
            area = calculate_area(width, height)
            print(f"矩形面积为：{area}")
        except ValueError:
            print("Error: 请输入有效的数据")
    
    if __name__ == "__main__":
        main()
    ```