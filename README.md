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

（CORS背后的思想，就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。）

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

