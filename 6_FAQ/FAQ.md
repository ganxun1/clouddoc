#F.A.Q


* 数据库连接数超限，{ [MongoError: Connect Number Excceed] name: 'MongoError', errmsg: 'Connect Number Excceed', ok: 0 }   

	答：在BAE目录中删除node_modules/mongodb目录并进行svn提交，本地目录不变。

* 在Controller中使用env.redirect()跳转后出险页面覆盖和重叠的问题？

	答：在Controller中使用onrender()渲染页面时需要使用转场效果，例如使用doRender("view", ['push','left']);这样就不会出险view覆盖和重叠的问题。具体转场效果请参考Controller
	
	
* 如何添加BAE上数据库的名称？

	答：在sumeru/src/frameworkConfig.js中12左右设置dbname=“your dbname”
	
* 为什么我的app在IE上不能加载出来？

	答：Clouda仅支持webkit浏览器
	





