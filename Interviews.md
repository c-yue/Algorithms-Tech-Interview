## 数据库 & 数据仓库

https://www.cnblogs.com/amyzhu/p/13513425.html

### 数据库
for customer requirement
tight and clean
比如，某银行某分行一个月发生多少交易，该分行当前存款余额是多少。如果存款又多，消费交易又多，那么该地区就有必要设立ATM了。显然，银行的交易量是巨大的，通常以百万甚至千万次来计算。事务系统是实时的，这就要求时效性，客户存一笔钱需要几十秒是无法忍受的，这就要求数据库只能存储很短一段时间的数据。

### 数据仓库
DW
for analytics 
redundant
而分析系统是事后的，它要提供关注时间段内所有的有效数据。这些数据是海量的，汇总计算起来也要慢一些，但是，只要能够提供有效的分析数据就达到目的了。

### fact table, dimension table
fact tables: what happened, eg.orders
dimension: world info, eg.users' attribute
**星型数据库**: 1.事实(Fact table); 2.维度(Dimension table); 一套星型数据结构，应该只有一个Fact，和多个Dimension，而每个dimension之间是没有任何联系的。



## Hadoop: HDFS and Mapreduce



## AB Test
https://mp.weixin.qq.com/s/0EC-W2Nn82KyD2BK5LQAGw

假设我们有了数据结果，策略A的转换率是10%，策略B的转换率是8%，那我们说策略A比策略B好，这样就可以了吗？不可以，因为可能是抽样误差引起的转换率差异，为了区分实验A和B的差异是由抽样误差引起的？还是本质差别引起的？我们需要做假设验证 (hypothesis testing)。统计学中有很多假设验证方法，例如：

T检验: 也称Student's t test，适用: **样本量较小(如n<30)，总体标准差未知**，正态/近似正态分布的样本。目的: 比较平均值之间差异是否显著。

Z检验: 也称U检验，适用: **大样本量(如n>30)**，**总体标准差已知**，正态/近似正态分布的样本。目的: 比较平均值之间差异是否显著。

F检验: 适用: 正态/近似正态分布的变量。目的:  检验两个正态分布变量的总体方差是否相等。

卡方检验: 也称chi-square test或X2 test，适用: 类别型变量。目的: 检验两个变量之间有无关系，例如性别和是否购买数码产品之间的关系。

我们做AB Test，“如果样本量足够大，那么Z检验和t检验将得出相同的结果。对于大样本，样本方差是对总体方差的较好估计，因此即使总体方差未知，我们也可以使用样本方差的Z检验”。[6]，但正常来说，除非是长期的实验(0.5-1年)，例如算法，会选择Z检验。**正常的短期AB Test基本是实验1个月内甚至说1-2周，那么此时建议选择T检验**。<- **Because the the short term variance is not stable, don't use Z TEST**






### 数据倾斜的处理

参考：https://zhuanlan.zhihu.com/p/64240857
原因：某个key对应太多的数据量
现象：某些key很快，某些key很慢，SQL在hive里前80%任务很快，后面很慢

定位：
- 找找程序里的shuffle操作，如**groupByKey、countByKey、reduceByKey、join**
- 看日志log

解决：
- 更细粒度的聚合
    ```
    select ... from ... group by province_id
    # 细化聚合粒度：
    select ... from ... group by province_id, city_id，date，area_id

    # 尽量去聚合，减少每个key对应的数量，也许聚合到比较粗的粒度之后，
    # 原先有10万数据量的key，现在只有1万数据量。减轻数据倾斜的现象和问题。
    ```

- 提高shuffle操作reduce并行度
    - 将增加reduce task的数量，就可以让每个reduce task分配到更少的数据量，这样的话，也许就可以缓解，或者甚至是基本解决掉数据倾斜的问题。
    - 缺点：治标不治本的意思，用后期的分reduce task来解决倾斜数据的计算，没有从根本上改变数据倾斜的本质和问题。
    - 经验：从无法计算到5小时计算，仍然很慢，考虑下一种方法

- 随机key实现双重聚合
    - 在key内部再切分二级key，先以二级key计算后，再以一级key聚合
    - 使用场景：（1）groupByKey（2）reduceByKey


### 特征选择的方法

### GBDT和XGBoost的不同

### 数学上为什么牛顿法比普通梯度下降更快

### 数据库
数据库调优
B+树
佐：https://github.com/joechan0427/LearnForJob/blob/main/database.md
高性能SQL笔记：https://zhuanlan.zhihu.com/p/36905207



JVM调优
淘宝热门商品信息在JVM哪个内存区域
Java锁的种类及区别







### 快排 & 伪代码

一趟排序将无序序列分为独立的两个序列，第一个序列的值均比第二个序列的值小。然后递归地排列两个子序列，以达到整个序列有序。

```
import random
class Solution:

    def randomPartition(self, arr, low, high):
        # get a random index/num from arr
        i = random.randint(low, high)
        arr[low], arr[i] = arr[i], arr[low]
        # bigger smaller sep
        return self.partition(arr, low, high)
    
    def partition(self, arr, low, high):
        pivot = arr[low]
        # bigger smaller sep
        i = low + 1
        for j in range(i, high+1):
            if arr[j] < pivot:
                arr[i], arr[j] = arr[j], arr[i]
                i += 1
        arr[i-1], arr[low] = arr[low], arr[i-1]
        return  i - 1

    def quickSort(self, arr, low, high):
        pi = self.randomPartition(arr, low, high)
        self.quickSort(arr, low, pi-1)
        self.quickSort(arr, pi+1, high)

    def sortArray(self, nums: list):
        return self.quickSort(nums)
```


