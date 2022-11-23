



## 数组

增、删、改、查  

```
# 插入
t = [1,2,3]
t.insert(0, 2)
# 结果 [2,1,2,3]

# 删除最后一个元素
arr = [0, 5, 2, 3, 7, 1, 6]
arr.pop()

# 删除某个元素
arr = [0, 5, 2, 3, 7, 1, 6]
i = 3
arr.pop(i)

# 删除某个定值元素
arr.remove(5)
```

## 冒泡排序 Bubble Sort

https://algo.itcharge.cn/01.Array/02.Array-Sort/01.Array-Bubble-Sort/  

**基本思想**：第 i (i = 1，2，… ) 趟排序时从序列中前 n - i + 1 个元素的第 1 个元素开始，相邻两个元素进行比较，若前者大于后者，两者交换位置，否则不交换。  

冒泡排序法是通过相邻元素之间的比较与交换，使值较小的元素逐步从后面移到前面，值较大的元素从前面移到后面，就像水底的气泡一样向上冒，故称这种排序方法为冒泡排序法。  

复杂度: $O(n^2)$  

优点：不会改变值相同元素的相对位置，因此，冒泡排序法是一种 **稳定排序法**  
缺点：效率低  

```
class Solution:
    def bubbleSort(self, arr):
        for i in range(len(arr)):
            for j in range(len(arr) - i - 1):
                if arr[j] > arr[j + 1]:
                    arr[j], arr[j + 1] = arr[j + 1], arr[j]
                    # python中交换两个两个数可以不使用temp暂存
        return arr

    def sortArray(self, nums: List[int]) -> List[int]:
        return self.bubbleSort(nums)
```




## 选择排序 

https://algo.itcharge.cn/01.Array/02.Array-Sort/02.Array-Selection-Sort/  

**基本思想**：第 i 趟排序从序列的后 n − i + 1 (i = 1, 2, …, n − 1) 个元素中选择一个值最小的元素与该 n - i + 1 个元素的最前面那个元素交换位置，即与整个序列的第 i 个位置上的元素交换位置。如此下去，直到 i == n − 1，排序结束。  

**简述**：每一趟排序中，从剩余未排序元素中选择一个最小的元素，与未排好序的元素最前面的那个元素交换位置。  

复杂度: $O(n^2)$  

**缺点**：由于值最小元素与未排好序的元素中第 1 个元素的交换动作是在不相邻的元素之间进行的，因此很有可能会改变值相同元素的前后位置，因此，选择排序法是一种**不稳定的排序方法**  

```
class Solution:
    def selectionSort(self, arr):
        for i in range(len(arr) - 1):
            # 记录未排序序列中最小数的索引
            min_i = i
            for j in range(i + 1, len(arr)):
                if arr[j] < arr[min_i]:
                    min_i = j
            # 如果找到最小数，将 i 位置上元素与最小数位置上元素进行交换
            if i != min_i:
                arr[i], arr[min_i] = arr[min_i], arr[i]
        return arr

    def sortArray(self, nums: List[int]) -> List[int]:
        return self.selectionSort(nums)
```



## 插入排序

https://algo.itcharge.cn/01.Array/02.Array-Sort/03.Array-Insertion-Sort/

**基本思想**：每一趟排序中，将剩余无序序列的第一个元素，插入到有序序列的适当位置上

**时间复杂度**: $O(n^2)$  
**空间复杂度**: $O(1)$, 只需要一个辅助空间 temp

**优点**：是一种 **稳定排序法**  
**缺点**：效率低 

```
class Solution:
    def insertionSort(self, arr):
        for i in range(1, len(arr)):
            temp = arr[i]
            j = i
            while j > 0 and arr[j - 1] > temp:
                arr[j] = arr[j - 1]
                j -= 1
            arr[j] = temp

        return arr

    def sortArray(self, nums: List[int]) -> List[int]:
        return self.insertionSort(nums)
```










## 快速排序 Quick Sort

https://algo.itcharge.cn/01.Array/02.Array-Sort/06.Array-Quick-Sort/

**基本思想**：通过一趟排序将无序序列分为独立的两个序列，第一个序列的值均比第二个序列的值小。然后递归地排列两个子序列，以达到整个序列有序。

**步骤**
- 从数组中找到一个基准数。
- 然后将数组中比基准数大的元素移动到基准数右侧，比他小的元素移动到基准数左侧，从而把数组拆分为左右两个部分。
- 再对左右两个部分分别重复第二步，直到各个部分只有一个数，则排序结束。

**时间复杂度**: 最差 $O(n^2)$，最佳 $O(nlog_2n)$
**空间复杂度**: $O(n)$

**优点**：有时，时间性能显然优于前面讨论的几种排序算法
**缺点**：不稳定，不宜用于链表，占用一定空间

```
import random
class Solution:
    # 从 arr[low: high + 1] 中随机挑选一个基准数，并进行移动排序
    def randomPartition(self, arr: [int], low: int, high: int):
        # 随机挑选一个基准数
        i = random.randint(low, high)
        # 将基准数与最低位互换
        arr[i], arr[low] = arr[low], arr[i]
        # 以最低位为基准数，然后将序列中比基准数大的元素移动到基准数右侧，比他小的元素移动到基准数左侧。最后将基准数放到正确位置上
        return self.partition(arr, low, high)
    
    # 以最低位为基准数，然后将序列中比基准数大的元素移动到基准数右侧，比他小的元素移动到基准数左侧。最后将基准数放到正确位置上
    def partition(self, arr: [int], low: int, high: int):
        pivot = arr[low]            # 以第 1 为为基准数
        i = low + 1                 # 从基准数后 1 位开始遍历，保证位置 i 之前的元素都小于基准数
        
        for j in range(i, high + 1):
            # 发现一个小于基准数的元素
            if arr[j] < pivot:
                # 将小于基准数的元素 arr[j] 与当前 arr[i] 进行换位，保证位置 i 之前的元素都小于基准数
                arr[i], arr[j] = arr[j], arr[i]
                # i 之前的元素都小于基准数，所以 i 向右移动一位
                i += 1
        # 将基准节点放到正确位置上
        arr[i - 1], arr[low] = arr[low], arr[i - 1]
        # 返回基准数位置
        return i - 1

    def quickSort(self, arr, low, high):
        if low < high:
            # 按照基准数的位置，将序列划分为左右两个子序列
            pi = self.randomPartition(arr, low, high)
            # 对左右两个子序列分别进行递归快速排序
            self.quickSort(arr, low, pi - 1)
            self.quickSort(arr, pi + 1, high)

        return arr

    def sortArray(self, nums: List[int]) -> List[int]:
        return self.quickSort(nums, 0, len(nums) - 1)
```








## 二分查找 Binary Search Algorithm

https://algo.itcharge.cn/01.Array/03.Array-Binary-Search/01.Array-Binary-Search/

**二分查找两种思路**
思路 1：「直接找」
思路 2：「排除法」
- 关于边界设置可以记忆为：只要看到 left = mid 就向上取整。或者记为：
    - left = mid + 1 和 right = mid 和 mid = left + (right - left) // 2 一定是配对出现的。
    - right = mid - 1 和 left = mid 和 mid = left + (right - left + 1) // 2 一定是配对出现的。

例子
```
leetcode 35
Given a sorted array of distinct integers and a target value, 
return the index if the target is found. 
If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

Example 1:
Input: nums = [1,3,5,6], target = 5
Output: 2
```

```
# 二分法
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        n = len(nums)
        left = 0
        right = n-1
        
        while left <= right:
            mid = left + (right - left) // 2
            if target == nums[mid]:
                return mid
            elif target > nums[mid]:
                left = mid + 1
            elif target < nums[mid]:
                right = mid - 1

        return left
```



## 枚举算法 Enumeration Algorithm

例子
```
leetcode 204
计数质数
描述：给定 一个非负整数 n。
要求：统计小于 n 的质数数量。
```

```
class Solution:
    def isPrime(self, x):
        for i in range(2, int(pow(x, 0.5)) + 1):
            if x % i == 0:
                return False
        return True

    def countPrimes(self, n: int) -> int:
        cnt = 0
        for x in range(2, n):
            if self.isPrime(x):
                cnt += 1
        return cnt
```












## 递归算法 Recursion

https://algo.itcharge.cn/09.Algorithm-Base/02.Recursive-Algorithm/01.Recursive-Algorithm/

**递归**：指的是一种通过重复将原问题分解为同类的子问题而解决的方法。

**步骤**
- 写出递推公式：找到将原问题分解为子问题的规律，并且根据规律写出**递推公式**。
- 明确终止条件：推敲出递归的终止条件，以及递归终止时的处理方法。**通常情况下，递归的终止条件是问题的边界值**。
- 将递推公式和终止条件翻译成代码：
    - 定义递归函数（明确函数意义、传入参数、返回结果等）。
    - 书写递归主体（提取重复的逻辑，缩小问题规模）。
    - 明确递归终止条件（给出递归终止条件，以及递归终止时的处理方法）。

**避免栈溢出**：1.限制递归调用的最大深度。2.递归算法变为非递归算法（即递推算法）来解决栈溢出的问题。
**避免重复运算**：为了避免重复计算，我们可以使用哈希表、集合、列表等结构来保存已经求解过的 f(k) 的结果。当递归调用用到 f(k) 时，先查看一下之前是否已经计算过结果，如果已经计算过，则直接从对应结构中取值返回，而不用再递推下去，这样就避免了重复计算问题。


```
leetcode 104

描述：给定一个二叉树的根节点 root。
要求：找出该二叉树的最大深度。


分析：根据递归三步走策略，写出对应的递归代码。

写出递推公式：当前二叉树的最大深度 = max(当前二叉树左子树的最大深度, 当前二叉树右子树的最大深度) + 1。
即：先得到左右子树的高度，在计算当前节点的高度。

明确终止条件：当前二叉树为空。

翻译为递归代码：
定义递归函数：maxDepth(self, root) 表示输入参数为二叉树的根节点 root，返回结果为该二叉树的最大深度。
书写递归主体：return max(self.maxDepth(root.left) + self.maxDepth(root.right)) + 1。
明确递归终止条件：if not root: return 0
```

```
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```







## 回溯算法 Backtracking
**回溯及其思想**：
回溯算法采用了一种 「走不通就回退」 的算法思想，通常用简单的递归方法来实现，在进行回溯过程中更可能会出现两种情况：1、找到一个可能存在的正确答案；2、在尝试了所有可能的分布方法之后宣布该问题没有答案。

**步骤**：
- 明确所有选择：画出搜索过程的决策树，根据决策树来确定搜索路径。
- 明确终止条件：推敲出递归的终止条件，以及递归终止时的要执行的处理方法。
- 将决策树和终止条件翻译成代码：
    - 定义回溯函数（明确函数意义、传入参数、返回结果等）。
    - 书写回溯函数主体（给出约束条件、选择元素、递归搜索、撤销选择部分）。
    - 明确递归终止条件（给出递归终止条件，以及递归终止时的处理方法）。

```
leetcode 46
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.
Example:
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # 全局变量，存储路径/当前状态，存储符合条件的结果
        path = []
        res = []
        
        # 定义回溯函数，返回结果可存储于全局变量中
        def backtracking(nums):
            # 明确递归终止条件
            if len(path) == len(nums):
                res.append(path.copy())
            
            # 书写回溯函数主体
            else: 
                for num in nums:
                    if num not in path:
                        path.append(num)
                        backtracking(nums)
                        path.pop()
        
        backtracking(nums)
        return res
```
