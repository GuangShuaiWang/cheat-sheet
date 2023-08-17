一般常用的是Github desktop，所以对Git的命令并不是很熟悉，在这里记录下相关知识，用到的时候可以直接复制：

![git-icon](../doc_images/learn/git1.jpeg)

## Git的应用目的和理论

目的是为了更好的版本控制。GIt本地有三个工作区域：工作目录（Working Directory），暂存区（Stage/Index），资源库（Repository）。外加上远程的git仓库（Remote Directory）。他们之间的关系如下所示：

![git-workflow](../doc_images/learn/git2.png)

## git status

用于查看文件现存的状态。

Untracked：未追踪，已经新创建的文件，但没有添加到git库中，不接受版本控制，通过git add添加为Staged状态

Unmodify：未修改。文件已在git库中，没有修改，这个有两种状态可以改变，修改后会变为Modified状态；使用git rm命令会使其变为 Untracted状态。

Modified：文件已修改。通过git add命令使其进入暂存状态；或者通过git checkout变为unmodify状态。

Staged：暂存状态。git commit可以使文件同步到库中，变为unmodify的状态；通过git reset HEAD filename可以取消暂存，变为modified状态。

## 忽略文件

.gitignore文件可以使用通配符来忽略一些文件

## Git常用命令

```bash
git config -l #查看环境配置
git status #查看文件现存的状态

#推送流程(项目推送到远程仓库）

git add .
git commit -m "comment"
git push

#获得仓库
git pull 
#创建仓库，两种方法
git init #（初始化项目）
git clone [url] #克隆到远程仓库
```

## Github push
Github删除掉了通过账号密码的方式进行push，转而使用token的方式，这里需要生成token，并且在输入密码时，选择通过token进行登陆。

## 如何不再每次push时都重新输入帐号密码呢（Chatgpt）？
这里有两种方法：

1. 通过`git config --global credential.helper store`命令，该命令将 Git 凭据存储在 ~/.git-credentials 文件中,但由于时明文保存很不安全。
2. 这种方式就很麻烦了，不过安全：
    - 在复制仓库的时候，使用 SSH URL 来克隆或访问代码库，而不是使用 HTTPS URL。然后，在您的本地代码库中，运行以下命令来设置 SSH URL：“git remote set-url origin git@github.com:username/repository.git”
    - 生成ssh密钥：`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`,都选择默认即可。
    - 添加公钥到 Github，将公钥添加到您的 Github 帐户中。首先，复制公钥文件中的内容：```cat ~/.ssh/id_rsa.pub```然后，登录到您的 Github 帐户，在右上角的头像下拉菜单中选择「Settings」。在左侧菜单中选择「SSH and GPG keys」，然后单击「New SSH key」。在「Title」字段中输入一个描述，然后将公钥粘贴到「Key」字段中。单击「Add SSH key」保存。
    - 修改本地 Git 配置，打开终端，输入以下命令配置 Git：```git config --global user.email "your_email@example.com"```,```git config --global user.name "Your Name"```其中，your_email@example.com 是您的电子邮件地址，Your Name 是您的用户名。
    - 测试连接 运行以下命令测试是否可以通过 SSH 连接到 Github：```ssh -T git@github.com```如果连接成功，您应该会收到以下消息：```Hi username! You've successfully authenticated, but GitHub does not provide shell access.```现在，您应该可以在不输入账号和密码的情况下进行 Github 操作了。
    
    


