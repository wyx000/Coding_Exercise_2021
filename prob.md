https://www.mmbyte.com/article/156114.html


https://www.nowcoder.com/discuss/400248

https://www.nowcoder.com/discuss/428774?type=2&channel=&source_id=1_post

[TOC]

## 智力题
#### 1. 扔鸡蛋 [扔铁球]
100层楼，只有2个鸡蛋，想要判断出那一层刚好让鸡蛋碎掉，给出策略（滴滴笔试中两个铁球跟这个是一类题）
* （给定了楼层数和鸡蛋数的情况）二分法+线性查找  从100/2=50楼扔起，如果破了就用另一个从0扔起直到破。如果没破就从50/2=25楼扔起，重复。
* 动态规划
* 
#### 2. 白鼠试毒药问题
1000瓶水，其中有一瓶可以无限稀释的毒药，要快速找出哪一瓶有毒，需要几只小白鼠
用二进制的思路解决问题。2的十次方是1024，使用十只小鼠喝一次即可。方法是先将每瓶水编号，同时10个小鼠分别表示二进制中的一个位。将每瓶水混合到水瓶编号中二进制为1的小鼠对应的水中。喝完后统计，将死亡小鼠对应的位置为1，没死的置为0，根据死亡小鼠的编号确定有毒的是哪瓶水，如0000001010表示10号水有毒。

> 把老鼠当做10个bit位就行了，可以编码出2^10=1024种喝法，同时把药水编号成为1-1000，凡是编码的2进制编码为1的位值，就要让对应老鼠喝对应号的药水，然后一周后直接通过对应10位bit数得到有毒药水数
> 10只老鼠按顺序排好，每桶酒按照编号转换成二进制，给相应位置上是1的老鼠喝。最后按死掉的老鼠是哪几只，然后排成二进制，再转成十进制就是第几桶酒。
比如：第70桶酒，70转换成二进制就是0001000110，那么就给第四、八、九只老鼠喝。如果最后死掉第三、七、八只老鼠，那么就是0010001100，转换成十进制就是140，即140桶酒有毒。
#### 3.  
#### 4. 先手必胜策略问题：
**100本书，每次能够拿1-5本，怎么拿能保证最后一次是你拿**

寻找每个回合固定的拿取模式。最后一次是我拿，那么**上个回合最少剩下6本**。那么只要保持每个回合结束后都剩下6的倍数，并且在这个回合中我拿的和对方拿的加起来为6（这样这个回合结束后剩下的还是6的倍数），就必胜。

**关键是第一次我必须先手拿（100%6=4）本（这不算在第一回合里面）**。



#### 5. 蚂蚁爬树问题
>放n只蚂蚁在一条树枝上，蚂蚁与蚂蚁之间碰到就各自往反方向走，**问总距离或者时间**。

**碰到就当没发生，继续走，相当于碰到的两个蚂蚁交换了一下身体**。

其实就是每个蚂蚁从当前位置一直走直到停止的总距离或者时间。
  
  
  
#### 6. 瓶子换饮料问题
> 1000瓶饮料，3个空瓶子能够换1瓶饮料，问最多能喝几瓶？


拿走3瓶，换回1瓶，相当于减少2瓶。但是**最后剩下4瓶的时候例外，这时只能换1瓶**。

所以我们计算1000减2能减多少次，直到剩下4.（1000-4=996，996/2=498）所以1000减2能减498次直到剩下4瓶，最后剩下的4瓶还可以换一瓶，

**所以总共是1000+498+1=1499瓶**。


#### 7. 在24小时里面时针分针秒针可以重合几次
24小时中时针走2圈，而分针走24圈，时针和分针重合24-2=22次，而只要时针和分针

忽略秒针重合次数，漏桶原理，决定结果的是转动次数最少的


#### 8. 找砝码问题
> 有一个天平，九个砝码，一个轻一些，用天平至少几次能找到轻的？

至少2次：第一次，一边3个，哪边轻就在哪边，一样重就是剩余的3个；

**第二次，一边1个，哪边轻就是哪个，一样重就是剩余的那个**；


#### 9. 找砝码问题2 
> 有十组砝码每组十个，每个砝码重10g，其中一组每个只有9g，有能显示克数的秤最少几次能找到轻的那一组砝码？

砝码分组1~10，第一组拿一个，第二组拿两个以此类推。。第十组拿十个放到秤上称出克数x，则y = 550 - x，第y组就是轻的那组

**累加后得到重量梯度**

#### 10. 生成随机数问题：
> 给定生成1到5的随机数Rand5()，如何得到生成1到7的随机数函数Rand7()？
思路：由大的生成小的容易，比如由Rand7()生成Rand5()，所以我们先构造一个大于7的随机数生成函数。
记住下面这个式子：
```
RandN^2()= N( RandN()-1 ) + RandN() ;// 生成1到N^2之间的随机数
可以看作是在数轴上撒豆子。N是跨度/步长，是RandN()生成的数的范围长度，RandN()-1的目的是生成0到N-1的数，是跳数。后面+RandN()的目的是填满中间的空隙
```
比如` Rand25= 5( Rand5()-1 ) + Rand5()`可以生成1到25之间的随机数。我们可以只要1到21（3*7）之间的数字，所以可以这么写 
`(RandX - 1)* Y + RandY() -> [1, X*Y]`


```
int rand7(){
  int x=INT_MAX;
  while(x>21){
    x=5*(rand5()-1)+rand5();
  }
  return x%7+1;
}
```
#### 11.赛马问题：
> 有25匹马，每场比赛只能赛5匹，至少要赛多少场才能找到最快的3匹马？
* 第一次，分成5个赛道ABCDE，每个赛道5匹马，每个赛道比赛一场，每个赛道的第12345名记为 A1,A2,A3,A4,A5  B1,B2,B3,B4,B5等等，这一步要赛5场。 
* 第二次，我们将每个赛道的前三名，共15匹。分成三组，然后每组进行比赛。这一步要赛3场。
* 第三次，我们取每组的前三名。共9匹，第一名赛道的马编号为1a,1b,1c，第二名赛道的马编号为2a,2b,2c，第三名赛道的马编号为3a,3b,3c。这时进行分析，1a表示第一名里面的第一名，绝对是所有马中的第一，所以不用再比了。2c表示第二名的三匹里头的最后一匹，3b和3c表示第三名里面的倒数两匹，不可能是所有马里面的前三名，所以也直接排除，剩下1b,1c,2a,2b,,3a，共5匹，再赛跑一次取第一第二名，加上刚筛选出来的1a就是所有马里面的最快3匹了。这一步要赛1场。
* 所以一共是5+3+1=9场。

#### 赛马II


思考题：64匹马，8个跑道，选跑最快的4匹马需要比赛多少次。
( 锦标赛排序算法 ) sum = 11
- 第一步：首先每8匹马跑一次，总共需要8次，假设结果中A1>A2>A3>......,B1>B2>B3>....等。 sum=8；
- 第二步：这8组中的第一名拉出来跑一次，那么这次最快的是总的第一名，假设是A1，同时假设B1>C1>D1。这时还要角逐2,3,4名， 那这一轮中的第五到第八组都可以直接舍弃 ，因为他们所有的马一定进不了前4名。sum=9。

- 第三步：从A组中选A2，A3，A4，B组中B1，B2，B3，C组中C1，C2，D组中D1，这些才有资格角逐2,3,4名。这时需要再比赛两次。 sum=11。（但是如果第10轮选择A4不上场，如果A3获得了第4名，那么A4就不需要比赛了，这样 sum=10 ）

#### 12. 烧香/绳子/其他时间问题 
> 确定时间问题：有两根不均匀的香，燃烧完都需要一个小时，问怎么确定**15分钟的时长**？ 

*本质在求中点/1/4分位*

（说了求15分钟，没说开始的15分钟还是结束的15分钟，这里是可以求最后的15分钟）

点燃一根A，同时点燃另一根B的两端，当另一根B烧完的时候就是半小时，这是再将A的另一端也点燃，从这时到A燃烧完就正好15分钟。

#### 13. 掰巧克力问题 / 辩论问题
> N*M块巧克力，每次掰一块的一行或一列，掰成1*1的巧克力需要多少次？（1000个人参加辩论赛，1V1，输了就退出，需要安排多少场比赛）

每次拿起一块巧克力，掰一下（无论横着还是竖着）都会变成两块，因为所有的巧克力共有N\*M块，所以要掰N\*M-1次，-1是因为最开始的一块是不用算进去的。

每一场辩论赛参加两个人，消失一个人，所以可以看作是每一场辩论赛减少一个人，直到最后剩下1个人，所以是1000-1=999场。

#### 14. 一副扑克牌，平均分成三堆，大小王分在同一堆的概率是多大？

17/53

> 一副扑克54张，先把大王拿出来，剩下53张牌，然后分成17，18，18 3份，求小王在17那一份的概率

> 组合数的逻辑： 分成三堆的组合数： M =C(54, (18,18,18)): 54!/(18!18!18!)
> 大小王同时在同一堆的组合数：N = C(52, (16,18,18)) : 52!/ (16! 18! 18!)
> 3 *N / M = 52!18!/ (54! 16!) = 3 * 17 * 18/ (53 * 54) =  **17/53**

#### 15. 圆周率里面是否可以取出任意数字？

因为圆周率是无理数
而无理数是无限并且不循环的
所以会包含所有的数字组

## 概率题
#### 1. 一根木棒，截成三截，组成三角形的概率

设第一段截 x，第二段截 y，第三段 1 - x - y。

考虑所有可能的截法。可能的截法中必须保证三条边都是正数且小于原来边长，则有 0 < x < 1，0 < y < 1，0 < 1 - x - y < 1。

画图可知，(x, y) 必须在单位正方形的左下角的半个直角三角形里，面积为 1 / 2。

然后考虑能形成三角形的截法。首先要满足刚才的三个条件：

0 < x < 1

0 < y < 1

0 < 1 - x - y < 1

 

然后必须符合三角形的边的要求，即两边之和大于第三边：

x + y > 1 - x - y

x + 1 - x - y > y

y + 1 - x - y > x

化简即得：

0 < x < 1/2

0 < y < 1/2

1/2 < x + y < 1

**画图可知，此时 (x, y) 必须在边长为 1/2 的三角形的右上角的半个直角三角形里，面积为 1/8。于是最终概率为 (1/8) / (1/2) = 1/4**

#### 2. 抛硬币吃苹果的概率
>  有一苹果，两个人抛硬币来决定谁吃这个苹果，先抛到正面者吃。问先抛这吃到苹果的概率是多少?

结果： **2/3**

- 可以计算先手者在不同次数上的尝试情况： 第一次： 1/2 第三次 (1/2)^3 第五次 (1/2)^5 ...
- 各种情况加和得到： 等比数列求和的极限： $1/2 *(1-(1/4)^n)/(1-1/4) = 2/3$


其他解释：
- 每一轮 抛硬币，A先抛赢得概率是1/2，B后抛赢得概率是（1/2）*（1/2）= 1/4。
- 那么 **每一轮A赢得概率都是B赢得概率的2倍**，总概率为1,所以A赢的概率是2/3


#### 3. 蚂蚁不相撞的概率
> 一个三角形， 三个端点上有三只蚂蚁，蚂蚁可以绕任意边走，问蚂蚁不相撞的概率是多少

- 首先，每个蚂蚁在方向的选择上有且只有 2 种可能，共有 3 只蚂蚁，所以共有 2 的 3 次方种可能，而不相撞有有 2 种可能，即全为顺时针方向或全为逆时针方向。

- **不相撞概率 = 不相撞 / 全部 = 2/8 = 1/4**

#### 4. 扔筛子问题



#### 5. 随机数生成

> 已知一随机发生器，产生 0 的概率是 p，产生 1 的概率是 1-p，现在要你构造一个发生器，**使得它产生 0 和 1 的概率均为 1/2**（随机数生成）

- 由题目有： 0 : p 1 : 1 - p
- 连续产生两个数，其组合以及概率如下：

> 00 : p2 01 : p(1 - p) 10 : (1 - p) p 11 : (1 - p)(1 - p)

**可以发现 01 和 10 组合的概率是相等的，只需要将其分别映射到 0 和 1 即可**。

- 即每次随机产生两个数，如果组合为00或11则丢弃，若为 01 则映射到 1，若为 10 则映射到 0，这样一来产生 0 和 1 的概率均为 1 / 2 。

#### 6. 随机数生成 II  
> 已知一随机发生器，产生的数字的分布不清楚，现在要你构造一个发生器，使得它产生 0 和 1 的概率均为 1/2

- **本质上都是在人为寻找两个分布相同的状态**
- 使用该随机发生器产生随机数 a，b，有以下 3 种情况：
  - a < b
  - a == b
  - a > b
  - 其中情况（1）和（3）是对称的，发生的概率相等，只需要将这两种情况分别映射到 0 和 1 即可，其中遇到 a == b 时忽略。

- 类似问题：**一硬币，一面向上概率0.7，一面0.3，如何公平？**
  - 抛两次， 正反 A胜， 反正 B胜