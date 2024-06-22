Decorator là một khái niệm intermediate hoặc advanced trong Python, cung cấp khả năng mở rộng hoặc thay đổi hành vi của các hàm hoặc lớp mà không cần phải chỉnh sửa mã nguồn của chúng. 

Decorator được xây dựng dựa trên nguyên tắc "hàm và lớp là những First class objects" trong Python. Trước hết, ta hãy cùng tìm hiểu về First-class   objects.

# First-class objects

Khái niệm về First-class objects lần đầu được giới thiệu bởi  Christopher Strachey vào những năm 1960, và vẫn được áp dụng trong các ngôn ngữ lập trình cho tới hiện nay, ví dụ như Kotlin, Swift, PHP, Python, JavaScript, Rust, ... . Nhìn chung, các ngôn ngữ lập trình đặt ra những hạn chế (restrictions) về cách chúng ta có thể thao tác với các phần tử (đối tượng/object) tính toán. Phần tử với ít hạn chế nhất sẽ được coi là first-class. Sau đây là 4 đặc tính phổ biến khi nói về first-class objects:

- Có thể được gán cho một biến,
- Có thể được truyền làm đối số cho hàm,
- Có thể được trả về từ hàm,
- Có thể được lưu trữ trong các cấu trúc dữ liệu.

Chi tiết hơn các bạn có thể tham khảo tại [đây](https://www.cs.iusb.edu/~dvrajito/teach/c311/c311_2_firstclass.html).

Trong các ngôn ngữ lập trình hàm như JavaScripts, PHP, Python, Go, Swift, hàm được coi là những first-class object. Tuy nhiên, trong một ngôn ngữ lập trình hàm khác là C++, hàm không phải là first-class object, mà được  coi là second-class object (một khái niệm ít phổ biến hơn) bởi nó không thể được biên dịch tại thời gian chạy. Bảng sau cho thấy sự khác biệt cơ bản giữa các classes (cấp bậc) của objects trong các ngôn ngữ lập trình:

![Classes of value in programming languages](https://images.viblo.asia/99ed0c14-a305-41cf-aee6-c0ebda8e7221.png)

## First-class objects trong Python: 
Hiểu đơn giản, một object đại diện cho một giá trị mà một biến có thể tham chiếu đến.
Xét ví dụ: 
```python
 x=1
```
Ở đây, biến x tham chiếu đến 1 object đại diện giá trị 1.

Theo Guido van Rossum, người sáng tạo ra Python, mọi objects trong Python (integers, strings, functions, classes, ...) đều là first-class, và đều có những "đặc quyền" như nhau.

Để hiểu hơn về 4 "đặc quyền" của 1 first-class object, bạn hãy theo dõi những ví dụ bên dưới, với các hàm đều là những **first-class functions**, đối tượng chính tạo nên **Decorator**.

## Ví dụ 1: Gán hàm cho biến
```python
def greeter(name):
  print(f"Hello, {name}!")

greet = greeter  # Assign function to variable

greet("Rossum")  # Call the function using the variable

```

```
Hello, Rossum!
```

Tương tự như gán 1 giá trị cho 1 biến: `x=1`, ta có thể gán 1 hàm cho 1 biến. Điều này sẽ tạo tham chiếu tới object *greeter*, với 1 địa chỉ cụ thể trong bộ nhớ.
```python
print(hex(id(1))) # get address of a Python object
0x21e70a56930
print(hex(id(greeter))) 
0x21e76758d30
```
## Ví dụ 2: Truyền như đối số cho hàm
```map``` và ```filter``` trong Python là 2 hàm built-in trong Python sử dụng đối số truyền vào là 1 hàm và áp dụng lên 1 iterable object.

**Ví dụ với ```filter```**

Ở đây, hàm ```is_even``` được truyền vào hàm ```filter```, mà không đi kèm (). Điều này nghĩa là một tham chiếu tới hàm ```is_even``` được truyền vào ```filter```, và ```is_even``` không được gọi ngay lập tức. 
```python
def is_even(x):
    return x % 2 == 0

numbers = [1, 2, 3, 4, 5]
even_filter = filter(is_even, numbers)
print(list(even_filter))  
```

```
[2, 4]
```
**Ví dụ với ```map```**

```double``` được truyền vào ```map```.
```python
def double(x):
    return x * 2

numbers = [1, 2, 3, 4, 5]
double_numbers = map(double, numbers)
print(list(double_numbers))  

```
```
[2, 4, 6, 8, 10]
```
Thay vì sử dụng built-in functions nhàm chán như ```map``` hay ```filter```, hãy xét 1 ví dụ thú vị sau đây, với hàm bậc cao tự định nghĩa, nhận 1 hàm làm đối số:

```python 
import random
def track_the_timeline (action, current_batman=''):
    """
    track the timeline of Batman before and after Barry
    went back in time to do {action}.
    """
    print(f"Before Barry went back in time to {action.__name__}, Batman was {current_batman}." )
    current_batman = action()
    print(f"After Barry went back in time to {action.__name__}, Batman is {current_batman}.")

def save_mom():
    print("Barry saved his mom and changed the timeline.")   
    batman_variants = ["Bruce Wayne", "Thomas Wayne", "Jason Todd", "Dick Grayson", "Jim Gordon"]
    return random.choice(batman_variants)

current_batman = "Bruce Wayne"
current_batman = track_the_timeline(save_mom, current_batman)
```


``` 
Before Barry went back in time to save_mom, Batman was Bruce Wayne.
Barry saved his mom and changed the timeline.
After Barry went back in time to save_mom, Batman is Thomas Wayne.
```
Ở ví dụ trên, ta đã sử dụng hàm ```track_the_timeline``` để theo dõi sự thay đổi của lịch sử khi The Flash Barry Allen - người hùng của chúng ta quay ngược thời gian để cứu mẹ của mình.  Cụ thể hơn, ta muốn biết danh tính của Kỵ sĩ Bóng đêm Batman trước và sau khi Barry thay đổi lịch sử. 
Do vậy, ta truyền hàm (hành động) ```save_mom``` vào hàm ```track_the_timeline``` như một đối số, và ```current_batman``` để theo dõi danh tính của Batman trước và sau khi "hành động" này được thực hiện.

Như vậy, ta đã thấy 3 ví dụ về việc truyền hàm như đối số cho hàm khác. Vói việc coi hàm như một first-class object, Python cho phép ta sử dụng hàm linh hoạt hơn, giúp giảm lặp code và tăng khả năng tái sử dụng code.

## Ví dụ 3: Trả về từ hàm
```python
def scale(factor):
  """
  3. Returns a function that scales a number by the given factor.
  """
  def inner(x):
    return x * factor
  return inner

# 1. Assign scale function's RETURNED VALUE (also a function) 
# to variables
doubler = scale(2)
tripler = scale(3)

# 2. Passed as arguments to map function
numbers = [1, 2, 3]
scaled_numbers = list(map(doubler, numbers))  
print(scaled_numbers)  

scaled_numbers = list(map(tripler, numbers))  
print(scaled_numbers)  

```
Ở ví dụ này, mình kết hợp cả 3 đặc tính của một first-class function. Hàm ```inner``` được trả về từ  ```scale```. ```inner``` sau đó được gán cho ```doubler``` và ```tripler```, hai biến này được truyền vào ```map```. Cuối cùng ta được kết quả khi áp dụng phép nhân 2 và 3 cho list ```number```.

```
[2, 4, 6]
[3, 6, 9]
```
Khác với 1 hàm thông thường, hàm mình tự định nghĩa ở trên: ```scale``` và hàm built-in: ```map``` lần lượt __1.trả về kết quả là 1 hàm__  và __2.nhận 1 hàm làm đối số truyền vào__. Trong Python, những hàm thoả mãn ít nhất 1 trong 2 điều kiện trên được gọi là __HIGHER ORDER FUNCTION__ hay __Hàm bậc cao__.

Lưu ý rằng đặc tính thứ 3 (được trả về từ hàm) ở ví dụ này nghĩa là hàm ```scale``` trả về object (hàm) ```inner```, chứ không phải __giá trị__ trả về của ```inner```. Nếu ngược lại, ta đã không thể sử dụng ```doubler``` làm đối số cho ```map```.

```python
print(doubler)
```

```>>> <function scale.<locals>.inner at 0x0000021E767DD310>```



## Closure
Ngoài ra, hàm ```scale``` còn được gọi là __enclosing function__, tức là hàm chứa 1 hàm khác.
Ngược lại, một __nested (inner) function__ là một hàm được định nghĩa bên trong hàm khác. __Closure__, trường hợp đặc biệt của _nested function_, là một hàm có tham chiếu đến một giá trị được khai báo bên trong _enclosing function_, có quyền truy cập đến tài nguyên trong scope nó thuộc về. 
Ở ví dụ trên, ```inner``` là 1 closure. Mặc dù ```factor``` không nằm trong phạm vi của hàm ```inner```, nhưng lại thuộc scope chứa ```inner```, do vậy ta có thể truy cập ```factor``` bên trong ```inner``` một cách bình thường.



## Ví dụ 4: Lưu trữ trong các cấu trúc dữ liệu.
Là 1 first-class object, hàm trong Python có thể được lưu trữ trong các cấu trúc dữ liệu như list, tuple, dictionary, set, ... .
```
def square(x):
  return x * x

def cube(x):
  return x * x * x

functions = [square, cube]  # Store functions in a list

for func in functions:
  print(func(3))  

```
```
9
27
```

__Như vậy__, qua 4 ví dụ trên, chúng ta đã hiểu được thế nào là một __first-class object__, __higher order function__, __closure__, các khái niệm nền tảng hữu ích cho việc hiểu và sử dụng _decorators_. Sang phần tiếp theo, ta hãy cùng tìm hiểu _decorators_, nội dung chính của bài viết này.

# Decorators


## Decorator là gì?
Như đã đề cập ở đầu bài viết, _decorators_ cung cấp khả năng thay đổi hoặc mở rộng hành vi của các hàm hoặc lớp mà không cần phải chỉnh sửa mã nguồn của chúng.  



## Function decorator

## Class decorator

# Ứng dụng của Decorators trong Python

## Machine leanring

@torch.no_grad()

@torch.enable_grad()

Tăng tốc Python với numba
@numba.jit(nopython=True)

# 1 số facts tráng miệng
- map và filter thực ra là class objects trong Python.

- hầu hết các framework sử dụng Python cho web development đều sử dụng decorator rất thường xuyên.
# Resources


 https://en.wikipedia.org/wiki/First-class_citizen

 Programming Language Pragmatics, Second Edition 3.5.2 First- and Second-Class Subroutines

 [First Class Objects](https://www.cs.iusb.edu/~dvrajito/teach/c311/c311_2_firstclass.html)

 [Structure and Interpretation of Computer Programs](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/full-text/book/book-Z-H-12.html#footnote_Temp_121:~:text=%60%60rights%20and%20privileges%27%27%20of%20first%2Dclass%20elements) 

[Python Objects Part IV: First-Class Everything](https://medium.com/@bdov_/python-objects-part-iv-first-class-everything-7da3945e3552)

[Higher Order Functions in Python](https://www.geeksforgeeks.org/higher-order-functions-in-python/)

[Python Deep Dive: Hiểu closures, decorators và các ứng dụng của chúng ](https://magz.techover.io/2021/12/04/python-deep-dive-hieu-closures-decorators-va-cac-ung-dung-cua-chung-phan-2/#closures)
<!-- # My links

https://softwareengineering.stackexchange.com/questions/39742/when-is-a-feature-considered-a-first-class-citizen-in-a-programming-language-p  

https://stackoverflow.com/questions/245192/what-are-first-class-objects


[Bài viết hay và đẩy đủ về object và first-class trong Python](https://medium.com/@bdov_/python-objects-part-iv-first-class-everything-7da3945e3552)

In Python both the classes and the objects are first class objects. (See [this answer](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949) for more details about classes as objects).

[Why Map and Filter Aren’t Really Functions in Python?](https://archive.ph/PEPa2#selection-657.0-657.242)

-->