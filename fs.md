### fs模块
fs模块用于对系统文件及目录进行读写操作。使用  
```
var fs = require('fs')
```
的方式载入模块，模块中所有方法都有同步和异步两种形式。  
异步方法回调函数中的第一个参数总是保留给异常参数（exception），如果执行成功，没有异常，则该参数值为null或者undefined。  
异步实例：
```
var fs = require('fs'); // 载入fs模块

fs.unlink('/tmp/shiyanlou', function(err) {
    if (err) {
        throw err;
    }
    console.log('成功删除了 /tmp/shiyanlou');
});
```
同步写法：
```
var fs = require('fs');

fs.unlinkSync('/tmp/shiyanlou');
console.log('成功删除了 /tmp/shiyanlou');
```
同步和异步的区别很明显：同步方法只有等该方法执行完成才能执行后续的代码，而异步方法不需要等待该方法执行完，它有一个回调函数，等执行完成以后会自动执行回调里面
