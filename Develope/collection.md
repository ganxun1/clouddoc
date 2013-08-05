#Collection

Collection是Model的集合，我们之前曾使用过的subscribe()返回的结果集即是Collection。

	session.studentCollection = env.subscribe("pub-allStudents",function(myCollection){

    });
    
session.studentCollection是返回的Collection。可对数据集进行“增、删、查、改”的操作：

* ### add()

	使用add()在Collection中添加一行数据。
	
		session.studentCollection.add({
			studentName: 'John',
			age: 18,
			gender:"male"			 		
		});
		

* ### save()

	save()是用于将collection的修改保存到Server，在通常情况下，调用save()方法会自动触发对应视图block的更新。
	
		session.studentCollection.save();

	
	
* ### find()

	使用find()查询Collection中符合条件的所有Model。
	
	    session.studentCollection.find();
	
	使用条件查询时，例如查找gender为“male”的Model;
	
		session.studentCollection.find({gender:'male'});
		
		
* ### destroy()

	使用destroy()从Collection中移除数据,
	
		session.studentCollection.destroy();
		
	使用条件删除时，例如删除gender为“male”的Model：
	
		session.studentCollection.destroy({gender:'male'});


更多Collection API 请参考附录：《API说明文档》	
