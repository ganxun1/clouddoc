# Router

router用于建立URL中hash与Controller之间的对应关系，添加router的操作通常在Controller文件中一些定义。

一个Controller可以对应多个URL，一个URL只能对应一个Controller。

* ### add()

	使用add()可以在router添加一组hash与Controller的对于关系，方法如下：


	sumeru.router.add(

		{
			pattern: '/studentList',
			action: 'App.studentList'
		}

	);

	* ### pattern

		URL中hash部分的值
	
	* ### action

		对应Controller的名称
	
在router中添加了hash与Controller的对应关系后，就可以使用localhost:8080/debug.html/studentList访问

同时我们还提供定义默认启动Controller的方法：

* ### setDefault()

		sumeru.router.setDefault('App.studentList');
	
在Controller中使用setDefault()后，浏览器中输入“localhost:8080/debug.html”就可以启动该Controller，不需要在URL中带hash。


在Clouda的最新版中加入了Server渲染的功能，框架能在Server端将数据以及view渲染完成后下发到客户端，这样加快view渲染的速度。**server渲染默认是开启的**，如果想**单独禁止某个View在Server渲染**，可在Router中添加

	sumeru.router.add({
    	pattern:'/test',
    	action : 'App.unittest',
    	server_render:false
	})



## 与BackBone等框架兼容

为兼容已有代码，sumeru可以将router移交给现有代码进行接管。

	sumeru.router.externalProcessor.add(backbone.router.navigate);