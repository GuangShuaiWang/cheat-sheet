## Tensorboard的介绍与在pytorch下的使用

### 简要介绍
Tensorboard原本是基于Tensorflow的可视化工具，但从pytorch1.2开始，它也支持Tensorboard的。它被用来理解。调试和优化训练过程中的模型和数据，它提供了一个直观的web界面，可以查看训练过程中的各种统计信息，图表，摘要和可视化结果。它主要有以下主要的功能：
1. 可视化模型图：Tensorboard可以可视化模型的计算图，用来查看模型的层级结构，张量流动路径和各个节点之间的依赖关系。
2. 监控训练指标：Tensorboard可以显示训练过程中的各种指标，比如损失函数，准确率，学习率等，可以根据训练的时间查看指标的变化，以及对不同模型和超参数之间进行比较。
3. 可视化张量数据：Tensorboard可以显示图像，音频和文本类型的张量数据，让你更直观的了解输入和输出的结果。
4. 显示直方图和分布：TensorBoard 提供了直方图和分布图，用于查看张量的值分布情况。这对于了解权重和梯度的分布、观察它们的变化以及检测潜在的问题非常有帮助。
5. 可视化嵌入向量：可以在高维空间中理解嵌入向量，了解他们之间的相似性关系。

### 安装
```shell
conda install tensorboard
```
通过conda进行安装是最省心的方式

### 最直白的使用
pytorch中使用Tensorboard只需要导入`SummaryWriter`函数即可，它将想要保存的数据以特定的格式写入特定的文件夹，再通过命令行命令读去该文件夹通过web显示。
```python
from torch.utils.tensorboard import SummaryWriter
write = SummaryWriter("./logs")
writer.add_scalar(tag,scalar_value,global_step)
```

```shell
tensorboard --logdir=./logs --port 10003 --host=ip_adress
```
### 常用的参数overview
官方文档里写了比较详细的内容，介绍了常用的[函数](https://pytorch.org/docs/stable/tensorboard.html)

`SummaryWriter` 类的 `add_` 方法用于将各种摘要数据（summaries）写入到 TensorBoard。以下是 `add_` 方法中常用的参数及其用途：

1. `add_scalar(tag, scalar_value, global_step=None, walltime=None)`
   用于添加标量数据（scalar）。可以使用该方法记录训练过程中的损失、准确率等指标。`tag` 是标签名称，`scalar_value` 是要记录的标量值，`global_step` 是可选的全局步数，用于指定记录的步骤，`walltime` 是可选的时间戳，用于指定记录的时间。

2. `add_image(tag, img_tensor, global_step=None, walltime=None, dataformats='CHW')`
   用于添加图像数据。可以使用该方法记录输入数据、模型输出等图像数据。`tag` 是标签名称，`img_tensor` 是图像张量，`global_step` 和 `walltime` 是可选的，用于指定记录的步骤和时间，`dataformats` 是可选的，用于指定图像的数据格式。

3. `add_figure(tag, figure, global_step=None, close=True, walltime=None)`
   用于添加图形数据。可以使用该方法将 Matplotlib 的图形对象添加到 TensorBoard。`tag` 是标签名称，`figure` 是 Matplotlib 的图形对象，`global_step`、`close` 和 `walltime` 是可选的，用于指定记录的步骤、是否关闭图形对象以及记录的时间。

4. `add_histogram(tag, values, global_step=None, bins='tensorflow', walltime=None)`
   用于添加直方图数据。可以使用该方法记录权重、梯度等数据的分布情况。`tag` 是标签名称，`values` 是要记录的数值数据，`global_step`、`bins` 和 `walltime` 是可选的，用于指定记录的步骤、直方图的分箱方式和记录的时间。

5. `add_embedding(mat, metadata=None, label_img=None, global_step=None, tag='default', metadata_header=None)`
   用于添加嵌入向量（embedding）数据。可以使用该方法可视化高维嵌入向量的聚类和相似性。`mat` 是嵌入向量的数据矩阵，`metadata` 是可选的元数据，`label_img` 是可选的图像数据，`global_step` 和 `tag` 是可选的，用于指定记录的步骤和标签名称，`metadata_header` 是可选的，用于指定元数据的标题。

**举两个例子**

对模型和标量进行可视化

### 可视化计算图
构建了一个最简单的模型来显示模型图
```python
from torch.utils.tensorboard import SummaryWriter
import torch
import torch.nn as nn

# 创建一个 SummaryWriter 对象，指定日志保存的目录
writer = SummaryWriter('logs')

# 创建一个模型
class MyModel(nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        self.fc = nn.Linear(10, 1)

    def forward(self, x):
        return self.fc(x)

model = MyModel()

# 将模型图写入 TensorBoard
dummy_input = torch.randn(1, 10)  # 创建一个虚拟输入
writer.add_graph(model, dummy_input)

# 关闭 SummaryWriter
writer.close()
```

<img src="../doc_images/learn/tensorboard-1.png" alt="git-workflow" style="width:50%;height:auto;">

### 训练过程的可视化
这一步应该是最常用的，可以查看想监控的数值，比如损失函数，准确率等数值的变化

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

# 定义一个简单的神经网络模型
class SimpleModel(nn.Module):
    def __init__(self):
        super(SimpleModel, self).__init__()
        self.fc = nn.Linear(10, 1)

    def forward(self, x):
        return self.fc(x)

# 创建一个 SummaryWriter 对象，指定日志保存的目录
writer = SummaryWriter('logs')
train_data = torch.randn(100, 10)
train_labels = torch.randn(100, 1)
model = SimpleModel()
criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.01)


epochs = 100
for epoch in range(epochs):
    outputs = model(train_data)
    loss = criterion(outputs, train_labels)
    # 反向传播和优化
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    # 记录训练过程中的损失函数，并提供唯一的步数标识
    writer.add_scalar('Loss', loss.item(), global_step=epoch)

writer.close()
```

可以在tensorboard的可视化界面中看到loss的变化曲线。

<img src="../doc_images/learn/tensorboard_loss.png" alt="git-workflow" style="width: 50%;height: auto;">

### Reference
[csdn_bolg](https://blog.csdn.net/qq_41573860/article/details/106674370)

[blog2](https://towardsdatascience.com/a-complete-guide-to-using-tensorboard-with-pytorch-53cb2301e8c3)

[官方文档](https://pytorch.org/docs/stable/tensorboard.html)

[chatgpt](https://chat.openai.com)