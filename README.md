# cross-domain
跨域技术

由于同源策略的限制，XmlHttpRequest 只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，
记录了几种跨域解决方案。

##jsonp
原理：动态插入script标签，通过script标签引入一个js文件，
这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。
优点：兼容性好，简单易用，支持浏览器与服务器双向通信。
缺点：只支持GET请求，服务端需要修改代码，添加回调函数callback。

##CORS
Xhr level2支持的新标准，允许发起ajax跨域请求，但是为了跨域安全，需要在服务器响应头部提供一些信息，供浏览器校验请求是否被允许。
服务端对于CORS的支持，主要是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域访问。

 res.setHeader('Access-Control-Allow-Origin', 'b.test.com');  
 //如果ajax发起的不是简单请求（不是get请求或设置了自定义头的请求为非简单请求），则还需设置以下信息，此时会先发起option请求，如果
 服务器响应的三条信息（请求域，请求方法，请求自定义信息）都正确，才会发起你真正的指定的请求
 res.setHeader('Access-Control-Allow-Methods', 'GET,POST,PUT,DELETE');  
 res.setHeader('Access-Control-Allow-Headers', 'test,other');  
 
function crossAjax() {  
      $.ajax('http://b.test.com/test', {  
          type: 'PUT',  
          headers: {test: 'haha'}  
      }).done(function(data) {  
        alert(data);  
      });
}

本文旨在加深对前端知识点的理解，资料来源于网络，由本人(博客：http://segmentfault.com/u/trigkit4) 收集整理。

一些开放性题目

1.自我介绍：除了基本个人信息以外，面试官更想听的是你与众不同的地方和你的优势。

2.项目介绍

3.如何看待前端开发？

4.平时是如何学习前端开发的？

5.未来三到五年的规划是怎样的？


position的值， relative和absolute分别是相对于谁进行定位的？

absolute :生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。

fixed （老IE不支持）生成绝对定位的元素，通常相对于浏览器窗口或 frame 进行定位。

relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。

static 默认值。没有定位，元素出现在正常的流中

sticky 生成粘性定位的元素，容器的位置根据正常文档流计算得出



如何解决跨域问题

JSONP：
原理是：动态插入script标签，通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。

由于同源策略的限制，XmlHttpRequest只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，可以通过script标签实现跨域请求，然后在服务端输出JSON数据并执行回调函数，从而解决了跨域的数据请求。

优点是兼容性好，简单易用，支持浏览器与服务器双向通信。缺点是只支持GET请求。

JSONP：json+padding（内填充），顾名思义，就是把JSON填充到一个盒子里

<script>
    function createJs(sUrl){

        var oScript = document.createElement('script');
        oScript.type = 'text/javascript';
        oScript.src = sUrl;
        document.getElementsByTagName('head')[0].appendChild(oScript);
    }

    createJs('jsonp.js');

    box({
       'name': 'test'
    });

    function box(json){
        alert(json.name);
    }
</script>
