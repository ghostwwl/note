
# 怎么把 nodejs 项目打包为可执行文件

## PKG 是干这个的

支持把 nodejs 项目打包为 freebsd, linux, alpine, macos, win 等平台的可执行文件

### 1, 全局安装 pkg

- npm i -g pkg

### 2, 修改package.json

- 添加以下配置

```json
{
  //其他配置
  "bin" : "./bin/www",
  "pkg": {
    "assets": ["public/**/*", "views/**/*"]
  }
}
```

- `bin` 项目启动文件，
- `pkg.assets` 项目的资源
    - 因为默认PKG会自动寻找依赖包括node_modules，routes。
    - 但不会寻找views视图模板和public静态资源，所以我们手动加进入。


### 3, 开始编译

- `pkg -t win package.json`
- `pkg -t linux package.json`


#### 当然你如果是单个文件的更简单

- 例如 后面的 hello.js 
- 同时打包 win/linux 平台可执行文件，打包平台可以是 win/linux/mac 都可以的，我是 linux下打包 win 和 linux 下跑
    - `pkg -t win hello.js`
        - 可以生成 hello.exe
        - 直接 cmd 下 hello.exe 
    - `pkg -t linux hello.js`
        - 生成 hello 
        - 直接 shell下  ./ hello 可以执行

- 以下是hello.js 

```js
// 输出全局变量 __filename 的值
console.log( __filename );
// 输出全局变量 __dirname 的值
console.log( __dirname );

// 输出到终端
process.stdout.write("Hello World!" + "\n");

// 通过参数读取
process.argv.forEach(function(val, index, array) {
   console.log(index + ': ' + val);
});

// 获取执行路径
console.log(process.execPath);
// 平台信息
console.log(process.platform);

console.log('-------------------- os info: ---------------\n');
var os = require("os");
// CPU 的字节序
console.log('endianness : ' + os.endianness());
// 操作系统名
console.log('type : ' + os.type());
// 操作系统名
console.log('platform : ' + os.platform());
// 系统内存总量
console.log('total memory : ' + os.totalmem() + " bytes.");
// 操作系统空闲内存量
console.log('free memory : ' + os.freemem() + " bytes.");

console.log("程序执行完毕。");
```

- 执行效果