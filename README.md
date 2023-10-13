# cmsis-v5
CMake based CPack for using cmsis v5 with other toolchains

## Targets
How to build for `STM32F031K6`
```
$ cmake --preset CPack_Clang_STM32F031K6
$ cmake --build --preset CPack_Clang_STM32F031K6 --target package
```
How to build for `STM32H7A3ZI`
```
$ cmake --preset CPack_Clang_STM32H7A3ZI
$ cmake --build --preset CPack_Clang_STM32H7A3ZI --target package
```
How to build for `STM32H743XI`
```
$ cmake --preset CPack_Clang_STM32H743XI
$ cmake --build --preset CPack_Clang_STM32H743XI --target package
```
