## rasm2 汇编/反汇编
```sh
rasm2 [选项] [参数]
```
选项|意义
-|-
-a 架构|指定架构，架构可选项见-L
-b 位数|指定架构位数
-d|反汇编hex字节串，否则汇编
-f|从文件读取，否则参数
-L|列出汇编插件
* 最后一个参数是`-`时，从标准输入读取
## rahash2 基于块的散列
```sh
rahash2 [选项] [参数]
```
选项|意义
-|-
-a 算法|指定算法
-L|列出可用算法
## rax2 进制转换
```sh
rax2 [选项] [参数]
```
选项|意义
-|-
无|十进制、0x hex互转
-b|binary转char
-S|raw转hex
-s|hex转raw
