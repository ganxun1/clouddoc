#pub/sub

## Publish
Clouda使用PubSub模型描述数据的传输，其中，publish是发布数据的方法，其运行在Server上，每一个publish文件均需要放置在publish/。

	 module.exports = function(sumeru){
	
    	sumeru.publish(modelName, publishName, function(callback){

      	}); 
      	  
  	 }

可以看到在sumeru.publish()中有三个参数，modelName、publishName和一个匿名方法function(callback){}，下面详细介绍这些参数的作用。 
  	
 * #### modelName:
  
  	被发布数据所属的Model名称
  	
 * #### publishName:
 
 	所定义的Publish的唯一名称，在一个App内全局唯一，该参数与Controller中subscribe()成对使用。
 	
 * #### function(callback){}
 
 	描述数据发布规则的自定义函数，在这里定义被发布数据所需要符合的条件。自定义函数自身也可接受由subcribe()传入的参数，如：。
 	
 	*  function(arg1,arg2,...,callback){}  
 	
 		其中arg1, arg2为传入参数，传入参数的数量不限，但需要与对应的subscribe()中所传递的参数数量一致。	
	
## Subscribe	
 	
与sumeru.publish()相对应，我们在Controller中使用env.subscribe()订阅被发布的数据。其中env是Controller中很重要的一个内置对象，稍后我们还会多次见到。

	 env.subscribe(publishName, function(collection){

     });
    
* #### publishName:

	所定义的Publish的唯一名称，在一个App内全局唯一，该参数与sumeru.publish(modelName, publishName,function(callback))中的publishName名称需要保持一致。
	
* #### function(collection)

	Subscribe成功获得数据时，被调用的响应方法。通常，我们主要在其中完成将订阅得到的数据与视图进行绑定(bind)的工作。
	
	*  collection:
	 
	 	订阅获得的数据Collection对象 		

如果需要向Publish传递参数（在上一节的最后我们曾经提到），则使用如下形式。

	 env.subscribe(publishName,arg1, arg2, ..., function(collection){});	
arg1,arg2...等任意数量的参数会被传入sumeru.publish()对应的function(arg1,arg2,...,callback)中。
 
## 一个PubSub的例子
 
现有一个学生信息的Model(student)，假设Controller希望获取全班同学的信息，我们使用Publish/Subscribe方式实现如下：
 
 * Publish
 
 	 	module.exports = function(sumeru){
	
    		sumeru.publish('student', 'pub-allStudents', function(callback){
    		
    			var collection = this;

          		collection.find({}, function(err, items){
              		callback(items);
          		});

      		});       	  
  	 	}
  	 	
 * Subscribe
 
 	 	env.subscribe("pub-allStudents", function(studentCollection){

     	});		
 	
假设我们在这个基础上加一个条件限制，现在只希望获取年龄大于18岁同学的信息。

 * Publish
 
 	 	module.exports = function(sumeru){
	
    		sumeru.publish('student', 'pub-adultStudents', function(callback){
    		
    			var collection = this;

          		collection.find({"age":    		
          							{$gt:18}
          						 }, function(err, items){
              		callback(items);
          		});

      		});       	  
  	 	}
  	 	
  	大家可以看到我们使用了{"age":{$gt:18}}的方式表达了“年龄大于age”的约束要求。
  	
  	相似的，“年龄小于18”的表达方式如下：
  	
  		{"age":
  			{$lt:18}
  		}
  		
  	“大于min且小于max”的表达方式如下：
  	
  		{"age":
  			{$gt:min},
  			{$lt:max}
  		}
	
 对应的Subscribe如下
  	 	
 * Subscribe
 
 	 	env.subscribe("pub-adultStudents",function(studentCollection){

     	}); 
 	
我们在上面的方式上再加一个条件，现在需要大于18岁男生或者女生的信息，性别由Subscribe来决定，如何实现呢？

 * Publish
 
 	 	module.exports = function(sumeru){
	
    		sumeru.publish('student', 'pub-adultStudentsWithGender', function(gender,callback){
    		
    			var collection = this;

          		collection.find({"age":{$gt:18}, 
          		          		  "gender":	gender
          		          		 }, function(err, items){
              		callback(items);
          		});

      		});       	  
  	 	}
  	 	
  	在这里可以看出所发布的学生的性别，是由Subscribe决定的。这样来看，一个Publish，可以通过不同的参数，为多个Subscribe服务。从这个角度来讲，Publish有点类似于OO语言中的Class的概念，可以理解为Publish发布的是一类数据。
  	
  	类似的，对应的Subscribe调用如下： 	
  	 	
 * Subscribe
 
 	 	env.subscribe("pub-adultStudentsWithGender","male",function(msgCollection){

     	}); 
