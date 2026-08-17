# C++14 & C++17 — Những điểm mới so với C++11

---

# PHẦN 1 — C++14

## 1. Generic lambda (tham số `auto`)

C++11 lambda phải khai báo kiểu tham số cụ thể. C++14 cho phép dùng `auto`, lambda trở thành template ẩn.

```cpp
// C++11 — phải cố định kiểu
auto add11 = [](int a, int b) { return a + b; };

// C++14 — generic, dùng được cho nhiều kiểu
auto add14 = [](auto a, auto b) { return a + b; };

add14(1, 2);        // int
add14(1.5, 2.5);     // double
add14(std::string("a"), std::string("b")); // string
```

## 2. Return type deduction cho hàm thường (`auto`)

C++11 chỉ cho phép `auto` return type kèm `-> decltype(...)` (trailing return type). C++14 cho phép compiler tự suy luận, không cần trailing.

```cpp
// C++11 — phải viết trailing return type
auto add11(int a, int b) -> int { return a + b; }

// C++14 — compiler tự suy luận kiểu trả về từ return
auto add14(int a, int b) { return a + b; }
```

## 3. `decltype(auto)`

Giữ nguyên tính chất reference/const mà `auto` thường làm mất.

```cpp
int x = 10;
int& getRef() { return x; }

auto a = getRef();          // a là int (copy), mất đi reference
decltype(auto) b = getRef(); // b là int& (giữ nguyên reference)
```

## 4. Relaxed `constexpr` (constexpr thoải mái hơn)

C++11: hàm `constexpr` chỉ được có đúng 1 câu lệnh `return`. C++14: cho phép loop, biến cục bộ, nhiều lệnh, if/else.

```cpp
// C++11 — chỉ 1 return, không loop được
constexpr int factorial11(int n) {
    return n <= 1 ? 1 : n * factorial11(n - 1); // phải viết đệ quy
}

// C++14 — viết như hàm bình thường, có loop, biến local
constexpr int factorial14(int n) {
    int result = 1;
    for (int i = 2; i <= n; ++i) {
        result *= i;
    }
    return result;
}
```

## 5. Variable templates

Template áp dụng cho biến, không chỉ cho hàm/class.

```cpp
template<typename T>
constexpr T pi = T(3.1415926535897932385);

float f = pi<float>;
double d = pi<double>;
```

## 6. Binary literal

```cpp
int mask = 0b1010'1100; // = 172, kết hợp luôn với digit separator
```

## 7. Digit separator (`'`)

Chỉ để dễ đọc, không ảnh hưởng giá trị.

```cpp
long population = 1'000'000'000;
```

## 8. `std::make_unique`

C++11 có `std::make_shared` nhưng thiếu `make_unique` (bị bỏ sót). C++14 bổ sung, tránh phải viết `new` trực tiếp.

```cpp
// C++11 — vẫn phải new thủ công cho unique_ptr
std::unique_ptr<Widget> w11(new Widget());

// C++14 — an toàn hơn với exception, gọn hơn
auto w14 = std::make_unique<Widget>();
```

---

# PHẦN 2 — C++17

## 1. Structured bindings

Giải nén tuple/pair/struct thành nhiều biến trong một lệnh.

```cpp
std::map<std::string, int> ages = {{"An", 20}};

// C++11 — dài dòng
for (const auto& kv : ages) {
    std::cout << kv.first << " = " << kv.second << "\n";
}

// C++17 — giải nén trực tiếp
for (const auto& [name, age] : ages) {
    std::cout << name << " = " << age << "\n";
}

// Cũng dùng được với struct
struct Point { int x, y; };
Point p{1, 2};
auto [x, y] = p;
```

## 2. `if`/`switch` với init-statement

Khai báo biến giới hạn phạm vi ngay trong `if`/`switch`, tránh biến "rò rỉ" ra ngoài scope.

```cpp
// C++17
if (auto it = myMap.find("key"); it != myMap.end()) {
    std::cout << it->second;
}
// 'it' không tồn tại ngoài if — tránh ô nhiễm namespace

switch (auto status = getStatus(); status) {
    case Status::Ok: break;
    default: break;
}
```

## 3. `if constexpr`

Rẽ nhánh biên dịch (compile-time branch) trong template — nhánh không thỏa điều kiện sẽ không được compile, rất hữu ích thay cho SFINAE phức tạp.

```cpp
template<typename T>
auto getValue(T t) {
    if constexpr (std::is_pointer_v<T>) {
        return *t;      // chỉ compile khi T là con trỏ
    } else {
        return t;        // chỉ compile khi T không phải con trỏ
    }
}
```

## 4. Class Template Argument Deduction (CTAD)

Không cần ghi rõ tham số template khi khởi tạo, compiler tự suy luận từ constructor.

```cpp
// C++11/14 — phải ghi rõ <int, int>
std::pair<int, int> p11(1, 2);

// C++17 — compiler tự suy luận kiểu
std::pair p17(1, 2);       // std::pair<int, int>
std::vector v17{1, 2, 3};   // std::vector<int>
```

## 5. `std::optional`

Biểu diễn giá trị "có thể không tồn tại" mà không cần con trỏ hay giá trị sentinel (-1, nullptr...).

```cpp
std::optional<int> parseInt(const std::string& s) {
    try {
        return std::stoi(s);
    } catch (...) {
        return std::nullopt; // không có giá trị hợp lệ
    }
}

if (auto result = parseInt("123"); result.has_value()) {
    std::cout << *result;
}
```

## 6. `std::variant`

Union kiểu an toàn — chỉ chứa 1 trong các kiểu đã khai báo, có kiểm tra kiểu tại runtime.

```cpp
std::variant<int, std::string> v = 42;
v = "hello"; // đổi sang giữ string

std::visit([](auto&& value) { std::cout << value; }, v);
```

## 7. `std::any`

Chứa giá trị của bất kỳ kiểu nào (khác `variant` là không giới hạn danh sách kiểu cố định).

```cpp
std::any a = 1;
a = std::string("text");
a = 3.14;

double d = std::any_cast<double>(a);
```

## 8. `std::string_view`

"View" (không sở hữu, không copy dữ liệu) trên chuỗi ký tự — tránh copy không cần thiết khi chỉ đọc.

```cpp
// C++11 — truyền const std::string& vẫn phải convert nếu input là const char*
void printOld(const std::string& s) { std::cout << s; }

// C++17 — không copy, hoạt động với string, const char*, substring...
void printNew(std::string_view s) { std::cout << s; }

std::string s = "hello world";
printNew(s);                       // không copy
printNew("literal");               // không tạo std::string tạm
printNew(std::string_view(s).substr(0, 5)); // "hello", không cấp phát bộ nhớ mới
```

## 9. `std::filesystem`

Thao tác đường dẫn/file/thư mục chuẩn hóa, thay thế code phụ thuộc OS (`<dirent.h>`, WinAPI...).

```cpp
namespace fs = std::filesystem;

for (const auto& entry : fs::directory_iterator(".")) {
    std::cout << entry.path().filename() << "\n";
}

if (!fs::exists("output")) {
    fs::create_directory("output");
}
```

## 10. Inline variables

Cho phép định nghĩa biến `inline` trong header mà không vi phạm One Definition Rule (ODR) khi header được include ở nhiều file `.cpp`.

```cpp
// header.h — C++11 phải dùng trick (hàm static hoặc extern + .cpp riêng)
// C++17:
inline int counter = 0;              // OK, không lỗi "multiple definition"
inline constexpr double kPi = 3.14159;
```

## 11. Nested namespace definition

```cpp
// C++11
namespace Company { namespace Project { namespace Module {
    void run();
}}}

// C++17
namespace Company::Project::Module {
    void run();
}
```

## 12. Attributes mới: `[[nodiscard]]`, `[[maybe_unused]]`, `[[fallthrough]]`

```cpp
[[nodiscard]] int computeChecksum(); // gọi mà không lấy giá trị trả về -> warning

void f([[maybe_unused]] int debugOnlyParam) {
    // tránh warning "unused parameter" khi build release
}

switch (x) {
    case 1:
        doSomething();
        [[fallthrough]]; // báo rõ ý định "rơi xuống" case tiếp theo, không phải quên break
    case 2:
        doMore();
        break;
}
```

---

## Ghi chú thêm (nhỏ, hay bị bỏ sót)

| Tính năng | Chuẩn | Ghi chú |
|---|---|---|
| `std::size()`, `std::empty()`, `std::data()` | C++17 | Hàm tự do dùng cho container/array, thay vì gọi method |
| `static_assert(condition)` không cần message | C++17 | C++11 bắt buộc phải có message |
| `[[nodiscard]]` trên attribute cấp class | C++17 | Áp cho toàn bộ hàm trả về type đó |
| `std::auto_ptr` bị xóa hẳn | C++17 | Đã deprecated ở C++11, dùng `unique_ptr` |
| Trigraph bị xóa | C++17 | Cú pháp cổ hiếm dùng, không còn hỗ trợ |
| `std::uncaught_exceptions()` (số nhiều) | C++17 | Thay cho `uncaught_exception()` (C++11) |

---

## Bước tiếp theo

Đọc qua từng mục, đánh dấu:
- Cái nào bạn **đã dùng thường xuyên** trong code automotive → có thể bỏ khỏi checklist học.
- Cái nào **lạ/mới** → giữ lại để luyện tập thêm (đặc biệt: `structured bindings`, `if constexpr`, `optional/variant/string_view` — rất hay gặp trong code hiện đại).
