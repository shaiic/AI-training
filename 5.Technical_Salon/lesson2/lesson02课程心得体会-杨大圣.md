## AIIC201-python导论、数学基础心得体会

#### Zeal 、Anaconda、Markdown

Bartuer老师课上推荐了三个AI学习中会使用到的工具软件：Zeal 、Anaconda、Markdown（全都是免费的）

Zeal对于我这种小白可以作为编程时的帮助文档使用，注意安装完成后要先下载各个语言的库才能使用

Anaconda是一个集合了用Python进行数据分析所需要的环境和库的傻瓜版安装包。jupyter lab、jupyter notebook、python各个版本和库里面都有。有了Anaconda再也不用求教大神解决编程环境问题了。

Markdown是我现在正在用的一个文字编辑格式，我现在用的软件是经大神推荐的Typora，原理是一样的，MD文件解决了排版问题，不管是博客还是微信公众号，使用MD格式都能非常方便地转化各种格式排版效果。

#### 《沃兹传》

Bartuer老师讲了一个关于乔布斯和他的合伙人沃兹改进电路板赚了5000美元但乔布斯只分给沃兹500美元的故事，并推荐了关于他合伙人的传记《沃兹传》。课后我自己下了一本翻了翻发现沃兹传里的故事都鲜有知悉很少被提及，但透过这本书我感觉他才是苹果真正的灵魂。沃兹作为一个置身喧嚣之外的科技顽童也跟作为商人的乔布斯很不一样，这也让沃兹能够看淡成功与名利，书里的沃兹有着相对简单容易实现的理想，比起乔布斯是个更真实的人。



#### Pandas、 Scipy、 Numpy、Matplotlib

Python为了兼容性和便利性牺牲了速度。下图解释了用Numpy Array比Python List快很多的原因。

![memory acces——array_vs_list](image/memory acces——array_vs_list.png)

所以我们引入Numpy和Pandas库可以改善Python做数学运算的速度慢的特点。Scipy可以解决科学计算的问题，有很多现成的函数可以调用。Matplotlib可以画图。

如果有跟我一样对Python不是太了解的小白推荐看：

###### 《Python新手一百天学习计划》（Ref： https://github.com/ZiniuLu/Python-100-Days）

推荐看第二阶段Python进阶和第十阶段数据处理机器学习

###### 《自学Python做科学计算数据分析》（Ref：https://github.com/lijin-THU/notes-python）

推荐看Numpy里的数组，以前我用C语言用的多，所以Python的数组用的不是很习惯，看了这个清楚很多。

###### 《Python meta-hacking》（Ref:https://jakevdp.github.io/blog/2014/05/09/why-python-is-slow/）

讲了python进行数学运算时的实际内部过程，这也是Python慢的主要原因

#### 矩阵运算

import...as则是将import A as B,给予A库一个B的别称，帮助记忆

from...import与import区别在于import直接导入指定的库，而from....import则是从指定的库中导入指定的模块

```python
##创建N维数矩阵
import numpy as np
m_empty = np.empty((3,4)) # 空矩阵（所有值都在0附近但不为0）
print(m_empty)

m_eye = np.eye(3) # 单位矩阵
print(m_eye)

m_zeros = np.zeros((2,3)) # 全零矩阵
print(m_zeros)

m_ones = np.ones((3,2)) # 全一矩阵
print(m_ones)

```

#### 傅里叶变换

Bartuer老师在说Numpy Vs. Scipy时提到了一个很有用的工具：“傅里叶变换”，“给一根杆子+一根杆子+一根杆子+......+一根杆子能画圆出任何函数“

傅里叶级数的本质是将一个周期的信号分解成无限多分开的（离散的）正弦波

![fourier](image/fourier.gif)

而傅里叶变换能把是时序变成频率，变成频率就可以分开分析进行运算。

这是一篇很有意思的讲傅里叶变换的文章：https://www.cnblogs.com/h2zZhou/p/8405717.html

`import numpy as np`

`import matplotlib.pyplot as plt`

`import seaborn`

#### 迭代：“上帝的指纹” MandelBrot 图形

![mandelbrot](image/mandelbrot.jpg)

Mandelbrot点集均出自公式:Zn+1=(Zn)^2+C，对于非线性迭代公式Zn+1=(Zn)^2+C，所有使得无限迭代后的结果能保持有限数值的复数C的集合，构成Mandelbrot（曼德勃罗）集，是人类有史以来做出的最奇异最瑰丽的几何图形，被称为“上帝的指纹”、“魔鬼聚合物”

![mandelbrot zoom in](image/mandelbrot_zoom_in.png)

这个集是我在研究matplotlib的操作时看到的，非线性迭代十分有趣，不管把图案放大多少倍，仍然有更加复杂的局部，图案结构变幻无穷，既与整体不同又有某种神似

`用matplotlib画一个Mandelbrot图形`

```python
import numpy as np
import matplotlib.pyplot as plt
def mandelbrot( h,w, maxit=20 ):
    """Returns an image of the Mandelbrot fractal of size (h,w)."""
    y,x = np.ogrid[ -1.4:1.4:h*1j, -2:0.8:w*1j ]
    c = x+y*1j
    z = c
    divtime = maxit + np.zeros(z.shape, dtype=int)

    for i in range(maxit):
        z = z**2 + c
        diverge = z*np.conj(z) > 2**2            # who is diverging
        div_now = diverge & (divtime==maxit)     # who is diverging now
        divtime[div_now] = i                     # note when
        z[diverge] = 2                           # avoid diverging too much

    return divtime
plt.imshow(mandelbrot(400,400))
plt.show()
```



#### 摩尔定律-C10K-深度学习

摩尔定律指每一美元所能买到的电脑性能，将每隔18-24个月翻一倍以上。这一定律揭示了信息技术进步的速度。PC的算力一直符合摩尔定律呈指数级增长。计算机很聪明：耳聪目明（计算能力）+过目不忘（记忆能力）。

C10K是个分水岭

C10K问题的出现是随着互联网的普及，应用的用户群体几何倍增长，此时服务器性能问题C10K就出现了。最初的服务器是基于进程/线程模型。新到来一个TCP连接，就需要分配一个进程。假如有C10K，就需要创建1W个进程，可想而知单机是无法承受的。那么如何突破单机性能是高性能网络编程必须要面对的问题，进而这些局限和问题就统称为C10K问题。

![clip_image001](image/clip_image001.png)

神经网络以及NLP的理论模型上世纪40年代就有了，但发现以来一直不温不火，直到最近计算机的算力够强大、数据也够多了，NeuralNetwork 的前提条件都满足了可以爆发了。

推荐小白看两篇关于神经网络、机器学习的解读

Ref：《神经网络深入浅出》https://www.cnblogs.com/subconscious/p/5058741.html

Ref：《从机器学习谈起》https://www.cnblogs.com/subconscious/p/4107357.html



#### 思维向量

Bartuer老师说向量是个很好的思维工具，一个思维向量就是一个想法的向量化形式，用以表示该想法与其他想法之间的关联。思维向量网络的训练目标是生成一个想法的上下文。就像词汇由语法相连接一样，不同想法之间是由思维链条、逻辑路径连接起来的。因此，让一种算法学习如何表示任意想法与其他想法之间关联的训练过程可以称为人工常识构建。神经网络可以依据一个给定的想法预测紧接着有可能会出现的想法，就像循环神经网络预测字符和词语一样。会话即搜索。

以下是几种目前用于思维向量化的方法：

Doc2vec：(Ref：https://deeplearning4j.org/doc2vec.html)，段落向量和语句向量基本上同一个意思。它不一定会考虑词的顺序，一般用于在有标签的词的组合之间建立关联（比如用于情感分析）

Seq2seq：(Ref:https://arxiv.org/pdf/1409.3215.pdf)双语翻译

Skip-thought向量(Ref:https://arxiv.org/abs/1506.06726)



#### SMARTEREVERYDAY

![Smartereveryday](image/Smartereveryday.jpg)

一个数学怪咖，前NASA的员工，现在是YouTube网红写的科普博客，也经常发一些Vlog将一些难懂的知识生动的演示给我们看。

很多用德语写，数学已经超越了语言

用Mathematica 画的曲线一步步迭代去接近、拟合

http://www.smartereveryday.com

#### 对数、指数

f（x）=e^x 

该函数的神奇之处：斜率等于截距以至于求导等于本身

10^100是这个世界的极限，名字叫做googol,也是Google最开始的名称。

古戈尔数学没有什么特别的意义或是有什么特别的应用。卡斯纳创造这个词是为了勾画出一个不可想象的大数和无穷大之间的区别，它唯一的用途是有时被用于数学教学上。

围棋的可能性有没有10^100大。19*19=361个交叉点 361！明显大于10^100

故事1：国际象棋放米

故事2：体育场下雨，非线性而是指数型，97%的雨都是在最后的一分钟下的。