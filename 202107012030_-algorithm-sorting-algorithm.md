# [Algorithm] Sorting Algorithm

#algorithm #sorting-algorithm #data-structure

|排序算法|平均时间复杂度|最好时间复杂度|最坏时间复杂度|空间复杂度|排序方式|是否稳定|
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|冒泡排序|$O(N^2)$|$O(N)$|$O(N^2)$|$O(1)$|In-place|稳定|
|选择排序|$O(N^2)$|$O(N^2)$|$O(N^2)$|$O(1)$|In-place|不稳定|
|插入排序|$O(N^2)$|$O(N)$|$O(N^2)$|$O(1)$|In-place|稳定|
|希尔排序|$O(NlogN)$|$O(Nlog^2 N)$|$O(Nlog^2 N)$|$O(1)$|In-place|不稳定|
|归并排序|$O(NlogN)$|$O(NlogN)$|$O(NlogN)$|$O(N)$|Out-place|稳定|
|快速排序|$O(NlogN)$|$O(NlogN)$|$O(N^2)$|$O(logN)$|In-place|不稳定|
|堆排序|$O(NlogN)$|$O(NlogN)$|$O(NlogN)$|$O(1)$|In-place|不稳定|
|计数排序|$O(N+K)$|$O(N+K)$|$O(N+K)$|$O(K)$|Out-place|稳定|
|桶排序|$O(N+K)$|$O(N+K)$|$O(N^2)$|$O(N+K)$|Out-place|稳定|
|基数排序|$O(NK)$|$O(NK)$|$O(NK)$|$O(N+K)$|Out-place|稳定|

N: 数据规模<br>
K: 桶的个数<br>
In-place: 占用常数内容，不占用额外内存<br>
Out-place: 占用额外内存<br>
稳定性: 排序后两个相等的element位置是否变化
## Bubble Sort (冒泡排序)
### 算法步骤
1. 从第一个元素开始，比较两个相邻的元素，并对这两个元素进行排序
2. 对每一对元素都进行步骤1，直到最后一对元素，那么此时，最后一个元素将会是最大的元素
3. 在完成步骤2之后，对剩余的N-1个元素进行步骤1，并重复此步，每当一个iteration完成时，我们需要比较的元素就少了一个
4. 持续对剩余元素进行比较，直到全部完成
### 算法动画
![Bubble Sort](figs/bubbleSort.gif)
### Python代码
```python
def BubbleSort(nums:list):
    if len(nums) == 1:
        return nums
    for j in range(len(nums)-1):
        for i in range(len(nums)-1-j):
            if nums[i] > nums[i+1]:
                tmp = nums[i+1]
                nums[i+1] = nums[i]
                nums[i] = tmp
    return nums
```
## 选择排序(Selection Sort)
### 算法步骤
1. 在所有元素中找到最小值，并将该元素放在未排序序列的起始位置
2. 在所有元素中找到最大值，并将该元素放在未排序序列的末尾位置
3. 重复步骤1和2，直到排序完毕
### 算法动画
![Selection Sort](figs/selectionSort.gif)
### Python代码
```python
def SelectionSort(nums):
    i, j = 0, len(nums)-1
    while i < j:
        min_val = nums[i]
        min_index = i
        max_val = nums[j]
        max_index = j
        for k in range(i, j+1):  # Min value
            if nums[k] <= min_val:
                min_val = nums[k]
                min_index = k
        nums[min_index], nums[i] = nums[i], nums[min_index]  # Exchange
        i += 1
        for l in range(i, j+1):  # Max value
            if nums[l] >= max_val:
                max_val = nums[l]
                max_index = l
        nums[max_index], nums[j] = nums[j], nums[max_index]  # Exchange
        j -= 1
    return nums
```
## 插入排序(Insertion Sort)
### 算法步骤
1. 将第一个元素看作已排序序列，第二个元素到最后一个元素看成未排序序列
2. 遍历未排序序列，将每个元素插入到已排序序列的适当位置
### 算法动画
![Insertion Sort](figs/insertionSort.gif)
### Python代码
```python
def InsertionSort(nums):
    sorted_index = 1
    while sorted_index < len(nums):
        cur = nums[sorted_index]
        i = sorted_index - 1
        while (i > -1 and cur <= nums[i]):
            i -= 1
        nums[i+2: sorted_index+1] = nums[i+1:sorted_index]
        nums[i+1] = cur
        sorted_index += 1
    return nums
```
## 希尔排序(Shell Sort)
### 算法步骤
1. 将整个序列，分为$t_1, t_2, t_3 ... t_k$共$k$个增量序列，其中$t_i>t_j$，$t_k=1$
2. 根据增量序列个数$k$，进行$k$次排序
3. 每次排序，根据对应的增量$t_i$，将待排序序列分为若干个长度为$m$的子序列，分别对各子序列进行插入排序，仅增量因子为1时，将整个序列作为一个表处理
### 算法动画
![Shell Sort](figs/shellSort.gif)
### Python代码
在实际编码中，我们采用另一种逻辑，根据不同的gap来进行希尔排序
```python
def InsertionSort(nums):
    sorted_index = 1
    while sorted_index < len(nums):
        cur = nums[sorted_index]
        i = sorted_index - 1
        while (i > -1 and cur <= nums[i]):
            i -= 1
        nums[i+2: sorted_index+1] = nums[i+1:sorted_index]
        nums[i+1] = cur
        sorted_index += 1
    return nums


def ShellSort(nums):
    gap = len(nums) // 2
    while gap > 0:
        nums[gap-1:] = InsertionSort(nums[gap-1:])
        gap = gap // 2
    return nums
```
## 归并排序(Merge Sort)
### 算法步骤
归并排序是利用分治法(Divide and Conquer)的一个典型应用，算法步骤如下
1. 申请空间，空间大小为两个待排序部分大小之和
2. 设定两个指针，位置分别为两个序列的起始
3. 比较两个序列中指针所指向的元素大小，将较小的元素放在申请的新序列中，并将指针移至下一位
4. 重复步骤3直到指针指向某一序列末尾
5. 将另一序列剩余元素添加到新序列末尾
6. 从上到下递归的调用1-5
### 算法动画
![Merge Sort](figs/mergeSort.gif)
### Python代码
```python
def MergeSort(nums):
    if len(nums) < 2:
        return nums
    mid = len(nums) // 2
    left_nums, right_nums = nums[:mid], nums[mid:]
    return Merge(MergeSort(left_nums), MergeSort(right_nums))  # Recursion

def Merge(left_nums, right_nums):
    new_nums = []
    while left_nums and right_nums:
        if left_nums[0] <= right_nums[0]:  # if left <= right
            new_nums.append(left_nums.pop(0))  # left pop, insert to new list
        else:
            new_nums.append(right_nums.pop(0))  # right pop, insert to new list
    # Insert rest numbers in left or right
    while left_nums:
        new_nums.append(left_nums.pop(0))
    while right_nums:
        new_nums.append(right_nums.pop(0))
    return new_nums
```
## 快速排序
### 算法步骤
快速排序也是分治法(Divide and Conquer)的一个典型应用，它将一个序列分为两个子序列，并在两个子序列上进行快速排序
>快速排序的最坏运行情况是 O(n²)，比如说顺序数列的快排。但它的平摊期望时间是 O>(nlogn)，且 O(nlogn) 记号中隐含的常数因子很小，比复杂度稳定等于 O(nlogn) 的归并>排序要小很多。所以，对绝大多数顺序性较弱的随机数列而言，快速排序总是优于归并排序。

1. 从序列中挑出一个元素称为基准(pivot)
2. 对序列进行调整，使得所有比pivot大的元素，都在pivot之后，比pivot小的元素都在pivot之前
3. 在调整完毕之后，pivot就处于整个序列的中间位置，该操作为分区(partition)操作
4. 递归的(recursive)将小于基准值的子序列和大于基准值的子序列进行排序
### 算法动画
![Quick Sort](figs/quickSort.gif)
### Python代码
```python
def partition(nums, low, high):
    i = low  # Left Index
    pivot = nums[high]

    for j in range(low, high):
        if nums[j] <= pivot:  # If current element less or equal to pivot
            nums[i], nums[j] = nums[j], nums[i]  # Exchang position
            i += 1  # Left Index ++
    nums[i], nums[high] = nums[high], nums[i]  # Exchang pivot and left index
    return i

def QuickSort(nums, low, high):
    if low < high:
        pi = partition(nums, low, high)

        QuickSort(nums, low, pi-1)
        QuickSort(nums, pi+1, high)

    return nums
```
## 堆排序(Heap Sort)
### 算法步骤
堆排序利用了堆(Heap)这一数据结构，堆可以被理解为近似一棵完全二叉树的结构，并同时满足堆的性质: 子节点的值总是大于(或小于)父节点的值。堆排序分为两种方法:
1. 大顶堆: 子节点的值都小于或等于父节点，用于升序排列
2. 小顶堆: 子节点的值都大于或等于父节点，用于降序排列

堆排序步骤如下:
1. 创建一个堆H[0,...,n-1]
2. 将堆首和堆尾进行互换
3. 把堆的尺寸缩小1，并对剩余的堆重新进行排序使其形成堆
4. 重复2和3，直到堆的size变为1
### 算法动画
![Heap Sort](figs/heapSort.gif)
![Heap Sort](figs/heapSort2.gif)
### Python代码
```python
def buildHeap(nums):
    # Build max heap
    for i in range(len(nums) // 2, -1, -1):
        heapify(nums, i)

def heapify(nums, i):
    # Adjust max heap
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    if left < arrLen and nums[left] > nums[largest]:
        largest = left
    if right < arrLen and nums[right] > nums[largest]:
        largest = right
    
    if largest != i:
        nums[largest], nums[i] = nums[i], nums[largest]
        heapify(nums, largest)

def heapSort(nums):
    global arrLen
    arrLen = len(nums)
    if len(nums) == 1:
        return nums
    buildHeap(nums)
    for i in range(len(nums)-1, -1, -1):
        nums[0], nums[i] = nums[i], nums[0]
        arrLen -= 1
        heapify(nums, 0)
    return nums
```
## 计数排序(Counting Sort)
### 算法步骤
计数排序需要额外的空间来存储输入的数据值，它是线性时间排序，我们需要额外开辟一个数组C，大小为(max-min+1)，因此对于数据范围很大的数组，我们需要大量的时间和内存，计数排序步骤如下:
1. 找出待排序序列中的最大元素和最小元素
2. 统计序列中值为i的元素出现的个数，并存入C[i]
3. 遍历C，并将C中每个元素放入新数组，每放入一个数组，C[i]就减一
### 算法动画
![Counting Sort](figs/countingSort.gif)
### Python代码
```python
def countingSort(nums):
    record = [0] * (max(nums) - min(nums) + 1)
    for num in nums:
        record[num - min(nums)] += 1
    result = []
    for i in range(len(record)):
        cur = i + min(nums)
        while record[i]:
            record[i] -= 1
            result.append(cur)
    return result
```
## 桶排序
### 算法步骤
桶排序是计数排序的升级版，我们利用了函数的映射关系，我们需要做到以下两点:
1. 在额外空间足够的情况下，增大桶的数量
2. 使用的映射函数能使N个数据均匀的分布到K个桶中
### 算法示意图
元素被分布到每个桶中<br>
![Bucket Sort](figs/bucketSort.png)

元素在桶中进行排序<br>
![Bucket Sort](figs/bucketSort2.png)
### Python代码
```python
def BucketSort(nums):
    record = {i: [] for i in range(int(min(nums)), int(max(nums)+1))}
    for num in nums:
        record[int(num)].append(num)
    result = []
    for i in record.values():
        result.extend(sorted(list(i)))
    return result
```
## 基数排序
### 算法步骤
基数排序是一种非比较性整数排序方法，其原理是将数字分为不同位，按每个位数分别比较
### 算法动画
![Radix Sort](figs/radixSort.gif)
### Python代码
```python
def RadixSort(nums):
    max_digit = 0
    tmp = max(nums)
    while tmp > 0:
        tmp = tmp // 10
        max_digit += 1
    record = {i: [] for i in range(0, 10)}
    for i in range(max_digit):
        for num in nums:
            radix = num / (10 ** i) % 10
            record[radix].append(num)
        nums = []
        for values in record.values():
            for value in values:
                nums.append(value)
        record = {i: [] for i in range(0, 10)}
    return nums
```









