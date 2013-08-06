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
		
* ### ensuresave()

	数据提交后只有服务端验证通过才能在前端展示。
	
		session.studentCollection.ensuresave();
		
		
* ### onValidation()

	在insert和update collection时对提交的数据进行验证，在使用ensuresave（）后会自动触发该方法。

		session.studentCollection.onValidation = function(ispass, runat, validationResult){

			//清除原有的错误信息
			if(ispass){clearError();}

			//显示验证结果
			g('sdvalidation').innerHTML = (runat=='client'?'客户端':'服务端')+(ispass==true?'验证通过':'验证失败')+'<br/>';

			//显示详细验证结果
			for(var i = validationResult.length-1; i>=0; i--){
				g('sd'+validationResult[i].key+'error').innerHTML +=  (runat=='client'?'客户端':'服务端')+'验证结果：'+validationResult[i].msg;
			}

			//回滚数据
			if(!ispass){
				this.rollback();
			}

			//ensureSave() 服务端验证通过 后需要 重新渲染一次数据
			//save() 服务端验证失败 后需要 重新渲染一次数据
			if(runat=='server'){
				if((ispass&&this.isEnsureSave())
					||(!ispass&&!this.isEnsureSave())){
					this.render();
				}
			}

		};


	
	
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
