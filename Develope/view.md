# View

sumeru使用handlebars组件作为模板引擎。在view/下新建student.html：

	<p>I'm a student!</p>
	
在上一篇文档中我们介绍过sumeru的一个重要特性“随动反馈”，那么“随动反馈”是怎么实现的呢？

Controller的onready()时态里，每一个bind的BLOCKID，都对应View中的一个"block"标签。

sumeru使用Block为粒度来标记当数据发生变化时View中需要更新的部分

	<block tpl-id="studentList">
	
		<p>I'm a student!</p>
	
	</block>

当绑定数据时：

	<block tpl-id="studentList">
	
		<p> 
			{{#each data}}
				{{this.studentName}}
			{{/each}}			
		</p>
			
	</block>

View中的data来源于Controller中的session.bind()

	env.subscribe("pub-allStudents",function(studentCollection){

   		session.bind('studentList', {
       		data : studentCollection.find(),
       	}); 
       	    
    }); 

通过以上方法，我们就建立了一个基本的"随动反馈"单位，当订阅的数据发生变化时，View中对应的部分将自动更新。

sumeru对handlebars的语法做了一些扩展：

* {{> viewname}}

	在一个View中引用另一个View。
	
* {{$ alert("data.length"); }}

	在View中直接执行Javascript代码，并将返回结果输出在View中。
	
一般情况下将编写的View文件都存放在view文件夹下，如果编写的view文件不在View文件夹下，我们也提供View文件路径配置的方法，方便框架找到需要的View文件:

	sumeru.config.defineModule('view');
	sumeru.config.view(path : '/your-view-path');