# **华农办公室电话查询开发过程**

## 引用Bootstrap和Jquery
```
<link rel="stylesheet"href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
<script src="https://cdn.bootcss.com/jquery/2.1.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<link rel="stylesheet" href="https://code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
<script src="https://code.jquery.com/jquery-1.12.4.js"></script>
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>
```
## 前端
* ### 导航栏
 ```
 <nav class="navbar navbar-default" role="navigation">
	<div class="container-fluid">
	<div class="navbar-header">
		<a class="navbar-brand" href="index.html">华农办公室电话查询</a>
	</div>
	<form class="navbar-form navbar-right" role="search" >
			<div class="form-group" class="ui-widget">
				<input type="text" class="form-contrtol" placeholder="Search" name="search" id="tags">
			</div>
		<button type="button" class="btn btn-default" id="button1" data-toggle="modal" data-target = "#check-modal" >查询</button>
	</form>
	</div>
</nav>
```
>向 `<nav>` 标签添加 class .navbar、.navbar-default。

>向上面的元素添加 role="navigation"，有助于增加可访问性。

>向` <div>` 元素添加一个标题 class .navbar-header，内部包含了带有 class navbar-brand 的 `<a>` 元素。这会让文本看起来更大一号。

>为了向导航栏添加链接，只需要简单地添加带有 class .nav、.navbar-nav 的无序列表即可。
###    查询按钮指向了一个id="check-modal"的虚拟框，当点击查询按钮时会触发虚拟框

* ### 分类按钮
```
<button type="button" class="btn btn-info btn-block" data-toggle="collapse" 
    data-target="****">
    ********
 </button>    
 <div id="****" class="collapse">
 </div>
 ```
>data-toggle="collapse" 添加到您想要展开或折叠的组件的链接上。

>href 或 data-target 属性添加到父组件，它的值是子组件的 id。
### 在id="****"的这个`<div>`中将会用javascript写入需要折叠的内容

* ### 虚拟框
```
<div id = "xunikuang">
</div>

<div id = "sousuo">
</div>
```
在这两个`<div>`中将会写入虚拟框的代码

## 后台

* ### Jquery
```
所有 jQuery 函数位于一个 document ready 函数中
$(document).ready(function(){
 
   // 开始写 jQuery 代码...
 
});
```
* ### Ajax
```
var xmlhttp;
		var availableTags = new Array;
		var number = new Array;
	if (window.XMLHttpRequest)
	{
		// IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
		xmlhttp=new XMLHttpRequest();
	}
	else
	{
		// IE6, IE5 浏览器执行代码
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange=function()
	{
		if (xmlhttp.readyState==4 && xmlhttp.status==200)
		{
			var data = eval(xmlhttp.responseText);
			for (var i = 76; i < data.length; i++) {
				availableTags[i-76] = data[i];
				number[i-76] = data[i-76];
			}
```
因为后台数据是json格式，用`eval()`将json格式转换为javascipt的一维数组

```
            # 写入虚拟框
			var allxunikuang  = "";
			for (var i = 0; i < 76; i++) {
				allxunikuang = allxunikuang +
				"<div class='modal fade' id=\'modal" + i + "\'" + " tabindex='-1' role='dialog' aria-labelledby='mymodellabel' aria-hidden='true'>"
	  				 +
	  				 "<div class='modal-dialog'>"
	  				 +
	  				 "<div class='modal-content'>"
	  				 +
	  				 "<div class='modal-header'>"
	  				 +
	  				 "<button type='button'class='close' data-dismiss='modal' aria-hidden='true'>&times;</button>"
	  				 +
	  				 "<h4 class='modal-title' id='myModalLabel'>" + availableTags[i] + "</h4>"
	  				 +
	  				 "</div>"
	  				 +
	  				 "<div class='modal-body'>" + "电话：" + number[i] + "</div>"
	  				 +
	  				 "<div class='modal-footer'>"
	  				 +
	  				 "<button type='button' class='btn btn-default' data-dismiss='modal'>关闭</button>"
	  				 +
	  				 "</div>"
	  				 +
	  				 "</div>"
	  				 +
	  				 "</div>"
	  				 +
	  				 "</div>";
			}
			document.getElementById("xunikuang").innerHTML=allxunikuang;
```
1.使用模态窗口，您需要有某种触发器。您可以使用按钮或链接。这里我们使用的是按钮。

2.如果您仔细查看上面的代码，您会发现在 `<button>` 标签中，data-target="#myModal" 是您想要在页面上加载的模态框的目标。您可以在页面上创建多个模态框，然后为每个模态框创建不同的触发器。现在，很明显，您不能在同一时间加载多个模块，但您可以在页面上创建多个在不同时间进行加载。

3.在模态框中需要注意两点：
   第一是 .modal，用来把 `<div>` 的内容识别为模态框。
   第二是 .fade class。当模态框被切换时，它会引起内容淡入淡出。

4.aria-labelledby="myModalLabel"，该属性引用模态框的标题。

5.属性 aria-hidden="true" 用于保持模态窗口不可见，直到触发器被触发为止（比如点击在相关的按钮上）。

6.`<div class="modal-header">`，modal-header 是为模态窗口的头部定义样式的类。

7.class="close"，close 是一个 CSS class，用于为模态窗口的关闭按钮设置样式。

8.data-dismiss="modal"，是一个自定义的 HTML5 data 属性。在这里它被用于关闭模态窗口。

9.class="modal-body"，是 Bootstrap CSS 的一个 CSS class，用于为模态窗口的主体设置样式。

10.class="modal-footer"，是 Bootstrap CSS 的一个 CSS class，用于为模态窗口的底部设置样式。

11.data-toggle="modal"，HTML5 自定义的 data 属性 data-toggle 用于打开模态窗口。

```
            #写入折叠的内容 为一个个的指向虚拟框的导航元素
            var allbtn = "<ul class='nav nav-pills nav-justified'>";
			for (var i = 0; i < 20; i++) {
				allbtn += "<li><a data-toggle='modal' href=\'#modal" + i +"\'" + ">" + availableTags[i] + "</a></li>";
				if ((i+2)%8 == 0) { allbtn += "</ul><br><br><br>" + "<ul class='nav nav-pills nav-justified'>" ; }
			}
			allbtn += "</ul><br><br><br>";
			document.getElementById("demo1").innerHTML=allbtn;

			var allbtn1 = "<ul class='nav nav-pills nav-justified'>";
			for (var i = 20; i < 45; i++) {
				allbtn1 += "<li><a data-toggle='modal' href=\'#modal" + i +"\'" + ">" + availableTags[i] + "</a></li>";
				if ((i+6)%8 == 0) { allbtn1 += "</ul><br><br><br>" + "<ul class='nav nav-pills nav-justified'>" ; }
			}
			allbtn1 += "</ul><br><br><br>";
			document.getElementById("demo2").innerHTML=allbtn1;

			var allbtn2 = "<ul class='nav nav-pills nav-justified'>";
			for (var i = 45; i < 56; i++) {
				allbtn2 += "<li><a data-toggle='modal' href=\'#modal" + i +"\'" + ">" + availableTags[i] + "</a></li>";
				if ((i+6)%8 == 0) { allbtn2 += "</ul><br><br><br>" + "<ul class='nav nav-pills nav-justified'>" ; }
			}
			allbtn2 += "</ul><br><br><br>";
			document.getElementById("demo3").innerHTML=allbtn2;

			var allbtn3 = "<ul class='nav nav-pills nav-justified'>";
			for (var i = 56; i < 63; i++) {
				allbtn3 += "<li><a data-toggle='modal' href=\'#modal" + i +"\'" + ">" + availableTags[i] + "</a></li>";
				//if ((i+4)%8 == 0) { allbtn2 += "</ul><br><br><br>" + "<ul class='nav nav-pills nav-justified'>" ; }
			}
			allbtn3 += "</ul><br><br><br>";
			document.getElementById("demo4").innerHTML=allbtn3;

			var allbtn4 = "<ul class='nav nav-pills nav-justified'>";
			for (var i = 63; i < 76; i++) {
				allbtn4 += "<li><a data-toggle='modal' href=\'#modal" + i +"\'" + ">" + availableTags[i] + "</a></li>";
				if ((i+7)%8 == 0) { allbtn4 += "</ul><br><br><br>" + "<ul class='nav nav-pills nav-justified'>" ; }
			}
			allbtn4 += "</ul><br><br><br>";
			document.getElementById("demo5").innerHTML=allbtn4;
			//document.getElementById("myDiv").innerHTML=data;
```


```
		}
	}
	xmlhttp.open("GET","http://139.199.79.172/office/index.php?range='all' " ,true); #向服务器发送请求
	xmlhttp.send();

```
```
$( "#tags" ).autocomplete({
      source: availableTags
    });
```
运用了jQuery Autocomplete 插件根据用户输入值进行搜索和过滤，让用户快速找到并从预设值列表中选择。通过给 Autocomplete 字段焦点或者在其中输入字符，插件开始搜索匹配的条目并显示供选择的值的列表。通过输入更多的字符，用户可以过滤列表以获得更好的匹配。

