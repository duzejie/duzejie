# 使用GitHub Pages + docsify快速搭建一个站点

话不多说，先看效果： [https://bytesfly.github.io/blog](https://bytesfly.github.io/blog)

![](attachments/github-page-docsify/2749183c7109d75883ce29fa33b84fa0_MD5.png)

![](attachments/github-page-docsify/54ebbf3b80b894133e76297399f502af_MD5.png)

## 为什么需要一个站点



肯定有人会问，既然有类似 [博客园](https://www.cnblogs.com/) 这样优秀的平台来写博客，为什么还需要自己搭建站点呢？

- 放在`GitHub`上托管，可以使用`Git`追踪博客内容的变更，就像维护代码一样，更加清晰明了，数据也不会丢失。
- 大多优秀的开源项目，官方文档也很正式，如果用博客园来写貌似有点不合适，此时就需要一个独立的官方文档站点。
- 如果你想免费搭建属于自己的个人站点，甚至用于一个公司、组织的官网，`GitHub Pages`也是一个不错的选择。

当然，对于我来说，我虽然会选择搭建一个属于自己的个人站点来记录成长的点滴，但同样还是会继续使用博客园。 因为，社区的力量很重要。  

> 博客园的使命是帮助开发者用代码改变世界。

这里，再次感谢博客园团队不忘初心，专注于为开发者打造一个纯净的技术交流社区，推动并帮助开发者通过互联网分享知识，从而让更多开发者从中受益。  



## 快速搭建



快速搭建非常简单，这里假定你已经有了`GitHub`账号，没有的话，注册一下。

- 第一步：`Fork`我的当前博客仓库，即 [https://github.com/bytesfly/blog](https://github.com/bytesfly/blog)

![](attachments/github-page-docsify/385fc68cf470e2ff7e024cf02b1b0ba4_MD5.png)

- 第二步：在刚`Fork`的仓库设置(`Settings`)页面开启`GitHub Pages`功能

![](attachments/github-page-docsify/5735d9433a27c97dbdd24eadf6e76c1a_MD5.png)

然后，你就可以打开`https://<yourname>.github.io/blog`看看效果了。接下来，不用我说了吧，`clone`自己的`blog`仓库，在本地修改你的相关信息，添加你的博客文章，`push`到`GitHub`，刷新页面(浏览器可能有缓存)，即可更新到最新的提交。  



再看一下整个过程，是不是与日常新建项目写代码没啥区别？对，就这么简单，你只需要专注于写你的博客内容(`Markdown`)，而且用了`Git`可以追踪所有的内容变更，这样你也不需要把写到一半的博客保存为文件传来传去，可以提交到`GitHub`，之后随时随地拉取最新的提交继续创作。



## 简单说明



`docsify`官方文档：[https://docsify.js.org/#/zh-cn/](https://docsify.js.org/#/zh-cn/)

> `docsify`可以快速帮你生成文档网站。不同于 GitBook、Hexo 的地方是它不会生成静态的 .html 文件，所有转换工作都是在运行时。如果你想要开始使用它，只需要创建一个 index.html 就可以开始编写文档并直接部署在`GitHub Pages`。

所以，上面也提到，你只需要专注于写你的博客内容(`Markdown`)，这对于只懂后端的程序员非常友好。当然，如果你了解前端的话，可以改`css`进一步美化，添加其他`js`插件让网站更加酷炫。



上面你看到的博客样式，是基于[`https://notebook.js.org`](https://notebook.js.org)修改的，在 [`关于本站`](https://bytesfly.github.io/blog/#/about/) 的致谢中也有明确说明。



一直以来可能有些朋友认为使用`GitHub Pages`搭建网站麻烦丑陋且不好维护，希望读完这篇能让你眼前一亮。
