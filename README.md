### Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```sh
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=DaniilRyb
```

```sh
# Переход в  рабочую директорию
$ cd ${GITHUB_USERNAME}/workspace
$ pushd . 
~/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace ~/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace
$ source scripts/activate
```

```sh
# Клонируем предыдущую ЛР2
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
Cloning into 'projects/lab03'...
remote: Enumerating objects: 34, done.
remote: Counting objects: 100% (34/34), done.
remote: Compressing objects: 100% (27/27), done.
remote: Total 34 (delta 5), reused 13 (delta 2), pack-reused 0
Unpacking objects: 100% (34/34), 7.51 KiB | 3.00 KiB/s, done.
$ cd projects/lab03
$ git remote remove  origin 

$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```sh

$ g++ -std=c++2a -I./include -c sources/print.cpp
$ ls print. 
$ nm print.o | grep print
00000076 t __GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
00000000 T __Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000001b T __Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E

$ ar rvs print.a print.o
C:\Program Files (x86)\mingw-w64\i686-8.1.0-posix-dwarf-rt_v6-rev0\mingw32\bin\ar.exe: creating print.a
a - print.o

$ file print.a
print.a: current ar archive
$ g++ -std=c++2a -I./include -c examples/example1.cpp

$ ls example1.o 
example1.o
$ g++ example1.o print.a -o example1

$ ./example1 && echo
hello
```


```sh

$ g++ -std=c++2a -I./include -c examples/example2.cpp
# Вывод информации о бинарном файле
$ nm example2.o
00000000 b .bss
00000000 d .ctors
00000000 d .data
00000000 r .eh_frame
00000000 d .gcc_except_table
00000000 r .rdata
00000000 r .rdata$zzz
00000000 t .text
         U ___gxx_personality_v0
         U ___main
000000d6 t ___tcf_0
00000116 t __GLOBAL__sub_I_main
         U __Unwind_Resume
000000e8 t __Z41__static_initialization_and_destruction_0ii
         U __Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
         U __ZNSaIcEC1Ev
         U __ZNSaIcED1Ev
         U __ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
         U __ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
         U __ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
         U __ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
         U __ZNSt8ios_base4InitC1Ev
         U __ZNSt8ios_base4InitD1Ev
00000000 r __ZStL19piecewise_construct
00000000 b __ZStL8__ioinit
         U _atexit
00000000 T _main

$ g++ example2.o print.a -o example2

$ ./example2
$ cat log.txt && echo
hello
```

Удаление всех созданных в процессе работы файлов

```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF

```
Устанавливаем стандарт языка и делаем его обязательным
```sh
$ cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
Сборка статической библиотеки с именем _print_ и исходником `print.cpp`
```sh
$ cat >> CMakeLists.txt <<EOF

add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```
Добавление директори с хэдерами и указание, где искать заголовочные файлы
```sh
$ cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```
Создание директории, которую CMake будет использовать в качестве корневого каталога сборки и указание каталога, где искать исходные файлы для проекта и последущая сборка
```sh
$ cmake -H. -B_build
-- Building for: Visual Studio 15 2017
-- Selecting Windows SDK version 10.0.17763.0 to target Windows 6.1.7601.
-- The C compiler identification is MSVC 19.16.27034.0
-- The CXX compiler identification is MSVC 19.16.27034.0
-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe
-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe - works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe
-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe - works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_build
$ cmake --build _build
Microsoft (R) Build Engine версии 15.9.21+g9802d43bc3 для .NET Framework
(C) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

  Checking Build System
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/CMakeLists.txt
  print.cpp
  print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/CMakeLists.txt
```



```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```


```sh
$ cmake --build _build # Сборка всего проекта
CMake is re-running because C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_build/CMakeFiles/generate.stamp is out-of-date.
  the file 'C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/CMakeLists.txt'
  is newer than 'C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_build/CMakeFiles/generate.stamp.depend'
  result='-1'
-- Selecting Windows SDK version 10.0.17763.0 to target Windows 6.1.7601.
-- Configuring done
-- Generating done
-- Build files have been written to: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_build
Microsoft (R) Build Engine версии 15.9.21+g9802d43bc3 для .NET Framework
(C) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

  print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/CMakeLists.txt
  example1.cpp
  example1.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\example1.exe
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/CMakeLists.txt
  example2.cpp
  example2.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\example2.exe

$ cmake --build _build --target print # Сборка только статической библиотеки
cmake --build _build --target print
Microsoft (R) Build Engine версии 15.9.21+g9802d43bc3 для .NET Framework
(C) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

  print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib

$ cmake --build _build --target example1 # Сборка example1
print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib
  example1.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\example1.exe

$ cmake --build _build --target example2 # Сборка example2

print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib
  example2.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\example2.exe
```


```sh
$ ls -la _build/libprint.a 
$ _build/example1 && echo 
hello
$ _build/example2 
$ cat log.txt && echo 
hello
$ rm -rf log.txt
```

```sh
$ git clone https://github.com/tp-labs/lab03 tmp # Клонирование удаленного репозитория в директорию
```
git clone https://github.com/tp-labs/lab03 tmp
Cloning into 'tmp'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 88 (delta 7), reused 4 (delta 2), pack-reused 73
Unpacking objects: 100% (88/88), 1.03 MiB | 95.00 KiB/s, done.


$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp 
```
Компилируем статическую библиотеку в подкаталог `_install`
```sh
$ cat CMakeLists.txt 
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)


$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install # Сборка файла _build в директорию install
cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
-- Selecting Windows SDK version 10.0.17763.0 to target Windows 6.1.7601.
-- Configuring done
-- Generating done
-- Build files have been written to: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_build
$ cmake --build _build --target install
 print.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\projects\lab03\_build\Debug\print.lib
  -- Install configuration: "Debug"
  -- Installing: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_install/lib/print.lib
  -- Installing: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_install/include
  -- Installing: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_install/include/print.hpp
  -- Installing: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_install/cmake/print-config.cmake
  -- Installing: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/projects/lab03/_install/cmake/print-config-debug.cmake
$ tree _install
install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

3 directories, 4 files
```

```sh
$ git add CMakeLists.txt
[master 57db870] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt
$ git commit -m "added CMakeLists.txt"
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    lab02

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        "C\357\200\272UsersDaniil-PCDaniil_RipDaniil_RipDaniilRybworkspaceprojectslab03_buildDebugprint.lib"

no changes added to commit (use "git add" and/or "git commit -a")

$ git push origin master
```

## Report
```sh
$ popd
~/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}\Cloning into 'tasks/lab03'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 88 (delta 7), reused 4 (delta 2), pack-reused 73
Unpacking objects: 100% (88/88), 1.03 MiB | 112.00 KiB/s, done.
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```


## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
