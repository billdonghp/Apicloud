1、引入js脚本文件
<script type="text/javascript"src="../../../../script/jquery-1.8.3.min.js"></script>
<script type="text/javascript"src="../../../../script/doT.min.js"></script>
2、定义区域模版
<script type="text/x-dot-template"id="cdsqListTmpl"> 

<div class="houseBox">
	<divclass="houseMain">
		<divclass="manageInfor fs14">
			<spanclass="Gray">第三方管理企业：</span>
			<spanclass="manageCmy">{{=it.regulatorsName || ''}}</span>
		</div>
	</div>
</div>
</script>
3、数据进入模版
在ajax返回函数里使用，其中P为返回的json格式的数据:
 
var interText = doT.template($("#cdsqListTmpl").text());
$("#cdsq_list_frm").html(interText(p));
4、doT.js详细使用介绍
使用方法：
{{= }} for interpolation //赋值
{{ }} for evaluation   //循环
{{~ }} for array iteration //数组
{{? }} for conditionals //条件
{{! }} for interpolation with encoding
{{# }} for compile-time evaluation/includesand partials
{{## #}} for compile-time defines
 
 
调用方式：
var tmpText = doT.template(模板);
tmpText(数据源);
 
 
例子一：
 
1、for interpolation 赋值
格式：
{{= }}
 
 
 
数据源：{"name":"Jake","age":31}
 
区域:<div id="interpolation"></div>
 
 
 
模板：
 
<script id="interpolationtmpl"type="text/x-dot-template">
<div>Hi {{=it.name}}!</div>
<div>{{=it.age || ''}}</div>
</script>
 
调用方式：
 
var dataInter ={"name":"Jake","age":31};
var interText =doT.template($("#interpolationtmpl").text());
$("#interpolation").html(interText(dataInter));
 
例子二：
 
2、for evaluation forin 循环
格式：
{{ for var key in data { }} 
{{= key }} 
{{ } }}
 
数据源：{"name":"Jake","age":31,"interests":["basketball","hockey","photography"],"contact":{"email":"jake@xyz.com","phone":"999999999"}}
 
区域：<div id="evaluation"></div>
 
模板：
 
<script id="evaluationtmpl"type="text/x-dot-template">
{{ for(var prop in it) { }}
<div>KEY:{{= prop }}---VALUE:{{=it[prop] }}</div>
{{ } }}
</script>
 
调用方式：
 
var dataEval ={"name":"Jake","age":31,"interests":["basketball","hockey","photography"],"contact":{"email":"jake@xyz.com","phone":"999999999"}};
var evalText = doT.template($("#evaluationtmpl").text());
$("#evaluation").html(evalText(dataEval));
 
例子三：
 
3、for array iteration 数组
格式：
{{~data.array :value:index }}
...
{{~}}
 
数据源:{"array":["banana","apple","orange"]}
 
区域：<div id="arrays"></div>
 
模板：
<script id="arraystmpl"type="text/x-dot-template">
{{~it.array:value:index}}
<div>{{= index+1 }}{{= value}}!</div>
{{~}}
</script>
 
调用方式：
 
var dataArr ={"array":["banana","apple","orange"]};
var arrText =doT.template($("#arraystmpl").text());
$("#arrays").html(arrText(dataArr));
 
 
例子四：
 
4、{{? }} forconditionals 条件
格式：
{{? }} if
{{?? }} else if
{{??}} else
 
数据源：{"name":"Jake","age":31}
 
区域：<div id="condition"></div>
模板：
<script id="conditionstmpl"type="text/x-dot-template">
{{? !it.name }}
<div>Oh, I love your name,{{=it.name}}!</div>
{{?? it.age === 0}}
<div>Guess nobody named youyet!</div>
{{??}}
You are {{=it.age}} and still dont have aname?
{{?}}
</script>
 
调用方式：
 
var dataEncode ={"uri":"http://bebedo.com/?keywords=Yoga","html":"<divstyle='background: #f00; height: 30px; line-height: 30px;'>html元素</div>"};
var EncodeText =doT.template($("#encodetmpl").text());
$("#encode").html(EncodeText(dataEncode));
 
例子五：
 
5、for interpolationwith encoding
数据源：{"uri":"http://bebedo.com/?keywords=Yoga"}
 
格式：
 {{!it.uri}}
 
区域：<div id="encode"></div>
 
模板：
<script id="encodetmpl"type="text/x-dot-template">
Visit {{!it.uri}} {{!it.html}}
</script>
 
调用方式：
 
var dataEncode ={"uri":"http://bebedo.com/?keywords=Yoga","html":"<divstyle='background: #f00; height: 30px; line-height: 30px;'>html元素</div>"};
var EncodeText =doT.template($("#encodetmpl").text());
$("#encode").html(EncodeText(dataEncode));
 
 
例子六：
 
6、{{# }} forcompile-time evaluation/includes and partials
{{## #}} for compile-time defines
 
数据源：{"name":"Jake","age":31}
 
区域：<div id="part"></div>
 
模板：
 
<script id="parttmpl"type="text/x-dot-template">
{{##def.snippet:
<div>{{=it.name}}</div>{{#def.joke}}
#}}
{{#def.snippet}}
{{=it.html}}
</script>
 
调用方式：
 
var dataPart ={"name":"Jake","age":31,"html":"<divstyle='background: #f00; height: 30px; line-height: 30px;'>html元素</div>"};
var defPart ={"joke":"<div>{{=it.name}} who?</div>"};
var partText =doT.template($("#parttmpl").text(), undefined, defPart);
$("#part").html(partText(dataPart));
————————————————
