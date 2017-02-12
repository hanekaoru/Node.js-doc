Node.js 文档学习笔记

基于 @6.9.5

----

## Buffer.from(), Buffer.alloc(), Buffer.allocUnsafe()

为了使 ```Buffer``` 实例的创建更可靠、更不容易出错，各种 ```new Buffer()``` 构造函数已被**废弃**，并由```Buffer.from()```、```Buffer.alloc()```、和 ```Buffer.allocUnsafe()``` 方法替代

* ```Buffer.from(array)``` 返回一个新建的包含所提供的字节数组的副本的 ```Buffer```

* ```Buffer.from(arrayBuffer[, byteOffset [, length]])``` 返回一个新建的与给定的 ```ArrayBuffer``` 共享同一内存的 ```Buffer```

* ```Buffer.from(buffer)``` 返回一个新建的包含所提供的 ```Buffer``` 的内容的副本的 ```Buffer```

* ```Buffer.from(string[, encoding])``` 返回一个新建的包含所提供的字符串的副本的 ```Buffer```

* ```Buffer.alloc(size[, fill[, encoding]])``` 返回一个指定大小的被填满的 ```Buffer``` 实例。 这个方法会明显地比 ```Buffer.allocUnsafe(size)``` 慢，但可确保新创建的 ```Buffer``` 实例绝不会包含旧的和潜在的敏感数据

```Buffer.allocUnsafe(size)``` 与 Buffer.allocUnsafeSlow(size) 返回一个新建的指定 ```size``` 的 ```Buffer```，但它的内容必须被初始化，可以使用 ```buf.fill(0)``` 或完全写满


## Buffer 与字符编码

```Buffer``` 实例一般用于表示编码字符的序列，比如 ```UTF-8``` 、 ```UCS2``` 、 ```Base64``` 、或十六进制编码的数据。 通过使用显式的字符编码，就可以在 ```Buffer``` 实例与普通的 ```JavaScript``` 字符串之间进行相互转换

```js
const buf = Buffer.from('hello world', 'ascii');

// 输出 68656c6c6f20776f726c64
console.log(buf.toString('hex'));

// 输出 aGVsbG8gd29ybGQ=
console.log(buf.toString('base64'));
```


## Buffer 与 ES6 迭代器

Buffer 实例可以使用 ES6 的 for..of 语法进行遍历

```js
const buf = Buffer.from([1, 2, 3]);

for (var b of buf) {
    console.log(b);
}
// 输出:
//   1
//   2
//   3
```

## Buffer 类

~~```new Buffer(array)```~~ 使用 ```Buffer.from(array)``` 代替

~~```new Buffer(buffer)```~~ 使用 ```Buffer.from(buffer)``` 代替

~~```new Buffer(arrayBuffer[, byteOffset [, length]])```~~ 使用 ```Buffer.from(arrayBuffer[, byteOffset [, length]]```) 代替

~~```new Buffer(size)```~~ 使用 ```Buffer.alloc()``` 代替（或 ```Buffer.allocUnsafe()```）

~~```new Buffer(string[, encoding])```~~ 使用 ```Buffer.from(string[, encoding])``` 代替

## 类方法：Buffer.alloc(size[, fill[, encoding]])

* ```size``` 新建的 ```Buffer``` 期望的长度

* ```fill``` 用来预填充新建的 ```Buffer``` 的值。 默认: 0

* ```encoding``` 如果 ```fill``` 是字符串，则该值是它的字符编码。 默认: 'utf8'

分配一个大小为 ```size``` 字节的新建的 ```Buffer``` 。 如果 ```fill``` 为 ```undefined``` ，则该 ```Buffer``` 会用 ```0``` 填充

```js
const buf = Buffer.alloc(5);

console.log(buf);
// 输出: <Buffer 00 00 00 00 00>
```

```size``` 必须小于或等于 ```buffer.kMaxLength```（分配给单个 ```Buffer``` 实例的最大内存） 的值，否则会抛出错误。 如果 ```size``` 小于或等于 ```0```，则创建一个长度为 ```0``` 的 ```Buffer```

```js
const buf = Buffer.alloc(5, 'a');

// 输出: <Buffer 61 61 61 61 61>
console.log(buf);
```

如果同时指定了 ```fill``` 和 ```encoding``` ，则会调用 ```buf.fill(fill, encoding)``` 初始化分配的 ```Buffer``` 

```js
const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');

// 输出: <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
console.log(buf);
```

调用 ```Buffer.alloc()``` 会明显地比另一个方法 ```Buffer.allocUnsafe()``` 慢，但是能确保新建的 ```Buffer``` 实例的内容不会包含敏感数据。

需要注意的是：如果 ```size``` 不是一个数值，则抛出错误


