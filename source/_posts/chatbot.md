---

title: 程序设计II大作业--chatbot
comments: true
toc: true
categories:
  - CSII
tags:
  - chatbot
abbrlink: 156324
cover: https://user-images.githubusercontent.com/74918703/155833471-10c2dd15-9579-4516-a86b-539be619066d.png
date: 2022-5-17 23:12:20
---

# 程序设计II大作业--chatbot设计

  

## 信息爬取

本部分通过网络爬虫实现了实时信息的获取，比如查询每日各个省份疫情的情况以及天气情况，微博热点新闻。在输入端输入对应的查询请求后，聊天机器人便可以伪装成浏览器访问对应的网站爬取数据并挑选出需要的数据并返回。

### 新冠疫情

聊天人可以识别与“新冠疫情”相关的问题，自动爬取各个省份今日新冠疫情的新增情况，具体实现功能如下：

- 查询省份：可以对某个省份的疫情状况（新增确诊和新增无症状）
- 点击右下方链接即可跳转到相关网站查询详情
- 通过echarts柱状图显示各个省份的确诊情况，支持拖动和缩放

```python
#alist 存储城市名，新增确诊和新增无症状
for i in citylist:
		if (i['today']['wzz_add'] == 0):
          clist.append([i['name'],i['today']['confirm'],0])
     else:
        clist.append([i['name'], i['today']['confirm'], i['today']['wzz_add']])
```

### 今日热搜

聊天机器人可以识别与“今日热搜”相关的问题，自动爬取当前的微博热搜，具体功能如下：

- 返回当前热搜中热度最高的新闻资讯
- 点击热搜内容的文本即可跳转至热搜的链接了解详情

```python
response = requests.get("https://weibo.com/ajax/side/hotSearch")
data_json = response.json()['data']['realtime']
for data_item in data_json:
    dic = {#微博词条的网址为'https://s.weibo.com/weibo?q=%23' + 词条名称 + '%23'
        'title': data_item['note'],
        'url': 'https://s.weibo.com/weibo?q=%23' + data_item['word'] + '%23',
        'num': data_item['num']
         }
```



### 天气查询

聊天机器人可以识别与“今日天气”相关的问题，并且根据输入的城市爬取该城市的当日天气的详细情况。

```python
#将输入的汉字用Pinyin转化为拼音字符串
city_pinyin=p.get_pinyin(sentence,'')
#通过'https://www.tianqi.com/'+城市的拼音名即可访问目的城市天气
try:
    url = 'https://www.tianqi.com/'+city_pinyin
    r = urllib.request.Request(url=url, headers=headers)
except urllib.error.URLError as e:#查找失败
     return "抱歉哈，找不到你所输入的城市"
#查找成功，利用beautifulsoup依次查找要找出的信息
weather.append(soup.select(...)[0].text)
```



## web前端

聊天机器人的前端效果如下所示：

![s1](E:/2022spring/CSii/chatbot/bot_mywork/report_img/s1.png)

### 输入框

每次按下按钮'发送'，且输入框不为空时，即可向机器人发送一条数据。

```html
<div class="b-footer">
		<input type="text" name="text" id="f-left" placeholder="输入你想说的话..."/>
		<div id="btn">发送</div>
</div>
```

### 对话框

- 发送完信息后，在对话框弹出该条文本，在机器人正在生成回答时显示加载动画

 ```js
 $(".b-body").append("<div class='mWord'><span><img  src=\"p1.png\" /></span><p>" + text.val() + "</p></div>");
 //在对话框'b-body'中将输入信息以mWord的格式添加新的Div标签，内容包含输入的文本信息和头像图片
 	$(".b-body").append("<div class='wait'><span><img  src=\"p2.png\" /></span> <p></p><div class='Ball'></div></div>");
 //显示加载动画
 	$(".b-body").scrollTop(10000000);
 //将对话界面拉到页面最下方显示最新对话
 ```

- 机器人将回复信息返回前端

```js
function(result)
{
		$('.wait').remove();//删除加载动画标签
		if(result[0]=='<')//返回的内容为可以点击跳转的链接格式
		$(".b-body").append("<div class='rotWord'><span><img src=\"p.png\"/></span> " + result + "</div>");
		else//返回的内容为普通文本
		$(".b-body").append("<div class='rotWord'><span><img src=\p.png\" /></span> <a id='member'>" + result + "</a></div>");
		$(".b-body").scrollTop(10000000);
}
```

- 时间信息

每隔一分钟显示一次当前时间信息，如图所示：

<img src="E:/2022spring/CSii/chatbot/bot_mywork/report_img/s3.png" alt="s3" style="zoom:47%;" />

```js
function setDate(){
	d = new Date();
	if (m != d.getMinutes()) 
 	 {//m为已记录的时间信息，在对话时每分钟更新m，在对话框中显示当前时间
	  m = d.getMinutes();
	  $(".b-body").append('<div class="timestamp">' +'-------'+ d.getHours() + ':' + m +'-------'+ '</div>');}
}
```

### CSS渲染

- 静态渲染：
  - 背景和文本框的渐变色
  - 边框圆角效果
  - a对话框半透明效果
  - 阴影效果

```css
background: -webkit-linear-gradient(0, #xxx 0, ..., #xxx 100%);/*渐变效果*/
color: rgba(55, 55, 55, 0.3);/*0.3为透明度*/
border-radius: 15px;/*圆角效果*/
box-shadow: 10px -5px 20px 10px #xxx;/*阴影效果*/
```

- 动态渲染

  - 加载动画,循环播放三个跳动的点

  <img src="E:/2022spring/CSii/chatbot/bot_mywork/report_img/s2.png" alt="s3" style="zoom:47%;" />

  ```css
  .dot {
  		/*基本属性*/
  		animation: pulse-outer 7.5s infinite ease-in-out;/*动画形式*/
  	  }
  .dot:nth-child(2) {/*第二个点*/
  		left: 65px;/*偏移*/
  		animation: pulse-inner 7.5s infinite ease-in-out;
  		animation-delay: 0.2s;/*动画延迟*/
   }
  /*第三个点...*/
  @keyframes pulse-outer {
  		0% { transform: scale(1); }
  		...
  		100% { height: 17.5px; }
  	  }
  ```

  - 新消息弹出动画：在产生新对话时，对新弹出的文本框进行一次缩放，实现弹出的效果

  ```css
  .Word{
  		transform: scale(0);
  		transform-origin: 0 0;
  		animation: bounce 350ms linear both;
  	}
   @keyframes bounce { 
  	0%{transform:matrix3d(0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1); }
      ...
  	100%{transform:matrix3d(1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1); } 
   }
  ```

  - 动态LOGO：参考了[Dribbble](https://dribbble.com/shots/10133153-Chatbot)的模型

  <img src="E:/2022spring/CSii/chatbot/bot_mywork/report_img/s4.png" alt="s3" style="zoom:47%;" />

### 其他

- echarts柱状图

  - 利用了echarts模型，当询问新冠疫情的信息时，可以弹出如图所示柱状图，更直观地观察数据

  <img src="E:/2022spring/CSii/chatbot/bot_mywork/report_img/s5.png" alt="s3" style="zoom:50%;" />

- 动态跳转按钮

  - 对于一些实时信息的查询，为了方便进一步了解详情，添加可以跳转的按钮

  <img src="E:/2022spring/CSii/chatbot/bot_mywork/report_img/s6.png" alt="s3" style="zoom:50%;" />

  ```js
  $("#btn2").click(function()
  	{
  		if(is_ask==1)//正在查询信息
  			window.location.href="https://xxx";//在新标签页打开链接
  		else
  			location.reload([bForceGet]); //停留在原页面
  	});
  ```

  

## 前后端交互

聊天机器人前后端的信息交互和同步部分是通过Flask框架和Ajax（异步JavaScript和XML）实现的。数据交互的基本模式如下：

Javascript部分:

```javascript
 $("#btn").click(function()
  {
	  chatbot();
  });
//每次检测到"发送"按钮按下且输入不为空时触发
function(){
	var args= {url: "url",//与python函数对应的"URL"相同
			type: "GET",//"GET"或"POST"
              //"GET"用于后端向前端发送信息,"POST"用于将输入的文字传到后端
			success:function(result)
			{options();}}
	$.ajax(args);
}
```

python部分：

```python
@app.route('/url')
def get_data():
    data=func()
    return data

@app.route('/')
@app.route('/index')
def index():
    return render_template('index.html')
    //默认显示的主页
```

当用户在前端输入想要对机器人说的话，便可利用JS中的Ajax工具向后端传输数据，并获取后端发的数据返回到聊天框中，这样就完成了一组对话；另外，对于新冠疫情确诊柱状图的更新，也是通过Ajax将爬取到的数据传入JS的函数中完成的。















