# Лабораторная работа 4:
Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса Travis CI



## Выполнение работы

### 1. Подготовка проекта

Создана структура проекта:
lab04/<br>
├── .gitignore<br>
├── .github/<br>
│   └── workflows/<br>
│       └── build.yml<br>
├── CMakeLists.txt<br>
├── formatter_lib/<br>
│   ├── CMakeLists.txt<br>
│   ├── formatter.h<br>
│   └── formatter.cpp<br>
├── formatter_ex_lib/<br>
│   ├── CMakeLists.txt<br>
│   ├── formatter_ex.h<br>
│   └── formatter_ex.cpp<br>
├── solver_lib/<br>
│   ├── CMakeLists.txt<br>
│   ├── solver.h<br>
│   └── solver.cpp<br>
├── hello_world_application/<br>
│   ├── CMakeLists.txt<br>
│   └── hello_world.cpp<br>
└── solver_application/<br>
    ├── CMakeLists.txt<br>
    └── equation.cpp<br>
  
## Создаем .gitignore для отсеивания лишних файлов
.gitignore 
```
build/
*/build/
*.a
*.so
*.o
*.out
*.exe
*~
*.swp
.DS_Store
.vscode/
.idea/
```
## Создаем .github/workflows/buld.yml:
```
name: Build on Linux and Windows

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-gcc:
    name: Build with GCC (Linux)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: sudo apt update && sudo apt install -y g++ cmake
      - name: Configure
        run: cmake -S . -B build -DCMAKE_CXX_COMPILER=g++
      - name: Build
        run: cmake --build build

  build-clang:
    name: Build with Clang (Linux)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: sudo apt update && sudo apt install -y clang cmake
      - name: Configure
        run: cmake -S . -B build -DCMAKE_CXX_COMPILER=clang++
      - name: Build
        run: cmake --build build

  build-msvc:
    name: Build with MSVC (Windows)
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - name: Configure (MSVC)
        run: cmake -S . -B build
      - name: Build (MSVC)
        run: cmake --build build --config Release

  build-mingw:
    name: Build with MinGW (Windows)
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install MinGW and CMake
        run: choco install mingw cmake -y
      - name: Add MinGW to PATH
        run: echo "C:\ProgramData\chocolatey\lib\mingw\tools\install\mingw64\bin" | Out-File -FilePath $env:GITHUB_PATH -Encoding utf8 -Append
      - name: Configure (MinGW)
        run: cmake -S . -B build -G "MinGW Makefiles"
      - name: Build (MinGW)
        run: cmake --build build
```
В результате получаем проверку всех тестов 
