# wxappUnpacker  微信小程序反编译


[反编译源码(修复报错)](https://github.com/Men-HuLu/wxappUnpacker)

源:[github项目:反编译源码](https://github.com/qwerty472123/wxappUnpacker)

#### 1.小程序位置：
###### 所在目录：==/data/data/com.tencent.mm/MicroMsg/{一串数字}/appbrand/pkg/==
一串数字：各个手机都不一样,翻一下看里面有appbrand文件就醒了


#### 2.工具准备：
1. 如果没有安装nodejs，请先安装一下
1. 安卓模拟器获取到.wxapkg文件(MuMu模拟器,re管理器)
1. 下载文件


#### 3.安装依赖
依赖这些 node.js 程序除了自带的 API 外还依赖于以下包

依次安装cssbeautify、CSSTree、VM2、Esprima、UglifyES、js-beautify
```
npm install esprima 
npm install css-tree -g
npm install cssbeautify 
npm install vm2 -g
npm install uglify-es 
npm install js-beautify 
npm install escodegen 
其他还有
反正缺啥补啥 npm install安装就行
```

#### 4.编译报错修复
解决反编译微信小程序解决$gwx is not defined报错

修改wxappUnpacker中wuWxss.js部分代码

```
let wxAppCode={},handle={cssFile:name};
let gg=new GwxCfg();
let tsandbox={
	$gwx:GwxCfg.prototype["$gwx"],
	__mainPageFrameReady__:GwxCfg.prototype["$gwx"],
	__wxAppCode__:wxAppCode,
	setCssToHead:cssRebuild.bind(handle)
}
let vm=new VM({sandbox:tsandbox});
vm.run(code);
```


#### 5.运行命令

```
node wuJs.js <files...> 
将 app-service.js (或小游戏中的 game.js ) 拆分成一系列原先独立的 javascript 文件，并使用 Uglify-ES 美化，从而尽可能还原编译前的情况。

node wuWxml.js [-m] <files...> 
将编译/混合到 page-frame.html ( 或 app-wxss.js ) 中的 wxml 和 wxs 文件还原为独立的、未编译的文件。如果加上-m指令，就会阻止block块自动省略，可能帮助解决一些相关过程的 bug 。

node wuWxss.js <dirs...> 
通过获取文件夹下的 page-frame.html ( 或 app-wxss.js ) 和其他 html 文件的内容，还原出编译前 wxss 文件的内容。


node wuWxapkg.js [-o] [-d] [-s=<Main Dir>] <files...>  (简单的直接运行这一句就行,files直接路径就行)
将 wxapkg 文件解包，并将包中上述命令中所提的被编译/混合的文件自动地恢复原状。如果加上-o指令，表示仅解包，不做后续操作。如果加上-d指令，就会保留编译/混合后所生成的新文件，否则会自动删去这些文件。同时，前面命令中的指令也可直接加在这一命令上。而如果需要解压分包，请先解压主包，然后执行node wuWxapkg.js [-d] -s=<Main Dir> <subPackages...>，其中Main Dir为主包解压地址。除-d与-s外，这些指令两两共存的后果是未定义的（当然，是不会有危险的）。
```


