# chosen.jquery
下拉列表美化插件
> 傻瓜式用法介绍：

1.引入文件
```
 <link href="../css/chosen.css" rel="stylesheet" /> <!--引入chosen.css-->
    <script src="../js/jquery-1.10.2.js"></script> <!--引入jquery-*.js-->
    <script src="../js/chosen.jquery.js"></script> <!--引入chosen.jquery.js-->
 ```
2.初始化下拉列表（配置js）
```
if($(".chosen-select").length !=0){
    	var config = {
    			'.chosen-select'           : {},
    			'.chosen-select-deselect'  : {allow_single_deselect:true},
    			'.chosen-select-no-single' : {disable_search_threshold:10},
    			'.chosen-select-no-results': {no_results_text:'Oops, nothing found!'},
    			'.chosen-select-width'     : {width:"100%"}
    	}
    	$(".chosen-select").chosen(config);
    	$(".chosen-select").trigger("chosen:updated");
    }
```
3.在select中加入相应的class名即完成
```
<select  name="arg" class="chosen-select" id="orgId">
```
### 项目实例
```
$("#searchForm #orgId").change(function(){
		  var orgId = $(this).val();
		  var html = '<option value="">请选择...</option>';
		  if(orgId == undefined || orgId == "" || orgId==null){
			  $('#searchForm #depId').html(html);
			  $("#searchForm #depId").trigger('chosen:updated');
		  }else{//动态获取加载列表
        $(this).chosenValChange({
				  data:{orgId:$(this).val()},
				  url:"getGroupData.shtml",
				  success:function(resultJson){
					  var resultObj = eval(resultJson);
			    	  if(resultObj.type == "S") {
			    		 var data = resultObj.dataList;
			    		 for(var i=0; i<data.length; i++){
			    			html = html + '<option value="'+data[i].depId+'">'+data[i].depName+'</option>';
			    		 }
			    		 $('#searchForm #depId').html(html);
						 $("#searchForm #depId").trigger('chosen:updated');
			     }
				  }
			  });
		  }
	  });
    
/**
 * 扩展方法下拉列表值变化，发送ajax请求
 */
$.fn.chosenValChange = function (options) {
	var defaults={
		url : "", 
		data: "",
		success:function(){}
	};
	var opts = $.extend(defaults,options);
	$.ajax(opts);
};
 ```
