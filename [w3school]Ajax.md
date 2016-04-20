@(Ajax)[马克飞象, Web前端, Ajax]

[TOC]
####AJAX 简介
**AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。**
####什么是 AJAX ？
**AJAX** 是一种用于创建快速动态网页的技术。
通过在后台与服务器进行少量数据交换，**AJAX** 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
传统的网页（不实用**AJAX** ）如果需要更新内容，必须重载整个网页面。
有很多 **AJAX** 的应用程序案例：新浪微博、Google 地图、开心网等。
Google Suggest
在 2005 年，Google 通过其 Google Suggest 使 AJAX 变得流行起来。
Google Suggest 使用 **AJAX** 创造出动态性极强的 WEB 页面：当您在谷歌的搜索框输入关键字时，**JavaScript** 会把这些字符发送到服务器，然后服务器会返回一个搜索建议的列表。
####AJAX - 创建 XMLHttpRequest 对象
**XMLHttpRequest** 是 AJAX 的基础。
所有现代浏览器均支持 **XMLHttpRequest** 对象（**IE5** 和 **IE6** 使用 **ActiveXObject**）。
**XMLHttpRequest** 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
####创建 XMLHttpRequest 对象
所有现代浏览器（**IE7+**、**Firefox**、**Chrome**、**Safari** 以及 **Opera**）均内建 **XMLHttpRequest** 对象。

创建 XMLHttpRequest 对象的语法：
`variable = new XMLHttpRequest();`

老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：
`variable = new ActiveXObject("microsoft.XMLHTTP);`

为了应对所有的现代浏览器，包括 **IE5** 和 **IE6** ，请检查浏览器是否支持 **XMLHttpRequest **对象。如果支持，则创建 **XMLHttpRequest **对象。如果不支持，则创建 **ActiveXOBject**：
``` javascript
var xmlhttp;
if(windwo.XMLHttpRequest)
{ //code for IE7+,Firefox,Chrome,Oprea,Safari
     xmlhttp = new XMLHttpRequest();
}
else
{ //code for IE6,IE5
     xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```
#####XMLHttpRequest 对象用于和服务器交换数据
**向服务器发送请求**
如需将请求发送到服务器，我们使用 `XMLHttpRequest` 对象的 `open()` 和 `send()` 方法 
```javascript
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```
`open(method,url,async)`规定	请求的类型、URL以及是否异步处理请求。
- metod: 请求的类型；GET 或 POST
- url: 文件在服务器上的位置
- async: true（异步）或 false （同步）

`send(string)`将请求发送到服务器
- string: 仅用于 POST 请求

####GET请求
一个简单的 GET 请求：
```javascript
xmlhttp.open("GET","./do.php",true);
xmlhttp.send();
```
一小段看不懂，那就再来一段长的：
```javascript
<html>
<head>
<script type="text/javascript">
function loadXMLDoc()
{
var xmlhttp;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","./do.php",true); //请求地址，请求的文件(这里是 do.php )里写入处理方法
xmlhttp.send();
}
</script>
</head>
<body>

<h2>AJAX</h2>
<button type="button" onclick="loadXMLDoc()">请求数据</button>
<div id="myDiv"></div>

</body>
</html>

```
上面例子中，可能得到的是缓存的效果
为了避免这种情况，请向 **URL** 添加一个唯一的 **ID**； 
`xmlhttp.open("GET","./do.php?t="+Math.random(),true);`
如果您希望通过 GET 方法发送信息，请向 URL 添加信息：
`xmlhttp.open("GET","./do.php?fname=Bill&lname=Gates",true);`
上面代码传递了一个 Bill 和 Gates 过去

####POST请求
一个简单 POST 请求：
```javascript
xmlhttp.open("POST","./postdo.php",true);
xmlhttp.send();
```
这里和 **GET** 方法一样 只是传递变成了 **POST**
如果需要像 **HTML**表单那样 **POST**数据，请使用 `setRequestHeader()` **来添加 **HTTP** 头。然后在 `send()` 方法中规定您希望发送的数据：
```javascript
xmlhttp.open("POST","./postdo.php".ture);
xmlhttp.setRequestHeader("Content-type","application/x-www-urlencoded");
xmlhttp.send("fname=Bill$lname=Gates");
```
方法：
`setRequestHeader(header,value)` 
向请求添加 **HTTP** 头。
- header：规定头的名称
- value：规定头的值

####url - 服务器上的文件
`open()` 方法的 **url** 参数是服务器上文件的地址：
`xmlhttp.open("GET","./do.php".true);`
该文件可以使任何类型的文件，比如 **.txt** 和 **.xml**，或者服务器脚本文件，比如 **.asp** 和 **.php** （在传回相应之前，能够在服务器上执行任务）。

####异步 - True 或 False？
**AJAX** 指的是异步 **JavaScript 和 XML（Asynchronous JavaScript and XML ）**。
`xmlhttp.open("GET","do.php",true);`
对于 web 开发人员来说，发送异步请求是一个巨大的进。很多在服务器执行的任务都相当费时。**AJAX** 出现前，这可能会引起应用程序挂起或停止。 

通过 **AJAX**， **JavaScript** 无需等待服务器的相应，
- 在等待服务器相应执行其他脚本
- 当相应就绪后对相应进行处理

####Async = true
当使用 **async=ture** 时，请规定在响应处于 **onreadystatechange** 事件中的就绪状态时执行的函数：
```javascript
xmlhttp.onreadystatechange=function()
{
	if(xmlhttp.readyState == 4 && xmlhttp.status == 200)
	{
		document.getElementById("myDiv").innerHTML=xml.reponseText;
	}
}
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```
####Async = false
如需使用 **async=false**，请将 `open()` 方法中的第三个参数改为 false:
`xmlhttp.open("GET","test1.txt",false)`
我们不推荐使用 **async=false**，但是对于一些小型的请求，也是可以的。
请记住，**JavaScript** 会等到服务器相应就绪才继续执行。如果服务器繁忙或缓慢，应用程序会挂起或停止。
注释：当您使用 **async=false** 时，请不要编写 **onreadystatechange** 函数 - 把代码放到 `send()` 语句后面即可：
```javascript
xmlhttp.open("GET","test1.txt",false);
xmlhttp.send();
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

####AJAX 服务器响应
####服务器响应
如需活得来自服务器的响应，请使用 **XMLHttpRequest** 对象 **responseText** 或 **responseXML** 属性。
属性：
`responseText` 获得字符串形式的相应数据。
`responseXML` 获得 XML 格式的响应数据。

#### #responseText 属性
如果来自服务器的响应并非 **XML**，请使用 **responseText** 属性。
**resoibseText** 属性返回字符串形式的响应，因此您可以这样使用：
```javascript
ducument.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```
####respnseXML 属性
如果来自服务器的响应是 **XML**，而且需要作为 **XML** 对象进行解析，请使用 **responseXML** 属性：
请求 book.xml 文件，并解析响应：
```javascript
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ATRIST");
for(i=0;i<x.length;i++)
{
txt=txt + x[i].childNodes[0].nodeValue + "<br />";
}
document.getElementById("myDiv").innerHTML=txt;
```
####onreadystatechange 事件
当请求被发送到服务器时，我们需要执行一些基于响应的任务。
每当 **readyState** 改变时，就会触发 **onreadystatechange** 事件。
**readyState** 属性存有 **XMLHttpRequest** 的状态信息。
下面是 **XMLHttpRequest** 对象的三个重要的属性：
`onreadystatechange` 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
`readyState` 存有 XMLHttpRequest 的状态，从 0 到 4 发生变化。
- 0：请求未初始化
- 1：服务器连接已建立
- 2：请求已接受
- 3：请求处理中
- 4：请求已完成，且响应已就绪
`status` 200：“OK”    404：未找到页面
在 **onreadystatechange** 事件中，我们规定当服务器相应已做好被处理的准备时所执行的任务。
当 **readyState **等于 **4** 且状态为 **200** 时，表示相应已就绪：
```javascript
xmlhttp.onreadystatechange=function()
{
if(xmlhttp.readyState==4 && xmlhttp.status==200)
	{
	document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
	}
}
```
注释：**onreadystatechange **事件被触发 5 次（0 - 4），对应着 **readyState **的每个变化。

####使用 Callback 函数
**callback **函数是一种以参数形式传递给另一个函数的函数。 
如果您的网站上存在多个 **AJAX **任务，那么您应该为创建 **XMLHttpRequest **对象编写一个**标准**的函数，并为每个 **AJAX** 任务调用该函数。 
该函数调用应该包含 URL 以及发生 onreadystatechange 事件时执行的任务（每次调用可能不尽相同）：
```javascript
function myFunction()
{
loadXMLDoc("ajax_info.txt",function(){
if(xmlhttp.readyState==4 && xmlhttp.status==200)
	{
	document.getElementById("myDiv").innerHTML=xmlhttp.responseText;}
});
}
```
####AJAX PHP请求实例
#####AJAX 用于创造动态性更强的应用程序
####实例解释 - showHint() 函数
当用户输入框输入字符时，会执行函数"**showHint()**"。该函数由 "**onkeyup**"事件触发：
实例：
```javascript
<html>
<head>
<script type="text/javascript">
function showHint(str)
{
var xmlhttp;
if (str.length==0)
  { 
  document.getElementById("txtHint").innerHTML="";
  return;
  }
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("txtHint").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","/ajax/do.php?q="+str,true);
xmlhttp.send();
}
</script>
</head>
<body>

<h3>请在下面的输入框中键入字母（A - Z）：</h3>
<form action=""> 
姓氏：<input type="text" id="txt1" onkeyup="showHint(this.value)" /> <!--onkeyup 事件会在键盘按键被松开时发生-->
</form>
<p>建议：<span id="txtHint"></span></p> 

</body>
</html>
```
源代码解释：
如果输入框为空（str.length==0），则该函数清空 txtHint 占位符的内容，并退出函数。
如果输入框不为空，showHint() 函数执行以下任务：
- 创建 XMLhttpRequest 对象
- 当服务器相应就绪时，执行函数
- 把请求发送到服务器上的文件
- 请注意我们向 URL 添加了一个参数 q （带有输入框的内容）      

####PHP处理文件
```php
<?php
// 用名字来填充数组
$a[]="Anna";
$a[]="Brittany";
$a[]="Cinderella";
$a[]="Diana";
$a[]="Eva";
$a[]="Fiona";
$a[]="Gunda";
$a[]="Hege";
$a[]="Inga";
$a[]="Johanna";
$a[]="Kitty";
$a[]="Linda";
$a[]="Nina";
$a[]="Ophelia";
$a[]="Petunia";
$a[]="Amanda";
$a[]="Raquel";
$a[]="Cindy";
$a[]="Doris";
$a[]="Eve";
$a[]="Evita";
$a[]="Sunniva";
$a[]="Tove";
$a[]="Unni";
$a[]="Violet";
$a[]="Liza";
$a[]="Elizabeth";
$a[]="Ellen";
$a[]="Wenche";
$a[]="Vicky";

//获得来自 URL 的 q 参数
$q=$_GET["q"];

//如果 q 大于 0，则查找数组中的所有提示
if (strlen($q) > 0)
  {
  $hint="";
  for($i=0; $i<count($a); $i++)
    {
    if (strtolower($q)==strtolower(substr($a[$i],0,strlen($q))))
      {
      if ($hint=="")
        {
        $hint=$a[$i];
        }
      else
        {
        $hint=$hint." , ".$a[$i];
        }
      }
    }
  }

// 如果未找到提示，则把输出设置为 "no suggestion"
// 否则设置为正确的值
if ($hint == "")
  {
  $response="no suggestion";
  }
else
  {
  $response=$hint;
  }

//输出响应
echo $response;
?>
```

AJAX 数据库实例
代码：
```javascript
<html>
<head>
<script type="text/javascript">
function showCustomer(str)
{
var xmlhttp;    
if (str=="")
  {
  document.getElementById("txtHint").innerHTML="";
  return;
  }
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("txtHint").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","/ajax/getcustomer.asp?q="+str,true);
xmlhttp.send();
}
</script>
</head>
<body>

<form action="" style="margin-top:15px;"> 
<label>请选择一位客户：
<select name="customers" onchange="showCustomer(this.value)" style="font-family:Verdana, Arial, Helvetica, sans-serif;">
<option value="APPLE">Apple Computer, Inc.</option>
<option value="BAIDU ">BAIDU, Inc</option>
<option value="Canon">Canon USA, Inc.</option>
<option value="Google">Google, Inc.</option>
<option value="Nokia">Nokia Corporation</option>
<option value="SONY">Sony Corporation of America</option>
</select>
</label>
</form>
<br />
<div id="txtHint">客户信息将在此处列出 ...</div>

</body>
</html>

```
实例解释 - **showCustomer()**函数
当用户在上面下拉列表中选择某个客户时，会执行名为“ **showCustomer()** ”的函数。 该函数由 “ **onchange** ”事件触发
**showCustomer()** 函数执行以下任务：
 - 检查是否已选择某个客户
 - 创建 XMLHttpRequest 对象
 - 当服务器相应就绪时执行所创建的函数
 - 把请求发送到服务器上的文件
 - 请注意我们向 URL 添加了一个参数 q （带有输入域中的内容）

AJAX 服务器页面：
```php
<?php
$q=$_GET["q"];

$con = mysql_connect('localhost', 'peter', 'abc123');
if (!$con)
 {
 die('Could not connect: ' . mysql_error());
 }

mysql_select_db("ajax_demo", $con);

$sql="SELECT * FROM user WHERE id = '".$q."'";

$result = mysql_query($sql);

echo "<table border='1'>
<tr>
<th>Firstname</th>
<th>Lastname</th>
<th>Age</th>
<th>Hometown</th>
<th>Job</th>
</tr>";

while($row = mysql_fetch_array($result))
 {
 echo "<tr>";
 echo "<td>" . $row['FirstName'] . "</td>";
 echo "<td>" . $row['LastName'] . "</td>";
 echo "<td>" . $row['Age'] . "</td>";
 echo "<td>" . $row['Hometown'] . "</td>";
 echo "<td>" . $row['Job'] . "</td>";
 echo "</tr>";
 }
echo "</table>";

mysql_close($con);
?>
```

#####AJAX XML实例
代码：
```javascript
<html>
<head>
<script type="text/javascript">
function loadXMLDoc(url)
{
var xmlhttp;
var txt,x,xx,i;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    txt="<table border='1'><tr><th>Title</th><th>Artist</th></tr>";
    x=xmlhttp.responseXML.documentElement.getElementsByTagName("CD");
    for (i=0;i<x.length;i++)
      {
      txt=txt + "<tr>";
      xx=x[i].getElementsByTagName("TITLE");
        {
        try
          {
          txt=txt + "<td>" + xx[0].firstChild.nodeValue + "</td>";
          }
        catch (er)
          {
          txt=txt + "<td> </td>";
          }
        }
      xx=x[i].getElementsByTagName("ARTIST");
        {
        try
          {
          txt=txt + "<td>" + xx[0].firstChild.nodeValue + "</td>";
          }
        catch (er)
          {
          txt=txt + "<td> </td>";
          }
        }
      txt=txt + "</tr>";
      }
    txt=txt + "</table>";
    document.getElementById('txtCDInfo').innerHTML=txt;
    }
  }
xmlhttp.open("GET",url,true);
xmlhttp.send();
}
</script>
</head>
<body>

<div id="txtCDInfo">
<button onclick="loadXMLDoc('/example/xmle/cd_catalog.xml')">获得 CD 信息</button>
</div>

</body>
</html>

```
XML文件：
```xml
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<!--  Edited with XML Spy v2007 (http://www.altova.com)  -->
<CATALOG>
<CD>
<TITLE>Empire Burlesque</TITLE>
<ARTIST>Bob Dylan</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>Columbia</COMPANY>
<PRICE>10.90</PRICE>
<YEAR>1985</YEAR>
</CD>
<CD>
<TITLE>Hide your heart</TITLE>
<ARTIST>Bonnie Tyler</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>CBS Records</COMPANY>
<PRICE>9.90</PRICE>
<YEAR>1988</YEAR>
</CD>
<CD>
<TITLE>Greatest Hits</TITLE>
<ARTIST>Dolly Parton</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>RCA</COMPANY>
<PRICE>9.90</PRICE>
<YEAR>1982</YEAR>
</CD>
<CD>
<TITLE>Still got the blues</TITLE>
<ARTIST>Gary Moore</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Virgin records</COMPANY>
<PRICE>10.20</PRICE>
<YEAR>1990</YEAR>
</CD>
<CD>
<TITLE>Eros</TITLE>
<ARTIST>Eros Ramazzotti</ARTIST>
<COUNTRY>EU</COUNTRY>
<COMPANY>BMG</COMPANY>
<PRICE>9.90</PRICE>
<YEAR>1997</YEAR>
</CD>
<CD>
<TITLE>One night only</TITLE>
<ARTIST>Bee Gees</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Polydor</COMPANY>
<PRICE>10.90</PRICE>
<YEAR>1998</YEAR>
</CD>
<CD>
<TITLE>Sylvias Mother</TITLE>
<ARTIST>Dr.Hook</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>CBS</COMPANY>
<PRICE>8.10</PRICE>
<YEAR>1973</YEAR>
</CD>
<CD>
<TITLE>Maggie May</TITLE>
<ARTIST>Rod Stewart</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Pickwick</COMPANY>
<PRICE>8.50</PRICE>
<YEAR>1990</YEAR>
</CD>
<CD>
<TITLE>Romanza</TITLE>
<ARTIST>Andrea Bocelli</ARTIST>
<COUNTRY>EU</COUNTRY>
<COMPANY>Polydor</COMPANY>
<PRICE>10.80</PRICE>
<YEAR>1996</YEAR>
</CD>
<CD>
<TITLE>When a man loves a woman</TITLE>
<ARTIST>Percy Sledge</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>Atlantic</COMPANY>
<PRICE>8.70</PRICE>
<YEAR>1987</YEAR>
</CD>
<CD>
<TITLE>Black angel</TITLE>
<ARTIST>Savage Rose</ARTIST>
<COUNTRY>EU</COUNTRY>
<COMPANY>Mega</COMPANY>
<PRICE>10.90</PRICE>
<YEAR>1995</YEAR>
</CD>
<CD>
<TITLE>1999 Grammy Nominees</TITLE>
<ARTIST>Many</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>Grammy</COMPANY>
<PRICE>10.20</PRICE>
<YEAR>1999</YEAR>
</CD>
<CD>
<TITLE>For the good times</TITLE>
<ARTIST>Kenny Rogers</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Mucik Master</COMPANY>
<PRICE>8.70</PRICE>
<YEAR>1995</YEAR>
</CD>
<CD>
<TITLE>Big Willie style</TITLE>
<ARTIST>Will Smith</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>Columbia</COMPANY>
<PRICE>9.90</PRICE>
<YEAR>1997</YEAR>
</CD>
<CD>
<TITLE>Tupelo Honey</TITLE>
<ARTIST>Van Morrison</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Polydor</COMPANY>
<PRICE>8.20</PRICE>
<YEAR>1971</YEAR>
</CD>
<CD>
<TITLE>The very best of</TITLE>
<ARTIST>Cat Stevens</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Island</COMPANY>
<PRICE>8.90</PRICE>
<YEAR>1990</YEAR>
</CD>
<CD>
<TITLE>Stop</TITLE>
<ARTIST>Sam Brown</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>A and M</COMPANY>
<PRICE>8.90</PRICE>
<YEAR>1988</YEAR>
</CD>
<CD>
<TITLE>Bridge of Spies</TITLE>
<ARTIST>T'Pau</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Siren</COMPANY>
<PRICE>7.90</PRICE>
<YEAR>1987</YEAR>
</CD>
<CD>
<TITLE>Private Dancer</TITLE>
<ARTIST>Tina Turner</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>Capitol</COMPANY>
<PRICE>8.90</PRICE>
<YEAR>1983</YEAR>
</CD>
<CD>
<TITLE>Midt om natten</TITLE>
<ARTIST>Kim Larsen</ARTIST>
<COUNTRY>EU</COUNTRY>
<COMPANY>Medley</COMPANY>
<PRICE>7.80</PRICE>
<YEAR>1983</YEAR>
</CD>
<CD>
<TITLE>Pavarotti Gala Concert</TITLE>
<ARTIST>Luciano Pavarotti</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>DECCA</COMPANY>
<PRICE>9.90</PRICE>
<YEAR>1991</YEAR>
</CD>
<CD>
<TITLE>The dock of the bay</TITLE>
<ARTIST>Otis Redding</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>Atlantic</COMPANY>
<PRICE>7.90</PRICE>
<YEAR>1987</YEAR>
</CD>
<CD>
<TITLE>Picture book</TITLE>
<ARTIST>Simply Red</ARTIST>
<COUNTRY>EU</COUNTRY>
<COMPANY>Elektra</COMPANY>
<PRICE>7.20</PRICE>
<YEAR>1985</YEAR>
</CD>
<CD>
<TITLE>Red</TITLE>
<ARTIST>The Communards</ARTIST>
<COUNTRY>UK</COUNTRY>
<COMPANY>London</COMPANY>
<PRICE>7.80</PRICE>
<YEAR>1987</YEAR>
</CD>
<CD>
<TITLE>Unchain my heart</TITLE>
<ARTIST>Joe Cocker</ARTIST>
<COUNTRY>USA</COUNTRY>
<COMPANY>EMI</COMPANY>
<PRICE>8.20</PRICE>
<YEAR>1987</YEAR>
</CD>
</CATALOG>
```


[更多AJAX的实例](http://www.w3school.com.cn/example/ajax_examples.asp)

