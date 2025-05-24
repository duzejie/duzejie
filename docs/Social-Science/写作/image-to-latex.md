# 百闻不如一试——公式图片转Latex代码

写博客时，数学公式的编辑比较占用时间，在上一篇中详细介绍了如何在`Markdown`中编辑数学符号与公式。

[https://www.cnblogs.com/bytesfly/p/markdown-formula.html](https://www.cnblogs.com/bytesfly/p/markdown-formula.html)



当然，有时候我们仅仅是想把现成的公式搬到`markdown`中来编辑，此时如果有工具能把公式截图直接解析成`Latex`代码就方便了。

刚好这几天看到好几个微信公众号都在推送`image-to-latex`这个开源项目：

[https://github.com/kingyiusuen/image-to-latex](https://github.com/kingyiusuen/image-to-latex)

> Convert images of LaTex math equations into LaTex code.

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/f96104174ccb6f5e3a88686a83e21788_MD5.gif)

该项目当前(2021年09月02日)star人数为631,Fork为81：



![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/2c80ab26fc02c2a5181c9da725f65c68_MD5.png)



最近正好也是在了解机器学习、深度学习相关的东西，于是打算上手感受一下转换效果。

## 百闻不如一试

其实`image-to-latex`这个项目的`README`写得算是比较清楚了，介绍了项目的来龙去脉、可以改进的地方、如何使用等等。

### 快速开始

下面我把自己第一次尝试的过程简单记录如下：

- 克隆项目

```bash
git clone --depth=1 https://github.com/kingyiusuen/image-to-latex.git

cd image-to-latex
```

多啰嗦一句：

> --depth: 用来指定克隆的深度，1表示克隆最近的一次commit。这种方法克隆是为了减小项目体积的，加快克隆速度，对于那种庞大且活跃的开源项目非常有效。



- 准备Python环境

该项目依赖Python环境，由于我用的是`conda`来管理虚拟环境的，不是用`venv`，所以这里的步骤可能与`README`上的有一点点差异。

此时应该是在项目目录下，即`image-to-latex`目录，该目录下有`requirements.txt`文件。

```bash
# 创建新的python3.6环境
conda create --name latex python=3.6

# 激活环境
conda activate latex

# 安装依赖
pip install -r requirements.txt
```

关于Python环境的搭建，可以参考我之前的博客：

[https://www.cnblogs.com/bytesfly/p/python-environment.html](https://www.cnblogs.com/bytesfly/p/python-environment.html)



- 下载模型

> For example, you can use the following command to download my best run.

到了这步本该是模型训练(`Model Training`)，我这里仅想体验一下，可以直接下载别人已经训练好的模型。

```bash
python scripts/download_checkpoint.py kingyiusuen/image-to-latex/1w1abmg1
```

此时shell显示如下：

```bash
(latex) ➜ python scripts/download_checkpoint.py kingyiusuen/image-to-latex/1w1abmg1
wandb: (1) Create a W&B account
wandb: (2) Use an existing W&B account
wandb: (3) Don't visualize my results
wandb: Enter your choice: 3
wandb: You chose 'Don't visualize my results'
Downloading model checkpoint...
Model checkpoint downloaded to image-to-latex/artifacts/model.pt.
```

下载需要稍微等等，模型有将近2个G的大小。



- 启动服务

(1) 启动后端服务，执行命令`make api`

> An API is created to make predictions using the trained model.

看下项目的`Makefile`文件，其实`make api`就是调用了下面的启动命令：

```bash
uvicorn api.app:app --host 0.0.0.0 --port 8000 --reload --reload-dir image-to-latex --reload-dir api
```

浏览器打开 [http://localhost:8000/docs](http://localhost:8000/docs) ，看到接口文档如下：

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/93f1a53e7c9f4212328f6e63ad3282d0_MD5.png)

(2) 启动前端界面，执行命令`make streamlit`

同样，看下项目的`Makefile`文件，其实`make streamlit`调用了下面的启动命令：

```bash
streamlit run streamlit/app.py
```

浏览器打开 [http://localhost:8501/](http://localhost:8501/) ，就是上传图片的界面：

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/0d1d9a70e67138ea727fcd92af93a9cb_MD5.png)

至此，`image-to-latex`就成功启动了，下面就期待转换公式的效果了！



### 上手体验



下面我作为一个小白用户，体验一下`image-to-latex`的转换效果。

我从之前的博客中截图了10个公式，使用下来，感觉当前的效果并非太理想。注意，个别解析出来仅是缺少了右`}`，这种也可以算解析出来了。如下：

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/7a06dc5a7898f50be4c34c0fa337a94f_MD5.png)



![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/9969a7e76b59e1e4f0280df091e25c02_MD5.png)



![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/5d60e90d639a0776d7b41a18d34850e2_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/1ba7791359e72e47256a57356358c6cd_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/82e8d9180d492828b4cc5b4372ba3692_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/932c8fbced8a9418a9573343f5730e7e_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/d9cefe9d1b16d56d13fb8ba757d13711_MD5.png)



![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/3029e166116a8c5962cb238025c97042_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/c9dfac0f8a67b79a1c93395ba1d5a0ca_MD5.png)

![](docs/Social-Science/%E5%86%99%E4%BD%9C/attachments/image-to-latex/7cf0b4bfdf49ea40f72a1429190fe98e_MD5.png)

测试来看，貌似对多行公式的解析不太好。当然了，有这样的免费工具来辅助我们把公式图片转成`Latex`代码已经让人挺惊喜了。相信以后随着更多的人参与算法的优化、模型的改善，解析的效果会更好。



## 写在后面

> I found a pretty established tool called Mathpix Snip that converts handwritten formulas into LaTex code.

`image-to-latex`这个项目的`README`里也提到了`mathpix`这个更加成熟的工具。免费版每月能识别50次公式图片。详情见：

[https://mathpix.com/](https://mathpix.com/)

下载试了下，识别的效果确实不错。(注意：非广告，本人与`mathpix`无任何关系，仅仅试了下而已!!!)



百闻不如一试，动手尝试之后才有发言权。后面有时间会看看`image-to-latex`的代码实现，学习学习。
