*2023.4.14*

## 通过MKdocs构建静态网站

### 需求
主要是构建一个可重复的文档。最开始的时候是基于Rmarkdown来构建静态网页文档的，但是在我的上一个项目TLimmuno2中，我是有大量的python代码（我后续的项目也会主要基于python），因此只能通过[reticulate](https://rstudio.github.io/reticulate/)在R中调用python。整体来说会麻烦，而且当时Rmakrdown的编译时间也较长。

那么有没有python中的文档构建包呢？肯定是有的

最开始是基于[sphinx](https://www.sphinx-doc.org/en/master/)，但是有点反人类（反我）的是，它的语法是基于[restrueturetext](https://docutils.sourceforge.io/rst.html)，要学新的语法，更麻烦。

后来在找pytorch的学习资料时，发现了一个非常好的的[文档](https://www.learnpytorch.io)，这个文档也同时可以读取jupyter notebook的信息，非常符合我的需求，经过[询问](https://github.com/mrdbourke/pytorch-deep-learning/discussions/395)了解到了[MKdocs](https://www.mkdocs.org)。

### 快速开始

这里我们主要是基于MKdocs的一个主题：[mkdocs-material](https://squidfunk.github.io/mkdocs-material/)，它会自动安装所有的依赖，包括最主要的MKdocs。

```pip install mkdocs-material```.

同样的，他也支持docker，这里我们并不会涉及。

在安装完成之后，通过```mkdocs new file-name```的方式，可以在当前文件夹，文件夹下包含两个内容 *docs/index.md*和mkdocs.yml。

其中重要的是**makdocs.yml**，它可以控制整个页面的展示效果，控制各种各样的功能，想要对网页进行定制化，则需要了解其相关的[选项](https://squidfunk.github.io/mkdocs-material/customization/)。


### Github pages
文档中给出了详细的[步骤](https://squidfunk.github.io/mkdocs-material/publishing-your-site/){:target="_blank"},主要是根据github action来进行自动的部署，需要注意的额外的内容是要根据自己的所安装的插件(plugin)来修改yml文件。

这里多写一点：直观的感觉github action会是一个非常有用且便利的方式，有机会的话可以去学习一下。

### 域名解析
在购买好域名之后，只需样相互理解解析就可以了，有详细的[教程](https://www.jianshu.com/p/d92ea8542673){:target="_blank"}。

## customization：

### 动态页面

如何去构建一个像mkdocs一样的动态的homepage？

根据作者的描述知道由于涉及到版权的问题，该部分的代码是不公布的，除非称为赞助者（insider）。但是在相应的[issue](https://github.com/squidfunk/mkdocs-material/issues/1996)中，已经有人提供了解决方案，就是在index.md文件中指定我们想要的html的template，即：

```
---
title: Home
template: home.html
---
```

而其中的home.html应该在material/override路径下（该路径应该在mkdocs.yml中指定）

因为这里需要的Html&css知识太多了，我是无法处理的，所以就基于现有的[项目](https://github.com/binbashar/le-ref-architecture-doc)来进行更改，这样不仅简便而且也好看。


### 更改图标
图标更改需要放到docs的assets文件夹下，并且要在mkdocs.yml中指定路径。图标绘制可以选择在线网站，例如[favicon.io](https://favicon.io)。

### mkdocs.yml中的feature元素
该元素是对页面的展示进行定制化调整，详细的[内容](https://squidfunk.github.io/mkdocs-material/blog/2021/12/27/the-past-present-and-future/?h=feature#features){:target="_blank"}在这里.

### change log的构建

好像可以自动同步每次提交的变化，

## 踩坑

### 报错信息：
`“ A relative path to 'MKdoc.md' is included in the 'nav' configuration, which is not found in the documentation file：“`

产生的原因是名称的问题，将名字改为123.md之后，就不存在这个问题了，可能是mkdoc.md的名称是被保留的，使用的话就会报错。

并不是上面的原因，重新修改其他的名称之后，只要包含字母就出错，原因？

好像确实是上面的原因，要完全避免MKdocs元素的存在，就很奇怪。


### 还有一个问题：table of content不显示
解答：https://github.com/squidfunk/mkdocs-material/issues/818#issuecomment-629646709

主要原因就是mkdocs只从h2标题开始抓取制作table of content，因此在使用的时候，h1标题只使用一个，后续多用h2的标题。

### github pages错误
最开始直接成功构建了页面，但是在第二次提交之后，页面构建失败了，
可能原因是
1.没有等待一会
2.代码内容有错误。

发现好像是safri的问题，自动把pages同步到我的域名，而我的域名被重新提交仓库导致取消，然后就404了。

重新提交仓库之后，自定义的域名取消了，如何自动同步呢？

通过询问chtagpt找到了解决方案，在Github action中添加相应的代码，让每次workflow中都自定于到特定域名上：
```
- name: Deploy to Custom Domain
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.PERSONAL_TOKEN }} #调用token
          external_repository: GuangShuaiWang/personal-website
          publish_dir: ./site
          cname: wanggsh.cn
          target_branch: gh-pages
          commit_message: 'Deploy to custom domain'
```

其中的secrets.PERSONAL_TOKEN要在github的主页Settings/Developer settings上生成，并将生成的token放在对应仓库的setting/secrets and variables里，名称为PERSONAL_TOKEN。
