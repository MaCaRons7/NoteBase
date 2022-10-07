# 旅行商(TSP)问题求解算法合集

来源：https://zhuanlan.zhihu.com/p/467270686

[https://zhuanlan.zhihu.com/p/467270686]()

## TSP问题的定义

-   输入：点的集合(代表城市)  
    
-   目标：遍历所有点获得的路径总长度最小化(起终点为同一个点)  
    

## TSP求解算法——近似解求解技术

### 1.最近邻算法

```
1.  V = {1, ..., n-1}    // 除 0 以外的顶点
2.  U = {0}              // 顶点 0
3.  while V not empty
4.    u = 最近添加到U的顶点
5.      找到V中离u最近的顶点v
6.      将顶点v加入U中，并从V中移除
7.  endwhile
8.  根据加入U的顺序输出顶点
```

### 2.Clarke-Wright 启发式

思路：

![](https://pic4.zhimg.com/v2-2fa40303c10c56dc5b09010a55b0653f_b.jpg)

C-W启发式思路

算法：

``` 
1.  定义一个中心顶点h
2.  V_H = V - {h}
3.  for each i,j != h
4.      计算savings(i,j)
5.  endfor
6.  sortlist = 根据savings对顶点对(vertex pairs)进行降序排序
7.  while |V_H| > 2
8.    按sortlist列表顺序尝试顶点对(i,j)
9.      if (i,j)的捷径没有创建一个环,并且所有的顶点v的degree(v) <= 2
10.     将(i,j)端添加到部分回路中
11.         if degree(i) = 2
12.             V_H = V_H - {i}
13.         endif
14.         if degree(j) = 2
15.             V_H = V_H - {j}
16.         endif
17.     endif
18.  endwhile
19.  将剩余的两个顶点和中心连在一起，形成最终的回路
```

### 3.MST启发式

思路：

![](https://pic3.zhimg.com/v2-b8fec346a5549a1b1622545101029486_b.jpg)

MST启发式思路part1

![](https://pic1.zhimg.com/v2-99bf8e24b3c4dc3e586adec5554ffb1c_b.jpg)

MST启发式思路part2

算法：

1.  首先找到最小生成树（任何 MST 算法）。
2.  选择任一顶点作为树的根。
3.  前序遍历最小生成树。
4.  按前序遍历顺序返回顶点。

### 4.Christofides启发式

思路：

![](https://pic1.zhimg.com/v2-3ae4dbc6e8efc7a5be17f0c7e04b2d00_b.jpg)

Christofides启发式思路

### 5.K-OPT

5.1 constructive vs. 局部搜索：

以上四个启发式方法都是_constructive_(逐步建立回路)。相反，局部搜索启发式方法如下：

![](https://pic2.zhimg.com/v2-a45f76e796b8669951b638a0550ea509_b.jpg)

K-OPT

```
1.   T = 一些tour(初始解)      // 比如用Christofides启发式方法.
    2.   noChange = true
    3.   repeat
    4.      T' = makeChangeTo (T)
    5.      if T' < T
    6.          T = T'
    7.          noChange = false
    8.      endif
    9.   until noChange
    10.  return T
```

5.2 2-OPT:

思路：

-   找到两条边和对应的端点
-   交换端点

![](https://pic1.zhimg.com/v2-9f17bdf864d94c7a8a6e8657b767cab0_b.jpg)

2-opt示例

算法1

```
1.   T = 一些tour(初始解)
    2.   noChange = true
    3.   repeat
    4.      for all T中可能的边-对
    5.         T' = 交换边-对中的端点获得的tour
    6.         if T' < T
    7.             T = T'
    8.             noChange = false
    9.             break      // 一旦发现改进就退出循环
    10.        endif
    11.     endfor
    12.  until noChange
    13.  return T
```

算法2(通过所有可能的交换找到最佳游览)

```
1.   T = 一些tour(初始解)
    2.   noChange = true
    3.   repeat
    4.      Tbest = T
    5.      for all T中可能的边-对
    6.         T' = 交换边-对中的端点获得的tour
    7.         if T' < Tbest
    8.             Tbest = T'
    9.             noChange = false
    10.        endif
    11.     endfor
    12.     T = Tbest
    13.  until noChange
    14.  return T
```

5.3 K-OPT

-   K-OPT 是通过考虑替换 3 个边来获得的
-   对于 K > 3， K-OPT 都可能很耗时。

### 6.局部最优和问题图景(Problem Landscape)

**局部最优**

![](https://pic4.zhimg.com/v2-2048e64dd1be09507b59a08def0ec13b_b.jpg)

局部最优

**问题图景**

![](https://pic2.zhimg.com/v2-83f240b0bb6008b8bee894a84bc130d5_b.jpg)

问题图景

以不同的起点多次重新运行局部搜索有助于跳出局部最小值

### 7.禁忌搜索

禁忌（Tabu Search）算法是一种亚启发式(meta-heuristic)随机搜索算法，它从一个初始可行解出发，选择一系列的特定搜索方向（移动）作为试探，选择实现让特定的目标函数值变化最多的移动。为了避免陷入局部最优解，TS搜索中采用了一种灵活的“记忆”技术，对已经进行的优化过程进行记录和选择，指导下一步的搜索方向，这就是Tabu表的建立。

**禁忌表的替代方案：**

-   始终添加所有邻域的最小值
-   仅添加局部最小值

**禁忌表过长的处理方案：**

-   删除最近最少使用
-   删除高成本的路径

### 8.模拟退火

**概念**

模拟退火算法来源于固体退火原理，将固体加温至充分高，再让其徐徐冷却，加温时，固体内部粒子随温升变为无序状，内能增大，而徐徐冷却时粒子渐趋有序，在每个温度都达到平衡态，最后在常温时达到基态，内能减为最小。根据Metropolis准则，粒子在温度T时趋于平衡的概率为e(-ΔE/(kT))，其中E为温度T时的内能，ΔE为其改变量，k为Boltzmann常数。用固体退火模拟组合优化问题，将内能E模拟为目标函数值f，温度T演化成控制参数t，即得到解组合优化问题的模拟退火算法：由初始解i和控制参数初值t开始，对当前解重复“产生新解→计算目标函数差→接受或舍弃”的迭代，并逐步衰减t值，算法终止时的当前解即为所得近似最优解，这是基于蒙特卡洛迭代求解法的一种启发式随机搜索过程。退火过程由冷却进度表(Cooling Schedule)控制，包括控制参数的初值t及其衰减因子Δt、每个t值时的迭代次数L和停止条件S。

![](https://pic4.zhimg.com/v2-a24aaa3854c07c0f0813042dd3621e73_b.jpg)

模拟退火示例

**算法**

```
Algorithm: TSP模拟退火
Input: 点的数组

     // 任何tour都可以，例如按输入顺序 
1.   s = 初始tour 0,1,...,n-1

     // 将初始tour视为目前最好的 
2.   min = cost (s)
3.   minTour = s

     // 选择"mobility"的允许的初始温度  
4.   T = selectInitialTemperature()

     // 迭代 "long enough" 
5.   for i=1 to 足够大的数
           // 随机选择一个相邻状态 
6.         s' = randomNextState (s) 
7.         if cost(s') < cost(s)
8.             s = s'
9.             if cost(s') < min
10.                min = cost(s')
11.                minTour = s'
12.            endif
13.        else if expCoinFlip (s, s')
               // 跳到s'即使目标没有变优
14.            s = s'
15.        endif       // 否则保持当前状态
           // 降低温度 
16.        T = newTemperature (T)
17.  endfor

18.  return minTour

Output: best tour found by algorithm


Algorithm: randomNextState (s)
Input: a tour s, an array of integers

    // ... 交换一对随机点 ... 

Output: a tour 


Algorithm: expCoinFlip (s, s')
Input: two states s and s'

1.   p = exp ( -(cost(s') - cost(s)) / T)
2.   u = uniformRandom (0, 1)
3.   if u < p
4.       return true
5.   else
6.       return false

Output: true (if coinFlip resulted in heads) or false
```

**温度问题**

-   初始温度：

-   需要选择一个可以接受大量成本增加的初始温度
-   选择初始T以使概率为0.95(接近1)

-   降低温度方法：

-   乘法递减：T=a\*T,a<1
-   加法递减：T=T-a,a>0
-   逆对数递减：T=a/log(n)

**实践**

-   优点：

-   实施简单。
-   不需要深入了解问题结构。
-   能产生合理的解决方案。
-   如果贪婪做得好，那么退火也会做得很好。

-   缺点

-   不佳的温度设定会妨碍对状态空间的充分探索。
-   需要一些实验。  
    
-   改进方法：

-   始终使用几种不同的起始解决方案重新运行。
-   始终尝试不同的温度选择方式。
-   始终选择初始温度以确保接受高成本跳跃的可能性很高。
-   尝试不同的邻域函数。

**优化**

-   使用 greedyNextState 代替上面的 nextState 函数。  
    
-   优点：保证找到局部最小值。  
    
-   缺点：可能很难或不可能爬出特定的局部最小值：

-   假设我们被困在局部最小值_s_。
-   我们有概率跳转到一个成本更优的状态_s'_。
-   当处于_s'_时，我们很可能会跳回 _s_（除非更好的状态位于“另一边”）。

-   选择一个随机的下一个状态更易于探索，但它可能不容易找到局部最小值。  
    
-   混合 nextState 函数：

-   不要考虑 2-swap 的整个邻域，而是检查邻域的一部分。
-   在迭代期间在不同的邻域函数之间切换。  
    
-   维护“禁忌”列表：

-   为避免跳转到之前已经看到的状态，请维护“已访问”状态的列表并将这些从每个邻域中排除。  
    
-   热循环：

-   定期升高温度并执行“重新启动”。
-   这个想法是为了强制更多地探索状态空间。

### 9. 1-tree下界&Held-Karp下界

这部分介绍两种获取TSP距离下界的技术。也就是说，对于一个给定的实例(V, d)，我们想要找到一个最大的可能数t，使得每个TSP遍历t都能得到 l(T) \\ge t 。1-tree界是一个比较基本的下界，Held-Karp界更精确一点。

**1-tree Bound**

```
1.  在V中选一个节点v0
2.  设r为(V-{V0},d)最小生成树的长度
3.  设s为以v0为顶点的最短的两条边的和，即s=min{d(v0,x)+d(v0,y):x,y属于V-{v0},x不等于y}
4.  输出t:=r+s
```

**Held-Karp Bound**

我们考虑顶点i的顶点权重$\\pi\_i$，定义一个图G‘：

-   $G$'的边和顶点与G相同
-   $e_{ij} =G$图中边(i,j)的权重
-   $c_{ij} =G$‘图中边(i,j)的权重
-   $c_{ij}=e_{ij}+\pi_i+\pi_j$

![](https://pic3.zhimg.com/v2-a2ee4570e1b3b0ba95fa939875d74d52_b.jpg)

Held-Karp下界示例

**算法**

-   令T为1-tree，T'为tour， d\_i^T 为节点i在T中的度，L(T,G)为图G的1-tree的cost，L(T',G)为图G的tour的cost。
-   $W(\pi)=min_TL(T,G)+\sum_{i\in T}(d_i^T-2)\pi_i$
-   最佳的**Held-Karp Bound**为 $max_\pi W(\pi)$

**顶点权重优化**

-   迭代结果为： $\pi_i^{(m+1)}=\pi_i^{(m)}+\alpha^{(m)}(d_i-2)$
-   直观来看是增加1-min-tree degree>2的顶点的权重，减少1-min-tree degree<2的顶点的权重

### 10.Lin-Kernighan算法(LK)

**算法**

![](https://pic3.zhimg.com/v2-d2ecf2ee2a5a0cd3dcf7b241a8313642_b.jpg)

LK算法

### 11.LKH-1算法

1999年的优化，关键思想：

-   使用K=5(实验表明，从4-opt到5-opt的改进比3-到4-opt要好得多)
-   用一组不同的M近邻替换最近的m个近邻
-   定义α-nearness：

-   假设T是长度为L(T)的最小1-tree，T+(i,j)表示一个包含边(i,j)的最小1-tree。那么一条边(i,j)的α-nearness可以定量的被表示为：α(i,j) = L(T+(i,j)) - L(T)

### 12.LKH-2算法

2009年的优化，基于LKH-1改进的关键思想：

-   让K增加到5以上  
    
-   将点分成簇->为每个集群找到最优subtour->组合在一起构成最终的tour  
    

![](https://pic4.zhimg.com/v2-e2c2cc5fdf253f1336490d81e3a5d0fb_b.jpg)

分簇示例

## TSP求解算法——精确解求解技术

**问题定义**

以欧几里德TSP为例：

变量 x\_{ij} 表示有向边(i,j)，令 c\_{ij}=c\_{ji}= 无向边(i,j)的成本

![](https://pic4.zhimg.com/v2-40f7381bc6e6b1e09f75f90028a37a73_b.jpg)

问题示例

可建立如下的TSP整数规划模型：

![](https://pic1.zhimg.com/v2-e943e53fb4dc6050ca25464ed907a6a8_b.jpg)

TSP整数规划模型

第三个约束是无子环约束，还有以下等价形式：

-   DFJ整数模型的该等价约束保证每个非空集合到其补集一定有边相连

- $$
  \ \sum_{i\ \in S}\   \ \ 非空S \newline \sum_{j \ \notin S}x_{ij}\ \newline \ge 1,\ \forall
  $$

-   

-   MTZ整数模型

- $$
  u_i-u_j+(n-1)x_{ij} \le n-2,2\le i = j \le n
  $$



**方法: **

### **1.原始方法**

-   删除整数约束(临时)以获得常规LP，并获得最优解
-   将改解四舍五入到最近的整数

但该解可能不能产生可行解

![](https://pic1.zhimg.com/v2-e56b509fc12bbfaf2901894778f0c2a4_b.jpg)

原始方法非可行解

### **2.分枝定界**

-   首先解释为0-1-IP问题(变量为二进制)  
    
-   考虑一个简单的穷举搜索  
    

![](https://pic3.zhimg.com/v2-cc09e45684311cf24049eb8110a21ef2_b.jpg)

穷举搜索示例

-   可以用多种方式探索树本身：  
    
-   广度优先
-   深度优先
-   成本优先（添加最小总成本的节点）  
    
-   如果一个节点的成本超过了目前最好的tour，则无需进行遍历

![](https://pic2.zhimg.com/v2-eecb7dc3ef733af80322a69161368da5_b.jpg)

分枝定界示例

### **3.割平面**

-   添加约束以将 LP 解强制为整数。  
    
-   通过一系列这样的约束，这样的过程可以收敛到整数解。  
    
-   但是耗费的时间可能较长  
    

![](https://pic2.zhimg.com/v2-0652f0224490a84eb85344004af064a5_b.jpg)

割平面示例

### **4.Gomory 算法**

-   适用于任何 IP 的通用切割平面算法。
-   想法：

-   解决 LP。
-   检查在角点（LP）满足的方程。
-   在涉及这些变量的不等式中舍入为整数。
-   将这些添加到约束中。
-   重复。

-   实际应用时速度较慢

## 参考

【1】[https://www2.seas.gwu.edu/~simhaweb/champalg/tsp/tsp.html](https://www2.seas.gwu.edu/~simhaweb/champalg/tsp/tsp.html)（本文是基于该英文网页做的汉化整理）

【2】Lin S, Kernighan B W. An effective heuristic algorithm for the traveling-salesman problem\[J\]. Operations research, 1973, 21(2): 498-516.

【3】[https://blog.csdn.net/yang2110862/article/details/109498872?spm=1001.2101.3001.6650.4&utm\_medium=distribute.pc\_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4.pc\_relevant\_default&depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4.pc\_relevant\_default&utm\_relevant\_index=5](https://blog.csdn.net/yang2110862/article/details/109498872?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-4.pc_relevant_default&utm_relevant_index=5)