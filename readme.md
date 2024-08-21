# 水箱水位控制系统（模糊PID控制器）

![image](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/001.jpg)

## 系统设定

容积：130升

进水电机模型（系统传递函数）：二次曲线型 Y = 0.75X*X；【0，7】有点简单 ——  改为 **2/（6x^+5x+2）**

放水（目标值）：

- PWM（脉宽调制波）
- 周期10s，占空比50%，振幅（未知）

## 系统设计

![image](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/002.jpg)

### 步骤

A. 确定模糊控制系统输入量与输出量之间关系，

B. 建立模糊规则库，输入变量与输出变量之间模糊关系

C.  确定模糊规则的权重和连接方式；（三角形）

D.  采用模糊推理方法，通过建立的模糊控制系统，构建输入与输出的映射关系；

E. 模糊系统进行仿真验证和优化。 确定模糊控制器的输入、输出变量（调参？）

模糊PID控制？

### 论域

为奇数个：3，5，7

输入参数：水位误差，误差速度

输出参数：

e：

| NB   | NS   | ZO   | PS   | PB   |
| ---- | ---- | ---- | ---- | ---- |
| -20  | -10  | 0    | 10   | 20   |

eu（导，速度）：

| NB   | NS   | ZO   | PS   | PB   |
| ---- | ---- | ---- | ---- | ---- |
| -20  | -10  | 0    | 10   | 20   |

![image](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/t01.png)

收敛速度

### 规则

|      | NB       | NS       | ZE       | PS       | PB       |
| ---- | -------- | -------- | -------- | -------- | -------- |
| NB   | PB NB PS | PB NB ZE | PB NB ZE | PS NB ZE | ZE ZE PS |
| NS   | PB NB NB | PS NS NB | PS NS NS | ZE ZE ZE | NS ZE PS |
| ZE   | PS NS NB | PS NS NB | ZE ZE NS | NS PS ZE | NS PS PS |
| PS   | PS NS NB | ZE ZE NS | NS PS NS | NS PS ZE | NB PB PS |
| PB   | ZE ZE PS | NS PS ZE | NS PS ZE | NB PB ZE | NB PB PS |

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/t02.PNG)

Matlab

添加规则

专家系统？根据工程经验计算规则

### matlab建模

- 利用fuzzy工具箱

多输入，多输出

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/004.png)

均采用三角形，为隶属函数（计算论域简单，方便）

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/005.png)

模糊规则借鉴论文

递进，逼近

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/007.png)

去模糊化

取重心计算简单方便

**模型**

上半部分：定参数pid控制

下半部分：模糊pid参数控制

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/009.png)

### PID

经典控制理论，无模型控制

暂时无法在飞书文档外展示此内容

现在，过去，将来

比例：

积分：

微分：

## 参数整定

#### 第一步：P，添加比例系统响应，在目标值附近振荡

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/pid01.png)

#### 第二步:D,添加微分阻尼抑制，线条平缓

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/pid02.png)

#### 第三步：I，引入积分，消除静态误差

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/pid03.png)

#### 参数整定完成

蓝线：普通PID

红线：模糊PID

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/pid04.png)

## 现象结论

#### 不同信号下两种方法动态表现

1. 振幅0-21

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/014.jpg)

1. 振幅0-4                                                           

![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/015.jpg)

**对比其控制效果差距微乎其微**

分析上方控制效果原因

- 系统**固定**的传递函数，对应着一个最优的PID控制区间参数
- 所谓模糊PID，就是变PID参数（上述动态调节就在最优控制区间内）

#### 当系统传递函数改变


![img](https://github.com/rein-wxy/_fuzzy_control/blob/main/photo/016.jpg)

**模糊****PID****控制器效果明显优于定参数PID**

- 此时最优控制区间改变
- 模糊PID控制器优点主要在于动态调节
- 应对不同状态工况场景
  - 例如水箱水泵，缠上水草，电机老化等，导致系统传递函数改变，都可以应对

**缺点**

- 对于简单系统控制成本大
- 关键在于专家系统（模糊规则）建立，否则其效果远不如定参数稳定

### 参考文献博客

[qikan.cqvip.com](http://qikan.cqvip.com/Qikan/Article/ReadIndex?id=36433026&info=mywmuqlZyS8pO4iq%2f44Qm0JNKqg51pkP%2fDY6T3ksLLg%3d) 基于模糊 PID控制器的控制方法研究

[【串级PID】浅谈串级PID作用及意义——快速理解串级PID结构优势(附图)-CSDN博客](https://blog.csdn.net/ReadAir/article/details/103030418?ops_request_misc=%7B%22request%5Fid%22%3A%22171920938716800186598539%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=171920938716800186598539&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-103030418-null-null.142^v100^pc_search_result_base7&utm_term=串级pid优势&spm=1018.2226.3001.4187)
