---
description: Rules cho dự án C++ hiện đại (C++17/20) — memory management, RAII, smart pointers, STL, CMake.
author: TaiVo-AI
version: 1.0
tags: ["cpp", "c++17", "c++20", "cmake", "systems", "performance"]
globs: ["**/*.cpp", "**/*.hpp", "**/*.h", "**/CMakeLists.txt"]
---

# C++ Modern Guidelines (C++17/20)

## Standard tối thiểu: C++17 — ưu tiên C++20 cho project mới

## Naming Conventions

| Loại | Convention | Ví dụ |
|------|-----------|-------|
| Class, Struct, Enum | PascalCase | `UserManager`, `NetworkPacket` |
| Function, Method | camelCase | `getUserById()`, `sendPacket()` |
| Variable | camelCase | `userId`, `packetSize` |
| Member variable | `m_camelCase` | `m_buffer`, `m_size` |
| Constant, constexpr | UPPER_SNAKE | `MAX_BUFFER_SIZE` |
| Namespace | lowercase | `network::utils` |
| Template param | PascalCase | `template<typename ValueType>` |

## Memory Management — RAII là nguyên tắc số 1

```cpp
// ✅ ĐÚNG — Smart pointers, không dùng raw new/delete
auto manager = std::make_unique<ResourceManager>(config);
auto sharedData = std::make_shared<DataBuffer>(1024);

// ✅ ĐÚNG — RAII cho resource management
class FileHandle {
public:
    explicit FileHandle(const std::string& path)
        : m_file(std::fopen(path.c_str(), "r"), &std::fclose) {
        if (!m_file) throw std::runtime_error("Cannot open: " + path);
    }
    FILE* get() const noexcept { return m_file.get(); }
private:
    std::unique_ptr<FILE, decltype(&std::fclose)> m_file;
};

// ❌ SAI — Raw pointers với manual memory
ResourceManager* mgr = new ResourceManager();  // BANNED
// ... (có thể leak nếu exception xảy ra)
delete mgr;

// ❌ SAI — malloc/free trong C++ code
char* buf = (char*)malloc(1024);  // BANNED — dùng std::vector<char> hoặc std::string
```

## Move Semantics & Perfect Forwarding

```cpp
// ✅ ĐÚNG — Move constructor & assignment
class Buffer {
public:
    Buffer(Buffer&& other) noexcept
        : m_data(std::exchange(other.m_data, nullptr))
        , m_size(std::exchange(other.m_size, 0)) {}

    Buffer& operator=(Buffer&& other) noexcept {
        if (this != &other) {
            delete[] m_data;
            m_data = std::exchange(other.m_data, nullptr);
            m_size = std::exchange(other.m_size, 0);
        }
        return *this;
    }
};

// ✅ ĐÚNG — Perfect forwarding
template<typename T, typename... Args>
std::unique_ptr<T> makeResource(Args&&... args) {
    return std::make_unique<T>(std::forward<Args>(args)...);
}

// ✅ ĐÚNG — Pass by value khi cần copy, by const ref khi read-only
void processName(std::string name);           // Sẽ move nếu caller pass rvalue
void readName(const std::string& name);       // Read-only, không copy
void takeName(std::string&& name);            // Yêu cầu rvalue (ownership transfer)
```

## Error Handling

```cpp
// ✅ ĐÚNG — Exceptions cho exceptional cases
// ✅ ĐÚNG — std::expected (C++23) hoặc std::optional cho expected failures

// C++17: std::optional
std::optional<User> findUser(int id) {
    auto it = m_users.find(id);
    if (it == m_users.end()) return std::nullopt;
    return it->second;
}

auto user = findUser(42);
if (user) processUser(*user);

// C++17: std::variant cho error types
using Result = std::variant<User, NetworkError, DatabaseError>;
Result getUser(int id);

// ❌ SAI — Return codes kiểu C
int getUser(int id, User* out);  // Tránh — dùng std::optional hoặc exception
```

## STL & Algorithms

```cpp
// ✅ ĐÚNG — Dùng STL algorithms thay vì raw loops
std::vector<int> values = {3, 1, 4, 1, 5, 9};

// Sort
std::ranges::sort(values);  // C++20

// Filter (C++20 ranges)
auto evens = values | std::views::filter([](int n){ return n % 2 == 0; });

// Transform
std::vector<std::string> names;
std::ranges::transform(users, std::back_inserter(names),
    [](const User& u){ return u.getName(); });

// ✅ ĐÚNG — Range-based for với const ref
for (const auto& [key, value] : myMap) {
    processEntry(key, value);
}
```

## Concurrency

```cpp
// ✅ ĐÚNG — std::mutex với lock_guard (RAII)
class ThreadSafeQueue {
    mutable std::mutex m_mutex;
    std::queue<Task> m_queue;
public:
    void push(Task task) {
        std::lock_guard lock(m_mutex);  // Tự unlock khi ra scope
        m_queue.push(std::move(task));
    }
    std::optional<Task> pop() {
        std::lock_guard lock(m_mutex);
        if (m_queue.empty()) return std::nullopt;
        auto task = std::move(m_queue.front());
        m_queue.pop();
        return task;
    }
};

// ✅ ĐÚNG — std::async cho simple async tasks
auto future = std::async(std::launch::async, [&]{
    return computeExpensiveResult(data);
});
auto result = future.get();

// ❌ SAI — Data races
// Truy cập shared data từ nhiều thread không có mutex → UNDEFINED BEHAVIOR
```

## CMake Setup

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Compiler warnings
if(MSVC)
  target_compile_options(MyLib PRIVATE /W4 /WX)
else()
  target_compile_options(MyLib PRIVATE -Wall -Wextra -Wpedantic -Werror)
endif()

# Library
add_library(MyLib STATIC
  src/UserManager.cpp
  src/NetworkClient.cpp
)
target_include_directories(MyLib PUBLIC include)

# Executable
add_executable(MyApp src/main.cpp)
target_link_libraries(MyApp PRIVATE MyLib)

# Tests
enable_testing()
add_subdirectory(tests)
```

## Header File Best Practices

```cpp
// ✅ ĐÚNG — Pragma once (thay #ifndef guards)
#pragma once

// ✅ ĐÚNG — Forward declarations để giảm compile time
class UserManager;  // Thay vì #include "UserManager.hpp" nếu chỉ dùng pointer/ref

// ✅ ĐÚNG — Include order: own header → C++ stdlib → C stdlib → third-party → project
#include "MyClass.hpp"
#include <string>
#include <vector>
#include <cstdint>
#include <fmt/format.h>
#include "utils/Logger.hpp"
```

## Không được làm

- KHÔNG dùng `new`/`delete` trực tiếp — dùng `std::make_unique`/`std::make_shared`
- KHÔNG dùng `reinterpret_cast` trừ khi thực sự cần thiết (ghi rõ lý do)
- KHÔNG bỏ qua warnings compiler (`-Wall -Wextra`)
- KHÔNG dùng C-style arrays — dùng `std::array` hoặc `std::vector`
- KHÔNG `using namespace std;` trong header files
- KHÔNG dùng `printf`/`scanf` — dùng `std::cout` hoặc `fmt::print`
- KHÔNG copy large objects khi có thể move
- KHÔNG viết destructor nếu không cần (Rule of Zero)
