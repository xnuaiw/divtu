## 服务端创建操作流程

1.将本项目fork至自己仓库修改`Deploy to Heroku`按键指向地址为自己仓库地址

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/xnuaiw/divtu) 

2.点击上面紫色`Deploy to Heroku`，会跳转到heroku app创建页面，填上应用程序名、选择节点（美国或者欧洲）、自定义UUID码、自定义CADDYIndexPage（参考：[Caddy主页配置](https://github.com/xnuaiw/divtu/blob/master/README.md#5caddy%E4%B8%BB%E9%A1%B5%E9%85%8D%E7%BD%AE))，其他建议保持默认，点击下面deploy，几秒后搞定！   

3.若出现`We couldn't deploy your app because the source code violates the Salesforce Acceptable Use and External-Facing Services Policy.`提示，则返回仓库，>`Setting`>`Repository name`修改仓库名。

4.然后修改app.json文件中的`name（不填写也能创建，名字会由heroku随机生成）`、`description`值，注意`repository`必须留空以免项目被禁

5.再修改`Deploy to Heroku`按键指向地址为自己仓库地址，重复`2`的操作

6.带有删除线的部分表示已经废弃或不适用



### CloudFlare Workers反代代码（支持VLESS\VMESS\Trojan-Go的WS模式，可分别用两个账号的应用程序名（UUID与path保持一致），单双号天分别执行，那一个月就有550+550小时）

1.适用单一应用的反代代码

```
addEventListener(
  "fetch", event => {
    let url = new URL(event.request.url);
    url.host = "appname.herokuapp.com";
    let request = new Request(url, event.request);
    event.respondWith(
      fetch(request)
    )
  }
)
```

2.适用单双日循环执行的反代代码

```
const SingleDay = '应用程序名1.herokuapp.com'
const DoubleDay = '应用程序名2.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
