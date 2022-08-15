## Node.js

## 一. 简介

​		Node.js 是一个开源与跨平台的 JavaScript 运行时环境。 

​		Node.js 在浏览器外运行 V8 JavaScript 引擎（Chrome 的内核）



## 二、命令行运行

```js
node app.js
```



## 三、模块

Node.js提供了一个简单的模块系统让Node.js的文件可以相互调用。模块加载采用的是同步加载的commonjs规范

### **commonjs:**

- 每个文件都是封闭的一个模块，模块里定义的变量、函数、类都是私有的
- module代表当前模块，module是封闭的，但它的exports属性向外提供调用接口
- require加载模块，读取并执行一个js文件，然后返回该模块的exports对象
- commonjs是同步加载的，因此模块加载的顺序严格按照代码书写的顺序执行
- 模块可以多次加载，但在第一次加载之后模块会被编译执行，放入缓存，后续的require直接从缓存里取值，模块代码不再编译执行

### require内部处理流程

- 检查Module._cache是否缓存了指定模块
- 如果缓存没有的话，就创建一个新的module实例将它保存到缓存
- module.load()加载指定模块
- 在解析的过程中如果发生异常，就从缓存中删除该模块
- 返回该模块的module.exports

### 模块化导出方式

- global.address = 'beijing';//导出全局变量，只要导入相应js文件即可调用
- module.exports = "str";//可以
- module.exports.msg = 'str'//可以
- exports.msg = 'str'//可以
- exports = 'str'//不行



## 四、HTTP

Node 内置模块HTTP可以用来创建来创建 HTTP 服务器

要使用 HTTP 服务器和客户端，则必须 `require('http')`。

```js
//搭建一个服务器
const http = require('http')

const port = 3000

const server = http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain; charset=utf8')
  res.end('hi~~Tina')
})

server.listen(port, () => {
  console.log(`服务器运行了`)
})
```

### 1.http.server

通过`http.createServer`或者`new http.Server()`可以创建一个http.server实例

- **close** 停止服务
- **listen **启动服务器监听

### 2.request响应体

- **write**  

    向客户端返回数据，该方法可调用多次，返回的数据会被拼接到一起

- **end** 

    结束请求，同时`response.end`方法也可以用来向前端返回数据

- **setHeader**

     设置响应头，如果该头信息已存在，则覆盖

- **statusCode**

    设置响应状态

- **statusMessage**

    设置响应状态信息



### 2.请求方式

- **request**

- **get**

    两种请求都接受以下参数：

    - `url` 请求地址
    - `options`请求配置
    - `callback` 请求成功回调

其中`options`请求配置包括：

  **host**:         表示请求网站的域名或IP地址（请求的地址）。 默认为'localhost'。

  **hostname**:    服务器名称，主机名是首选的值。

  **port**:         请求网站的端口，默认为 80。

  **method**:      HTTP请求方法，默认是 ‘GET'。(http.get时值只能为"GET")

  **path**:         请求的相对于根的路径，默认是'/'。QueryString应该包含在其中。例如：/index.html?page=12

  **headers**:     请求头对象



## 五、fs

在 NodeJS 中，所有与文件操作都是通过 fs 核心模块来实现的，包括文件目录的创建、删除、查询以及文件的读取和写入，在 fs 模块中，所有的方法都分为同步和异步两种实现，具有 sync 后缀的方法为同步方法，不具有 sync 后缀的方法为异步方法

### 1.文件读取

```js
const fs = require("fs");

// 同步读取 readFileSync
let buf = fs.readFileSync("demo.txt");
let data = fs.readFileSync("demo.txt", "utf8");

console.log(buf); // <Buffer 48 65 6c 6c 6f>
console.log(data); // Hello

// 异步读取 readFile
fs.readFile("demo.txt", "utf8", (err, data) => {
    console.log(err); // null
    console.log(data); // Hello
});
```

### 2.文件写入

```js
const fs = require("fs");

// 同步写入 writeFileSync
fs.writeFileSync("demo.txt", "Hello world");
let data = fs.readFileSync("demo.txt", "utf8");

console.log(data); // Hello world

// 异步写入 writeFile
fs.writeFile("demo.txt", "Hello world", err => {
    if (!err) {
        fs.readFile("demo.txt", "utf8", (err, data) => {
            console.log(data); // Hello world
        });
    }
});

```

### 3.追加写入

```js
const fs = require("fs");

// 同步追加 appendFileSync
fs.appendFileSync("demo.txt", " world");
let data = fs.readFileSync("demo.txt", "utf8");

console.log(data); // Hello world

// 异步追加 appendFile
fs.appendFile("3.txt", " world", err => {
    if (!err) {
        fs.readFile("demo.txt", "utf8", (err, data) => {
            console.log(data); // Hello world
        });
    }
});
```

### 4.文件拷贝写入

```js
const fs = require("fs");

// 同步拷贝 copyFileSync
fs.copyFileSync("demo.txt", "newdemo.txt");
let data = fs.readFileSync("newdemo.txt", "utf8");

console.log(data); // Hello world

// 异步拷贝 copyFile
fs.copyFile("demo.txt", "newdemo.txt", () => {
    fs.readFile("newdemo.txt", "utf8", (err, data) => {
        console.log(data); // Hello world
    });
});

```

### 5.创建文件

```js
const fs = require("fs");

//同步创建文件目录
//在已有的a目录下创建b目录
fs.mkdirSync("a/b");
//在刚才创建的b目录下创建c.text
fs.mkdirSync("a/b/c.text");

// 异步创建文件目录
fs.mkdir("a/b/c", err => {
    if (!err) console.log("创建成功");
});
```



### 6.读取文件目录

```js
const fs = require("fs");

// 同步读取目录
let data = fs.readdirSync("a/b");
console.log(data); // [ 'c', 'index.js' ]

// 异步读取目录
fs.readdir("a/b", (err, data) => {
    if (!err){
        console.log(data);
    } 
});
```



### 7.删除文件目录

删除的目录需要是空文件夹

```js
const fs = require("fs");
// 同步删除目录
fs.rmdirSync("a/b");

// 同步删除文件
fs.rmdir("a/b", err => {
    if (!err) console.log("删除成功");
});
```



### 8.删除文件操作

```js
const fs = require("fs");

// 同步删除文件
fs.unlinkSync("a/index.js");

// 异步删除文件
fs.unlink("a/index.js", err => {
    if (!err) console.log("删除成功");
});
```

### fs常用方法：

- `fs.access()`: 检查文件是否存在，以及 Node.js 是否有权限访问。
- `fs.appendFile()`: 追加数据到文件。如果文件不存在，则创建文件。
- `fs.chmod()`: 更改文件（通过传入的文件名指定）的权限。相关方法：`fs.lchmod()`、`fs.fchmod()`。
- `fs.chown()`: 更改文件（通过传入的文件名指定）的所有者和群组。相关方法：`fs.fchown()`、`fs.lchown()`。
- `fs.close()`: 关闭文件描述符。
- `fs.copyFile()`: 拷贝文件。
- `fs.createReadStream()`: 创建可读的文件流。
- `fs.createWriteStream()`: 创建可写的文件流。
- `fs.link()`: 新建指向文件的硬链接。
- `fs.mkdir()`: 新建文件夹。
- `fs.mkdtemp()`: 创建临时目录。
- `fs.open()`: 设置文件模式。
- `fs.readdir()`: 读取目录的内容。
- `fs.readFile()`: 读取文件的内容。相关方法：`fs.read()`。
- `fs.readlink()`: 读取符号链接的值。
- `fs.realpath()`: 将相对的文件路径指针（`.`、`..`）解析为完整的路径。
- `fs.rename()`: 重命名文件或文件夹。
- `fs.rmdir()`: 删除文件夹。
- `fs.stat()`: 返回文件（通过传入的文件名指定）的状态。相关方法：`fs.fstat()`、`fs.lstat()`。
- `fs.symlink()`: 新建文件的符号链接。
- `fs.truncate()`: 将传递的文件名标识的文件截断为指定的长度。相关方法：`fs.ftruncate()`。
- `fs.unlink()`: 删除文件或符号链接。
- `fs.unwatchFile()`: 停止监视文件上的更改。
- `fs.utimes()`: 更改文件（通过传入的文件名指定）的时间戳。相关方法：`fs.futimes()`。
- `fs.watchFile()`: 开始监视文件上的更改。相关方法：`fs.watch()`。
- `fs.writeFile()`: 将数据写入文件。相关方法：`fs.write()`。



## 六、path

`path` 模块提供了许多函数来访问文件系统并与文件系统进行交互

### 1.path.basename()

返回路径的最后一部分。 第二个参数可以过滤掉文件的扩展名：

```js
require('path').basename('/test/something') //something
require('path').basename('/test/something.txt') //something.txt
require('path').basename('/test/something.txt', '.txt') //something
```

### 2.path.dirname()

返回路径的目录部分：

```javascript
require('path').dirname('/test/something') // /test
require('path').dirname('/test/something/file.txt') // /test/something
```

### 3.path.extname()

返回路径的扩展名部分。

```javascript
require('path').extname('/test/something') // ''
require('path').extname('/test/something/file.txt') // '.txt'
```

### 4.path.isAbsolute()

如果是绝对路径，则返回 true。

```javascript
require('path').isAbsolute('/test/something') // true
require('path').isAbsolute('./test/something') // false
```

### 5.path.join()

连接路径的两个或多个部分：

```javascript
const name = 'joe'
require('path').join('/', 'users', name, 'notes.txt') //'/users/joe/notes.txt'
```

### 6.path.normalize()

当包含类似 `.`、`..` 或双斜杠等相对的说明符时，则尝试计算实际的路径：

```javascript
require('path').normalize('/users/joe/..//test.txt') //'/users/test.txt'
```

### 7.path.parse()

解析对象的路径为组成其的片段：

- `root`: 根路径。
- `dir`: 从根路径开始的文件夹路径。
- `base`: 文件名 + 扩展名
- `name`: 文件名
- `ext`: 文件扩展名

```javascript
require('path').parse('/users/test.txt')

/**
{
  root: '/',
  dir: '/users',
  base: 'test.txt',
  ext: '.txt',
  name: 'test'
}
*/

```

### 8.path.relative()

接受 2 个路径作为参数。 基于当前工作目录，返回从第一个路径到第二个路径的相对路径。

例如：

```javascript
require('path').relative('/Users/joe', '/Users/joe/test.txt') //'test.txt'
require('path').relative('/Users/joe', '/Users/joe/something/test.txt') //'something/test.txt'
```

### 9.path.resolve()

可以使用 `path.resolve()` 获得相对路径的绝对路径计算：

```javascript
path.resolve('joe.txt') //'/Users/joe/joe.txt' 如果从主文件夹运行
```

通过指定第二个参数，`resolve` 会使用第一个参数作为第二个参数的基准：

```javascript
path.resolve('tmp', 'joe.txt') //'/Users/joe/tmp/joe.txt' 如果从主文件夹运行
```

如果第一个参数以斜杠开头，则表示它是绝对路径：

```javascript
path.resolve('/etc', 'joe.txt') //'/etc/joe.txt'
```



## 七、事件模块

事件模块=events模块只提供了一个对象events.EventEmitter，EventEmitter 的核心是事件发射与事件监听器。

Node.js中大部分的模块，都继承自Event模块。

EventEmitter 支持若干个事件监听器。当事件发射时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。

### 1.addListener()  / on()

为事件注册一个监听

- 参数1：事件名

- 参数2：回调函数

```js
const EventEmitter = require('events')
const door = new EventEmitter()

//注册一个叫connection的事件，事件执行函数是后面的后调函数
door.on('connection', (stream) => {
  console.log('someone connected!');
});
door.emit("connection")
```



### 2.emit()

触发事件。 按照事件被注册的顺序同步地调用每个事件监听器。

- 参数1：事件名
- 参数2：事件回调函数需要的参数
- 。。。

```js
door.emit("connection")
```



### 3.once()

添加当事件在注册之后首次被触发时调用的回调函数。 该回调只会被调用一次，不会再被调用,调用完成之后就被移除了。

- 参数1：事件名

- 参数2：回调函数

```javascript

door.once('my-event', () => {
  //只调用一次回调函数。
    console.log('my-event被调用')
})

door.emit("my-event")
door.emit("my-event")
```



### 4.eventNames()

返回包含当前`EventEmitter` 对象上注册的事件的数组：

```javascript
door.eventNames()
```



### 5.removeListener()

移除特定的监听器。

- 参数1：要移除的事件名
- 参数2：时间名对应的事件回调（需要是注册时的那个事件）

```javascript
const doSomething = () => {}
door.on('open', doSomething)
door.removeListener('open', doSomething)
```



### 6.removeAllListeners()

移除 `EventEmitter` 对象的所有监听特定事件的监听器

- 参数1：事件名

- 如果不传时间名，则移除所有事件

    ```js
    door.removeAllListeners('open')
    ```



### 7.listeners

获取作为参数传入的事件监听器的数组：

- 参数1：需要获取的事件名

```javascript
door.listeners('open')
```



### 8.setMaxListeners()

置可以添加到 `EventEmitter` 对象的监听器的最大数量（默认为 10，但可以增加或减少）

```js 
door.setMaxListeners(10)
```



## 八、buffer(缓冲区)

Buffer 是内存区域。它表示在 V8 JavaScript 引擎外部分配的固定大小的内存块（无法调整大小）。

可以将 buffer 视为整数数组，每个整数代表一个数据字节。

Buffer 被引入用以帮助开发者处理二进制数据，在此生态系统中传统上只处理字符串而不是二进制数据。

Buffer 与流紧密相连。 当流处理器接收数据的速度快于其消化的速度时，则会将数据放入 buffer 中。

一个简单的场景是：当观看 YouTube 视频时，红线超过了观看点：即下载数据的速度比查看数据的速度快，且浏览器会对数据进行缓冲。

### 1.创建buffer

- **Buffer.from()** 返回新的 `Buffer`

    ```js
    const Buffer = require('buffer').Buffer;
    
    // 创建包含字符串 'buffer' 的 UTF-8 字节的新缓冲区。
    const buf = Buffer.from([ 0x68, 0x65, 0x6c, 0x6c, 0x6f]);
    
    var str = buf.toString("utf-8");  // hello
    
    
    const buf = Buffer.from('Hey!') //<Buffer 48 65 79 21>
    ```

    

- **Buffer.alloc** 返回指定大小的新初始化 `Buffer`。 此方法比 **Buffer.allocUnsafe(size)** 慢，但保证新创建的 `Buffer` 实例永远不会包含可能敏感的旧数据。 如果 `size` 不是数值，则会抛出 `TypeError`。

    ```js
    const buf = Buffer.alloc(5);
    
    console.log(buf);
    // 打印: <Buffer 00 00 00 00 00>
    ```

    

- **Buffer.allocUnsafe(size)** 和 **Buffer.allocUnsafeSlow(size)** 分别返回指定 `size` 的新的未初始化的 `Buffer`。 

    ```js
    const buf = Buffer.allocUnsafe(10);
    
    console.log(buf);
    //  <Buffer a0 8b 28 3f 01 00 00 00 50 32>
    
    
    ```

    

### 2.使用 buffer

Buffer（字节数组）可以像数组一样被访问：

```javascript
const buf = Buffer.from('Hey!')
console.log(buf[0]) //72
console.log(buf[1]) //101
console.log(buf[2]) //121
```

这些数字是 Unicode 码，用于标识 buffer 位置中的字符（H => 72、e => 101、y => 121）。

可以使用 `toString()` 方法打印 buffer 的全部内容：

```javascript
console.log(buf.toString())
```



### 3.获取 buffer 的长度

```javascript
const buf = Buffer.from('Hey!')
console.log(buf.length)
```

迭代 buffer 的内容

```javascript
const buf = Buffer.from('Hey!')
for (const item of buf) {
  console.log(item) //72 101 121 33
}
```



### 4.更改 buffer 的内容

可以使用 `write()` 方法将整个数据字符串写入 buffer：

```javascript
const buf = Buffer.alloc(4)
buf.write('Hey!')
```

就像可以使用数组语法访问 buffer 一样，你也可以使用相同的方式设置 buffer 的内容：

```javascript
const buf = Buffer.from('Hey!')
buf[1] = 111 //o
console.log(buf.toString()) //Hoy!
```



### 5.复制 buffer

使用 `copy()` 方法可以复制 buffer：

```javascript
const buf = Buffer.from('Hey!')
let bufcopy = Buffer.alloc(4) //分配 4 个字节。
buf.copy(bufcopy)
```

默认情况下，会复制整个 buffer。 另外的 3 个参数可以定义开始位置、结束位置、以及新的 buffer 长度：

```javascript
const buf = Buffer.from('Hey!')
let bufcopy = Buffer.alloc(2) //分配 2 个字节。
buf.copy(bufcopy, 0, 0, 2)
bufcopy.toString() //'He'
```



### 6.切片 buffer

如果要创建 buffer 的局部视图，则可以创建切片。 切片不是副本：原始 buffer 仍然是真正的来源。 如果那改变了，则切片也会改变。

使用 `slice()` 方法创建它。 第一个参数是起始位置，可以指定第二个参数作为结束位置：

```javascript
const buf = Buffer.from('Hey!')
buf.slice(0).toString() //Hey!
const slice = buf.slice(0, 2)
console.log(slice.toString()) //He
buf[1] = 111 //o
console.log(slice.toString()) //Ho
```



## 九、流

流是数据的集合 - 就像数组或者字符串。他们之间的区别是流可能不是一次性获取到的，它们不需要匹配内存。这让流在处理大容量数据，或者从一个额外的源每次获取一块数据的时候变得非常强大。

流基本上提供了两个主要优点：

- **内存效率**: 无需加载大量的数据到内存中即可进行处理。
- **时间效率**: 当获得数据之后即可立即开始处理数据，这样所需的时间更少，而不必等到整个数据有效负载可用才开始。

几乎所有的 Node.js 应用程序，无论多么简单，都以某种方式使用流

Node.js，Stream 有四种流类型：

- **Readable** - 可读操作。
- **Writable** - 可写操作。
- **Duplex** - 可读可写操作.
- **Transform** - 操作被写入数据，然后读出结果。

所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：

- **data** - 当有数据可读时触发。

- **end** - 没有更多的数据可读时触发。

- **error** - 在接收和写入过程中发生错误时触发。

- **finish** - 所有数据已被写入到底层系统时触发。

    

### 1.读取流

 读取流包括：

- [客户端上的 HTTP 响应](http://nodejs.cn/api/http.html#http_class_http_incomingmessage)
- [服务器上的 HTTP 请求](http://nodejs.cn/api/http.html#http_class_http_incomingmessage)
- [文件系统读取流](http://nodejs.cn/api/fs.html#fs_class_fs_readstream)
- [压缩流](http://nodejs.cn/api/zlib.html)
- [加密流](http://nodejs.cn/api/crypto.html)
- 。。。。

创建流对象：

```js
const Stream= require('stream');
//创建读取流对象
const readableStream = new Stream.Readable();

//实现_read方法  所有 Readable 流实现都必须提供 readable._read() 方法的实现
readableStream._read = ()=>{}

//推入读取队列的数据块
readableStream.push('tina')
readableStream.push('你好')

readableStream.on('data', function(data) {
    console.log("写入完成。",data.toString());
});
```



```javascript
var fs = require("fs");
var data = '';
// 创建可读流
var readStream = fs.createReadStream('demo.txt');

// 设置编码为 utf8。
readStream.setEncoding('UTF8');

readStream.on("open",(data)=>{
    console.log('打开了',data)
})
readStream.on("data",(data)=>{
    console.log("数据来了！");
    console.log("已经读取的字节数",readStream.bytesRead);
})
readStream.on("close",(data)=>{
    console.log('close')
})

console.log("程序执行完毕");
```



### 2.写入流

写入流的包括：

- [客户端上的 HTTP 请求](http://nodejs.cn/api/http.html#http_class_http_clientrequest)
- [服务器上的 HTTP 响应](http://nodejs.cn/api/http.html#http_class_http_serverresponse)
- [文件系统写入流](http://nodejs.cn/api/fs.html#fs_class_fs_writestream)
- [压缩流](http://nodejs.cn/api/zlib.html)
- [加密流](http://nodejs.cn/api/crypto.html)
- 。。。

创建写入流：

若要创建可写流，需要继承基本的 `Writable` 对象，并实现其 `_write()` 方法。

```javascript
const Stream = require('stream')
//创建写入流对象
const writableStream = new Stream.Writable()

writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())
  next()
}
writableStream.write('hello', () => {
   console.log('写入了hello');
})
```

```js
var fs = require("fs");
var data = '我是要写入的内容';

// 创建一个可以写入的流，写入到文件 demo2.txt 中
var writerStream = fs.createWriteStream('demo2.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> finish、error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```



### 3.可读写流

```js
const Stream = require('stream')
//创建可读流
const readableStream = new Stream.Readable({
  read() {}
})
//创建写入流
const writableStream = new Stream.Writable()
//实现_write方法
writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())
  next()
}
//讲可读流里的数据流入可写流
readableStream.pipe(writableStream)
//可读流push数据
readableStream.push('hi!')
readableStream.push('ho!')
//可读流内可读的数据输出
readableStream.on('readable', () => {
    console.log(readableStream.read().toString())
  })
//可写流写入数据
writableStream.write('hey!\n')
```



## Node特点

### 1.异步非阻塞I/O

在Node中，绝大多数的操作都以异步的方式进行调用，从文件读取到网络请求，均是如此，异步I/O意味着每个调用之间无须等待之前的I/O调用结束。

### 2.事件回调

在Node中回调也是无处不在的，事件的处理基本都是依赖回调来实现的，在JavaScript中，可以将函数作为对象传递给方法作为实参进行调用。

### 3.单线程

单线程的运行会导致无法利用多核CPU，一旦程序发生错误就会引起整个程序退出，并且大量的计算会占用CPU从而阻塞后续的程序运行

Node实质上是给javaScript提供了一个处web以外的运行环境，所以我们其实也是在Node内编写javaScript代码。

Node内的I/O事件是异步的，整个事件驱动过程中不会阻塞新的任务发起，理论上NodeJS能支持比Java、PHP程序更高的并发量.虽然维护事件队列也需要成本，再由于NodeJS是单线程，事件队列越长，得到响应的时间就越长。



## Node使用场景

### 1.I/O密集

​     Node异步I/O的特点可以轻松面对I/O密集型的业务场景，处理效率将比同步I/O高，虽然同步I/O可以采用多线程或者多进程的方式进行，但是相比Node自带异步I/O的特性来说，将增加对内存和CPU的开销。但是由于Node是单线程，所以如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起。**解决方案：分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起；**

### 2.高并发

Node可以处理数万条连接，本身没有太多的逻辑，只需要请求API，组织数据进行返回即可。它本质上只是从某个数据库中查找一些值并将它们组成一个响应。由于响应是少量文本，入站请求也是少量的文本，因此流量不高，可以轻松应对。

