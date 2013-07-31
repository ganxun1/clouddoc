# Library

有的时候我们会遇到这样的麻烦，比如Model中有一个数据类型为“date”的时间字段，而在View上我想显示的是年，我们可以在View使用{{$ }}方法嵌入JavaScript来实现。

虽然这种方法可以实现，但是不易代码管理，我们需要一个library库的管理机制来解决这个问题，你可以将这个时间格式化函数存放在library/下：

* /library/getTime.js

		Library.timeUtils = sumeru.Library.create(function(exports){	
			exports.formatDate = function(time){
				return time.getFullYear();
			};
			
		});

* /view/student.html

		<block tpl-id="studentList">
	
			<p> 
				{{#each data}}
					{{$Library.timeUtils.formatDate(this.time)}}
				{{/each}}			
			</p>
			
		</block>
		
* /controller/student.js


		session.bind('studentList', {
			year : Library.timeUtils.formatDate(time)
		});