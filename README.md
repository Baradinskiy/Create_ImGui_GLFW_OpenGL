# 🧊 ImGui + GLFW + OpenGL

Простой пример того, как создать графическое окно с меню ImGui, используя GLFW и OpenGL. Этот репозиторий поможет вам быстро настроить проект с помощью `CMake`.

---

## 📁 Структура проекта

```plaintext
your-project/
├── CMakeLists.txt
├── include/
│   └── imgui/        # Сюда помещаем ImGui
├── src/
│   └── main.cpp      # Точка входа
```

---

## ⚙️ Шаг 1: Создание базовых файлов

### `CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.31)
project(YourProjectName)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(${PROJECT_NAME} src/main.cpp)
```

### `main.cpp`

```cpp
#include <iostream>
using namespace std;

int main(int argc, char* argv[]) {
    cout << "Hello, ImGui + GLFW + OpenGL!" << endl;
    return 0;
}
```

---

## 📦 Шаг 2: Установка зависимостей

### 🔹 ImGui

Клонируем официальный репозиторий:

```bash
git clone https://github.com/ocornut/imgui.git include/imgui
```

Удалите ненужные файлы, оставив только:

* `imgui.cpp`, `imgui_draw.cpp`, `imgui_tables.cpp`, `imgui_widgets.cpp`, `imgui_demo.cpp`
* `backends/imgui_impl_glfw.cpp`, `backends/imgui_impl_opengl3.cpp`
* Все соответствующие заголовки (`*.h`)

---

### 🔹 GLFW

Добавим его через `FetchContent`:

```cmake
include(FetchContent)

FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)
FetchContent_MakeAvailable(glfw)
```

---

### 🔹 OpenGL

Рекомендуемый способ — использовать `vcpkg`:

```bash
vcpkg install opengl
vcpkg integrate install
```

И подключить в `CMake`:

```cmake
find_package(OpenGL REQUIRED)
```

---

## 🔧 Финальный `CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.31)
project(ImGuiApp)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ImGui
set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/include/imgui)
set(IMGUI_SOURCES
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

add_library(imgui STATIC ${IMGUI_SOURCES})

target_include_directories(imgui PUBLIC
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# GLFW
include(FetchContent)
FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)
FetchContent_MakeAvailable(glfw)

# OpenGL
find_package(OpenGL REQUIRED)

# Executable
add_executable(${PROJECT_NAME} src/main.cpp)

target_link_libraries(${PROJECT_NAME}
    PRIVATE imgui glfw OpenGL::GL
)
```

---

## ▶️ Запуск

1. Склонируйте репозиторий:

   ```bash
   git clone https://github.com/Baradinskiy/Create_ImGui_GLFW_OpenGL.git
   cd Create_ImGui_GLFW_OpenGL
   ```

2. Создайте папку сборки и соберите проект:

   ```bash
   mkdir build && cd build
   cmake ..
   cmake --build .
   ```

---

## 🧠 Результат

После запуска вы получите окно с базовым GUI от ImGui, работающим на OpenGL + GLFW. Отличная стартовая точка для вашего проекта!

---

## 📜 Лицензия

Этот проект распространяется под лицензией MIT. Подробнее см. [LICENSE](LICENSE).
