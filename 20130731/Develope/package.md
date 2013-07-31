# package.js

package.js用于将文件之间的依赖关系添加到sumeru中，我们可以使用下面的语法编写该文件：

	 sumeru.packages(
	 	'student.js,
	 	.....
	 )
	 
并不是在所有文件夹下新建文件或者文件夹后就要修改package.js文件，view文件夹和publish文件夹例外。