# Controller

如果你曾经接触过MVC模型，那么将会很熟悉Controller的概念。在Clouda中，Controller是每个场景的控制器，负责实现App的核心业务逻辑。每一个Controller文件都放在controller/下。

	 App.studentList = sumeru.controller.create(function(env, session){
	
	 });
	 
使用sumeru.controller.create()创建一个名为studentList的Controller。

Controller具有以下几个时态：onload()、onrender()、onready()、onsleep()、onresume()、ondestroy()，下面将详细介绍这几个时态的作用和用法。

## onload()

onload()是Controller的第一个时态，Controller中需要使用的数据都在这个时态中加载，我们上面谈到过的subscribe()也多在这个时态中使用,方法如下。

	 App.studentList = sumeru.controller.create(function(env, session){
	 
	 	var getAllStudents = function(){
	 	
	 		env.subscribe("pub-allStudents",function(studentCollection){

     		});
	 		
	 	};
	 	
	 	env.onload = function(){	 		
	 		return [getAllStudents];	 		
	 	};
	
	 });
	 
## onrender()
	
当数据获取完成后，这些数据需要显示在视图(View)上，这个过程通过onrender()中的代码来实现，这是Controller的第二个时态，负责完成对视图(View)的渲染和指定转场方式。

	env.onrender = function(doRender){
	
		doRender(viewName,transition);
		
	};

* viewName 

	需要渲染的视图(View)名称。
	
* transition

	定义视图转场，形式如下：
	
		['push', 'left']      
		
	转场方式：我们提供'none', 'push'、'rotate'、'fade'、'shake'五种转场方式
	
	转场方向：不同的转场方式有不同的转场方向，请参考附录：《API说明文档》
	
## onready()

这是Controller的第三个时态，在View渲染完成后，事件绑定、DOM操作等业务逻辑都在该时态中完成；每段逻辑使用session.event包装，从而建立事件与视图block的对应关系。

	 env.onready = function(){
	
		session.event(blockID,function(){		
		
		});
	
	 };
	
* blockID

	View中block的id，关于block在接下View中会做详细的介绍。
	
* function(){}

	事件绑定、DOM操作等业务逻辑在这里完成。例如有一个View如下：
	
		<block tpl-id="studentList">		
			<button id="submit"> </button>		
		</block>	
	
	如何对view中的submit做事件绑定呢？可以通过下面代码实现：
	
		env.onready = function(){
	
			session.event("studentList",function(){		
				Library.touch.on('#submit', 'touchstart', 'submitMessage');
			});
	
	 	 };
	 	 
	Library.touch是集成在框架中的WebApp事件与手势库，他可用于实现复杂交互手势，兼容鼠标与触摸屏事件等各种场景。关于WebApp事件与手势库的详细介绍，请参考《API说明文档》。


## onsleep（）

当Controller长时间不处于活动状态时，可能会被置为睡眠状态，以确保正在运行中的Controller具有足够的资源。如果需要在Controller被暂停之前运行一些逻辑（比如暂存状态等），可以在onsleep()中完成。

	 env.onsleep = function(){
	 
	 };
	 
	
## onresume()

当处在睡眠状态的Controller被唤醒时，onresume()时态将被调用。该时态可用于执行一些恢复性业务逻辑

	 env.onresume = function(){
	 };	
	 
## ondestroy()

当Controller被销毁时，ondestroy()将被调用。

	 env.ondestroy = function(){
	 };
	 
	 
## Controller之间传递参数

*  使用env.redirect()方法

	当一个Controller（起始Controller）跳转到另一个Controller（目标Controller）时，可以使用env.redirect()方法来实现参数的传递，方法如下：
	
	* 在起始Controller中
	
			env.redirect(queryPath ,paramMap);
		
		第一个queryPath： 目标Controller在router中“pattern”的值；
	
		paramMap：需要传递的参数
		
	* 目标Controller中使用“param”对象接受参数
	
			sumeru.controller.create(function(env, session, param){
			
				
			});
			
	* 实例
	
		* SourceController.js
		
		
				sumeru.router.add(
					{
					
						pattern: '/sourcepage',
						action: 'App.SourceController'
					
					}
				
				);
		
				App.SourceController = sumeru.controller.create(function(env, session){
							
						env.redirect('/destinationpage',{a:100,b:200});							
				});
				
		*  DestinationController.js
		
		
				sumeru.router.add(
					{
					
						pattern: '/destinationpage',
						action: 'App.DestinationController'
					
					}
				
				);
		
				App.DestinationController = sumeru.controller.create(function(env, session, param){
			
					console.log(param.a);	
					console.log(param.b);
				
				});
		
			
	跳转后的URL为：http://localhost:8080/debug.html/destinationpage!a=100&b=200&

	开发者也可按照上面的URl格式来拼接一个带参数的URL。
		
	