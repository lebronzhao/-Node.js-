# -Node.js-



# 简述io

Node.js 是以 IO 密集型业务著称. 那么问题来了, 你真的了解什么叫 IO, 什么又叫 IO 密集型业务吗?

## Buffer

Buffer 是 Node.js 中用于处理二进制数据的类, 其中与 IO 相关的操作 (网络/文件等) 均基于 Buffer. Buffer 类的实例非常类似整数数组, **\*但其大小是固定不变的***, 并且其内存在 V8 堆栈外分配原始内存空间. Buffer 类的实例创建之后, 其所占用的内存大小就不能再进行调整.

在 Node.js v6.x 之后 `new Buffer()` 接口开始被废弃, 理由是参数类型不同会返回不同类型的 Buffer 对象, 所以当开发者没有正确校验参数或没有正确初始化 Buffer 对象的内容时, 以及不了解的情况下初始化 就会在不经意间向代码中引入安全性和可靠性问题.

| 接口                   | 用途                   |
| -------------------- | -------------------- |
| Buffer.from()        | 根据已有数据生成一个 Buffer 对象 |
| Buffer.alloc()       | 创建一个初始化后的 Buffer 对象  |
| Buffer.allocUnsafe() | 创建一个未初始化的 Buffer 对象  |

### TypedArray

Node.js 的 Buffer 在 ES6 增加了 TypedArray 类型之后, 修改了原来的 Buffer 的实现, 选择基于 TypedArray 中 Uint8Array 来实现, 从而提升了一波性能.

使用上, 你需要了解如下情况:

```
const arr = new Uint16Array(2);
arr[0] = 5000;
arr[1] = 4000;

const buf1 = Buffer.from(arr); // 拷贝了该 buffer
const buf2 = Buffer.from(arr.buffer); // 与该数组共享了内存

console.log(buf1);
// 输出: <Buffer 88 a0>, 拷贝的 buffer 只有两个元素
console.log(buf2);
// 输出: <Buffer 88 13 a0 0f>

arr[1] = 6000;
console.log(buf1);
// 输出: <Buffer 88 a0>
console.log(buf2);
// 输出: <Buffer 88 13 70 17>
```





在 ECMAScript 2015 (ES6) 引入 TypedArray 之前，JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 Node.js API 的一部分，使其可以在 TCP 流和文件系统操作等场景中处理二进制数据流。

现在 TypedArray已经被添加进 ES6 中，Buffer 类以一种更优与更适合 Node.js 用例的方式实现了 uint8arrayAPI。

Buffer类的实例类似于整数数组，除了其是大小固定的、且在 V8 堆外分配物理内存。Buffer 的大小在其创建时就已确定，且不能调整大小。

Buffer类在 Node.js 中是一个全局变量，因此无需 `require('buffer').Buffer`。



Buffer 可以处理二进制数据流,是计算机运算更加方便,解放内存,所以我感觉是必须要掌握的技术.

Buffer与字符串类似，除了可以用.length属性得到字节长度外，还可以用[index]方式读取指定位置的字节:buffer[0] ; // 0x68;

Buffer与字符串能够互相转化，例如可以使用指定编码将二进制数据转化为字符串：

var str = buffer.toString("utf-8");  // hello

将字符串转换为指定编码下的二进制数据：

var buffer= new Buffer("hello", "utf-8") ; // <Buffer 68 65 6c 6c 6f>



#### Buffer与字符串有一个重要区别。字符串是只读的，并且对字符串的任何修改得到的都是一个新字符串，原字符串保持不变。

Javascript对字符串处理十分友好，无论是宽字节还是单字节字符串，都被认为是一个字符串。Node中需要处理网络协议、操作数据库、处理图片、文件上传等，还需要处理大量二进制数据，自带的字符串远不能满足这些要求，因此Buffer应运而生。

Buffer是一个典型的Javascript和C++结合的模块，性能相关部分用C++实现，非性能相关部分用javascript实现。

Node在进程启动时Buffer就已经加装进入内存，并将其放入全局对象，因此无需require.



无论就业还是什么 都要一直学习node.js 