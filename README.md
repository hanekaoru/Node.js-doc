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


