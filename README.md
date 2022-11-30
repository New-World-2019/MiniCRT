# MiniCRT

来自 程序员的自我修养——链接、装载与库

简单 C/C++ 运行时库实现

## 原理

https://www.sohu.com/a/560159289_121119002


## 支持平台

|         | X86  | X64  | ARM  | AARCH64 |
| :-----: | :--: | :--: | :--: | :-----: |
|  WIN10  |    |      |      |         |
|   OSX   |    |  ✅  |      |         |
| Manjaro |     |  ✅  |      |         |
| Ubuntu  |     | ✅ |      |         |


## 使用方法

```shell
git clone https://github.com/MRNIU/MiniCRT.git
cd MiniCRT
mkdir build
cmake ..
make
```

构建完成后，库文件保存位置：build/lib/libminicrt.a
测试程序位置： build/bin/test


