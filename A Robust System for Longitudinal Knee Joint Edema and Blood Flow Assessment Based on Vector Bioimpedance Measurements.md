### A Robust System for Longitudinal Knee Joint Edema and Blood Flow Assessment Based on Vector Bioimpedance Measurements
#### Abstract
我们呈现了一个鲁棒的矢量生物电阻抗测量系统，以纵向评价膝关节健康，可以获取静态（在几小时到几天缓慢变化）和动态（在毫秒级别快速变化）的生物电阻和生物电抗信号。占地面积78 x 90 mm2，需要±5V电源供电，消耗功率0.25 w，前端在0.1-20 Hz带宽内，实现345 欧动态范围，本底噪声为0.018 $m\Omega_{rms}$（对于电阻）与0.055 $m\Omega_{rms}$（对于电抗）。一个微控制器可以实现实时校准以最小化来自实验室外环境变化的误差，并使数据存储在一个微型安全数字卡里。采集到的信号之后利用定制胜利驱动算法来提取肌肉骨骼相关（水肿）和心脑血管相关（局部血容积脉冲）特征。在可行性研究中，我们发现，与七名对照组受试者相比，两名近期单侧膝关节受伤者的患侧与对侧静态膝关节生物电阻抗具有统计学差异。具体来说，患侧膝关节的电阻抗更低，支持了关于水肿增加和细胞膜破损的生理学预期。在第二个可行性研究中，我们通过冷加压实验证明了动态电阻的敏感性，脉动电阻降低20 m欧，下肢血管电阻增加。提出的系统将会作为未来工作的基础，旨在在日常生活中不断量化关节健康状况。


#### 1 Introduction

该技术的其他应用：评估骨关节炎、评价下肢肌肉损伤相关水肿、评价全膝关节置换手术相关水肿及其康复。估计心输出量或肢体的局部血流速度。

动态生物电阻抗的变化频率为0.8-20 Hz。静态生物电阻抗的变化幅度从欧姆到几十欧姆，动态生物电阻抗的变化小到几十欧姆。

关节损伤后局部血流量增加可能与炎症和瘢痕组织形成有关，而局部血流量减少可能表明状态改善。

主要工作：

1. 设计了模拟前端，具有较高分辨率
2. 设计自校准程序
3. 设计生理驱动算法，以为基于IPG的心跳检测算法提供基础

#### 2 System Level Design

微控制器采用TI MSP430。

##### 2.1 Analog Front-End

电流频率50 kHz，幅值800 mVpp。

截止频率为7.2 kHz的差分高通滤波器降低肌电图的干扰。

对静态信号，使用截止频率为2 Hz的Sallen-Key低通滤波器。

对动态信号，使用贷款为0.1-20 Hz，增益为51的带通滤波器。

膝关节的阻抗一般是80 欧，输入电流Ipp一般是2 mA，但也可调整。

##### 2.2 Calibration and Current Control

使用校准系数$c_f=2 mA/I_{pp}$，校准电流的变化。

校准步骤包括相位校准和最小二乘线性回归。

由于解调器开关的非理想切换时间，会导致相位差，因此需要相位校准，方法为
$$
\begin{bmatrix}
	\hat{i}(t)\\
	\hat{q}(t)\\
\end{bmatrix}

=
\begin{bmatrix}
	cos\phi		&	sin\phi\\
	-sin\phi	&	cos\phi\\
\end{bmatrix}

\begin{bmatrix}
	i(t)\\
	q(t)\\
\end{bmatrix}
$$
在相位校准的基础上，进行最小二乘法回归
$$
\begin{bmatrix}
	r(t)\\
	x(t)\\
\end{bmatrix}

=

\begin{bmatrix}
	m_R\\
	m_X\\
\end{bmatrix}

\begin{bmatrix}
	cos\phi		&	sin\phi\\
	-sin\phi	&	cos\phi\\
\end{bmatrix}

\begin{bmatrix}
	i(t)\\
	q(t)\\
\end{bmatrix}

+

\begin{bmatrix}
	c_R\\
	c_X\\
\end{bmatrix}
$$
其中，校准参数通过测量四个阻抗获取，四个阻抗分为别：

1. R1 = 23.8 欧，C1 = 47 nF
2. R2 = 98.2 欧
3. R3 = 56.4 欧，C3 = 94 nF
4. R4 = 75.4 欧，C4 = 68 nF

通过测量R2，校准相位差。

通过测量所有四个电阻，进行最小二乘线性回归。

动态生物电阻抗的矫正方法为
$$
\begin{bmatrix}
	\delta r(t)\\
	\delta x(t)\\
\end{bmatrix}

=

\begin{bmatrix}
	m_R\\
	m_X\\
\end{bmatrix}

\begin{bmatrix}
	cos\phi		&	sin\phi\\
	-sin\phi	&	cos\phi\\
\end{bmatrix}

\begin{bmatrix}
	i(t)\\
	q(t)\\
\end{bmatrix}
$$
为了消除电流漂移的影响，在每次校准之前，调整数字电位计，以保持刺激电流在2 mApp左右。

##### 2.3 Preprocessing and Feature Extraction Algorithms

kljjk'

