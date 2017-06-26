---
title: 在macOS上搭建TensorFlow环境
date: 2017-06-26 19:23:13
tags:
- TensorFlow
- macOS
- pip
- Python
category: TensorFlow
---

在mac上安装TensorFlow的方法（官方推荐用virtualenv，以Python2.7为例）：

1. 安装pip和virtualenv:
```bash
sudo easy_install pip
sudo pip install --upgrade virtualenv
```

2. 创建TensorFlow的目录（在~目录下创建tensorflow目录）:
```bash
virtualenv --system-site-packages ~/tensorflow
```

3. 激活virtualenv环境:
```bash
source ~/tensorflow/bin/activate
```

4. 终端会变成这样：
```bash
（tensorflow）$
```

5. 退出：
```bash
(tensorflow)$ deactivate
```

6. 安装TensorFlow，这里需要注意的是macOS Sierra以后由于SIP机制（System Integrity Protection），直接安装会提示”...OSError: [Errno 1] Operation not permitted..."，我们需要通过`--user -U`将其安装到用户目录下:
```bash
pip install --upgrade tensorflow --user -U
```

7. 成功安装会提示(这些都是TensorFlow需要用到的库):
```bash
Successfully installed backports.weakref-1.0rc1 bleach-1.5.0 funcsigs-1.0.2 html5lib-0.9999999 markdown-2.2.0 mock-2.0.0 numpy-1.13.0 pbr-3.1.1 protobuf-3.3.0 setuptools-36.0.1 six-1.10.0 tensorflow-1.2.0 werkzeug-0.12.2 wheel-0.29.0
```

8. 这个时候进入python的交互环境测试:
```python
>>>python
import tensorflow as tf
hello = tf.contant('Hello, TensorFlow!')
sess = tf.Sesssion()
print(sess.run(hello))
```

9. 不出意外的话，TensorFlow会提示一些奇怪的东西:
```bash
The TensorFlow library wasn't compiled to use SSE instructions, but these are available on your machine and could speed up CPU computations" in "Hello, TensorFlow!" program
```

10. 按照github上Carmezim的说法，这些warning是告诉我们如果自己编译TensorFlow会使其运行速度更快，我们可以忽略这些警告（例如在我的.zshrc中）:
```bash
# in your ~/.zshrc file
export TF_CPP_MIN_LOG_LEVEL=2
```

相关链接：
1. [TensorFlow官方安装教程](https://www.tensorflow.org/install/install_mac)
2. [解决mac osx下pip安装ipython权限的问题](http://xiaorui.cc/2016/03/27/解决mac-osx下pip安装ipython权限的问题/)
3. [Github issue of TensorFlow warning](https://github.com/tensorflow/tensorflow/issues/7778)
