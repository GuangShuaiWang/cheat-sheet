## 自动获得Steam游戏库中游戏时间

现在的问题是不知道在github action中将文件写到特定的路径里是怎么做的

找到问题了，**github action运行脚本的路径是当前仓库根目录**，但是我这里的问题是，git不会上传空文件夹，导致文件夹不存在，出现问题。修改后就解决了。

游戏库同步完成，教程之后再写，目前还存在的问题：

时间转化为小时分钟形式。再添加一些文字的描述。

## 教程

基本上是非常简单的爬虫函数，通过调用request的库来进行数据的抓取，关键的因素是有两个：1.获得setam的API Key，这个非常关键，决定是否能够访问steam提供的对应的API，2.想要查看的steam的库的个人ID，这个是一个数字ID，可以从对应用户的商店页面中，在网址栏里找到。

### 申请Steam的API key
要获得自己的Steam Web API密钥，请按照以下步骤操作：（chatgpt）

1. 转到 Steam 开发者网站：https://steamcommunity.com/dev/

2. 点击页面右上角的“登录”按钮，并使用您的 Steam 帐户登录。

3. 点击“我的帐户”按钮，然后在下拉菜单中选择“注册应用程序”。

4. 输入您的应用程序名称和描述，然后选择“Web API”作为应用程序类型。

5. 在应用程序详情页面中，您将找到“API 密钥”一栏。单击“获取新密钥”按钮，系统会自动生成一个新的API密钥。

6. 将 API 密钥保存到安全的位置，并在您的应用程序代码中使用它来访问 Steam Web API。

但是由于API key的私密性，不能明文保存在代码中，因此将其写到了Github仓库的serects里，在github action中进行调用方式如下：

```
 - name: Run script
        env : 
          API_KEY: ${{ secrets.STEAM_API }}
        run: |
          pip install requests
          pip install pandas
          pip install tabulate
          python ./script/steam.py
```

python脚本里：
```
API_KEY = os.environ["API_KEY"]
```

### 其他步骤
我所想访问的是游戏名，游戏封面图，游戏时间和最后游玩日期，根据具体的api提交request.get请求即可获得，最终将其自动的写入到markdown里面。