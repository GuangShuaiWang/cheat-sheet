## Neptune

在训练一个神经网络模型（炼丹）的时候，最常用的步骤如何对自己loss和acc等参数进行可视化，因此会有大量重复的代码进行这部分工作。但同时还存在的一个问题是每次实验定义的超参数，和对应的结果都需要进行记录，不然无法对流程进行复现，也不能理解为什么当时要这么做。那么就需要一个实验管理工具来能解决相应的问题。

目前市面上也有很多的实验管理工具，比如[Tensorboard](https://www.tensorflow.org/tensorboard?hl=zh-cn)，[Neptune](https://neptune.ai)和[Wandb](https://wandb.ai/site)，他们各有不同，这里就主要介绍neptuen了，因为我自己直观的感觉他的界面最舒服。neptune和wandb这类工具好用的原因是它们不需要对原始的模型代码进行大刀阔斧的修改，只需要在导入包之后，然后再特定的位置添加代码指定想记录的参数即可。

图片路径（../doc_images/neptune）

### 简介

Neptune是一个服务于机器学习研究人员的元数据存贮仓库，可以用来追踪实验流程（如记录一些必要的参数）和保存自己的模型。我们可以通过Neptune记录，追踪，展示和对比某一个项目中的全部数据。

### 安装与注册
Neptune提供了两种方式：pip和conda

```pip install neptune```

```conda install -c conda-forge neptune```

但由于Neptune属于线上应用，需要将训练结果同步到云端，因此需要注册账号，并且得到个人的API Token令牌。

可以直接从他的[主页](https://neptune.ai/register)通过邮箱进行注册,注册完成之后可以在左下角的个人菜单中找到**Get Your API Token**获得私人的令牌。

![](https://docs.neptune.ai/img/app/get_api_token.png)

由于该令牌是提交项目的关键凭证，因此最好不要显式的写在之后可能公开的代码中，linux和mac系统可以将以下命令写在`.profile`文件里：

```
export NEPTUNE_API_TOKEN="uyVrZXkiOiIzNTd..."
```

其他的系统的设置方式可以查看文档[链接](https://docs.neptune.ai/setup/setting_api_token/)。

除了APItoken之外，Neptune提交数据时还需要指定你的项目名称（**project name**），这个在网页上创建仓库时即可获得:

![project_name](https://docs.neptune.ai/img/app/tutorial/create_new_project.png)

其中的**Project_key**会用于一个项目不同实验的命名，**Private**会指定为隐私仓库

再设置好这些前置因素之后，可以进行自己的后续的操作了。

### 快速开始
这里主要分为两步：代码层面和线上网页层面的操作。

**首先介绍的是代码层面的操作：**

1.可以通过几行简单的代码来导入和初始化neptune
```python
import neptune
neptune.init_run(
    project="workspace-name/project-name",
    api_token="Your Neptune API token here",
)
```

2.记录的超参数
```python
run["algorithm"] = "ConvNet"
params = {
    "activation": "sigmoid",
    "dropout": 0.25,
    "learning_rate": 0.1,
    "n_epochs": 100,
}
run["model/parameters"] = params #保存在model的parameters路径下
#更新和添加新的参数
run["model/parameters/activation"] = "ReLU"
run["model/parameters/batch_size"] = 64
```
3.监控指标的变化, 要将该流程写在循环里，append方法可以记录参数的变化。
```python
for epoch in range(params["n_epochs"]):
    # this would normally be your training loop
    # fake code
    run["train/loss"].append(0.99**epoch)
    run["train/acc"].append(1.01**epoch)
    run["eval/loss"].append(0.98**epoch)
    run["eval/acc"].append(1.02**epoch)

```
4.（可选）可以上传自己的模型文件或其他的文件，可以通过upload函数上次任何文件。
```python
run["data_versions/train"].track_files("sample.csv")
run["data_sample"].upload("sample.csv")
run["f1_score"] = 0.95
```
5.结束
```python
run.stop()
```

**网页层面**

经过上面的操作之后，可以在网页上看到我们所定义的超参数：
![](https://docs.neptune.ai/img/app/tutorial/all_metadata.png)

同时，通过append函数监控的准确率变化可以在**chat**选项卡中找到：
![](https://docs.neptune.ai/img/app/tutorial/charts.png)

以上的内容就简单介绍了如何使用neptune，可以通过官方文档来进行更细致的了解。

### jupyter扩展

可以通过jupyter的扩展软件`neptune-notebooks`直接在jupyter notebook和lab中上传当前笔记本到仓库中。

**bug：目前的扩展不能支持`Jupyter lab==4.0`的版本**

### Reference
[neptune官方文档](https://docs.neptune.ai)