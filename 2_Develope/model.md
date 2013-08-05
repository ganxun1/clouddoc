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
	
	更多内置验证方法和自定义验证方法，请参考附录：《API说明文档》


* ### model

	当type值为model和collection时，表示该字段包含一个指向其他model的1:1 或 1:n 的关系。
	此时，需同时提供model字段以声明指向的model对象。

		{name: 'classes', type: 'model', model: 'Model.classes'}