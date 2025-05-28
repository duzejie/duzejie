## LeakyReLU层 {#leakyrelu}

---

LeakyRelU是修正线性单元（Rectified Linear Unit，ReLU）的特殊版本，当不激活时，LeakyReLU仍然会有非零输出值，从而获得一个小梯度，避免ReLU可能出现的神经元“死亡”现象。即，`f(x)=alpha * x for x <0 f(x) = x for x>=0`

## PReLU层 {#prelu}

---

该层为参数化的ReLU（Parametric ReLU），表达式是：`f(x) = alpha * x for x <0`,`f(x) = x for x>=0`，此处的`alpha`

为一个与xshape相同的可学习的参数向量。

## ELU层 {#elu}

---

ELU层是指数线性单元（Exponential Linera Unit），表达式为： 该层为参数化的ReLU（Parametric ReLU），表达式是：

`f(x) = alpha * (exp(x) - 1.) for x < 0`,`f(x) = x for x>=0`

## ThresholdedReLU层 {#thresholdedrelu}

---

该层是带有门限的ReLU，表达式是：`f(x) = x for x >theta`,`f(x) = 0 otherwise`



激活函数的作用：

1，激活函数是用来加入非线性因素，解决模型所不能解决的问题。

2，激活函数可以用来组合训练数据的特征，特征的充分组合。

下面我分别对激活函数的两个作用进行解释。

  


1

加入非线性因素，解决非线性问题

  


  


![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/b096a4755d708e9c6a3284651efeff42_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/c6d11c780dda5a807521523d374590d3_MD5.webp)

好吧，很容易能够看出，我给出的样本点根本不是线性可分的，一个感知器无论得到的直线怎么动，都不可能完全正确的将三角形与圆形区分出来，那么我们很容易想到用多个感知器来进行组合，以便获得更大的分类问题，好的，下面我们上图，看是否可行

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/2d48ad384a601b64bd91df7713692a79_MD5.webp)

好的，我们已经得到了多感知器分类器了，那么它的分类能力是否强大到能将非线性数据点正确分类开呢~我们来分析一下：

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/bdfc7a6c6e23975194a4c862afa47249_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/652acf89964ae7c7f406748e9ba37d0d_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/983f68a667e52650959a43deb20c099c_MD5.webp)

如果我们的每一个结点加入了阶跃函数作为激活函数的话，就是上图描述的

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/f2798cff6508e3589d2e254688142ca4_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/c75a76f9267739358329d2958b2edafa_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/34e2ca04c3a6bd4e5eb2e133800824f9_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/1428f9d3e95b526538cc37d032ad5415_MD5.webp)

那么随着不断训练优化，我们也就能够解决非线性的问题了~

所以到这里为止，我们就解释了这个观点，加入激活函数是用来加入非线性因素的，解决线性模型所不能解决的问题。

  


_下面我来讲解另一个作用_

  


2

 激活函数可以用来组合训练数据的特征，特征的充分组合

  


![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/2f605113a83624d5cec8637272c70673_MD5.webp) 


![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/19293aad6cc586d18bf0e54e61df537f_MD5.webp)

我们可以通过上图可以看出，立方激活函数已经将输入的特征进行相互组合了。

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/21932b9fc635f25061d9886e398cf647_MD5.webp)

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/eb4d613d91b6b9a73762a510d20f6805_MD5.webp)

通过泰勒展开，我们可以看到，我们已经构造出立方激活函数的形式了。

于是我们可以总结如下：

  


3

总结

  


  


![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/01_layers/attachments/activation/310e960fcd30025525f5e0616ed0f38d_MD5.webp)

这就把原来需要领域知识的专家对特征进行组合的情况，在激活函数运算后，其实也能够起到特征组合的作用。（只要激活函数中有能够泰勒展开的函数，就可能起到特征组合的作用）

这也许能给我们一些思考。

