### node path模块
#### path.normalize
```
path.normalize(path); // path路径
```
路径解析，得到规范化的路径。  
对于window系统路径分隔符为'\',unix系统路径分隔符为'/',针对'..'返回上一级；/与\\都被统一转换
```
console.log(path.dirname('./test/index.html'),__dirname);
var myPath = path.normalize(__dirname + '/crypto.js'); //  /Users/zhaojiao/study/node/launch/crypto.js
```
### path.join
```
path.join([path1],[path2]..[pathn]);
```
路径结合，合并，拼成的路径前后都不带目录分隔符。实例：
```
var path1 = './Users/zhaojiao',
	path2 = '/study/node/',
	path3 = '/aunch/crypto.js';
console.log(path.join(path1,path2,path3)); // Users/zhaojiao/study/node/aunch/crypto.js
```

### path.resolve
```
path.resolve(path1, [path2]..[pathn]);
```
获取绝对路径，以应用程序为起点，根据参数字符串解析出一个绝对路径。path至少必须一个路径字符串值，带[]为可选的。
//  /Users/zhaojiao/study/node/launch/test/index.html
```
var myPath = path.resolve('test', 'index.html'); // 当前路径为/Users/zhaojiao/study/node/launch
console.log(myPath);
```

### path.relative
```
path.relative(from, to)
```
获取两个路径之间的相对路径。 from 当前路径，并且方法返回值是基于from指定到to的相对路径，to到哪路径。
//  /Users/zhaojiao/study/node/launch/test/index.html
```
var from = '/Users/zhaojiao',
    to = '/Users/zhaojiao/study/node/launch/test';
var _path = path.relative(from, to);
console.log(_path);  // study/node/launch/test
```
