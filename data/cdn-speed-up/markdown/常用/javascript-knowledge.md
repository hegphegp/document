### 1 JavaScirpt知识点

#### 1.1 页面加载完html后，马上执行Jquery函数
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
</head>
<body>
</body>
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
<script type="text/javascript">
    (function() {
        alert('jQuery版本：' + $.fn.jquery);
    })(jQuery)
</script>
```


#### 1.2 页面加载完html后，马上加载layui库
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
</head>
<body>
</body>
<script src="https://www.layuicdn.com/layui-v2.5.4/layui.all.js"></script>
<script type="text/javascript">
    (function() {
        var layer = layui.layer;
        layer.msg('Hello World');
    })()
</script>
```

#### 1.2 使用layui库内嵌的jQuery
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
</head>
<body>
</body>
<script src="https://www.layuicdn.com/layui-v2.5.4/layui.all.js"></script>
<script type="text/javascript">
    (function() {
        var $ = layui.$ //重点处
        alert('jQuery版本：' + $.fn.jquery);
    })()
</script>
```