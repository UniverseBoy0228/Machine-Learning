# EM算法

## 1、算法收敛性证明

> EM：最大期望算法
>
> $MLE: \theta_{MLE}=arg \max_{\theta} \log P(x\vert\theta)$
>
> EM算法参数更新规则：$\theta^{(t+1)}=arg \max_{\theta} \int_z \log P(x,z\vert \theta)P(z\vert x,\theta^{(t)})\mathrm{d}z$
>
> 收敛性：$\theta^{(t)}\longrightarrow \theta^{(t+1)},\log P(x\vert\theta^{(t)}) \le \log P(x\vert\theta^{(t+1)})$ 
>
> ---
>
> 证明：
> $$
> \begin{align*}
> \log P(x\vert \theta)=\log P(x,z\vert \theta)-\log P(z\vert x,\theta) \\
> \end{align*}
> $$
>
> $$
> \begin{align*}
> 左式 &= \int_z P(z\vert x,\theta^{(t)}) \log P(x\vert\theta) \mathrm{d}z \\
> &= \log P(x\vert\theta) \int_z P(z\vert x, \theta^{(t)}) \mathrm{d}z \\
> &= \log P(x\vert\theta)
> \end{align*}
> $$
>
> $$
> \begin{align*}
> 右式 &= \int_z P(z\vert x,\theta^{(t)})\log P(x,z\vert\theta)\mathrm{d}z - \int_z P(z\vert x,\theta^{(t)})\log P(z\vert x,\theta)\mathrm{d}z \\
> &= Q(\theta,\theta^{(t)}) - H(\theta,\theta^{(t)})
> \end{align*}
> $$
>
> 因为$\theta^{(t+1)}=arg \max_{\theta} Q(\theta, \theta^{(t)})$
>
> 所以$Q(\theta^{(t+1)}, \theta^{(t)}) \ge Q(\theta, \theta^{(t)})$
>
> 右边取$\theta=\theta^{(t)}$，得$Q(\theta^{(t+1)}, \theta^{(t)}) \ge Q(\theta^{(t)}, \theta^{(t)})$
> $$
> \begin{align*}
> H(\theta^{(t+1)}, \theta^{(t)}) - H(\theta^{(t)}, \theta^{(t)}) &= \int_z P(z\vert x,\theta^{(t)})\log P(z\vert x,\theta^{(t+1)})\mathrm{d}z - \int_z P(z\vert x,\theta^{(t)})\log P(z\vert x,\theta^{(t)})\mathrm{d}z \\
> &= \int_z P(z\vert x,\theta^{(t)})\log \dfrac{P(z\vert x,\theta^{(t+1)})}{P(z\vert x,\theta^{(t)})}\mathrm{d}z \\
> &= -KL(P(z\vert x,\theta^{(t)}) \Vert P(z\vert x,\theta^{(t+1)})) \\
> &\le 0
> \end{align*}
> $$
> 因此$H(\theta^{(t+1)}, \theta^{(t)}) \le H(\theta^{(t)}, \theta^{(t)})$
> $$
> \begin{align*}
> \Longrightarrow Q(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t+1)},\theta^{(t)}) \ge Q(\theta^{(t)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)})
> \end{align*}
> $$
>
> $$
> 即\log P(x\vert\theta^{(t)}) \le \log P(x\vert\theta^{(t+1)})
> $$
>
> 因此EM算法是收敛的
>
> ---

## 2、EM算法公式推导

> $MLE: \theta_{MLE}=arg \max_{\theta} \log P(x\vert\theta)$
>
> $X:Observed\ data$
>
> $Z:Unobserved\ data(latent\ variable)$隐变量
>
> $(X,Z):Complete\ data$
>
> $\theta:Parameters$
>
> ---
>
> $$
> EM公式：\theta^{(t+1)}=arg \max_{\theta} \int_z P(x,z\vert\theta)P(z\vert x,\theta^{(t)})
> $$
>
> $$
> E-Step：E_{z\vert x,\theta^{(t)}}[P(x,z\vert\theta)] = \int_z P(x,z\vert\theta)P(z\vert x,\theta^{(t)})
> $$
>
> $$
> M-Step：\theta^{(t+1)}=arg \max_{\theta} E_{z\vert x,\theta^{(t)}}[P(x,z\vert\theta)]
> $$
>
> ---
>
> 公式推导(1):
> $$
> \begin{align*}
> \log P(x\vert\theta) &= \log P(x,z\vert \theta) - \log P(z\vert x,\theta) \\
> &= \log \dfrac{P(x,z\vert \theta)}{q(z)} - \log \dfrac{P(z\vert x,\theta)}{q(z)}
> \end{align*}
> $$
> 两边求$q(z)$的期望
> $$
> \begin{align*}
> &左式 = \int_z P(z)\log P(x\vert\theta) \mathrm{d}z = \log P(x\vert\theta) \int_z P(z) \mathrm{d}z = \log P(x\vert\theta) \\
> &右式 = \int_z q(z)[\log \dfrac{P(x,z\vert \theta)}{q(z)} - \log \dfrac{P(z\vert x,\theta)}{q(z)}] \mathrm{d}z = ELBO + KL \ge ELBO
> \end{align*}
> $$
> 推导后可知：$\log P(x\vert\theta)$由两部分组成，分别是ELBO和KL。因为KL散度大于等于0，因此$\log P(x\vert\theta)\ge ELBO$
>
> 当$q(z)=P(z\vert x,\theta)$时，$KL(q(z)\Vert P(z\vert x, \theta))=0$
>
> 因此：
> $$
> \begin{align*}
> \theta^{(t)} &= arg \max_{\theta} \log P(x\vert\theta) \\
> &= arg \max_{\theta} ELBO \\
> &= arg \max_{\theta} \int_z q(z)\log \dfrac{P(x,z\vert \theta)}{q(z)} \mathrm{d}z \\
> &= arg \max_{\theta} \int_z P(z\vert x,\theta^{(t)})\log \dfrac{P(x,z\vert \theta)}{P(z\vert x,\theta^{(t)})} \mathrm{d}z \\
> &= arg \max_{\theta} \int_z P(z\vert x,\theta^{(t)}) [\log P(x,z\vert \theta) - \log P(z\vert x,\theta^{(t)})] \mathrm{d}z \\
> &= arg \max_{\theta} \int_z [P(z\vert x,\theta^{(t)}) \log P(x,z\vert \theta) - P(z\vert x,\theta^{(t)}) \log P(z\vert x,\theta^{(t)})] \mathrm{d}z \\
> &= arg \max_{\theta} \int_z P(z\vert x,\theta^{(t)}) \log P(x,z\vert \theta) \mathrm{d}z \\
> \end{align*}
> $$
> 上式中被积函数第二项的$\theta^{(t)}$是$t$时刻已知的，因此与$\theta$无关，则对于优化任务可以忽略。
>
> ---
>
> 公式推导(2):
> $$
> \begin{align*}
> \log P(x\vert \theta) &= \log \int_z P(x,z\vert \theta) \mathrm{d}z \\
> &= \log \int_z \dfrac{P(x,z\vert \theta)}{q(z)} q(z) \mathrm{d}z \\
> &= \log E_{q(z)}[\dfrac{P(x,z\vert \theta)}{q(z)}] \\
> &\ge E_{q(z)}[\log \dfrac{P(x,z\vert \theta)}{q(z)}] = ELBO
> \end{align*}
> $$
> 当$\dfrac{P(x,z\vert \theta)}{q(z)} = C$时，$"="$成立。
> $$
> \Longrightarrow q(z) = \dfrac{1}{C} P(x,z\vert \theta)
> $$
>
> $$
> \int_z q(z) \mathrm{d}z = \int_z \dfrac{1}{C} P(x,z\vert \theta) \mathrm{d}z
> $$
>
> $$
> 1 = \dfrac{1}{C} P(x\vert \theta)
> $$
>
> $$
> \Longrightarrow C = P(x\vert \theta)
> $$
>
> $$
> \Longrightarrow q(z) = \dfrac{P(x,z\vert \theta)}{P(x\vert \theta)} = P(z\vert x,\theta)
> $$
>
> 因此
> $$
> \begin{align*}
> \theta^{(t+1)} &= arg \max_{\theta} \log P(x\vert \theta) \\
> &= E_{q(z)}[\log \dfrac{P(x,z\vert \theta)}{q(z)}] \\
> &= E_{z\vert x,\theta^{(t)}}[\log \dfrac{P(x,z\vert \theta)}{P(z\vert x,\theta^{(t)})}] \\
> &= E_{z\vert x,\theta^{(t)}}[\log P(x,z\vert \theta)]
> \end{align*}
> $$

## 3、广义EM

> 上面EM推导中，$q(z) = P(z\vert x,\theta^{(t)})$，而在实际中$q(z)$很难求，即$q(z)$无法取到$P(z\vert x,\theta^{(t)})$。
> $$
> \log P(x\vert \theta) = ELBO + KL
> $$
> 给定样本数据$x$的情况下，等式左边$\log P(x\vert \theta)$是固定的。因此，最大化$ELBO$即最小化$KL$散度。
>
> 因此，在实际中，应该让$q\longrightarrow p$，此时$KL\longrightarrow 0$。
>
> 广义EM：
> $$
> \begin{align*}
> & E-Step: \quad q^{(t+1)}=arg \max_{q} ELBO=E_{q(z)}[\log \dfrac{P(x,z\vert \theta^{(t)})}{q(z)}] \\
> & M-Step: \quad \theta^{(t+1)}=arg \max_{\theta} ELBO=E_{q^{(t+1)}}[\log \dfrac{P(x,z\vert \theta)}{q^{(t+1)}}]
> \end{align*}
> $$
> 广义EM又称为MM或F-MM，采用的思想实际是坐标上升法(SMO)。

## 4、EM的变种

> VBEM NEM
>
> MCEM
>
> 