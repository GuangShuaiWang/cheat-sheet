## 记录一些常用的知识点

### 小命令
1. 增加运行权限 chmod +x file.sh
2. pip镜像：
    - 中国科学技术大学 : https://pypi.mirrors.ustc.edu.cn/simple
    - 豆瓣：http://pypi.douban.com/simple/
    - 阿里云：http://mirrors.aliyun.com/pypi/simple/
    - 清华：https://pypi.tuna.tsinghua.edu.cn/simple
3. corn可以定时执行脚本，```crontab -e```来启动，数据格式为```0 0 * * * /path/to/your/script.sh```,前五个通配符分别为min，hour，days，month，year。
4. shell脚本中，可以用date命令来获得时间，date +%m/%d/%Y 输出的是04/18/2023。
5. bashrc中的前缀显示可以按照自己的需求更改，主要改PS1参数，示例：```expert PS1='\[\033[42m\]\[\033[1;37m\][\d \t]\[\033[0m\] \[\033[1;30m\]\u\[\033[0m\]:\[\033[1;34m\]\w\[\033[0m\]\$ '```
6. chatgpt网址：[chatgpt](https://chat.openai.com){:target="_blank"}
7. torch.manual_seed(3407) is all you need?
9. 表格图片在线制作：https://www.canva.cn。 目前好像比不好用
10. axs.flatten() 拉平轴线
11. next()函数在处理迭代器或可迭代对象时非常有用。它可以让我们逐个访问元素，而不需要显式地使用for循环或将所有元素加载到内存中。它特别适用于处理大型数据集或需要按需获取数据的情况。
12. iter()函数的工作原理如下：如果iterable本身已经是一个迭代器对象，iter()函数会直接返回它自己，否则，它会调用可迭代对象的__iter__()方法，返回一个新的迭代器对象。
13. torch.randint(start,end,size) 其中size是输出的张量形状；squeeze是压缩向量，降低纬度，相反的操作是upsqueeze。
14. batch_normalization 是样本之间进行标准化，layer_normalization是样本自身进行标准化
15. tensor.view()可以改变张量形状,其中的-1表示有计算觉得维度，.transpose()转置，可以交换维度
16. scores.masked_fill(mask == 0, float('-inf')) 将score中为0的数值，替换为负无穷，经过softmax会变为0.
17. Transformer详细的梳理[文章](http://nlp.seas.harvard.edu/2018/04/03/attention)
18. torch中只要是模型运行，就一定是调用forward方法，输入要符合forward结构
19. Dropout的原理是将某些神经原失活，其实现方式是将输出的某些位置的向量变成0.
20. Jupyter中增加kernel：`Ipython kernel install --user --name=kernel_name`. `--user`表示当前用户，`--name`制定安装的名称。
21. `jupyter-lab --no-browser --ip "" --port 00000` 固定IP和端口，提前设置密码（jupyter server password）
22. MPI和OpenMPI是用于高性能服务器之间进行信息交互的。
23. `hostname -I` 可以显示当前服务器的IP.
24. salloc命令交互模式申请资源`salloc --job-name=test --nodes=1 --gres=gpu:2 --partition=AI4Molecule --quotatype=reserved`，`srun --jobid=<jobid> --pty bash`进入申请的资源。
25. tqdm是对循环写成进度条的形式
26. 神经网络可视化的工具：tensorboard，npitune,wandb。太多了不知道用哪个，npitune吧，其扩展包`neptune-notebooks`在Jupyter lab4.0版本不能用。
27. python中如何获得环境变量：`os.environ["ENV_VALUE_Name"]`
28. hugging Face 模型保存网站
29. forword会再模型实例化之后，确定输入就会被调用。
30. g++和gcc安装之后，需要通过软连接放在具体环境变量的bin中

