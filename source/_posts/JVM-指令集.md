---
title: JVM 指令集
date: 2019-04-05 08:22:21
tags: 字节码
---

| 指令码    | 助记符          | 描述                                                         |
| :-------- | :-------------- | :----------------------------------------------------------- |
| 0x00      | nop             | 无操作                                                       |
| 0x01      | aconst_null     | null 进栈                                                    |
| 0x02      | iconst_m1       | int 型常量值 -1 进栈                                         |
| 0x03      | iconst_0        |                                                              |
| 0x04      | iconst_1        |                                                              |
| 0x05      | iconst_2        |                                                              |
| 0x06      | iconst_3        |                                                              |
| 0x07      | iconst_4        |                                                              |
| 0x08      | iconst_5        |                                                              |
| 0x09      | lconst_0        |                                                              |
| 0x0A      | lconst_1        |                                                              |
| 0x0B      | fconst_0        |                                                              |
| 0x0C      | fconst_1        |                                                              |
| 0x0D      | fconst_2        |                                                              |
| 0x0E      | dconst_0        |                                                              |
| 0x0F      | dconst_1        |                                                              |
| 0x10      | bipush          | 将一个 byte 型常量值(-128~127)推送至栈顶                     |
| 0x11      | sipush          | 将一个 short 型常量值(-32768~32767)推送至栈顶                |
| 0x12      | ldc             | 将 int、float 或 String 型常量值从常量池中推送至栈顶         |
| 0x13      | ldc_w           | 将 int、float 或 String 型常量值从常量池中推送至栈顶(宽索引) |
| 0x14      | ldc2_w          | 将 long 或 double 型常量值从常量池中推送至栈顶(宽索引)       |
| 0x15      | iload           | 将指定的 int 型本地变量推送至栈顶                            |
| 0x16      | lload           |                                                              |
| 0x17      | fload           |                                                              |
| 0x18      | dload           |                                                              |
| 0x19      | aload           | 将指定的引用类型的本地变量推送至栈顶                         |
| 0x1A      | iload_0         | 将第 0 个 int 型本地变量推送至栈顶                           |
| 0x1B      | iload_1         | 将第 1 个 int 型本地变量推送至栈顶                           |
| 0x1C      | iload_2         | 将第 2 个 int 型本地变量推送至栈顶                           |
| 0x1D      | iload_3         | 将第 3 个 int 型本地变量推送至栈顶                           |
| 0x1E      | lload_0         |                                                              |
| 0x1F      | lload_1         |                                                              |
| 0x20      | lload_2         |                                                              |
| 0x21      | lload_3         |                                                              |
| 0x22      | fload_0         |                                                              |
| 0x23      | fload_1         |                                                              |
| 0x24      | fload_2         |                                                              |
| 0x25      | fload_3         |                                                              |
| 0x26      | dload_0         |                                                              |
| 0x27      | dload_1         |                                                              |
| 0x28      | dload_2         |                                                              |
| 0x29      | dload_3         |                                                              |
| 0x2A      | aload_0         |                                                              |
| 0x2B      | aload_1         |                                                              |
| 0x2C      | aload_2         |                                                              |
| 0x2D      | aload_3         |                                                              |
| 0x2E      | iaload          | 将 int 型数组指定索引的值推送至栈顶                          |
| 0x2F      | laload          |                                                              |
| 0x30      | faload          |                                                              |
| 0x31      | daload          |                                                              |
| 0x32      | aaload          | 将引用类型数组指定索引的值推送至栈顶                         |
| 0x33      | baload          | 将 boolean 或 byte 型数组指定索引的值推送至栈顶              |
| 0x34      | caload          | 将 char 型数组指定索引的值推送至栈顶                         |
| 0x35      | saload          | 将 short 型数组指定索引的值推送至栈顶                        |
| 0x36      | istore          | 将栈顶 int 型数值存入指定本地变量                            |
| 0x37      | lstore          | 将栈顶 long 型数值存入指定本地变量                           |
| 0x38      | fstore          |                                                              |
| 0x39      | dstore          |                                                              |
| 0x3A      | astore          |                                                              |
| 0x3B      | istore_0        | 将栈顶 int 型数值存入第 0 个本地变量                         |
| 0x3C      | istore_1        |                                                              |
| 0x3D      | istore_2        |                                                              |
| 0x3E      | istore_3        |                                                              |
| 0x3F      | lstore_0        |                                                              |
| 0x40      | lstore_1        |                                                              |
| 0x41      | lstore_2        |                                                              |
| 0x42      | lstore_3        |                                                              |
| 0x43      | fstore_0        |                                                              |
| 0x44      | fstore_1        |                                                              |
| 0x45      | fstore_2        |                                                              |
| 0x46      | fstore_3        |                                                              |
| 0x47      | dstore_0        |                                                              |
| 0x48      | dstore_1        |                                                              |
| 0x49      | dstore_2        |                                                              |
| 0x4A      | dstore_3        |                                                              |
| 0x4B      | astore_0        |                                                              |
| 0x4C      | astore_1        |                                                              |
| 0x4D      | astore_2        |                                                              |
| 0x4E      | astore_3        |                                                              |
| 0x4F      | iastore         | 将栈顶 int 型数值存入指定数组的指定索引位置                  |
| 0x50      | lastore         |                                                              |
| 0x51      | fastore         |                                                              |
| 0x52      | dastore         |                                                              |
| 0x53      | aastore         |                                                              |
| 0x54      | bastore         | 将栈顶 boolean 或 byte 型数值存入指定数组的指定索引位置      |
| 0x55      | castore         | 将栈顶 char 型数值存入指定数组的指定索引位置                 |
| 0x56      | sastore         | 将栈顶 short 型数值存入指定数组的指定索引位置                |
| 0x57      | pop             | 将栈顶数值弹出(数值不能是 long 或 double 类型)               |
| 0x58      | pop2            | 将栈顶一个(long 或 double 类型)或两个(其它类型)数值弹出      |
| 0x59      | dup             | 复制栈顶数值并将复制值压入栈顶                               |
| 0x5A      | dup_x1          | 复制栈顶数值并将复制值压入栈顶两次                           |
| 0x5B      | dup_x2          | 复制栈顶数值并将复制值压入栈顶两次或三次                     |
| 0x5C      | dup2            | 复制栈顶一个(long 或 double 类型)或两个(其它类型)数值并将复制值压入栈顶 |
| 0x5D      | dup2_x1         | <待补充>                                                     |
| 0x5E      | dup2_x2         | <待补充>                                                     |
| 0x5F      | swap            | 将栈最的两个数值互换(数值不能是 long 或 double 类型)         |
| 0x60      | iadd            | 将栈顶的两个 int 型数值相加并将结果压入栈顶                  |
| 0x61      | ladd            | 将栈顶的两个 long 型数值相加并将结果压入栈顶                 |
| 0x62      | fadd            | 将栈顶的两个 float 型数值相加并将结果压入栈顶                |
| 0x63      | dadd            | 将栈顶的两个 double 型数值相加并将结果压入栈顶               |
| 0x64      | isub            | 将栈顶的两个 int 型数值相减并将结果压入栈顶                  |
| 0x65      | lsub            | 将栈顶的两个 long 型数值相减并将结果压入栈顶                 |
| 0x66      | fsub            | 将栈顶的两个 float 型数值相减并将结果压入栈顶                |
| 0x67      | dsub            | 将栈顶的两个 double 型数值相减并将结果压入栈顶               |
| 0x68      | imul            | 将栈顶的两个 int 型数值相乘并将结果压入栈顶                  |
| 0x69      | lmul            |                                                              |
| 0x6A      | fmul            |                                                              |
| 0x6B      | dmul            |                                                              |
| 0x6C      | idiv            | 将栈顶的两个 int 型数值相除并将结果压入栈顶                  |
| 0x6D      | ldiv            |                                                              |
| 0x6E      | fdiv            |                                                              |
| 0x6F      | ddiv            |                                                              |
| 0x70      | irem            | 将栈顶的两个 int 型数值作取模运算并将结果压入栈顶            |
| 0x71      | lrem            |                                                              |
| 0x72      | frem            |                                                              |
| 0x73      | drem            |                                                              |
| 0x74      | ineg            | 将栈顶 int 型数值取负并将结果压入栈顶                        |
| 0x75      | lneg            |                                                              |
| 0x76      | fneg            |                                                              |
| 0x77      | dneg            |                                                              |
| 0x78      | ishl            | 将 int 型数值左移位指定位数并将结果压入栈顶                  |
| 0x79      | lshl            | 将 long 型数值左移位指定位数并将结果压入栈顶                 |
| 0x7A      | ishr            | 将 int 型数值右(符号)移位指定位数并将结果压入栈顶            |
| 0x7B      | lshr            | 将 long 型数值右(符号)移位指定位数并将结果压入栈顶           |
| 0x7C      | iushr           | 将 int 型数值右(无符号)移位指定位数并将结果压入栈顶          |
| 0x7D      | lushr           | 将 long 型数值右(无符号)移位指定位数并将结果压入栈顶         |
| 0x7E      | iand            | 将栈顶的两个 int 型数值作<按位与>并将结果压入栈顶            |
| 0x7F      | land            | 将栈顶的两个 long 型数值作<按位与>并将结果压入栈顶           |
| 0x80      | ior             | 将栈顶的两个 int 型数值作<按位或>并将结果压入栈顶            |
| 0x81      | lor             | 将栈顶的两个 long 型数值作<按位或>并将结果压入栈顶           |
| 0x82      | ixor            | 将栈顶的两个 int 型数值作<按位异或>并将结果压入栈顶          |
| 0x83      | lxor            | 将栈顶的两个 long 型数值作<按位异或>并将结果压入栈顶         |
| 0x84      | iinc            | 将指定 int 型变量增加指定值, 可以有两个变量, 分别表示 index 和 const, index 指第 index 个 int 型本地变量, const增加的值 |
| 0x85      | i2l             | 将栈顶 int 型数值强制转换成 long 型数值并将结果压入栈顶      |
| 0x86      | i2f             | 将栈顶 int 型数值强制转换成 float 型数值并将结果压入栈顶     |
| 0x87      | i2d             | 将栈顶 int 型数值强制转换成 double 型数值并将结果压入栈顶    |
| 0x88      | l2i             | 将栈顶 long 型数值强制转换成 int 型数值并将结果压入栈顶      |
| 0x89      | l2f             | 将栈顶 long 型数值强制转换成 float 型数值并将结果压入栈顶    |
| 0x8A      | l2d             | 将栈顶 long 型数值强制转换成 double 型数值并将结果压入栈顶   |
| 0x8B      | f2i             | 将栈顶 float 型数值强制转换成 int 型数值并将结果压入栈顶     |
| 0x8C      | f2l             | 将栈顶 float 型数值强制转换成 long 型数值并将结果压入栈顶    |
| 0x8D      | f2d             | 将栈顶 float 型数值强制转换成 double 型数值并将结果压入栈顶  |
| 0x8E      | d2i             | 将栈顶 double 型数值强制转换成 int 型数值并将结果压入栈顶    |
| 0x8F      | d2l             | 将栈顶 double 型数值强制转换成 long 型数值并将结果压入栈顶   |
| 0×90      | d2f             | 将栈顶 double 型数值强制转换成 float 型数值并将结果压入栈顶  |
| 0×91      | i2b             | 将栈顶 int 型数值强制转换成 byte 型数值并将结果压入栈顶      |
| 0×92      | i2c             | 将栈顶 int 型数值强制转换成 char 型数值并将结果压入栈顶      |
| 0×93      | i2s             | 将栈顶 int 型数值强制转换成 short 型数值并将结果压入栈顶     |
| 0×94      | lcmp            | 比较栈顶的两个 long 型数值大小, 并将结果 (1, 0, -1) 压入栈顶 |
| 0×95      | fcmpl           | 比较栈顶的两个 float 型数值大小, 并将结果 (1, 0, -1) 压入栈顶, 当其中一个数值为 NaN 时, 将 -1 压入栈顶 |
| 0×96      | fcmpg           | 比较栈顶的两个 float 型数值大小, 并将结果 (1, 0, -1) 压入栈顶, 当其中一个数值为 NaN 时, 将 1 压入栈顶 |
| 0×97      | dcmpl           | 比较栈顶的两个 double 型数值大小, 并将结果 (1, 0, -1) 压入栈顶, 当其中一个数值为 NaN 时, 将 -1 压入栈顶 |
| 0×98      | dcmpg           | 比较栈顶的两个 double 型数值大小, 并将结果 (1, 0, -1) 压入栈顶, 当其中一个数值为 NaN 时, 将 1 压入栈顶 |
| 0x99      | ifeq            | 当栈顶 int 型数值等于 0 时跳转                               |
| 0x9A      | ifne            | 当栈顶 int 型数值不等于 0 时跳转                             |
| 0x9B      | iflt            | 当栈顶 int 型数值小于 0 时跳转                               |
| 0x9C      | ifge            | 当栈顶 int 型数值大于等于 0 时跳转                           |
| 0x9D      | ifgt            | 当栈顶 int 型数值大于 0 时跳转                               |
| 0x9E      | ifle            | 当栈顶 int 型数值小于等于 0 时跳转                           |
| 0x9F      | if_icmpeq       | 比较栈顶的两个 int 型数值大小, 当结果等于 0 时跳转           |
| 0xA0      | if_icmpne       | 比较栈顶的两个 int 型数值大小, 当结果不等于 0 时跳转         |
| 0xA1      | if_icmplt       | 比较栈顶的两个 int 型数值大小, 当结果小于 0 时跳转           |
| 0xA2      | if_icmpge       | 比较栈顶的两个 int 型数值大小, 当结果大于等于 0 时跳转       |
| 0xA3      | if_icmpgt       | 比较栈顶的两个 int 型数值大小, 当结果大于 0 时跳转           |
| 0xA4      | if_icmple       | 比较栈顶的两个 int 型数值大小, 当结果小于等于 0 时跳转       |
| 0xA5      | if_acmpeq       | 比较栈顶的两个引用型数值, 当结果相等时跳转                   |
| 0xA6      | if_acmpne       | 比较栈顶的两个引用型数值, 当结果不相等时跳转                 |
| 0xA7      | goto            | 无条件跳转                                                   |
| 0xA8      | jsr             | 跳转至指定 16 位 offset 位置, 并将 jsr 下一条指令地址压入栈顶 |
| 0xA9      | ret             | 返回至本地变量指定的 index 的指令位置 (一般与 jsr, jsr_w 联合使用) |
| 0xAA      | tableswitch     | 用于 switch 条件跳转, case 值连续 (可变长度指令)             |
| 0xAB      | lookupswitch    | 用于 switch 条件跳转, case 值不连续 (可变长度指令)           |
| 0xAC      | ireturn         | 从当前方法返回 int                                           |
| 0xAD      | lreturn         | 从当前方法返回 long                                          |
| 0xAE      | freturn         | 从当前方法返回 float                                         |
| 0xAF      | dreturn         | 从当前方法返回 double                                        |
| 0xB0      | areturn         | 从当前方法返回对象引用                                       |
| 0xB1      | return          | 从当前方法返回 void                                          |
| 0xB2      | getstatic       | 获取指定类的静态域, 并将其值压入栈顶                         |
| 0xB3      | putstatic       | 为指定的类的静态域赋值                                       |
| 0xB4      | getfield        | 获取指定类的实例域, 并将其值压入栈顶                         |
| 0xB5      | putfield        | 为指定的类的实例域赋值                                       |
| 0xB6      | invokevirtual   | 调用实例方法                                                 |
| 0xB7      | invokespecial   | 调用超类构造方法, 实例初始化方法, 私有方法                   |
| 0xB8      | invokestatic    | 调用静态方法                                                 |
| 0xB9      | invokeinterface | 调用接口方法                                                 |
| 0xBA      | -               | 因为历史原因, 该码点为未使用的保留码点                       |
| 0xBB      | new             | 创建一个对象, 并将其引用值压入栈顶                           |
| 0xBC      | newarray        | 创建一个指定原始类型 (如 int, float, char…) 的数组, 并将其引用值压入栈顶 |
| 0xBD      | anewarray       | 创建一个引用型 (如 类, 接口, 数组) 的数组, 并将其引用值压入栈顶 |
| 0xBE      | arraylength     | 获得数组的长度值并压入栈顶 (栈顶的数组引用 <arrayref> 出栈, 该数组的长度进栈. 如果 <arrayref> 的值为 null, 会抛出 NullPointerException) |
| 0xBF      | athrow          | 将栈顶的异常抛出                                             |
| 0xC0      | checkcast       | 检验类型转换, 检验未通过将抛出 ClassCastException            |
| 0xC1      | instanceof      | 检验对象是否是指定的类的实例, 如果是将 1 压入栈顶, 否则将 0 压入栈顶 |
| 0xC2      | monitorenter    | 获得对象的锁, 用于同步方法或同步块                           |
| 0xC3      | monitorexit     | 释放对象的锁, 用于同步方法或同步块                           |
| 0xC4      | wide            | 当本地变量的索引超过 255 时使用该指令扩展索引宽度            |
| 0xC5      | multianewarray  | 创建指定类型和维度的多维数组(执行该指令时, 栈中必须包含各维度的长度值), 并将其引用值压入栈顶 |
| 0xC6      | ifnull          | 为 null 时跳转                                               |
| 0xC7      | ifnonnull       | 不为 null 时跳转                                             |
| 0xC8      | goto_w          | 无条件跳转 (宽索引)                                          |
| 0xC9      | jsr_w           | 跳转至指定 32 位 offset 位置, 并将 jsr_w 下一条指令地址压入栈顶 |
| 0xCA      | breakpoint      | reserved for breakpoints in Java debuggers; should not appear in any class file |
| 0xCB~0xFD |                 | these values are currently unassigned for opcodes and are reserved for future use |
| 0xFE      | impdep1         | reserved for implementation-dependent operations within debuggers; should not appear in any class file |
| 0xFF      | impdep2         | reserved for implementation-dependent operations within debuggers; should not appear in any class file |



参考链接：

1. [The Java Virtual Machine Instruction Set](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-6.html)
2. https://blog.csdn.net/zhangpan19910604/article/details/52254053