Decorator là một khái niệm intermediate hoặc advanced trong Python, cung cấp khả năng mở rộng hoặc thay đổi hành vi của các hàm hoặc lớp mà không cần phải chỉnh sửa mã nguồn của chúng. 

Decorator được xây dựng dựa trên nguyên tắc "hàm và lớp là những First class objects" trong Python. Trước hết, ta hãy cùng tìm hiểu về First-class objects.

# First-class object

Khái niệm về First-class và second-class objects lần đầu được giới thiệu bởi  Christopher Strachey vào những năm 1960, và vẫn được áp dụng trong các ngôn ngữ lập trình cho tới hiện nay, ví dụ như Kotlin, Swift, PHP, Python, JavaScript, Rust, ... . Nhìn chung, các ngôn ngữ lập trình đặt ra những hạn chế(restrictions) về cách chúng ta có thể thao tác với các phần tử tính toán. Phần tử với ít hạn chế nhất sẽ được coi là first-class. Sau đây là 4 đặc tính phổ biến khi nói về first-class object:

- Có thể được gán cho một biến,
- Có thể được truyền làm đối số cho hàm,
- Có thể được trả về từ hàm,
- Có thể được lưu trữ trong các cấu trúc dữ liệu.


## First-class objects trong Python: 
Hiểu đơn giản, 1 object đại diện cho một giá trị mà một biến có thể tham chiếu đến.
Xét ví dụ: 
```python
 x=1
```
Ở đây, biến x tham chiếu đến 1 object đại diện giá trị 1.

Theo Guido van Rossum, người sáng tạo ra Python, mọi objects trong Python (integers, strings, functions, classes, ...) đều là first-class, và đều có những "đặc quyền" như nhau.

Để hiểu hơn về 4 "đặc quyền" của 1 first-class object, hạn hãy theo dõi những ví dụ bên dưới. Mình sẽ tập trung cụ thể vào first-class function, đối tượng chính tạo nên **Decorator**.

## Ví dụ 1
```
def greeter(name):
  print(f"Hello, {name}!")

greet = greeter  # Assign function to variable

greet("Alice")  # Call the function using the variable

```
## Ví dụ 2
```
def apply_twice(func, arg):
  return func(func(arg))  # Call the function twice on the argument

def yell(text):
  return text.upper() + "!!!"

result = apply_twice(yell, "hello")  # Pass yell function as argument
print(result)  # Output: HELLO!!!

```
## Ví dụ 3

# Decorators

## Decorator là gì?

## Function decorator

## Class decorator

# Ứng dụng của Decorators trong Python

## Machine leanring

@torch.no_grad()

@torch.enable_grad()

Tăng tốc Python với numba
@numba.jit(nopython=True)

# References

 https://en.wikipedia.org/wiki/First-class_citizen

 [Structure and Interpretation of Computer Programs](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/full-text/book/book-Z-H-12.html#footnote_Temp_121:~:text=%60%60rights%20and%20privileges%27%27%20of%20first%2Dclass%20elements) 

[Python Objects Part IV: First-Class Everything](https://medium.com/@bdov_/python-objects-part-iv-first-class-everything-7da3945e3552)
<!-- # My links

https://softwareengineering.stackexchange.com/questions/39742/when-is-a-feature-considered-a-first-class-citizen-in-a-programming-language-p  

https://stackoverflow.com/questions/245192/what-are-first-class-objects


[Bài viết hay và đẩy đủ về object và first-class trong Python](https://medium.com/@bdov_/python-objects-part-iv-first-class-everything-7da3945e3552)

In Python both the classes and the objects are first class objects. (See [this answer](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949) for more details about classes as objects).


-->