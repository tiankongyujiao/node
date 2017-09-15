## fs模块
fs模块用于对系统文件及目录进行读写操作。使用  
```
var fs = require('fs');
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
同步和异步的区别很明显：同步方法只有等该方法执行完成才能执行后续的代码，而异步方法不需要等待该方法执行完，会立即执行后续代码，异步方法有一个回调函数，等执行完成以后会自动执行回调里面内容。  
同步和异步的使用区别就是没有回调函数，下面只讲异步使用。

#### 读取文件
读取文件语法：  
```
fs.readFile(filename,[option],callback);
```
filename是文件名，opiton是可选参数，callback回调函数。回调函数接收两个参数，第一个参数如上所述是留给异常的error，第二个参数是读取的文件内容。实例：
```
var fs = require('fs');
fs.readFile('./index1.txt','UTF-8',function(err,data){ 
    if(err){
        throw err;
    }
    console.log('utf-8:' + data.toString());  //data是在缓冲区读取的原始二进制数据，所以需要进行转码或者toString转换
    //Today you get through with nothing down is the tomorrow the man who dead yesterday eager for.
});
```
### 写入文件
写入文件语法：  
```
fs.writeFile(filename,data,[option],callback);
```
filename文件名，data写入的内容，option可选参数，callback回调函数。默认写入情况情况会先清空里面的内容，而不是追加。实例：
```
var fs = require('fs');
fs.writeFile('./index2.txt','tomorrow is another day!',{'flag':'a'}，function(err){ 
    if(err){
        throw err;
    }
    console.log('saved');
    // 写入成功后读取测试
    fs.readFile('./index2.txt', 'utf-8', function(err, data) {
        if (err) {
            throw err;
        }
        console.log(data); // tomorrow is another day!
    });
});
```
flag传值，r代表读取文件，w代表写文件，a代表追加。

### fs.read和fs.write读写文件
使用fs.read和fs.wirte读写文件的时候需要使用fs.open打开文件和fs.close关闭文件，它和fs.readFile，fs.writeFile类似，但是提供了更底层的操作。  
**fs.open**
```
fs.open(path,flag,[mode],callback);  //打开文件，一边fs.read读取和fs.wirte写入
```
path打开路径，flag打开方式，mode文件的权限，callback回调函数  
flag：
r ：读取文件，文件不存在时报错；  
r+ ：读取并写入文件，文件不存在时报错；  
rs ：以同步方式读取文件，文件不存在时报错；  
rs+ ：以同步方式读取并写入文件，文件不存在时报错；  
w ：写入文件，文件不存在则创建，存在则清空；  
wx ：和w一样，但是文件不存在时会报错；  
w+ ：读取并写入文件，文件不存在则创建，存在则清空；  
wx+ ：和w+一样，但是文件不存在时会报错；  
a ：以追加方式写入文件，文件不存在则创建；  
ax ：和a一样，但是文件不存在时会报错；  
a+ ：读取并追加写入文件，文件不存在则创建；  
ax+ ：和a+一样，但是文件不存在时会报错。  
**fs.close**
```
fs.close(fd,calllback); //用于关闭文件，fd是所打开文件的文件描述符
```
### fs.read
```
fs.read(fd,buffer,offset,length,position,callback); //接收6个参数。
```
fd 文件描述符，必须接收fs.open()方法中的回调函数返回的第二个参数。  
buffer 是存放读取到的数据的Buffer对象。  
offset 指定 向buffer中存放数据的起始位置。  
length 指定 读取文件中数据的字节数。  
position 指定 在文件中读取文件内容的起始位置。  
callback 回调函数，参数如下:  
1. err 用于抛出异常   
2. bytesRead 从文件中读取内容的实际字节数。  
3. buffer 被读取的缓存区对象。  
example:
```
var fs = require('fs');
fs.open('./fs/index3.txt','r',function(err,fd){
    if(err){
        throw err;
    }
    console.log('文件打开成功');
    var buffer = new Buffer(255);
    fs.read(fd,buffer,0,10,0,function(err,bytesRead,buffer){
        if(err){
            throw err;
        }
        console.log('bytesRead', buffer.slice(0,bytesRead).toString()); //10 nothing is
        fs.close(fd);
    });
});
```
### fs.write
```
fs.read(fd,buffer,offset,length[,position],callback(err,bytesWritten,buffer));
```
fd 文件描述符，必须接收fs.open()方法中的回调函数返回的第二个参数。  
buffer 是存放 将被写入的数据，buffer尺寸的大小设置最好是8的倍数，效率较高。  
offset  buffer写入的偏移量。  
length (integer)指定 写入文件中数据的字节数。  
position (integer) 指定 在写入文件内容的起始位置。  
callback 回调函数，参数如下  
1. err 用于抛出异常
2. bytesWritten从文件中读取内容的实际字节数。
3. buffer 被读取的缓存区对象。
实例：
```
var fs = require('fs');
fs.open('./index4.txt','w',function(err,fd){
    if(err){
        throw err;
    }
    console.log('打开成功');
    var buffer = new Buffer('tomorrow is another day!'); //写入的内容
    fs.write(fd,buffer,0,10,0,function(err,bytesWritten,buffer){
        if(err){
            throw err;
        }
        console.log('写入成功');
        // 打印出buffer中存入的数据
        console.log(bytesWritten, buffer.slice(0, bytesWritten).toString()); // 10 tomorrow i
        // 关闭文件
        fs.close(fd);
    });
});
```
## fs目录操作
### fs.mkdir
```
fs.mkdir(path,[mode],callback);
```
path需要创建的目录，mode目录的权限（默认是0777），callback回调函数。实例：
```
var fs = require('fs');
fs.mkdir('./newDir',function(err){
    if(err){
        throw err;
    }
    console.log('目录创建成功');
});
```
删除目录可以用fs.rmdir(path,callback);但是只能删除空目录。

### fs.readdir
```
fs.readdir(path,callback);
```
path读取文件路径，callback回调函数。
```
var fs = require('fs'); 
fs.readdir('./newdir', function(err, files) {
    if (err) {
        throw err;
    }
    // files是一个数组
    // 每个元素是此目录下的文件或文件夹的名称
    console.log(files);
});
```






