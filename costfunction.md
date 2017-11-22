### 根据误差更新权重
在Rosenblatt Perceptron中
使用iris数据集的前100个数据，并且只使用sepal和petal两个特征。<br>
感知机只有三个权重
w1 for feature1, w2 for feature2, w0 for bias。

$$\hat y^{(i)}=\phi(z^{(i)})=w_0+w_1x^{(1)}_1+w_2x^{(i)}_2$$
<br>

$$
\hat y^{(i)}=\phi(z^{(i)})
\begin{cases}
1\ ,\ z^{(i)}>0\\
-1\ ,\ others
\end {cases}
$$

每输入一个样本就得到一个输出，使用这个输出更新一次模型，然后输入下个样本。
按照一个解方程的思想，如果输入一个样本x，想要得到正确输出y，则应满足如下关系

$$y^{(i)}=\hat y^{(i)}=w_0+w_1x^{(i)}_1+w_2x^{(i)}_2$$

那么对每个样本而言，三个权重应该满足如下的等式

$$\begin {cases} 
w_0=y^{(i)}-(w_1x^{(i)}_1+w_2x^{(i)}_2)\\
\\
w_1=\frac {y^{(i)}-(w_0+w_2x^{(i)}_2)}{x^{(i)}_1}\\
\\
w_2=\frac {y^{(i)}-(w_0+w_1x^{(i)}_1)}{x^{(i)}_2}
\end {cases}$$

## 每个样本施加同等的影响到模型上

对于一个样本而言，这个方程的解有无数个，它们只受收这一个样本的影响。对于一个样本集而言，要找出一个通用的解，需要对比每个样本的每个解，这不是我们想要的。<br>
我们希望每个样本同时同等的作用于一个学习结果。于是就有了以下两个公式，用于更新模型。模型初始值是随机的，设定迭代次数，每次迭代每个样本只输入一次，如果这次预测出错，模型就只会更新一次，如果预测没错，模型就不更新。

$$w_j=w_j+\Delta w_j$$
<br>

$$\Delta w_j=-\eta(y^{(i)}-\hat y^{(i)})x^{(i)}_j$$
### 不能无限更新模型，并且应该使训练集的误差收敛。
## 损失函数的出现重新定义权重更新
仍然使用预测与真实值的误差来更新权重，但是使用了更复杂的损失函数，而且因此模型的更新不再是一个样本更新一次，现在是样本集迭代一次更新一次模型。
Adaline中定义了一个损失函数，如下

$$J(\bold w)=\frac {1}{2}\sum_i(y^{(i)}-\hat y^{(i)})^2$$
值越大，误差越大。值越小，误差越小。
权重更新如下

$$\bold w=\bold w+\Delta \bold w$$
权重增量定义如下

$$\Delta \bold w = -\eta \Delta \bold J(\bold w)$$
/**
这套公式是认为定义还是推导而来？但是它的好处显而易见，损失函数的出现使程序能够评估评估模型是否最优，解决学习过程的不能自动收敛的问题，像Perceptron那样就会无限更新模型。
*/
求最优模型的问题也就变成了求函数收敛问题。收敛问题可以通过求极值来解决，但是函数的解析解不一定存在，所以使用梯度下降做收敛。
对每个权重求增量，就要对损失函数求偏导

$$\Delta w_j=-\eta \frac{\partial J}{\partial w_j}=-\eta (-\sum_i (y^{(i)}-\phi (z^{(i)}))x^{(i)}_j)$$
求偏导的过程中需要对激活函数求导，所以要求激活函数可微。

注意求J(w)的偏导，它就是梯度。可以把梯度简单理解为方向导数，方向导数的平行方向上已被证明函数变化率最大，使用梯度下降可以快速找到函数的极小值。
更新权重，使用学习率乘负梯度，得到一个权重增量，更新权重，使权重适当的朝着收敛方向移动。最终找到损失函数的极小值，那么此时的权重就是一个理想的结果。负梯度有利于寻找极小值。