# 水箱水位控制系统（模糊PID控制器）

![](F:\open_github\fuzzy_control\photo\001.jpg)

## 系统设定

容积：130升

进水电机模型（系统传递函数）：二次曲线型 Y = 0.75X*X；【0，7】有点简单 ——  改为 **2/（6x^+5x+2）**

放水（目标值）：

- PWM（脉宽调制波）
- 周期10s，占空比50%，振幅（未知）

## 系统设计

![](F:\open_github\fuzzy_control\photo\002.jpg)

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

![](F:\open_github\fuzzy_control\photo\003.jpg)

收敛速度

### 规则

|      | NB       | NS       | ZE       | PS       | PB       |
| ---- | -------- | -------- | -------- | -------- | -------- |
| NB   | PB NB PS | PB NB ZE | PB NB ZE | PS NB ZE | ZE ZE PS |
| NS   | PB NB NB | PS NS NB | PS NS NS | ZE ZE ZE | NS ZE PS |
| ZE   | PS NS NB | PS NS NB | ZE ZE NS | NS PS ZE | NS PS PS |
| PS   | PS NS NB | ZE ZE NS | NS PS NS | NS PS ZE | NB PB PS |
| PB   | ZE ZE PS | NS PS ZE | NS PS ZE | NB PB ZE | NB PB PS |

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWViYjczZTMzN2Q0ZDkwMmVmMjYwMWRjZTQ1OGFhNmVfMTM3VlZVM2p6SUNzWW54VWpaMUdwSUttcFhIVTJrOHVfVG9rZW46UGpBNGJkQzZkb1VMTnF4V0JjVGNnMWhUbmI3XzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

Matlab

添加规则

专家系统？根据工程经验计算规则

### matlab建模

- 利用fuzzy工具箱

多输入，多输出

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=NjE1MTI4NzE4ZDBiMDBiNDUwOTlkOGZkZDE5Mjk3MGVfeE16ZVczMUhlMzBvRDZRaWtDOW9rVlllZFNnczg2REVfVG9rZW46RWs1NGJXVlRLb0dRS0l4OFYwN2NvWEIzbm1iXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

均采用三角形，为隶属函数（计算论域简单，方便）

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=MmNjMjVmMGRkMjI0MGY5Y2VmOTc2MjQ1Zjg4MDM4OTBfRXFJZ0JIMXNCcE12eEdBSWlhdFB4VVhkR0ZGQzNZYUlfVG9rZW46UnlIOWJrTmZDb0FQaVl4a3RZYmNQRFZlbksyXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

模糊规则借鉴论文

递进，逼近

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=MDBjYTIwZTQ1MjU2ZjUzM2IzNzZhZmJkZThlY2JhMmRfaFlqSnYzZnZ2N21uQVY1TTU3dU1UYWZWR1R3V2x4SXBfVG9rZW46SGhXR2JHSFEyb3FkQzl4YnppQ2NPeWNsbmlmXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

去模糊化

取重心计算简单方便

**模型**

上半部分：定参数pid控制

下半部分：模糊pid参数控制

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmVjZDkwOTg0YzYzMDcyZWNmNjM3YmQzNWI2MTM0NzVfV3J6Y1d1WHVCdGFGbXoySjJSWGRuNnlNR2M3Q21XSnpfVG9rZW46Q2xnWmJ1VGtub3V1VUV4ZVhIQ2N6N0dUbkNmXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

### PID

经典控制理论，无模型控制

暂时无法在飞书文档外展示此内容

现在，过去，将来

比例：

积分：

微分：

## 参数整定

#### 第一步：P，添加比例系统响应，在目标值附近振荡

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=MWYxNDBlOTA2Y2FiYWJjZmNkNjVmM2M3YmNmOTU3YjVfYkZucHpOV29lNzI1ZHU3UXpBR20xekJlMDM0Y0pobllfVG9rZW46WGtHbGJ1TG5Xb2ptS2x4b1Z2RWNxTVByblhlXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

#### 第二步:D,添加微分阻尼抑制，线条平缓

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=NWQzZWI0YjM2MWRmYTY3NWM4NjQ0MGVlMTFhOGZiM2Ffc2RSWHdHcEtjb2xMOUpKY1VDOTE1dlBxU2RkdjBaVWNfVG9rZW46SldVM2JyMkc5b2ZpM3Z4TXN2emMzc09zbkJmXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

#### 第三步：I，引入积分，消除静态误差

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=NDdlNGQ3MTQ1OGEyOTE1Y2Q0MWFlYWMzZDY2OTMzODVfemQ3QzhYSlJMU3dxamNuVXNaS1ZoMk9uWDVjSTZVUlBfVG9rZW46QTJ5OGIzMFp6b3Q1N2p4cFJRMmNoMFVpbnloXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

#### 参数整定完成

蓝线：普通PID

红线：模糊PID

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=YTEwMjUxMmQ3ZGNiZWY0ODIxZjhlZjBjYTg5ZDIyNmJfRUhVb2xISm9xRWNBNlhSWm12YTFRWFVqMUtHU2NyNFVfVG9rZW46QlBhTGJDUFZsb0x3c3l4ZXVrZWN3Zm1nbmtoXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

## 现象结论

#### 不同信号下两种方法动态表现

1. 振幅0-21

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=ODUwMzgyZTg3ZjI2OTlhN2NiNjdjZjZlZTMyY2E1OGNfQWYycFc4MDdxbWJpQUViZndrbnlCNWVxemdYUHU2dE5fVG9rZW46QzR3b2JETmYyb1VCOTR4ck1wcGNhd1FVblhnXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=MjkwYTlmZjMzZjgwZjRlMjhkMWYwMTdlNWE2MjMxYjVfNVl6ZDNOSU5HcmhNdExWaVBuOFFuYnVrc2F6bTB1VnRfVG9rZW46TWNpaGJWd1hOb045VWt4YjZqZWN3dUpEbkZTXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

1. 振幅0-4                                                           

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTc3MThhODQzMmY3ZjA4YWI2YjM3MTg5YmMyMzNkYTRfU004UXRqTTV5MXdMSlRiMTBVUjVkVjJxSUk5WjVVN2VfVG9rZW46VlRJMWJCOWN0b1hRUUp4NGMzOGNZY2Z2bkIxXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2Q3MzEwNDM4YzA2ZTJkNTIzYTU5Mzc1OGQ0NTNkYWFfSGwweTNHanA5dTgweXo1UHZJRU10QU5qOGVDSVZDbmhfVG9rZW46Tm1nWmJwN2cwb1NkMVR4T1dubWNTQjEybkVnXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

**对比其控制效果差距微乎其微**

分析上方控制效果原因

- 系统**固定**的传递函数，对应着一个最优的PID控制区间参数
- 所谓模糊PID，就是变PID参数（上述动态调节就在最优控制区间内）

#### 当系统传递函数改变

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=ODZkNjc0MjIxYWQxNjUyYzc0NDNiOTJjOWJhM2Y0N2JfSFNjYUJnTkRmM0U2SXlBV3RoWGlST2lYYmc0MlBVQWtfVG9rZW46SkFmc2J3aTRxb1lxaGZ4VnZYWWNEQTRqbkdlXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=MzYyNDc0YmM5M2ExNDc3NTlkYjllYTFiNmRmZjc5NGZfNFlKMWRWSktSeEZXMXdyTFB3S1pOWTRXQTVJY2RnNElfVG9rZW46UE04T2JZT1BibzEyT0J4WTQzZWNLUmQxbndnXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)![img](https://k10kkja70f2.feishu.cn/space/api/box/stream/download/asynccode/?code=YzE3ZTFiMWQzZjQ5MTAyOWU1ZDY1MTg4ZGVlMzUyYTRfNXBkMEs4UXhlOGJ0Y21xOUxqcVFMeldiVkFDTlRzNHhfVG9rZW46V2VWUGJPV2Vrb2EwMk54WERTdGNDZ2ZFbjJkXzE3MjM4MjE1MTg6MTcyMzgyNTExOF9WNA)

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