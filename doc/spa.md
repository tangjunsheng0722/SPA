# SPA
## SPA 概念
> single-page application是一种特殊的Web应用。它将所有的活动局限于一个Web页面中，仅在该Web页面初始化时加载相应的HTML、JavaScript、CSS。一旦页面加载完成，SPA不会因为用户的操作而进行页面的重新加载或跳转，而是利用JavaScript动态的变换HTML（采用的是div切换显示和隐藏），从而实现UI与用户的交互。
## SPA 优点
* 由于避免了页面的重新加载，SPA可以提供较为流畅的用户体验。得益于Ajax，可以实现无跳转刷新，由于与浏览器的history机制，可以使用hash的b变化从而可以实现推动界面变化。
* 只要使用支持HTML5和CSS3的浏览器就可以执行复杂的SPA,因此，开发人员不必为了写SPA网站而特别学习另一个开发方式，而使用者也不额外安装软件，所以，让开发SPA网页程序的入门和使用门槛降低不少。
## SPA 缺点
* 以SPA方式开发的网站不容易管理也不够安全。
* 因为没了一页一页的网页给搜索引擎的爬虫来爬，所以，在搜索引擎最佳化（SEO）的工作上，需要花费额外的功夫。
* 因为没有换页，需要自定义状态来取代传统网页程序以网址来做判断
## 实现 SPA
* 处理#后面的字符
* 局部刷新
> location 对象属性
* hash 设置或返回#开始的URL锚
* host 设置或返回主机名和当前的URL端口号
* hostname 设置或返回主机名
* href 设置或返回完整的URL
* pathname 设置或返回当前的URL路径部分
* port 设置或返回当前的URL端口号
* protocol 设置或返回当前URL的协议
* search 设置或返回？开始的URL查询部分
> 获取#后面的URL锚
```
var hash = location.hash;
```
> 实现
* HTML 代码
```
<div class="pageview" style="background: #3b76c0" id="main-view">
    <h3>首页</h3>
    <div title="-list-view" class="right-arrow"></div>
</div>
<div class="pageview" style="background: #58c03b;display: none" id="list-view">
    <h3>列表页面</h3>
    <div class="left-arrow"></div>
    <div title="-detail-view" class="right-arrow"></div>
</div>
<div class="pageview" style="background: #c03b25;display: none" id="detail-view">
    <h3>列表详情页面</h3>
    <div class="left-arrow"></div>
```
* JS 代码
```
<script>
        var states ;
        var currentState;
        $(document).ready(function() {
            states = registState();
            currentState = init();
            //监听hash路由变化
            window.addEventListener("hashchange", function() {
                var nextState;
                console.log(window.location.hash);
                //判断地址是否为空，若为空，则默认到main-view页面
                if (window.location.hash == "") {
                    nextState = "main-view";
                }
                else {
                    //若不为空，则获取hash路由信息，得到下一个状态
                    nextState = window.location.hash.substring(1);
                }
                //判断当前状态是否注册过(是有存在这个状态)0g
                var validState = checkState(states, nextState);
                //若不存在，则返回当前状态
                if (!validState) {
                    console.log("you enter the false validState");
                    window.location.hash = "#" + currentState;
                    return;
                }
                $('#'+ currentState).css("display", "none");
                $('#'+ nextState).css("display", "block");
                currentState = nextState;
            })

        })
        //状态注册
        function registState() {
            var states = [];
            //状态注册
            $(".pageview").map(function() {
                return states.push($(this)[0].id);
            })
            return states;
        }
        //初始化，对用户一开始输入的url进行处理
        function init() {
            var currentState = window.location.hash.substring(1);
            if (currentState == "") {
                currentState = "main-view";
            }
            if (currentState != "main-view") {
                $('#main-view').css("display", "none");
                $('#'+ currentState).css("display", "block");
            }
            return currentState;
        }
        //判断状态是否存在
        function checkState(states, nextState) {
            var tof = false;
            states.forEach(function(element) {
                if (element == nextState) {
                    tof = true;
                }
            })
            return tof;
        }
    </script>
```
> 修改URL后#锚点 切换页面(需要引入JQ)