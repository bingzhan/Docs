# 大前端
记录前端的点滴

#### js 的数据类型
Undefined, Null, Boolean, String, Number, Object, Symbol

#### js 严格模式 ("use strict")
* 目的：
    * 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
    * 消除代码运行的一些不安全之处，保证代码运行的安全；
    * 提高编译器效率，增加运行速度；
    * 为未来新版本的Javascript做好铺垫。
* 行为改变：
    * 全局变量显式声明
    * 禁止使用with语句
    * 创设eval作用域
    * 禁止this关键字指向全局对象
    * 禁止在函数内部遍历调用栈
    * 禁止删除变量
    * 对一个对象的只读属性进行赋值会**显示报错**
    * 对象不能有重名的属性
    * 函数不能有重名的参数
    * 禁止八进制表示法
    * 不允许对arguments赋值
    * arguments不再追踪参数的变化
    * 禁止使用arguments.callee
    * 函数必须声明在顶层
    * 保留字(`implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`)

#### 事件处理
```JavaScript
function addHandler(target, type, handler){
    if (target.addEventListener){
        target.addEventListener(type, handler, false);
    } else if(target.attachEvent){
        target.attachEvent('on'+type, handler);
    } else {
        target['on'+type] = handler;
    }
}
function removeHandler(target, type, handler){
    if(target.removeEventListener){
        target.removeEventListener(type, handler, false);
    } else if(target.detachEvent){
        target.detachEvent('on'+type, handler);
    } else {
        target['on'+type] = null;
    }
}
function preventDefault(e){
    if (window.event){
        window.event.returnValue = false;
    } else {
        e.preventDefault();
    }
}
function stopPropagation(e){
    if (window.event){
        window.event.cancelBubble = true;
    } else {
        e.stopPropagation();
    }
}
```

#### 高效使用内存
* 避免使用全局作用域
* 合理的使用闭包

#### HTTP协议
http请求由三部分组成，分别是：请求行、消息报头、请求正文
http响应也是由三个部分组成，分别是：状态行、消息报头、响应正文
* 特点：
    * 支持客户／服务器模式
    * 简单快速，客户向服务器请求服务时，只需传送请求方法和路径。
    * 灵活，HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记
    * 无连接，限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接
    * 无状态，HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。

#### 创建ajax过程
* 创建XMLHttpRequest对象,也就是创建一个异步调用对象
* 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
* 设置响应HTTP请求状态变化的函数.
* 发送HTTP请求.
* 获取异步调用返回的数据

#### 浏览器本地存储
* `sessionStorage`用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储
* `localStorage`用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。可以跨会话访问数据
* `Cookie`大小是受限的，并且每次你请求一个新的页面的时候Cookie都会被发送过去，这样无形中浪费了带宽，另外cookie还需要指定作用域，不可以跨域调用。

#### Cookie
* `Cookie`数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被截掉
* 安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。

#### 跨域问题
* `JSONP`, 动态插入script标签，通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。优点是兼容性好，简单易用，支持浏览器与服务器双向通信。缺点是只支持GET请求。
* `CORS`, 服务器端对于CORS的支持，主要就是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问
* 使用`window.name`来进行跨域
* 使用HTML5中新引进的`window.postMessage`方法来跨域传送数据

#### web性能优化
* 代码层面：避免使用css表达式，避免重定向
* 缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等
* 请求数量：合并样式和脚本，使用css图片精灵，CSS Sprites，拆分初始化负载，划分主域
* 请求带宽：开启GZIP，精简JavaScript，压缩文件，移除重复脚本，图像优化

#### react最佳实践
* 避免传递一个新闭包（Closures）给子组件
* 代码分割，懒加载
* 使用类
* 扁平化设计state，使用immutable state

#### Http 2.0
* 引入了“服务端推（server push）”的概念，它允许服务端在客户端需要数据之前就主动地将数据发送到客户端缓存中，从而提高性能
* 增加了头压缩（header compression），因此即使非常小的请求，其请求和响应的header都只会占用很小比例的带宽
* 提供更多的加密支持
* 使用多路技术，允许多个消息在一个连接上同时交差

#### Proxy
在目标对象前架设一层拦截层
```javascript
const obj = new Proxy({}, {
    get: function(target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
    },
    set: function(target, key, value, receiver) {
        console.log(`setting ${key}!`);
        return Reflect.set(target, key, value, receiver);
    },
    apple: function(target, obj, args) {
        console.log('拦截Proxy实例作为函数调用');
        return Reflect.apple(...arguments);
    },
    construct: function(target, args) {
        console.log('拦截Proxy实例作为构造函数调用');
        return new target(...args);
    }
    ...
});
```

#### Reflect 设计目的
* 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。也就是说，从`Reflect`对象上可以拿到语言内部的方法
* 修改某些`Object`方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`
* 让`Object`操作都变成函数行为。某些`Object`操作是命令式，比如`name in obj和delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为
* `Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为
