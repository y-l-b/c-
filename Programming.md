<!--
 * @Date         : 2021-07-23 12:02:46
 * @Author       : MemoryShadow
 * @LastEditors  : MemoryShadow
 * @LastEditTime : 2021-07-23 12:23:57
 * @Description  : 程序设计的样例
-->

# 对于游戏:贪吃蛇

在设计中,我们遵循自顶向下的设计模式，我们将所有软件都分为两大板块进行细分，他们便是输入与输出。

* [输入](#input "点击前往")
  * [CheckKey](#CheckKey "点击前往")
* [输出](#out "点击前往")

## input

设计理念来自于[CLI_Key_Response_Game.h](https://github.com/MemoryShadow/CLI_GUI_Rendering/blob/master/CLI_Key_Response_Game.h "点击前往")

输入方面，我们一般都是获取用户的输入，因为是命令行程序，我们只需要考虑键盘的输入信号

在获取输入时，为了不堵塞渲染进程的运作，我们使用非堵塞式的方式进行处理，同时，为了增加程序的可读性，我们预计此函数具有监听的功能

为了实现同时监听多个键的功能，我们对能监听的键创建按键列表

```c
// 按键列表
typedef enum
{
    NO_FunctionKeys = 0, // (0000 0000)
    Up = 1,              // (0000 0001)
    Down = 2,            // (0000 0010)
    Left = 4,            // (0000 0100)
    Right = 8,           // (0000 1000)
    Delete = 16,         // (0001 0000)
    PageUp = 32,         // (0010 0000)
    PageDown = 64,       // (0100 0000)
    ESC = 128            // (1000 0000)
} FunctionKeys;

// 监听指定的按键，并把按钮转换为信号反馈回来，正在监听的按键被称为功能键(Function Key)
// 此函数直接从键盘拉取指定的值
FunctionKeys CheckKey(FunctionKeys Key);
```

使用这种方式后，我们便可以使用如下语法来同时监听多个键

```c
switch(functionKeys = CheckKey(Up | Down | Left | Right)){
    case Up:
        ...
    break;
    ...
    default:
        ...
    break;
}
```

### CheckKey

此函数的功能是监听指定的按键，并把按钮转换为信号反馈回来，正在监听的按键被称为功能键(Function Key) **此函数直接从键盘拉取指定的值**

`FunctionKeys CheckKey(FunctionKeys Key);`

要达成此效果，我们需要使用非堵塞式的函数`kbhit()`来实现键盘的检测,并配合`getch()`来完成数据的获取,在获取后进行检测

## out

输出方面，我们一般都是输出到屏幕(标准输出设备)，因为是命令行程序，我们只需要考虑在命令行中的显示
