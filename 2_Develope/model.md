# Model

我们使用Model来定义App的数据模型，例如在model/下创建一个student.js

	 Model.student = function(exports){    
    
	 };
	 	 
在"student"中添加"studentName"、"age"和"gender"三个字段：

	 Model.student = function(exports){
    	exports.config = {
        	fields: [
        		{name : 'studentName', type: 'string'},
           		{name :	'age',         type: 'int'},
          		{name :	'gender',      type: 'string'}
        	]
    	};
     };

* ### name

	字段的名称
	
* ### type

	字段的数据类型，包括"int"、"date"、"string"、"object"、"array"、"model"、"collection"。
	
除以上两种，常用的属性还包括:

* ### defaultValue

	字段的默认值
	
		{name: 'gender', type: 'string', defaultValue: 'male'}
		
	若不提供"gender"值时，则字段的默认值为"male"。
	
	
	再看一个时间的例子：
	
		{name: 'time', type: 'date', defaultValue: 'now()'}
		
	若不提供"time"值时，则字段的默认值为当前服务器时间。

* ### validation

	字段的验证，validation包括以下方法：
	
	* length[min,max] 
		
		字段值得长度在min-max的范围。
		
	* mobilephone
	
		必须为手机号码格式
	
	* required
	
		字段值不能为空
		
	* number	
		
		字段值必须为数字
	
	* unique
	
		字段值必须唯一
		
* ### 如何自定义验证方法
	
	如果您需要的验证方法并不在上述方法内，您可以使用下面的方法自定义验证方法：
			
	打开sumeru/src/validationRules.js：
	
	添加语法如下：
	
		fw.validation.addrule("RuleName" , {										"runat":"both",										"regexp":"(^0?[1][358][0-9]{9}$)",										"msg":"$1必须为手机号码格式。"									});
									
	参数说明：
		RuleName：	验证规则名称
		runat： 规则运行地方选择（服务器端/客户端）
				both：服务器端和客户端都运行
				server:服务器端运行
				client：在客户端运行
		regexp: 规则的正则表达式
		msg: 当验证错误时返回的信息
		$1 : 如果在Model中定义了属性的label则显示label的内容，如果没有，则显示该属性的名称（name）
				
	
	
	更多内置验证方法和自定义验证方法，请参考附录：《API说明文档》
	


* ### model

	当type值为model和collection时，表示该字段包含一个指向其他model的1:1 或 1:n 的关系。
	此时，需同时提供model字段以声明指向的model对象。

		{name: 'classes', type: 'model', model: 'Model.classes'}