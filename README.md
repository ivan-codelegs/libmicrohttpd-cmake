# libmicrohttpd-cmake
The Cmake Port of **Windows** *.sln* `libmicrohttpd v0.9.75` project

Next examples show you how to build `libmicrohttpd` project with `cmake`
- Debug Example
```
mkdir bin-debug
cmake -B bin-debug ^
-G "CodeBlocks - Ninja" ^
-DCMAKE_C_COMPILER=clang-cl ^
-DCMAKE_CXX_COMPILER=clang-cl ^
-DHTTPD_ROOT_PATH="C:/path/to/libmicrohttpd/" ^
-DCMAKE_BUILD_TYPE=Debug .

cmake --build bin-debug --target all
```
- Release with Debug Information Example
```
mkdir bin-release
cmake -B bin-release ^
-G "CodeBlocks - Ninja" ^
-DCMAKE_C_COMPILER=clang-cl ^
-DCMAKE_CXX_COMPILER=clang-cl ^
-DHTTPD_ROOT_PATH="C:/path/to/libmicrohttpd/" ^
-DCMAKE_BUILD_TYPE=RelWithDebInfo .

cmake --build bin-release --target all
```

## Pay Attention!
- Debug configuration is built as dynamic library linked to **debug dynamic runtime** libraries
- Release configuration is built as static library linked to **ordinary static runtime** libraries

*If you get any crashes at start up or compile/linkage issues* you should change runtime linkage in this cmake file
