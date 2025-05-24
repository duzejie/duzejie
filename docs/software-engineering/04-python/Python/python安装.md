


## 安装包、换源
1、临时使用
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

2、永久更改pip源
```
pip install pip -U  #升级 pip 到最新的版本 (>=10.0.0) 后进行配置
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
如果您到 pip 默认源的网络连接较差，临时使用镜像站来升级 pip： 
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U`  

查看当前pip源
`pip config list`

3. 从list file安装包
```shell
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```




