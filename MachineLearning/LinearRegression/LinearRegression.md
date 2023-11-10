# 1. 一元线性回归
给定数据D={(x1，y1),(x2，y2)，...，(xm,ym)}D=\{ (\boldsymbol{x_{1}}， y_{1}), (\boldsymbol{x_{2}}， y_{2})，...，(\boldsymbol{x_{m}}, y_{m})\}，**考虑一元回归问题**：我们希望寻找到f(x)=ωx+bf(x) = \boldsymbol{\omega}x + \boldsymbol{b} 这样一条直线，使得数据集DD上的点到直线f(x)f(x)的距离之和的绝对值最小（L1L_{1}范数），即min∑mi=1|f(xi)−yi|min \sum_{i =1}^{m}|f(x_{i}) -y_{i}|。
![线性回归](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ob3NoaW5vcnkuY29tL3dwLWNvbnRlbnQvdXBsb2Fkcy8yMDE5LzEyLyVFNyVCQSVCRiVFNiU4MCVBNyVFNSU5QiU5RSVFNSVCRCU5Mi1zY2FsZWQucG5n?x-oss-process=image/format,png)
![尖点不可导](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ob3NoaW5vcnkuY29tL3dwLWNvbnRlbnQvdXBsb2Fkcy8yMDE5LzEyLyVFNyVCQSVCRiVFNiU4MCVBNyVFNSU5QiU5RSVFNSVCRCU5Mi0lRTUlQjAlOTYlRTclODIlQjklRTQlQjglOEQlRTUlOEYlQUYlRTUlQUYlQkMtMS1zY2FsZWQucG5n?x-oss-process=image/format,png)
由于函数|f(x)−yi||f(x) -y_{i}|存在尖点，不可导，因此采用|f(xi)−yi|2|f(x_{i}) -y_{i}|^{2}来替代其原有的距离，因此，对于数据集DD，我们希望最小拟合直线与数据点之间的距离，构造如下损失函数：

```math
    L = min \sum_{i=1}^{m} |f(x_{i}) -y_{i}|^{2} \Leftrightarrow min \sum_{i=1}^{m} (f(x_{i}) -y_{i})^{2} =  min \sum_{i=1}^{m} ( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2}
```
$$ L = min \sum_{i=1}^{m} |f(x_{i}) -y_{i}|^{2} \Leftrightarrow min \sum_{i=1}^{m} (f(x_{i}) -y_{i})^{2} =  min \sum_{i=1}^{m} ( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2} $$

为了方便数学运算，习惯上将损失函数LL写作：
```math
    L = min \ \frac{1}{2} \sum_{i=1}^{m} ( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2} \\

    (\boldsymbol{\omega^{*}}，\boldsymbol{b^{*}})= \underset{\omega,b}{arg} \ min \ L= \underset{\omega,b}{arg} \ min \ \frac{1}{2} \sum_{i=1}^{m} ( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2}
```

我们从损失函数LL中取出一项(ωxi+b−yi)2(\boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2}，分别求其关于ω\boldsymbol{\omega}和b\boldsymbol{b}的偏导，就有：
```math
    \frac{\partial (\boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2}}{\partial \boldsymbol{\boldsymbol{\omega}}} = 2x_{i}( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})  \\

    \frac{\partial (\boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})^{2}}{\partial \boldsymbol{b}} = 2( \boldsymbol{\omega}x_{i} + \boldsymbol{b} -y_{i})
```

因此就有:
```math
    \frac{\partial L}{\partial \boldsymbol{\omega}} =  \frac{1}{2} [ 2x_{1}( \boldsymbol{\omega}x_{1} + \boldsymbol{b} -y_{1}) + 2x_{2}( \boldsymbol{\omega}x_{2} + \boldsymbol{b} -y_{2}) +，...，+ 2x_{m}( \boldsymbol{\omega}x_{m} + \boldsymbol{b} -y_{m})]
```

整理一下：
```math
    \frac{\partial L}{\partial \boldsymbol{\omega}} = \boldsymbol{\omega}(x_{1}^{2} + x_{2}^{2} +，...，+x_{m}^{2}) + \boldsymbol{b}(x_{1} + x{2} +，...，+x_{m}) - (x_{1}y_{1} + x_{2}y_{2} + ，...，+x_{m}y_{m})  \\

    \frac{\partial L}{\partial \boldsymbol{\omega}} = \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}^{2}  + \boldsymbol{b}\sum_{i =1}^{m}x_{i} -\sum_{i =1}^{m}x_{i}y_{i}
```

同样的，我们对b\boldsymbol{b}求偏导就有：
```math

    \frac{\partial L}{\partial \boldsymbol{b}} = \frac{1}{2} [ 2( \boldsymbol{\omega}x_{1} + \boldsymbol{b} -y_{1}) + 2( \boldsymbol{\omega}x_{2} + \boldsymbol{b} -y_{2}) +，...，+ 2( \boldsymbol{\omega}x_{m} + \boldsymbol{b} -y_{m})]

```

整理一下：

```math
    \frac{\partial L}{\partial \boldsymbol{b}} = \boldsymbol{\omega}(x_{1} + x_{2} +，...，+x_{m}) + \boldsymbol{b}(1 + 1 +，...，+1) - (y_{1} + y_{2} + ，...，+y_{m}) \\

    \frac{\partial L}{\partial \boldsymbol{b}} = \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}  + \boldsymbol{b}\sum_{i =1}^{m}1 - \sum_{i =1}^{m}y_{i}
```

分别令其偏导等于0，联立方程组求解可得：
```math 
    \begin{cases}
    &\frac{\partial L}{\partial \boldsymbol{\omega}} = \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}^{2}  + \boldsymbol{b}\sum_{i =1}^{m}x_{i} -\sum_{i =1}^{m}x_{i}y_{i} = 0    \ \ \   \text{(1)}\\
    &\frac{\partial L}{\partial \boldsymbol{b}} = \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}  + \boldsymbol{b}\sum_{i =1}^{m}1 - \sum_{i =1}^{m}y_{i} =0    \ \ \   \text{(2)}
    \end{cases}
```

通过$(2)$式我们可以求得：
```math
    \boldsymbol{b} = \frac{ \sum_{i =1}^{m}y_{i} -  \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}}{\sum_{i=1}^{m} 1} = \frac{\sum_{i=1}^{m} (y_{i} - \boldsymbol{\omega x_{i}}) }{m}  \ \ \   \text{(3)}

```

将$(3)$式代入$(1)$式可以得到：
```math
    \boldsymbol{\omega}\sum_{i =1}^{m}x_{i}^{2}  + \frac{\sum_{i=1}^{m} (y_{i} - \boldsymbol{\omega x_{i}}) }{m} \sum_{i =1}^{m}x_{i} -\sum_{i =1}^{m}x_{i}y_{i} = 0

```

将$\boldsymbol{\omega}$提出来，就有：
```math
    \boldsymbol{\omega}(\sum_{i =1}^{m}x_{i}^{2} - \frac{1}{m}(\sum_{i =1}^{m}x_{i})(\sum_{i =1}^{m}x_{i})) = \sum_{i =1}^{m}x_{i}y_{i}  - \frac{1}{m}(\sum_{i=1}^{m} y_{i})(\sum_{i =1}^{m}x_{i})

```

因此可以解得：
```math
    \boldsymbol{\omega} = \frac{\sum_{i =1}^{m}x_{i}y_{i}  - \frac{1}{m}(\sum_{i=1}^{m} y_{i})(\sum_{i =1}^{m}x_{i})}{\sum_{i =1}^{m}x_{i}^{2} - \frac{1}{m}(\sum_{i =1}^{m}x_{i})(\sum_{i =1}^{m}x_{i})}

```

如果令$\bar{x} =\frac{1}{m} \sum_{i=1}^{m}x_{i}$

则可以得到：
```math
    \boldsymbol{\omega} =\frac{\sum_{i=1}^{m} y_{i}(x_{i} - \bar{x})}{\sum_{i =1}^{m}x_{i}^{2} - \sum_{i=1}^{m}x_{i} \bar{x}}

```

# 2.多元线性回归
给定数据集$D=\{ (\boldsymbol{x_{1}}， y_{1}), (\boldsymbol{x_{2}}， y_{2})，...，(\boldsymbol{x_{m}}, y_{m})\}$，不同于一元线性回归，多元线性回归中，每一个$\boldsymbol{x_{i}}$都是一个$n$维特征向量，即$\boldsymbol{x_{i}} = (x_{i}^{1}，x_{i}^{2}，x_{i}^{3}，...，x_{i}^{j}，...，x_{i}^{n})$，其中$x_{i}^{j}$表示第$i$个样本的第$j$个特征值，如果我们将所有的样本特征值放入一个矩阵，将会得到如下：
```math
    \begin{bmatrix}
    \boldsymbol{x_{1}} \\
    \boldsymbol{x_{2}} \\
    ...\\
    \boldsymbol{x_{m}}
    \end{bmatrix}_{m \times 1} = \begin{bmatrix}
    x_{1}^{1}& x_{1}^{2} & ，...，& x_{1}^{n} \\ 
    x_{2}^{1}& x_{2}^{2} & ，...，& x_{2}^{n} \\ 
    ...& ... & ，...，& ... \\ 
    x_{m}^{1}& x_{m}^{2} & ，...，& x_{m}^{n}
    \end{bmatrix}_{m \times n}
```

因此我们希望寻找到这样一条直线去拟合或者说靠近$y$：
```math
    f(x) =\boldsymbol{b} + \omega_{1} x^{1} + \omega_{2} x^{2}+，...，+\omega_{n} x^{n} \simeq y

```

为了方便矩阵表示，不妨令$\boldsymbol{b} = \omega_{0}$，$x_{i}^{0} = 1$则就有：
```math
    f(x_{i}) =\omega_{0} x_{i}^{0} + \omega_{1} x_{i}^{1} + \omega_{2} x_{i}^{2}+，...，+\omega_{n} x_{i}^{n} \simeq y_{i}

```

为了方便数学运算，我们将其表示为矩阵形式：
```math
    f(x_{i}) =  
    \begin{bmatrix}
    x_{1}^{0}&x_{1}^{1}& x_{1}^{2} & ，...，& x_{1}^{n} \\ 
    x_{2}^{0}&x_{2}^{1}& x_{2}^{2} & ，...，& x_{2}^{n} \\ 
    ...&...& ... & ，...，& ... \\ 
    x_{m}^{0}&x_{m}^{1}& x_{m}^{2} & ，...，& x_{m}^{n}
    \end{bmatrix}_{m \times (n+1)} \times \begin{bmatrix}
    \omega_{0} \\
    \omega_{1} \\
    ...\\
    \omega_{n}
    \end{bmatrix}_{(n+1) \times 1} \simeq \begin{bmatrix}
    y_{1} \\
    y_{2} \\
    ...\\
    y_{m}
    \end{bmatrix}_{m \times 1}
```

其中$x_{i}^{0} =0$，分别令三个字母$X，\boldsymbol{\omega}，Y$，上式则表示为如下：
```math
    f(x_{i}) = X \boldsymbol{\omega} \simeq Y

```

与一元线性回归类似，我们构造一个损失函数$L$，并将其最小化以求得$\boldsymbol{\omega}$：
```math
    \boldsymbol{\omega^{*}} = \underset{\boldsymbol{\omega}}{arg} \ min \ L

```
参考一元线性回归：
```math
    L = ||X \boldsymbol{\omega} - Y||^{2} = (X \boldsymbol{\omega} - Y)^{T}(X \boldsymbol{\omega} - Y)
```

将$(X \boldsymbol{\omega} - Y)^{T}(X \boldsymbol{\omega} - Y)$展开：

```math
    L = (\boldsymbol{\omega}^{T} X^{T} - Y^{T})(X \boldsymbol{\omega} - Y)  \\

    L = \boldsymbol{\omega}^{T} X^{T} X \boldsymbol{\omega} - \boldsymbol{\omega}^{T} X^{T} Y - Y^{T} X \boldsymbol{\omega} + Y^{T}Y
```

对于矩阵相乘，有如下说明：
```math
    X_{m \times (n+1)} \Rightarrow X_{(n+1) \times m}^{T} \\

    \boldsymbol{\omega}_{(n+1) \times 1} \Rightarrow \boldsymbol{\omega}_{1 \times (n+1)}^{T} \\

    Y_{m \times 1} \Rightarrow Y_{1 \times m}^{T}
```

因此，对于$\boldsymbol{\omega}^{T} X^{T} Y$ 和$Y^{T} X \boldsymbol{\omega}$就有：
```math
    \boldsymbol{\omega}_{1 \times (n+1)}^{T} X_{(n+1) \times m}^{T} Y_{m \times 1} = (\boldsymbol{\omega}^{T} X^{T} Y)_{1 \times 1} \\

    Y_{1 \times m}^{T} X_{m \times (n+1)} \boldsymbol{\omega}_{(n+1) \times 1} = (Y^{T} X \boldsymbol{\omega})_{1 \times 1} \\

    Y_{1 \times m}^{T}Y_{m \times 1} = (Y^{T}Y)_{1 \times 1}
```


据此，就可以将$L$化简为如下：
```math
    L = \boldsymbol{\omega}^{T} X^{T} X \boldsymbol{\omega} - 2\boldsymbol{\omega}^{T} X^{T} Y + Y^{T}Y

```
对$\boldsymbol{\omega}$求偏导：
```math
    \frac{\partial L}{\partial \boldsymbol{\omega}} = \frac{\partial (\boldsymbol{\omega}^{T} X^{T} X \boldsymbol{\omega})}{\partial \boldsymbol{\omega}} - 2X^{T}Y

```

由于
```math
    \frac{d(U^{T}V)}{dx} = \frac{d U^{T}}{dx} V + \frac{d V^{T}}{dx} U \Rightarrow \frac{d (\boldsymbol{\omega}^{T}\boldsymbol{\omega})}{d\boldsymbol{\omega}} = \frac{d \boldsymbol{\omega}^{T}}{d\boldsymbol{\omega}} \boldsymbol{\omega} + \frac{d \boldsymbol{\omega}^{T}}{d\boldsymbol{\omega}} \boldsymbol{\omega} = 2\boldsymbol{\omega}

```

那么，如果给定$A$是一个方阵，就有：
```math
    \frac{d (\boldsymbol{\omega}^{T} A \boldsymbol{\omega})}{d\boldsymbol{\omega}} = \frac{d \boldsymbol{\omega}^{T}}{d\boldsymbol{\omega}} A \boldsymbol{\omega} + \frac{d( \boldsymbol{\omega}^{T} A^{T})}{d\boldsymbol{\omega}} \boldsymbol{\omega} = (A + A^{T})\boldsymbol{\omega}

```

由于$(X_{(n+1) \times m}^{T} X_{m \times (n+1)}) = (X^{T}X)_{(n+1) \times (n+1)}$，是一个方阵，不妨令$A = X^{T}X$，则：
```math
    \frac{\partial (\boldsymbol{\omega}^{T} X^{T} X \boldsymbol{\omega})}{\partial \boldsymbol{\omega}} = (X^{T}X + (X^{T}X)^{T}) \boldsymbol{\omega} = 2X^{T}X\boldsymbol{\omega}
```

所以：
```math
    \frac{\partial L}{\partial \boldsymbol{\omega}} = 2X^{T}X\boldsymbol{\omega} - 2X^{T}Y
```

令$\frac{\partial L}{\partial \boldsymbol{\omega}} = 0$，求出$\boldsymbol{\omega^{*}}$得：
```math
    \boldsymbol{\omega^{*}} = (X^{T} X)^{-1} X^{T} Y
```


