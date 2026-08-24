# 🐍 Python cơ bản (cho dân C++) — Ôn thi phỏng vấn

> Mục tiêu: đọc 1 lần là code được Python cơ bản, đủ để trả lời phỏng vấn "bạn có biết Python không?".
> Cách dùng: đọc lướt → gõ lại code trên máy → nhớ syntax khác C++ ở đâu.

---

## 0. Bản chất ngôn ngữ (nói được 30 giây)

- **Thông dịch** (interpreted): code chạy qua **CPython** (bản chính thức), biên dịch sang **bytecode** `.pyc` → chạy trên **PVM** (Python Virtual Machine). Không cần compile như C++.
- **Dynamic typing**: biến không cần khai báo kiểu, kiểu gắn với **giá trị** chứ không phải biến.
- **Strong typing**: `"1" + 2` → lỗi (khác JavaScript).
- **Everything is object**: cả `int`, `function`, `class` đều là object.
- **Garbage Collector**: reference counting + cycle detector → không cần `delete` như C++.
- **GIL (Global Interpreter Lock)**: mỗi lúc chỉ 1 thread chạy Python bytecode → CPU-bound phải dùng `multiprocessing`, I/O-bound dùng `threading`/`asyncio` là được.
- **Indentation = block**: không có `{}`, thụt lề chuẩn **4 spaces**.
- Phiên bản hiện dùng: **Python 3.11/3.12** (Python 2 đã EOL 2020).

---

## 1. Cài & chạy

```bash
python --version              # Windows/macOS
python3 --version             # Linux/macOS
python file.py                # chạy script
python -m venv .venv          # tạo virtual environment
.venv\Scripts\activate        # Windows PowerShell
source .venv/bin/activate     # Linux/macOS
pip install requests numpy    # cài package
pip freeze > requirements.txt # export
pip install -r requirements.txt
```

**IDE phổ biến**: VS Code (+ extension Python + Pylance) → tốt nhất cho beginner. Khác: PyCharm, Jupyter Notebook (data), Spyder.

---

## 2. Kiểu dữ liệu cơ bản

```python
# Số
a = 10              # int (không giới hạn độ dài!)
b = 3.14            # float
c = 2 + 3j          # complex

# Chuỗi
s = "hello"         # hoặc 'hello' — như nhau
s2 = """nhiều
dòng"""
name = "World"
print(f"Hello, {name}!")           # f-string (Python 3.6+)
print(f"{a:.2f}")                  # format 2 chữ số thập phân
print(f"{a=}")                     # debug: "a=10"

# Bool
t = True; f = False                 # viết hoa!

# None (giống nullptr / null)
x = None

# Ép kiểu
int("42"); float("3.14"); str(10); bool(0)  # bool(0)=False, bool("")=False
```

### Toán tử khác C++
```python
a / b       # chia thường → float (5/2 = 2.5)
a // b      # chia lấy nguyên (5//2 = 2)
a % b       # dư
a ** b      # lũy thừa (a^b)
# Không có ++, --
a += 1
```

---

## 3. Chuỗi (string) — bất biến (immutable)

```python
s = "Hello Python"
len(s)              # 12
s[0]                # 'H'
s[-1]               # 'n' (index âm)
s[0:5]              # 'Hello' (slice)
s[::-1]             # đảo ngược
s.upper(); s.lower(); s.strip(); s.replace("H", "J")
s.split(" ")        # ['Hello', 'Python']
",".join(["a","b"]) # 'a,b'
"Py" in s           # True
```

---

## 4. Cấu trúc dữ liệu (thuộc lòng!)

### List (mutable, giống `std::vector`)
```python
xs = [1, 2, 3, "mix", True]
xs.append(4)                # thêm cuối
xs.insert(0, 99)            # chèn
xs.pop(); xs.pop(0)         # xóa cuối / theo index
xs.remove(2)                # xóa theo giá trị
xs.sort(); xs.reverse()
sorted(xs); reversed(xs)    # không sửa gốc
xs[1:3]                     # slice
len(xs); 3 in xs
```

### Tuple (immutable)
```python
t = (1, 2, 3)
x, y, z = t                 # unpacking
a, b = b, a                 # swap
```

### Dict (giống `std::unordered_map`)
```python
d = {"name": "Sang", "age": 27}
d["name"]; d.get("age", 0)  # get có default
d["job"] = "dev"            # thêm/sửa
del d["age"]
"name" in d
for k, v in d.items():
    print(k, v)
d.keys(); d.values()
```

### Set (giống `std::unordered_set`)
```python
s = {1, 2, 3}
s.add(4); s.remove(2)
a & b; a | b; a - b         # giao/hợp/hiệu
```

### Comprehension (đặc sản Python)
```python
squares = [x*x for x in range(10)]
evens   = [x for x in range(20) if x % 2 == 0]
d = {x: x*x for x in range(5)}
s = {x % 3 for x in range(10)}
```

---

## 5. Điều khiển luồng

```python
# if / elif / else — không dùng ()
if x > 0:
    print("dương")
elif x == 0:
    print("bằng 0")
else:
    print("âm")

# ternary
label = "chẵn" if x % 2 == 0 else "lẻ"

# while
while x > 0:
    x -= 1

# for — luôn duyệt qua iterable
for i in range(5):          # 0..4
    print(i)
for i in range(1, 10, 2):   # start, stop, step
    print(i)

for i, v in enumerate(xs):  # có index
    print(i, v)

for a, b in zip(xs, ys):    # 2 list song song
    print(a, b)

# break / continue / else (chạy nếu vòng không break)
for x in xs:
    if x == target: break
else:
    print("not found")

# match (Python 3.10+, giống switch)
match code:
    case 200: print("OK")
    case 404: print("Not Found")
    case _:   print("Other")
```

---

## 6. Hàm

```python
def add(a, b):
    return a + b

# Tham số mặc định
def greet(name, msg="Hi"):
    return f"{msg}, {name}"

# Keyword arguments
greet(name="Sang", msg="Hello")

# *args, **kwargs
def f(*args, **kwargs):
    print(args)     # tuple
    print(kwargs)   # dict
f(1, 2, 3, a=10, b=20)

# Type hints (không bắt buộc, dùng cho Pylance/mypy)
def add(a: int, b: int) -> int:
    return a + b

# Lambda
square = lambda x: x*x

# Trả về nhiều giá trị (thực chất là tuple)
def divmod2(a, b):
    return a // b, a % b
q, r = divmod2(10, 3)
```

---

## 7. OOP

```python
class Animal:
    species = "Unknown"           # class attribute (giống static)

    def __init__(self, name):     # constructor
        self.name = name          # instance attribute

    def speak(self):              # method — luôn có self
        return f"{self.name} makes a sound"

    def __str__(self):            # giống operator<<
        return f"Animal({self.name})"

    def __repr__(self):
        return self.__str__()

class Dog(Animal):                # kế thừa
    species = "Dog"

    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def speak(self):              # override
        return f"{self.name} barks"

d = Dog("Rex", "Husky")
print(d.speak())
print(isinstance(d, Animal))       # True

# Property (getter/setter)
class Temp:
    def __init__(self, c): self._c = c
    @property
    def celsius(self): return self._c
    @celsius.setter
    def celsius(self, v):
        if v < -273: raise ValueError
        self._c = v

t = Temp(25); t.celsius = 30; print(t.celsius)

# Dataclass (Python 3.7+) — auto __init__/__repr__/__eq__
from dataclasses import dataclass
@dataclass
class Point:
    x: float
    y: float
```

**Đặc biệt / khác C++**:
- Không có `private` thật, quy ước `_name` = protected, `__name` = name-mangling.
- Đa kế thừa OK (MRO — C3 linearization).
- Duck typing: "nếu kêu quack là vịt" → không cần interface.

---

## 8. Exception

```python
try:
    x = int(input("nhập số: "))
    y = 10 / x
except ValueError as e:
    print("không phải số:", e)
except ZeroDivisionError:
    print("chia 0")
except Exception as e:              # bắt tất cả
    print("lỗi khác:", e)
else:
    print("OK, y =", y)             # chạy nếu không lỗi
finally:
    print("luôn chạy")              # giống destructor RAII

# Ném lỗi
raise ValueError("giá trị sai")

# Custom exception
class MyError(Exception): pass
```

---

## 9. File I/O & Context Manager

```python
# with = RAII, tự đóng file
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()              # đọc hết
    # for line in f: ...            # đọc từng dòng

with open("out.txt", "w", encoding="utf-8") as f:
    f.write("hello\n")
    f.writelines(["a\n", "b\n"])

# JSON
import json
with open("data.json") as f:
    data = json.load(f)              # dict
json.dumps(data, indent=2, ensure_ascii=False)

# CSV
import csv
with open("data.csv") as f:
    for row in csv.reader(f):
        print(row)
```

---

## 10. Module & Package

```python
# file math_utils.py
def add(a, b): return a + b

# file main.py
import math_utils
math_utils.add(1, 2)

from math_utils import add
add(1, 2)

from math_utils import add as plus

# Standard library thường dùng
import os, sys, time, datetime, math, random, re, json
from pathlib import Path       # thay os.path (mới hơn)
from collections import defaultdict, Counter, deque
from itertools import chain, product, combinations
from functools import reduce, lru_cache, partial

# Chạy như script
if __name__ == "__main__":
    main()
```

---

## 11. Iterator, Generator, Decorator

```python
# Generator — lazy, tiết kiệm RAM
def count_up(n):
    i = 0
    while i < n:
        yield i               # trả về từng phần, giữ state
        i += 1
for x in count_up(3): print(x)

gen = (x*x for x in range(5))  # generator expression

# Decorator — bọc hàm
import time
def timer(fn):
    def wrapper(*args, **kw):
        t0 = time.time()
        r = fn(*args, **kw)
        print(f"{fn.__name__} took {time.time()-t0:.3f}s")
        return r
    return wrapper

@timer
def slow():
    time.sleep(0.5)
slow()
```

---

## 12. Async (I/O-bound)

```python
import asyncio

async def fetch(url):
    print("start", url)
    await asyncio.sleep(1)      # giả lập I/O
    return f"data from {url}"

async def main():
    results = await asyncio.gather(
        fetch("a"), fetch("b"), fetch("c")
    )
    print(results)

asyncio.run(main())
```

**Nhớ**: `async def` → coroutine, phải `await` để chạy, `asyncio.run` để start từ sync code.

---

## 13. Threading vs Multiprocessing (GIL!)

```python
# I/O-bound → threading OK
from threading import Thread
t = Thread(target=fn, args=(1,))
t.start(); t.join()

# CPU-bound → multiprocessing (bỏ qua GIL)
from multiprocessing import Process, Pool
with Pool(4) as p:
    result = p.map(square, [1,2,3,4])
```

---

## 14. Regex (nhanh)

```python
import re
re.search(r"\d+", "abc 123")            # Match hoặc None
re.findall(r"\w+", "hello world")       # ['hello', 'world']
re.sub(r"\s+", "_", "a  b  c")          # 'a_b_c'
m = re.match(r"(\w+)@(\w+)", "sang@bk")
m.group(0); m.group(1)                  # 'sang@bk', 'sang'
```

---

## 15. Ứng dụng phổ biến (nhớ để nói khi phỏng vấn)

| Lĩnh vực | Thư viện chính |
|---|---|
| **Test automation / factory** | `pyserial`, `pymodbus`, `pyvisa` (đo lường), `pytest` |
| **Web/API** | `requests`, `flask`, `fastapi`, `django` |
| **Data / AI** | `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `pytorch`, `tensorflow` |
| **Automation / script** | `os`, `pathlib`, `subprocess`, `shutil`, `argparse` |
| **GUI** | `tkinter` (có sẵn), `PyQt5/PySide6`, `Kivy` |
| **Embedded / IoT** | `RPi.GPIO`, `gpiozero`, `micropython` |
| **Database** | `sqlite3` (có sẵn), `pymysql`, `psycopg2`, `sqlalchemy` |
| **Testing** | `unittest` (có sẵn), `pytest` (phổ biến), `mock` |

---

## 16. Ví dụ nhanh cho phỏng vấn EMS/Factory

**Đọc Serial (COM port):**
```python
import serial
with serial.Serial("COM3", 9600, timeout=1) as ser:
    ser.write(b"MEAS?\n")
    reply = ser.readline().decode().strip()
    print("Value:", reply)
```

**Modbus TCP:**
```python
from pymodbus.client import ModbusTcpClient
c = ModbusTcpClient("192.168.1.10", port=502)
c.connect()
rr = c.read_holding_registers(0, 10, slave=1)
print(rr.registers)
c.close()
```

**Xử lý log test → thống kê PASS/FAIL:**
```python
from collections import Counter
with open("test.log") as f:
    results = [line.split(",")[2].strip() for line in f]
c = Counter(results)
print(c["PASS"], c["FAIL"])
print(f"Yield = {c['PASS']/sum(c.values())*100:.2f}%")
```

**REST API test bằng requests:**
```python
import requests
r = requests.get("https://api.example.com/data", timeout=5)
r.raise_for_status()
print(r.json())
```

**Unit test với pytest:**
```python
# test_math.py
def add(a, b): return a + b

def test_add():
    assert add(2, 3) == 5
```
Chạy: `pytest test_math.py -v`

---

## 17. Python vs C++ — cheat sheet nhớ nhanh

| C++ | Python |
|---|---|
| `int x = 10;` | `x = 10` |
| `std::string` | `str` (immutable) |
| `std::vector` | `list` |
| `std::unordered_map` | `dict` |
| `std::unordered_set` | `set` |
| `nullptr` | `None` |
| `//` comment | `#` comment |
| `{}` block | thụt lề 4 space |
| `&&`, `\|\|`, `!` | `and`, `or`, `not` |
| `for (int i=0; i<n; ++i)` | `for i in range(n):` |
| `printf`/`cout` | `print(f"...")` |
| `class : public Base` | `class Sub(Base):` |
| `this` | `self` (tường minh, phải viết) |
| `virtual` + override | mặc định virtual, dùng lại tên |
| RAII destructor | `with ...:` context manager |
| Template | Duck typing / `typing.Generic` |
| Header + `.cpp` | 1 file `.py`, `import` |
| `make`/`cmake` | `pip` + `venv` |

---

## 18. Câu hỏi phỏng vấn Python — trả lời đầy đủ (tập trung cơ chế)

> Đây là câu trả lời "kể chuyện" như đang phỏng vấn thật. Mỗi câu ~ 20–40 giây nói. Nhấn vào **cơ chế** — nếu quên syntax cứ nói "lâu rồi không code, nhưng nguyên lý là…" là hoàn toàn OK với vị trí không phải Python-first.

### Q1. Python là ngôn ngữ biên dịch hay thông dịch? Nó chạy như thế nào?

Python thường bị gọi là "thông dịch" nhưng thực chất là **hybrid**. Khi mình chạy `python file.py`, interpreter phổ biến là **CPython** sẽ làm 2 bước:

1. **Compile** source `.py` thành **bytecode** — dạng lệnh trung gian, cache lại ở `__pycache__/*.pyc` để lần sau chạy nhanh hơn.
2. **Execute** bytecode đó trên **PVM (Python Virtual Machine)** — một máy ảo stack-based, đọc từng lệnh bytecode và thực thi.

Khác C++: C++ compile thẳng ra machine code cho CPU. Python compile ra bytecode cho VM → chậm hơn nhưng đổi lại **portable** (cùng bytecode chạy được nhiều OS) và **dynamic** (thay đổi code runtime, `eval`, `exec` được). Ngoài CPython còn có PyPy (JIT, nhanh hơn), Jython (chạy trên JVM), IronPython (.NET), MicroPython (embedded).

### Q2. GIL là gì? Vì sao Python có GIL và nó ảnh hưởng thế nào?

**GIL = Global Interpreter Lock** — một mutex trong CPython đảm bảo tại một thời điểm chỉ có **một thread** được thực thi bytecode Python. Nguyên nhân lịch sử: CPython quản lý bộ nhớ bằng **reference counting**, không thread-safe → nếu 2 thread cùng tăng/giảm refcount 1 object → race condition → memory leak/crash. GIL là giải pháp đơn giản để giữ interpreter thread-safe.

**Hệ quả**:
- **CPU-bound** (tính toán nhiều): dù có 8 thread cũng chỉ chạy tuần tự → phải dùng `multiprocessing` (mỗi process 1 GIL riêng) hoặc gọi C extension như NumPy (thả GIL khi vào code C).
- **I/O-bound** (đọc file, network, serial, sleep): thread bị block chờ I/O sẽ **thả GIL** ra → thread khác chạy được → `threading` và `asyncio` vẫn tăng tốc rất tốt.

Python 3.13+ đang thử nghiệm **"free-threaded" build** (loại bỏ GIL) nhưng chưa production.

### Q3. Python quản lý bộ nhớ và giải phóng object thế nào? So với C++?

Python có **automatic memory management**, không cần `new/delete`. Cơ chế 2 tầng:

1. **Reference counting** (chính): mỗi object có 1 bộ đếm số reference trỏ đến. Khi count về 0 → object bị giải phóng ngay lập tức. Rất nhanh, deterministic (giống RAII).
2. **Cycle detector / Generational GC** (phụ): giải quyết vòng tham chiếu (A → B → A) mà refcount không xử lý được. Chạy định kỳ, chia object thành 3 thế hệ, thế hệ trẻ được quét thường xuyên hơn.

Object nhỏ (int, tuple ngắn) được quản lý trong **memory pools** riêng cho hiệu năng. Object cache: các số nhỏ [-5, 256] và một số string được **intern** → cùng object cho nhiều biến, đó là lý do `a is b` đôi khi `True` bất ngờ với số nhỏ.

So với C++: C++ mình tự quản lý (hoặc dùng smart_ptr với refcount tương tự `shared_ptr`), Python làm tự động → dễ code nhưng khó kiểm soát khi cần realtime hoặc ép giải phóng.

### Q4. Dynamic typing hoạt động thế nào? Vì sao Python "chậm hơn" C++?

Trong Python, **kiểu gắn với giá trị (object), không gắn với biến**. Biến chỉ là 1 cái "nhãn" trỏ đến object. `x = 5` rồi `x = "hello"` hoàn toàn hợp lệ.

Cơ chế: mọi thứ trong Python đều là **PyObject** — 1 struct C chứa `type`, `refcount`, và data. Khi mình viết `a + b`, interpreter phải:
1. Nhìn vào `type(a)` → tìm method `__add__` phù hợp.
2. Nếu không có → thử `type(b).__radd__`.
3. Gọi method → có thể raise `TypeError`.

Mỗi phép toán = 1 vài lookup + function call → chậm hơn C++ (nơi `int + int` = 1 lệnh CPU) khoảng 10–100 lần. Bù lại: linh hoạt, duck typing, viết nhanh.

Cách tăng tốc: **NumPy/Pandas** (vector hóa trong C), **Cython/Numba** (compile ra machine code), **C extension**, hoặc gọi C++ qua `pybind11`/`ctypes`.

### Q5. `is` khác `==` thế nào? Cho ví dụ dễ sai.

- `==` gọi `__eq__` → so sánh **giá trị/nội dung**.
- `is` so sánh **identity** — 2 biến có cùng trỏ đến 1 object trong bộ nhớ không (thực ra là so `id(a) == id(b)`).

```python
a = [1, 2]; b = [1, 2]
a == b   # True — cùng nội dung
a is b   # False — 2 object khác nhau

x = None
x is None    # ĐÚNG cách check None
x == None    # cũng ra True nhưng chậm hơn và không Pythonic
```

Bẫy hay gặp: `a = 256; b = 256; a is b` → `True` (vì Python cache số nhỏ). Nhưng `a = 257; b = 257; a is b` → có thể `False`. Đây là **implementation detail**, đừng dựa vào.

Quy tắc: dùng `is` chỉ khi so với `None`, `True`, `False`, hoặc sentinel object; còn lại dùng `==`.

### Q6. Mutable vs Immutable — hệ quả với hàm và dict?

**Immutable**: `int`, `float`, `str`, `tuple`, `frozenset`, `bool` — không thể sửa in-place, mọi "sửa" đều tạo object mới.
**Mutable**: `list`, `dict`, `set`, class tự viết — sửa in-place được.

**Hệ quả 1 — argument mặc định (bẫy kinh điển)**:
```python
def add(x, xs=[]):   # SAI!
    xs.append(x)
    return xs
add(1)   # [1]
add(2)   # [1, 2]  ← chia sẻ list giữa các lần gọi!
```
Vì default value được tạo **một lần** khi định nghĩa hàm. Fix: dùng `xs=None` rồi `if xs is None: xs = []`.

**Hệ quả 2 — key của dict/set**: phải là immutable + hashable. `dict` không cho dùng `list` làm key, nhưng `tuple` thì được (nếu bên trong cũng immutable).

**Hệ quả 3 — truyền tham số**: Python truyền **reference to object** (không phải "by value" hay "by reference" theo cách C++). Nếu object mutable → sửa trong hàm sẽ ảnh hưởng bên ngoài. Nếu immutable → không thể sửa.

### Q7. Truyền tham số trong Python: pass-by-value hay pass-by-reference?

Thực chất là **pass-by-object-reference** (còn gọi "pass-by-assignment"). Khi gọi `f(x)`, tham số bên trong hàm nhận **cùng reference** trỏ đến object mà `x` đang trỏ.

- Nếu object **mutable** và mình sửa in-place (`list.append`, `dict[k]=v`) → thay đổi thấy được ngoài hàm.
- Nếu mình **gán lại** biến (`x = new_value`) → chỉ đổi nhãn local, ngoài hàm không bị ảnh hưởng.

```python
def f(lst, s):
    lst.append(99)      # sửa in-place → ngoài thấy
    s = s + "!"         # gán lại → local thôi
```

Đây là điểm hay confuse người từ C++ (`&` reference vs value). Ở Python **luôn** là reference, khác biệt chỉ ở object đó có mutable không.

### Q8. Iterator, Iterable, Generator khác nhau thế nào?

- **Iterable**: object có method `__iter__()` trả về iterator. Ví dụ: `list`, `dict`, `str`, file object.
- **Iterator**: object có `__next__()` (và `__iter__()` trả về chính nó). Mỗi lần gọi trả 1 giá trị, hết thì raise `StopIteration`.
- **Generator**: một cách viết iterator gọn hơn, dùng `yield`. Khi hàm có `yield`, nó không chạy ngay khi gọi mà trả về generator object; mỗi lần `next()` thì code chạy đến `yield` rồi **pause, giữ state** (biến local, vị trí lệnh), lần sau chạy tiếp từ đó.

Lợi ích generator: **lazy evaluation** + **tiết kiệm RAM**. Đọc file 10GB dòng-theo-dòng, hoặc stream dữ liệu vô hạn, đều dùng generator.

`for x in something:` thực chất là:
```python
it = iter(something)          # gọi __iter__
while True:
    try: x = next(it)         # gọi __next__
    except StopIteration: break
```

### Q9. Decorator hoạt động thế nào ở tầng ngôn ngữ?

Trong Python, **hàm là first-class object** — có thể gán vào biến, truyền vào hàm khác, trả về từ hàm. Decorator lợi dụng điều đó.

`@my_decorator` phía trên `def foo` chỉ là **cú pháp đường** cho:
```python
foo = my_decorator(foo)
```

Nghĩa là: định nghĩa `foo` → truyền nó vào `my_decorator` → gán lại tên `foo` bằng thứ mà decorator trả về (thường là 1 wrapper function bao quanh hàm gốc).

Ứng dụng thực tế: `@staticmethod`, `@classmethod`, `@property`, `@lru_cache` (cache kết quả), `@app.route(...)` của Flask, logging, timing, retry, authentication. Trong test station mình có thể viết `@retry(3)` để tự thử lại đọc thiết bị 3 lần khi timeout.

### Q10. Threading, Multiprocessing, Asyncio — chọn cái nào khi nào?

Ba mô hình xử lý concurrency, chọn theo bản chất công việc:

| Loại việc | Ví dụ | Chọn | Vì sao |
|---|---|---|---|
| **I/O-bound**, ít task | Đọc 3 file, gọi vài API | `threading` | Đơn giản, GIL nhả khi I/O |
| **I/O-bound**, rất nhiều task | 10.000 kết nối, scrape web | `asyncio` | 1 thread, event loop, cực nhẹ |
| **CPU-bound** | Xử lý ảnh, tính toán ma trận | `multiprocessing` hoặc NumPy | Bỏ qua GIL bằng cách tách process |

`asyncio` khác `threading` ở chỗ **cooperative** — task tự "nhả" quyền chạy tại điểm `await`, không có preemption → không cần lock cho hầu hết trường hợp, nhưng **1 hàm blocking là kẹt cả loop**. `threading` là preemptive nhưng có GIL, cần lock cho shared state.

Nhớ: `time.sleep()` block thread → trong asyncio phải `await asyncio.sleep()`. Đọc `pyserial` blocking → trong asyncio phải chạy trong thread pool (`run_in_executor`) hoặc dùng lib async như `pyserial-asyncio`.

### Q11. Duck Typing và Polymorphism trong Python?

**Duck typing**: *"If it walks like a duck and quacks like a duck, it is a duck."* — Python không quan tâm object thuộc class gì, chỉ quan tâm nó **có method/attribute cần dùng không**.

```python
def print_all(source):
    for x in source:      # cần source có __iter__
        print(x)
# gọi được với list, tuple, dict, set, generator, file, string...
```

Khác C++ nơi interface/template phải khớp chính xác. Ở Python không cần `interface` — cứ implement method là dùng được. Nếu muốn "chính thức hóa" thì có **Protocol** (PEP 544 — structural subtyping) hoặc **ABC** (abstract base class).

Cách check: thay vì `isinstance()`, dùng `hasattr(obj, 'read')` hoặc try/except gọi thẳng ("EAFP" — Easier to Ask Forgiveness than Permission — triết lý Python).

### Q12. Context Manager (`with`) hoạt động thế nào?

`with` là cách Python cung cấp **RAII kiểu tường minh**. Cú pháp:
```python
with open("f.txt") as f:
    data = f.read()
```
Tương đương:
```python
f = open("f.txt")
f.__enter__()
try:
    data = f.read()
finally:
    f.__exit__(exc_type, exc_val, exc_tb)
```

Bất kỳ object nào có `__enter__` và `__exit__` đều dùng được với `with`. Ứng dụng: đóng file, đóng socket, release lock, rollback DB transaction khi có exception, tắt kết nối serial, dừng timer.

Tự viết nhanh bằng `@contextmanager`:
```python
from contextlib import contextmanager
@contextmanager
def timer():
    t0 = time.time()
    yield
    print(f"{time.time()-t0:.3f}s")

with timer():
    do_something()
```

Vì Python có GC không deterministic (không biết khi nào `__del__` chạy), context manager là **cách chuẩn** để cleanup tài nguyên — quan trọng hơn cả trong C++ vì C++ ít nhất có destructor tự động ở scope exit.

### Q13. Class trong Python: `self`, `__init__`, `__new__`, MRO?

- **`self`** = tường minh của `this` trong C++. Method thực chất là **hàm thường** với tham số đầu là instance. `obj.method(x)` = `Class.method(obj, x)`.
- **`__new__(cls, ...)`**: tạo và trả về instance mới (giống factory). Rất hiếm khi override, trừ khi làm singleton, immutable class, hoặc metaclass.
- **`__init__(self, ...)`**: khởi tạo giá trị cho instance đã tạo. Đây là cái quen gọi "constructor".
- **`__del__`**: destructor — nhưng KHÔNG đảm bảo chạy đúng lúc do GC, nên đừng dựa vào để cleanup resource; dùng `with` thay thế.
- **MRO (Method Resolution Order)**: khi đa kế thừa, Python dùng thuật toán **C3 linearization** để quyết định thứ tự tìm method. Xem bằng `ClassName.__mro__`. `super()` follow MRO chứ không phải parent trực tiếp.
- **Không có `private` thật**: `_x` = convention "đừng đụng", `__x` bị **name-mangled** thành `_ClassName__x` để tránh trùng ở subclass. Không phải bảo mật, chỉ là quy ước.

### Q14. Import và Module trong Python?

Mỗi file `.py` là 1 **module**. Thư mục có `__init__.py` (Python 3.3+ có thể không cần) là **package**.

Khi `import foo`:
1. Python tìm `foo` theo thứ tự trong `sys.path` (thư mục hiện tại, PYTHONPATH, site-packages).
2. Nạp và **thực thi toàn bộ code top-level** của file 1 lần (cache trong `sys.modules`).
3. Gán module object vào tên `foo` trong namespace hiện tại.

Import lần 2 không chạy lại — lấy từ cache. Đây là lý do có `if __name__ == "__main__":` — khi file được import, `__name__` = tên module; khi chạy trực tiếp `python file.py`, `__name__` = `"__main__"`. Nhờ vậy code test/demo ở dưới không chạy khi bị import.

Circular import (A import B, B import A) thường gây lỗi → nên tổ chức code theo tầng, dùng lazy import khi cần.

### Q15. `virtualenv` và `pip` — vì sao quan trọng?

Python cài package vào **site-packages** dùng chung → 2 project cần `numpy` version khác nhau sẽ đụng nhau. `venv` tạo 1 thư mục con có Python interpreter và site-packages **riêng**.

Workflow chuẩn:
```bash
python -m venv .venv
.venv\Scripts\activate       # Windows
pip install -r requirements.txt
```
`pip freeze > requirements.txt` để lock version, deploy máy khác dựng lại đúng môi trường.

Ngoài `venv` còn có `pipenv`, `poetry`, `conda` (data science, quản lý cả non-Python deps), `uv` (mới, rất nhanh). Trong công nghiệp mình có thể gặp `conda` cho AI/ML, `poetry` cho web project.

### Q16. Ngoại lệ (Exception) trong Python khác C++ thế nào?

Ở Python, exception là **chuẩn** — được dùng rất phổ biến kể cả cho luồng bình thường (ví dụ `StopIteration` khi hết iterator). Triết lý **EAFP** (Easier to Ask Forgiveness): cứ thử làm, sai thì bắt.

```python
try:
    v = d["key"]
except KeyError:
    v = default
```
Được ưa hơn:
```python
if "key" in d: v = d["key"]     # LBYL — Look Before You Leap
else: v = default
```

Tất cả exception kế thừa từ `BaseException` → `Exception` → cụ thể. Nên `except Exception` (bỏ qua `SystemExit`, `KeyboardInterrupt`), không dùng `except:` bare.

`finally` chạy luôn; `else` chạy khi try không có lỗi (ít người biết, hữu ích để tách logic thành công).

Khác C++: không có exception specification, không cần `noexcept`; performance cost của raise/catch trong Python cao hơn code thường nên đừng dùng như control flow trong vòng lặp nóng.

### Q17. Câu hỏi bẫy — "Python có thực sự đa luồng không?"

Trả lời chuẩn: **Có thread thật (OS thread), nhưng bị GIL giới hạn chỉ 1 thread chạy bytecode tại 1 thời điểm**. Nên:

- Với **I/O** (đọc file, network, serial, HTTP call) → GIL được nhả khi thread block chờ I/O → thread khác chạy → đa luồng "thật" ở góc nhìn tổng thể.
- Với **tính toán thuần Python** → không tăng tốc dù có nhiều core → coi như single-thread.

Muốn dùng nhiều core cho CPU → dùng `multiprocessing` (mỗi process 1 interpreter + 1 GIL riêng, giao tiếp qua pipe/queue), hoặc gọi thư viện C như NumPy (giải phóng GIL bên trong).

Đây là câu hỏi rất hay hỏi và cần trả lời **có kèm cơ chế + trường hợp sử dụng**, không chỉ "Python bị GIL".

### Q18. Python có mạnh không? Vì sao vẫn dùng dù chậm?

Trả lời chín chắn (không phòng thủ):
- **Không nhanh bằng C++/Rust** cho CPU-bound thuần, đó là đánh đổi cho **tốc độ phát triển**, **hệ sinh thái khổng lồ**, **đọc dễ**.
- Trong thực tế, phần "chậm" luôn được **đẩy xuống C** qua NumPy, Pandas, PyTorch. Python đóng vai **glue** — kết dính, điều phối.
- Với **factory automation / test station / data analysis / script**, tốc độ CPU không phải bottleneck → bottleneck là I/O thiết bị, đọc DB, network → Python đủ nhanh và code nhanh gấp 5–10 lần C++.
- Với hệ realtime cứng, embedded firmware → dùng C/C++; Python chỉ ở phía host/PC.

Câu này thể hiện mình **hiểu chỗ đứng** của Python trong stack công nghệ — điểm cộng lớn với interviewer senior.

### Q19. Tôi lâu không code Python — nếu bạn giao 1 task tôi cần bao lâu?

Câu trả lời khôn ngoan (thành thật + tự tin):
> "Em nền C++ rất chắc, syntax Python em có thể phải tra lại trong 1–2 ngày đầu, nhưng **cơ chế ngôn ngữ** — dynamic typing, GIL, GC, decorator, context manager, generator — em vẫn nắm. Với task automation/test em có thể lên productivity trong tuần đầu. Điểm em cần thời gian là ecosystem cụ thể (Pandas, framework web…), nhưng Python đọc dễ, docs tốt, em học rất nhanh."

Đây là cách "quản trị kỳ vọng" — nhà tuyển dụng đánh giá cao hơn là bạn cố tỏ ra thành thạo rồi vào việc bị vỡ.
