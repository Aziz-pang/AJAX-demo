# 使用node-js实现简单的AJAX demo

**AJAX:Async JavaScript And XML**

第一步：设置server.js 服务端文件  
第二步：编写 html、css、main.js等响应文件  

<br />
--- 

## 使用 node-dev 开发

直接使用`node server`开发需要不断重启，通过`node-dev`每次保存都可以自动重启 nodejs 服务器
```sh
yarn global add node-dev
#或者
npm i node-dev
```
<br />
---  

## nodejs服务器端响应设置

详细可以查看`server.js`文件
```js
if (path === '/') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/html;charset=utf-8')
        response.write(fs.readFileSync('public/index.html'))
        response.end()
    } else if (path === '/main.js') {
        response.statusCode = 200
        response.setHeader('Content-Type', 'text/javascript;charset=utf-8')
        response.write(fs.readFileSync(`public/main.js`))
        response.end()
    } else {
        response.statusCode = 404
        response.setHeader('Content-Type', 'text/html;charset=utf-8')
        response.write(`你输入的路径不存在对应的内容`)
        response.end()
    }
```
响应路径可以不同。如`/xxxmain.js`，在nodejs里指定为`public/main.js`

```html
<!-- html命名可以不一样，只要服务端的路径是正确即可 -->
<script type="text/javascript" src="xxxmain.js"></script>
```
避免有歧义，最好语义化命名
```html
<script type="text/javascript" src="/main.js"></script>
```
<br />
---  

## 请求相响应的过程

**readyState**

1. `const request = new XMLHttpRequest()` ---> 0
2. `request.open("[GET | POST]","路径")`---> 1
3. `request.send()` ---> 2
4. 开始下载 ---> 3
5. 下载完成 ---> 4

可以通过`onreadystatechange`监听`readyState`判断响应的状态以及`status`路径是否正确
```js
getXML.onclick = () => {
    const request = new XMLHttpRequest();
    request.open('GET', '/xmlDemo.xml')
    request.onreadystatechange = () => {
        if (request.readyState === 4 && request.status === 200) {
            const dom = request.responseXML
            const text = dom.getElementsByTagName('warning')[0].textContent
            getXML.textContent = text.trim()
        }else{
            console.log('加载失败')
        }
    }
    request.send()
}
```
<br />
--- 

## 总结  
1. http里可以添加：HTML、CSS、JS、XML、JSON
2. 注意设置正确的 content-Type：UTF-8

JSON:JavaScript Object Notation
JS对象标记语言，属于一个独立的语言

### JSON使用规范  

value值：`string、number、object、array、null`六种类型

string：只支持双引号  
number：支持科学计数法  
bool：true、false  
null：没有 Undefined  
object与array相差不大  

<br />

语法错误可以使用try catch捕获错误  

[MDN文档使用方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch)  

<br />
---  
end.
