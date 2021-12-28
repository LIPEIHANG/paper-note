Task:forecast sea surface temperatrue(SST)
standard physical method: 用ocean-atmosphere耦合的预测系测系统，基于Navier Stokes equations
2. Physical Motivation:
fluid transport poblem
transport = advection(平流） + diffusion(扩散）
advection: 流体传递守恒量I i.e.
   I(x,t)=I(x+deltax, t+deltat）
   对右边进行一阶近似,得到advection equation
   (i.e.亮度约束方程Brightness Constancy Constraint Equation（BCCE）)
    
    
diffusion:流体把数量I从浓度高的地方扩散到浓度低的地方
同时考虑advestion 和 diffusion则BCCE的右边由0变为___
此即advection-diffusion equation

若已知初始条件I_0 , w, 和扩散系数D，则I(x,t)可以解出来


但以上的东西都是未知的，所以我们用一个deep learning的办法发
法来预测SST

3. Model
  - 2 parts：
    1. 根据输入图像,用CNN来估计w(motion)(i.e.预测运动场）
    2. 利用1中的运动场对输入图像进行扭曲，产生图像预测
    （只使用SST的图像监督）
  - notation：
    1.每一个SST的图像I_t都是在一个有边界的矩形R^2中获得的
    2.I_t(x)表示在时间t，位置x处的温度
    3.w_t(x)表示在时间t，位置x处的二维速度向量
    4.目标：给定k个连续的SST图像I_(t-k-1),...,I_t
            预测I_(t+1)
  - 网络结构
    1. 用skip connection(允许第一层的精细信息以更直接的方式流动)
    2. 每个卷积层间用一个池化层
    3. 卷积层和转置卷积层间放一个Leaky ReLU(参数设置为0.1)
    4. 用4个连续图像，i.e. k=4 作为输入训练
  - warping Scheme（挠曲）
    1. 使用warping Scheme 可以让我们对速度场进行弱监督
    2. 假设t+1位于x,则t时位于x-w,把高斯分布的中心放在x-w
       根据每个点到x-w的距离计算其权重
       由以上提到的方程的解
       I_(t+1)=Sigma[k(x-w(x),y]I_t(y),(把I_t作为初始值)
    3. 可适用于任何发生了平流和扩散的问题
    4. warping Scheme是收到了Spatial Transformer Network（STN）（作为卷积神经网络的一个层，获得几何变换下的不变性）的启发
   - Loss function

4.Experiments
  1. Dataset description
  2. 
            
    
