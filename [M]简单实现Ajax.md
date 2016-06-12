---
title: 【慕课网】Ajax 学习
tags: [慕课网][AjAX]
notebook: Ajax
---
###使用JQ的$.get的方法实现简单的交互
HTML文件
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试Ajax</title>
    <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>
</head>
<body>
    <h1>点击按钮刷新数据</h1>
    <button id="cl">点击</button>
    <div class="box">
        这是原本的信息
    </div>
</body>
<script type="text/javascript">
$(function(){
    $("#cl").click(function(event) {
        $.get('moremss.php',{'id':'1','name':'youngdee'},function(data){
            $(".box").html(data);
        });
    });
});

</script>
</html>
```
PHP文件
```php
<?php echo "hello world"; 
    echo "<pre>";
    var_dump($_GET);
    echo "</pre>";
?>
```