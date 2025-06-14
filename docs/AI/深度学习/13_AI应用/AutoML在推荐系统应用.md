今天，推荐系统的模型和应用已经相当成熟，然而部署一套全新的推荐系统，甚至仅在已有系统上添加数据维度和模型优化依然是非常耗时耗力的事情。

这是由于不同数据源的分布不尽相同，要达到满意的建模效果，每个建模的环节，包括数据处理、特征工程、模型的选择和超参数选择等都需要随之变动和优化。

以往这些工作都是建模工程师通过 A/B Test 和 Grid Search 等方式来手动调试有限的几种建模组合方式，并挑出最好的配置作为上线用的系统配置。

然而要想从少量的尝试中找到优质的模型方案，不仅要求工程师有丰富的建模经验，可能还需要一点点运气，成本和风险都比较高。

近几年在机器学习领域兴起的自动机器学习（AutoML）技术，便是为了解决机器学习模型训练难，落地难这个痛点所做的努力。

我们同样可以把 AutoML 技术应用到推荐系统的建模中，这次分享主要介绍用哪些方法来打造一个 AutoML 系统，并用于提升推荐系统的搭建效率。

如果我们看今天的机器学习应用（以监督学习为主），它大致可以分为传统机器学习和深度学习两大类。

传统机器学习用的比较多的模型有 LR、Gradient Boosting Machine、Random Forest、KNN 等，模型本身比较简单和成熟，但是由于这些模型无法拟合非常复杂的非线性函数，我们需要通过特征工程把原问题空间转化到一个机器学习模型容易学的表述空间，才能得到好的效果。

相对传统机器学习，近几年兴起的深度学习，由于其强大的模型表达能力，相对弱化了特征工程的重要性，具有端到端学习的能力。

尤其在处理图像，文字和语音等非结构化数据时，我们发现深度学习模型具有学习表述空间的能力（representation learning），从一定程度上实现了特征工程的自动化。

由于传统机器学习模型和深度学习模型在建模过程中侧重点不同，AutoML 也大致分为自动传统机器学习和自动深度学习（如图 1）。

其中自动传统机器学习关注自动数据预处理，自动特征处理和自动算法选择和配置，而自动深度学习则需要解决神经网络的自动训练和网络结构搜索的问题。我们下面就根据图 1 来逐一探讨 AutoML 的各个技术要点。

![](docs/AI/03_%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/13_AI%E5%BA%94%E7%94%A8/attachments/AutoML%E5%9C%A8%E6%8E%A8%E8%8D%90%E7%B3%BB%E7%BB%9F%E5%BA%94%E7%94%A8/52f239ad2efdead77a27d4a6a8f4c04f_MD5.webp)

图 1：自动机器学习组成部分

### **1. 自动传统机器学习**

当我们有了用户行为数据后，我们通常需要经过数据清洗、数据预处理、特征工程、选择模型、配置模型、融合模型等步骤来构建一整个机器学习管道。

自动机器学习需要尽可能的自动化其中每个环节。除了数据清洗环节和数据本身耦合度很高，需要更多的人为操作以外，数据预处理和之后的步骤都已经在自动机器学习领域存在可行的方案。

#### **1.1 数据预处理**

由于模型对数据都存在一定假设，我们需要使用一些数据预处理的方法将进入模型的数据处理成适合模型学习的分布。

比如神经网络模型需要输入的数据符合正态分布，那么要对原始数据做归一化处理；比如 Gradient Boosting Machine 不适合对类别数量非常高的离散特征建模，所以在前期要考虑删除类别太多的离散特征。

在自动机器学习中，选择哪种数据预处理方式和模型息息相关，根据上面所述的经验构造一个固定模版，比如已知神经网络需要归一化处理，GBM 需要剔除高维离散特征，LR 模型需要线性分形等，把这些知识 hard code 进 AutoML 系统中，可以用一种模型来学习最优组合。

这里介绍两个可行的方向：一是使用贝叶斯优化的方法，通过尝试，反馈，优化这一循环的过程来找到各个模型对应的最佳数据预处理方法，我们会在后面对贝叶斯优化做更详细介绍；

另一个方向是元学习，我们在很多数据集上通过实验不同的预处理-模型组合，记录下每个数据集上最优的组合，当在新数据集上开始探索时，会首先计算数据集的元特征，并在元特征空间中找到几个最相似的曾经试验过的数据集，并借用它们的最优预处理方案。

这两个方向也可以结合起来，用元学习帮助热启动，再使用贝叶斯优化探索适合新任务的方案。

#### **1.2 自动特征处理**

有人说，世界上的数据科学家，平均花 80% 的时间做特征，20% 的时间建模型，我们在工作中也意识到特征工程无比的重要性。

因此在自动机器学习系统中，特征也同样是极其重要的环节。在这里讨论一下特征组合，如何处理时序特征，使用变分自编码器构造特征等方法。

##### 1.2.1 多粒度离散化

推荐系统常用的 LR 模型，在处理高维离散特征上非常强大，然而其简单的线性模型本质使它对非线性的连续特征解释效果较差，并且在连续值特征尺度变化较大时效果不稳定。

分桶是一种常见的连续特征离散化方法，然而分桶数目对建模结果影响较大。因此我们使用第四范式自研的线性分形分类器（LFC）来解决这个问题。

使用 LFC 我们可以让模型从数据中自动选取最合适的分桶方式，同时 LFC 可以实现在特征粒度的离群点检测，使得模型更为鲁棒。通过这种技术，我们在业务数据上都能相比 LR 提升一个百分点。

##### 1.2.2 自动特征组合

原始数据中有的隐藏的关系，机器学习模型并不容易学到，所以需要通过构造特征把这些隐性关系表达出来。针对离散特征和连续特征分别介绍基于启发式算法的自动特征组合方法。

对于离散特征，由于简单的线性模型无法学到多个特征的交互，需要通过笛卡尔积来生成组合特征。

举个例子，如果要给决定是否给用户推荐一款很受年轻女性欢迎的化妆品，原始数据里只有年龄段和性别两个字段，可以把年龄段\_性别作为一个新的特征，模型便能很容易从历史数据中学出这款化妆品推荐给年轻女性接受度很高。

如果把所有组合特征都生成出来，那么组合特征的个数是随着阶数呈指数性增长的（搜索空间大于 AlphaGo），也就是我们很快就会产生出系统无法承受的数据量来。

针对这种情况，我们提出了一个自动特征组合算法 FeatureGo，结合集束搜索（Beam Search）和回溯（Backtracking）策略，去逐步搜索特征空间。

另外，基于 Boosting 的思想，提出了一系列替换损失函数来高效的评估特征重要性。我们在第四范式的大规模分布式机器学习平台 GDBT 实现了该算法，并依据实际应用场景定制化开发，能够在短时间内快速搜索到有效组合特征。我们发现在实际应用中都可以得到可观的效果提升，在所有实际应用中得到了超过 5 个千分点的提升。

##### 1.2.3 自动时序特征

在业界的实际场景中，数据一般包含时序信息，因此需要考虑如何自动构建时序特征，然而时序特征对系统性能要求较高。为了去的更好的建模效果，也要求时序特征算子尽可能多以覆盖各种情况。

基于 GDBT，我们实现了非常高效的自动时序特征生成和选择算子：TemporalGo，它包括时序统计信息、隐式向量等方法，也涵盖如循环神经网络 RNN 等方法，显著提升了建模效果。

##### 1.2.4 变分自编码器 \(VAE\)

变分自编码器（VAE）是一种基于神经网络的生成模型，其目标是给定原始数据，VAE 经过编码和解码后要尽可能地还原出原始数据。

可以把 VAE 用作一个基于模型的特征生成手段，而且经过 VAE 编码后的数值分布会尽可能的接近正态分布，这样的新特征可以直接给很多机器学习模型使用。

当然训练 VAE 本身很耗时间，而且需要较大的数据量才可能有效果，在实际应用当中，优先考虑其他特征工程方法。

#### **1.3 模型选择**

在拿到一个问题开始建模之前，都会面临一个问题，用什么样的模型？你可以很容易地根据自己的经验，面对分类问题还是回归问题，图像还是表类数据，列出几个候选模型，然后你可能会把候选模型用这个数据都训练一遍，并挑出那个验证效果最好的模型用在生产中。在自动机器学习中，我们也会把模型选择分成两步。

首先，拿到一个新问题时，我们获得这个问题的 meta 信息，比如数据是什么格式，数据量大小，训练指标是什么等，通过查询预先准备的问题映射到模型的查找表，找到适合这个问题的几款候选模型及相关超参数设置（或者超参数的搜索空间）。

接下来便是挑选效果好的模型。最朴素的做法是把所有可能的模型和超参数配置都训练一遍，挑出最好的模型即可，然而现实情况通常都有时间和计算资源的限制，导致我们无法训练所有可能的模型参数组合。

我们需要一个更加节省资源的方法，对于一个问题，很多模型不一定需要到训练结束才能做出判断哪个模型效果好或者差，可能在训练过程中我们通过观测验证指标，就能提前剔除一些效果太差的模型。

#### **1.4 模型超参数优化**

一个模型在开始训练前，可能需要人设置一些参数，比如 LR 有 L1、L2 正则系数用来控制模型过拟合，GBM 有树棵树，学习率等，这些参数配置的好坏会直接影响最终的模型效果，而且参数配置的好坏又和数据本身有很强的相关性。

也就是说，不存在一组黄金配置能在所有数据集上都表现良好。因此建模工作中一个不可或缺的工作便是模型超参数的优化。

如果是我们手动优化参数，一般是选取几组我们认为值得尝试的参数配置，然后训练模型并做交叉验证，最后挑出验证指标最好的模型用作生产。

这种做法对一两个超参数做优化还能应付，然而传统机器模型 GBM 就有小十个需要调试的超参数，更不用说深度学习模型会有更多的参数选择，这使得自动优化超参数技术越来越多的应用到实际建模中。

最常见的做法是 Grid Search 和 Random Search。Grid Search 是让用户在每个超参数的选择范围里取几个点，然后机器会将所有可能的参数组合都尝试一遍，最后选出最好的模型，这种方法有两个问题，一是需要用户对每个超参数都要取点，二是由于需要尝试所有参数组合，对计算资源的消耗非常高。

Random Search 是给定超参数选择的空间范围，然后在这个空间里随机采样N组超参数配置，交给模型做交叉验证，并选出最好的模型。

在实际应用中，Random Search 在超参数较多的情况下比 Grid Search 更快而且效果更好。

目前提到的两种做法实现起来都很简单，但缺点是它们都是在参数空间里盲目的搜寻，效率较低。

接下来我们介绍几种在提升效率上努力的思路：

##### 1.4.1 贝叶斯优化

贝叶斯优化是一种用于全局优化的搜索策略，早期多用于工业工程方向，来优化工业流程设计的配置。近几年贝叶斯优化开始广泛出现在机器学习领域的研究中，尤其在超参数优化领域。

叶斯优化的思路是将超参数空间映射到验证指标空间的函数作为优化的目标函数，然而这个函数的形式是未知的，而且要计算一个点的函数值需要消耗很多资源（等同于用一组超参数配置来训练模型并做交叉验证）。

所以贝叶斯优化会把已经尝试过的超参数配置和对应的交叉验证指标作为历史数据，并用它训练一个机器学习模型。

这个模型和通常的机器学习模型略有不同，它不仅需要提供预测值（prediction），还要提供对于这个预测的不确定度（uncertainty）。

这是因为接下来的优化策略会同时根据预测值和不确定度来决定尝试哪组新的超参数。贝叶斯优化中的优化策略往往需要考虑发掘（exploitation）和探索（exploration）两个因素。

发掘是指根据目前的模型预测，找到预测效果最好的超参数；探索是指目前的模型也许还没有触及到搜索空间中真正的全局最优，所以需要去探索那些区域，而这些区域一般可以通过不确定度来知晓。

为了兼顾这两个因素，优化策略会把预测值和不确定度两个指标融合在一起搜索下一个要尝试的超参数。

因为贝叶斯优化很好的平衡了发掘和探索，这类方法在解决全局优化问题中都表现出极高的效率，收敛速度很快，所以在超参数优化问题中也取得了很好的效果。

##### 1.4.2 进化算法

进化算法是一种启发式优化算法，正如其字面意思，这个算法模仿了进化理论，通过优胜劣汰的机制选出好的配置。

##### 1.4.3 强化学习

强化学习中有一类最简单的问题叫做多臂老虎机，这类问题源于赌博，大概是这样的：赌场里有N多台老虎机，每台机器的赢率是固定且未知的，赌徒想要通过实验找到赢率最高的那台机器，这样他的期望回报才是最优的。

最傻的办法就是在每台机器上试验 M 次，统计一下每台机器的赢的次数，并选出那台赢率最高的机器。

然而这个方法很显然有很多可提高之处，比如有的机器在玩了 K&lt;M 次就发现赢率很低，那就没必要浪费钱试验满 M 次了，于是大家便想了很多策略来提升找到赢率最高的机器的效率，于是这个问题变成了一个研究领域。可是这和超参数优化有什么关系呢？

事实上，我们可以想象每组可能的超参数配置是一台老虎机，它内部藏着一个未知的数字，在我们这里可以认为是用这组配置训练模型能达到的验证指标。为了了解这个未知数字，我们只能通过训练模型，训练时间越久，我们投入的资源就越多。

于是多臂老虎机的策略也可以应用到我们的问题上，也就是为了找到最优的超参数，决定每组超参数配置要投入多少资源训练模型的问题。

这里仅粗略介绍了三个优化超参数的方向，其实最近几年涌现了很多优秀的工作，包括使用元学习，对学习曲线建模，或者将上述的几个思路融合等方式，使超参数优化变得愈加高效。

#### **1.5 采样优化**

当数据量很大时，用全量数据训练一个模型会花费很长时间，而探索的过程需要训练多次模型，那么总开销就太大了。

也许我们在探索时只使用少量的部分数据训练模型，并且得到的关于模型和参数的选择又能帮助到全量数据训练情况下的选择，那我们就有机会节省大量资源。

这个设想在几年前就有工作进行了证实，通过观察不同采样率下训练模型的效果和超参数的关系分布，发现低采样率时超参数和效果的分布与全量数据训练下的分布具有很强的相关性。

于是我们在实际应用中，可以使用预定的降采样率选择少部分数据，并在这部分数据上进行模型和超参数的优化，然后将找到的最优选择直接放到全量数据上训练生产用模型。

我们发现这种方法尽管朴素，实际应用中却能达到很好的效果。学术界也有提出更成熟的做法，比如对采样率建模\[2\]，以期望通过一个配置使用低采样率训练的模型效果来预测全量数据下的模型效果，并用预测值来指导超参数的搜索。

### **2. 自动深度学习**

深度学习由于具有模型表达能力强，自动学习特征的端到端特性等优点，是今天机器学习领域最受欢迎的模型框架。

然而训练深度学习模型并非易事，不仅有大量的模型超参数需要人工调试，而且模型效果对超参数的选择极其敏感，因此需要大量的尝试和行业经验才能得到优质的效果。

自动深度学习面临同样的挑战，所以除了会共用前面介绍的自动机器学习的思路和方法，自动深度学习有一些独特的方向值得在这里探讨。下面我们会围绕自动训练和网络结构搜索两个方面展开。

#### **2.1 自动训练**

深度学习模型和传统机器学习模型相比，需要配置的超参数会多很多，训练时对资源的消耗也会较大，因此自动训练是一个更有挑战性的工作。

朴素的 Grid Search 和 Random Search 基本得不到满意的效果，必须使用更成熟的搜索策略和精心设计的搜索空间才能实现自动化。

由于神经网络的训练速度较慢，我们希望能尽可能地从训练过程中得到最多的信息和信息再利用。

我们总结一下目前工作的几个方向，和大家分享。

##### 2.1.1 模型再利用

想象一下一个神经网络模型的训练是一个小人在模型的权重空间（weight space）里漫步，靠着 SGD 指引他一步步接近最优权重，而使用一组好的配置，就是为了使这个路径能够通往最有权重所在的位置，而不是中途就卡在一个局部最优不能动弹，或者来回跳动不能收敛，甚至到了一个过拟合的地方。

目前为止我们提到过的搜索模型配置的方法，都是选一组配置，然后让这个小人从一个初始化的位置开始走，如果这个配置让小人走偏了，那我们换一组配置，再让小人从头开始走。

但这样每次小人走过的路就都白费了，我们完全可以让小人从一个虽然不是最优的，但还是不错的位置作为起点，继续去寻找那个最优地点。

这是 Deepmind 在 2017 年发表的题为 population based training 的论文 \[3\] 的思想之一，通过再利用已经训练过的模型，来加速训练和调参的过程。

此外，南京大学的周志华教授还提出了“学件”的构想，“学件”由模型和用于描述模型的规约两部分构成。当需要构建新的机器学习应用时，不用再从头建模，可以直接需要寻找适合的学件直接使用。

##### 2.1.2 拟合 Learning curve

用 TensorFlow 训练模型的同学可能用过 Tensorboard，这个可视化工具可以展示模型训练过程中各种指标的变化，我们称之为学习曲线（Learning curve）。

这个曲线是有规律可循的，比如验证 AUC，随着训练，会不断的上升，到收敛的时候可能开始持平波动，之后也许由于过拟合又会下降。我们可以用一个模型来拟合这条曲线 \[4\]。

这样做的目的是，假如我有一个靠谱的拟合模型，那么试验一组新的配置，我可能只用让模型训练较短的时间，并用前面一小段学习曲线和拟合模型来预测最终这组配置能让模型训练到什么程度，那么我们便可以用少量的资源对模型配置做一个初步的筛选，提升效率。

##### 2.1.3 Meta learning

元学习（Meta Learning）的目标是给一个新的问题，它能生成一个解决这个问题的模型。这一思路也可以用到自动深度学习上，同样是 2.1.1 中小人的例子，我们可以找到一个权重空间里的位置，它对于很多类似的新问题都是一个还不错的位置，只要用对应问题的数据让小人再走两步就能达到最优了。

有一篇论文 \[5\] 便用到了这个思想，它训练一个神经网络模型，但损失函数并不是用某一任务的数据直接计算的，而是让任意一个采样的训练任务的数据再训练一步，之后的损失作为目标函数。

也就是说，它要让小人站在一个理想的多岔口，能够离任意一个具体任务的最优位置很近。这和 2.1.1 想要达到的加速训练的目的类似，只不过是用一个元模型显性地去寻找“理想的多岔口”。

##### 2.1.4 模型融合

由于深度学习模型的损失函数平面非常复杂，使训练时找到一个鲁棒的最优点很困难。为解决这个问题，我们可以用不同的初始化，训练多个模型，并将它们融合起来。

这是比较标准的做法，最近有两篇论文给出了更有趣的方案：第一篇 \[6\] 的思想类似于 2.1.1，在小人找到第一个最优点，记录下当前的权重，然后增大学习率，让小人跳出当前的最优点，去寻找附近的另一个最优点，如此反复几次，把记录的权重对应的模型融合起来，会相比标准的融合做法省去从头训练模型的时间。

第二篇 \[7\] 使用类似第一篇的循环学习率设置，但它不再记录多个模型，而是将存下来的权重直接取平均，这样做的好处是在预测阶段，只有一个模型预测，节省了普通模型融合需要多个模型同时预测的耗费。另外论文中也表明直接取平均能得到更鲁棒的模型。感兴趣的话可以去阅读下这两篇论文。

#### **2.2 网络结构搜索**

不管是图像，文字还是语音，都有几款大家耳熟能详的神经网络结构，这些网络结构的巨大成功，归功于背后的研究人员的学识，灵感和不懈尝试。随着深度学习应用到越来越多的现实场景，对模型包括网络结构的需求也在变的更多样化。

举一个例子，在手机设备上的人脸识别软件，由于硬件设备的差异性，软件供应商需要对每种手机做相应的模型优化，如果全部依靠人力来做调试，很显然对资源的要求和耗费非常巨大。

这使我们不得不思考，是否有可能让机器来取代一部分这样的工作，将人力解放出来。早在 2016 年，Google Brain 就在这方面做了尝试 \[8\]，通过强化学习的方式训练一个能搭建网络结构的 RNN，并构造出了当时在图像数据集 CIFAR10 和自然语言数据集 Penn Treebank 上效果最好的模型。网络结构搜索（Neural Architecture Search）的名字也是由这篇论文而来。

尽管到今天网络结构搜索的历史不长，却已经涌现了很多优秀的工作，这里我们介绍几个思路和方向。

##### 2.2.1 基于强化学习

最早提出 NAS 的方案便是基于强化学习，后来出现的很多结构搜索的论文对这个方法做了改动和优化，沿用至今。

这个思路大概是说，我们在构造网络结构时，就好像是在堆乐高积木，从第一层开始，我们有几个基本元件，和几种拼接方法，我们按照一定流程一层一层拼出一个网络结构来。

而强化学习就是要学出一套构造优质网络结构的流程。由于这个流程是一个序列，那用 RNN 来建模就再适合不过了，于是我们让这个 RNN 每一步输出一个决策，来决定选择哪个基本元件，或者使用哪种拼接方法。

当 RNN 输出足够的决策后，一个网络结构变生成了，我们拿它在一个数据集上训练并测试，得到的验证指标便成为奖励用来训练 RNN。

最终，被训练的 RNN 便学会了构造好的网络结构来。听起来非常有道理，但这种做法其实有一个问题，就是训练 RNN 需要很多样本，而这个问题里一个样本便意味着训练一个神经网络模型，因此获取样本是很贵的。

事实也是如此，文章 \[8\] 里动用了 400 个 GPU 同时训练，一个训练了 1 万多个模型后才超越了当时最好的模型。大概只有 Google Brain 这样有巨量计算资源的地方才有可能做这样的尝试。

后续有很多工作都尝试减少资源的耗费，使搜索变得更高效，比如使搜索空间变得更小 \[9\]，模型间共享权重 \[10\] 等。

##### 2.2.2 共享权重

刚才提到了每个模型都要从头训练是非常低效的，ENAS\[10\] 提出了模型共享权重的理念。

文章作者认为，一个网络结构图是一个更大的图的子图，于是作者索性存下包含整个结构搜索空间的母图的所有权重，并且边训练权重边训练如前所述的 RNN。

由于 RNN 构造出来的新结构直接从母图中获取权重，便省去了从头训练模型的过程，使整个搜索比以前的方法快了上百倍。

##### 2.2.3 元学习

由 ENAS 的共享权重受到启发，一篇新的工作 \[11\] 使用母图作为元模型，通过 dropout 的方式来训练元模型。

于是没有了构造结构的 RNN，而是以随机 dropout 的形式来让元模型找出什么样的结构是重要的。作者在文中展示的效果和 ENAS 类似，我觉得两种方法不好说孰好孰坏，都可以拿来尝试下。

##### 2.2.4 贝叶斯优化

最近有一个叫 Auto-Keras 的开源软件受到了一定关注，这个软件包致力于帮助人们自动训练深度学习模型，而软件的“自动”部分基于一篇该作者发表的论文\[12\]，文中使用贝叶斯优化作为结构搜索策略，并用 Network Morphism 来加速模型的训练。

作者定义了不同结构之间的“距离”，也就是不相似度，并基于此来构建贝叶斯优化中所需要的贝叶斯模型。

有了贝叶斯优化来指导结构搜索后，对于新结构，作者并非从头开始训练模型，而是使用 Network Morphism，将已经训练过的模型通过变换转变成要训练的新模型，而同时能保留原来模型的功能，如此一来，只需要用比从头训练少得多的资源就能训练出新的模型。

除了以上介绍的几种思路之外，很多其它用于优化的方法也都出现在结构搜索的应用中，比如前面提到过的进化算法 \[5\]，基于模型的迭代式搜索 \[13\] 等。

#### **2.3 适用于宽表数据的自动深度学习**

目前的自动深度学习训练和网络结构搜索，主要集中在语音、图像和文本等领域，尚未见到针对宽表业务数据的神经网络结构搜索，然而这正是工业界最迫切的需求之一，其对应的自动深度学习价值较大。

针对宽表业务数据对应的大规模离散特征数据集，我们研发了深度稀疏网络（Deep Sparse Network，DSN）及其自动版本 Auto-DSN。

DSN 采用多层级网络架构，综合利用数据采样、注意力机制和动态维度表达等方法，能够有效的对宽表数据进行建模。

Auto-DSN 综合利用上述各种技术，使得用户配置一个和资源相关的参数，即可在合理时间内，搜索到对宽表业务数据最佳的模型结构及超参数。我们在一些实际业务中验证了它的有效性。

### **3. 模型评估**

自动机器学习根据评估指标来优化模型，在这次分享的最后，我们再探讨一下怎样对模型的评估是可靠的。

首先评估指标的选择应该和具体业务相结合，根据业务目标来制定对模型的评估方式，如果不考虑业务相关指标，机器学习中我们常用的指标有 AUC、logloss、MSE、MAE 等，关于其定义和用法网上有很多资料解释，这里就不赘述了。我想主要分享的是关于如何对抗过拟合的一些经验。

这里的过拟合是指，在优化模型的配置或者参数的过程中，我们找到一组配置可能在我们的验证集上表现效果很好，然而使用这个模型生产却并未得到最好的效果。

原因是多方面的，可能我们使用固定的验证集来优化配置，导致这个配置仅仅在当前验证集上的效果最好，没有普遍性；也可能是训练模型时由于一定的随机性把某个次优的配置当成了最优配置。

为解决以上的问题，我们分别做了些尝试。对于固定验证集导致的过拟合，标准的做法是使用交叉验证来计算指标，然而带来的问题是交叉验证所需的资源是固定验证集的折数倍。

比如常用的五折交叉验证就需要五倍于固定验证集的资源来优化。当模型训练时间很长时，我们没有足够的资源计算完整的交叉验证，于是我们会依然按照交叉验证的方式来切分数据。

但每次验证时我们只会随机选取其中一份验证集来计算验证指标，这样指标的期望值就是无偏的。

当然这又引入一个新的问题，虽然期望是无偏的，却由于我们的随机选取导致方差变大了，也就是我们把次优选择当成最优选择的风险变大了。

这里我们引用 \[14\] 的“intensification mechanism”来解决这个问题，这个过程是我们将第一组搜索的配置用完整的交叉验证计算出平均指标，并记为“最优配置”，后续搜索到的新配置都会和“最优配置”比较，比的方式是计算新配置在某一折验证集上的指标。

如果当前新配置的平均指标低于“最优配置”，则放弃这个新配置并开始新的搜索，反之则再选一折验证集计算指标，如果所有验证集都已经计算完，新配置的平均指标还是更优的，便把这个配置作为新的“最优配置”。

这样一来，我们只会把更多的计算量放在有潜力成为最优配置的配置上，总体消耗还是低于标准的交叉验证的。

根据我们目前在推荐业务中的尝试，上述方法中：自动特征离散化会给模型带来最明显的泛化能力提升和 AUC 明显升高、自动特征组合可以最有效地提高模型对物料和人群的精准刻画能力和精准个性化推荐效果、采样优化和模型超参数优化功能对机器资源和训练时间的优化效果最为明显，给业务方留下了深刻的印象，欢迎大家进行尝试。

上述内容便是我们在实际应用 AutoML 中的感想和经验，希望能对大家有用。我们也希望更多的人开始了解和运用这个领域的方法，帮助他们加快机器学习系统的研发和生产。

