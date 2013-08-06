#Server渲染介绍

在Clouda的最新版中加入了Server渲染的功能，框架能在Server端将数据以及view渲染完成后下发到客户端，这样加快view渲染的速度。

## 如何开启和禁止Server渲染

**server渲染默认是开启的**，当需要全部禁止时，修改config/sumeru.js中，添加一行

	sumeru.config({
    	runServerRender:false
	})
	
该命令会禁止所有view在Server渲染的功能。

如果想**单独禁止某个View在Server渲染**，可在Router中添加

	sumeru.router.add({
    	pattern:'/test',
    	action : 'App.unittest',
    	server_render:false
	})
	
	
注意：如果在Controller中需要执行onload方法时，需确保onload中，没有使用前端的js中的变量或函数，

比如window，document，Localstorage等

