## 数据结构与算法之美

## 1. 复杂度分析

它是评价算法的 "标尺"，告诉我们执行某个算法需要的时间和空间，也可以此来展开不同算法之间的对比。而大 $O$ 复杂度表示法表示的只是一种变化趋势。

### 1.1 时间复杂度

我们只需要去关注循环执行次数最多的代码，其他部分代码忽略，并使用以下两个法则

- 加法法则: 代码总的复杂度等于量级最大的那段代码的复杂度
- 乘法法则: 嵌套代码的复杂度等于嵌套内外代码复杂度的乘积

- $O(n^2)$

```java
int sum = 0;
for(int i = 0; i < n; i++){
  sum += i;
  for (int j = 0; j < n; j++) {
    sum += j;    
  }
}
```

第5行代码执行次数为 $n$，第3行代码执行次数为 $n$，根据乘法法则，嵌套代码复杂度需要进行乘积

- $O(logn)$

```java
int i = 1;
while (i <= n) {
    i = i * 2;    
}
```

我们只需要关注第3行代码，而i的变化序列为 $2, 4, 8, 16...$，所以这段代码的时间复杂度为$O(logn)$

### 1.2 空间复杂度
空间复杂度指的是**除原本的数据存储空间以外**，算法运行过程中还需要的额外存储空间。

比如用数组实现栈数据结构，栈中虽有大小为$n$的数组，但是并不代表它的空间复杂度为$O(n)$，而是只关注数组之外的一些临时变量，因此它的空间复杂度为$O(1)$

## 2. 数据结构

**所有数据结构都是基于数组或链表或两者组合实现的**

### 2.1 数组
数组是线性的数据结构，占用连续的内存空间存储一组相同类型的数据，正因如此，数组**支持随机访问**，即在 $O(1)$ 时间复杂度内按照索引快速访问数组中的元素。
但是它的插入、删除操作也因此变得低效，平均时间复杂度为 $O(n)$

### 2.2 链表
链表也是线性数据结构，与数组不同的是它不需要占用连续的内存空间，但是需要额外的空间保存后继节点的指针，以此将所有的链表节点串联起来。
它的删除和插入操作比较高效，时间复杂度为 $O(1)$，但是想访问链表中某个值时，需要对链表进行遍历，时间复杂度为 $O(n)$

### 2.3 背包
支持添加和遍历元素，但是不支持删除元素。

### 2.4 栈
- 应用: 函数调用栈、括号的匹配、双栈实现浏览器的前进和后退功能、JVM栈、电子邮件的存放、算数表达式的求值(操作数栈和运算符栈)

## 3. 递归
**不要试图模拟计算机递归调用的过程！** **不要试图用你聪明的大脑去分解递归的每个步骤！** 而是思考递推公式，找出终止条件，然后将以上信息"翻译"成代码！

递归调用存在如下两个问题
- 警惕递归代码出现堆栈溢出
- 警惕递归代码出现的重复计算问题: 使用 **备忘录** 来解决

比如，爬n阶台阶的问题
```java
    /**
     * 爬台阶问题
     * <p>
     * 递推公式: f(n) = f(n - 1) + f(n - 2)
     * 终止条件: 爬到第1或第2阶台阶
     * <p>
     * 备忘录: 当计算5阶台阶时，会计算4阶台阶和3阶台阶，计算4阶台阶时还会计算3阶台阶，3阶台阶的计算是重复的
     * 所以我们可以依靠备忘录来解决重复计算的问题
     */
    private static int recursion(int n) {
        if (n == 1 || n == 2) {
            return n;
        }

        if (map.containsKey(n)) {
            return map.get(n);
        }
        int res = recursion(n - 1) + recursion(n - 2);
        map.put(n, res);
        
        return res;
    }

    // 备忘录
    private static final HashMap<Integer, Integer> map = new HashMap<>(16);
```

- 空间复杂度: $O(n)$ ，递归代码的空间复杂度等于最大深度乘以每层递归调用的空间消耗，每层递归空间消耗为常量级，而栈的最大深度为n所以为 $O(n)$

## 4. 排序算法
- 算法特性
  - 稳定性: 经排序后，若等值元素之间的相对位置不变则为稳定排序算法，否则为不稳定排序算法
  - 原地排序: 是否借助额外辅助空间
  - 自适应性: 自适应性排序受输入数据的影响，即最佳/平均/最差时间复杂度不等，而非自适应排序时间复杂度恒定

- 排序算法执行效率的分析
  - 最佳/平均/最差时间复杂度或**有序度和逆序度**的方法
  - 时间复杂度的系数、常数和低阶，我们在就散时间复杂度时会忽略这些，但是在实际开发中需要将这些因素根据数据规模考虑进来
  - 比较和交换次数

### 4.1 $O(n^2)$
**插入排序**在实际开发中应用广泛，冒泡排序和选择排序熟悉就好

#### 4.1.1 冒泡排序
```java
    private static void bubbleSortImprove(int[] nums) {
        for (int i = nums.length - 1; i > 0; i--) {
            // 优化标志位: 如果未发生交换则证明已经有序
            boolean flag = false;
            for (int j = 0; j < i; j++) {
                // 比较并交换位置
                if (nums[j] > nums[j + 1]) {
                    int temp = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = temp;

                    flag = true;
                }
            }

            if (!flag) {
                break;
            }
        }
    }
```
- **空间复杂度**: $O(1)$
- **原地排序**
- **稳定排序**
- **自适应排序**: 经过优化后最佳时间复杂度为 $O(n)$

#### 4.1.2 插入排序

- 核心思想: 取未排序区间中的某个元素为基准数 `base`，将 `base` 与其左侧已排序区间元素依次比较大小，并"插入"到正确位置

```java
    private static void insertSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            // 选取未排序区间的基准数
            int base = nums[i];

            // 排序区间数若比基准数大，则后移，直到找到合适的位置插入
            int j = i - 1;
            while (j >= 0 && nums[j] > base) {
                nums[j + 1] = nums[j];
                j--;
            }
            nums[j + 1] = base;
        }
    }
```

- **空间复杂度**: $O(1)$
- **原地排序**
- **稳定排序**
- **自适应排序**: 最佳时间复杂度为 $O(n)$

#### 4.1.3 选择排序

- 核心思想: 每次从未排序区间中找到最小的元素，将其放到已排序区间的末尾

```java
    private static void selectionSort(int[] nums) {
        // 循环n-1次即可
        for (int i = 0; i < nums.length - 1; i++) {
            int minIndex = i;
            // 寻找最小值
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[minIndex]) {
                    minIndex = j;
                }
            }

            // 交换位置
            int temp = nums[i];
            nums[i] = nums[minIndex];
            nums[minIndex] = temp;
        }
    }
```

- **空间复杂度**: $O(1)$
- **原地排序**
- **非稳定排序**: 会改变等值元素之间的相对位置
- **非自适应排序**: 最好/平均/最坏时间复杂度均为 $O(n^2)$

### 4.2 $O(nlogn)$

归并排序和快速排序都是基于分治算法的思想实现的排序，但是快速排序应用更加广泛

#### 4.2.1 归并排序

- 核心思想: 采用分治算法思想，分**划分**和**合并**两个阶段。
  划分阶段不断将数组从中点位置分开，直到划分长度为1；合并阶段是将划分后的排序数组合并排序的长数组

```java
    private static void mergeSort(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }

        // 划分阶段，不断地从中点划分
        int mid = (left + right) / 2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid + 1, right);
        
        // 合并阶段
        merge(nums, left, mid, right);
    }

    private static void merge(int[] nums, int left, int mid, int right) {
        // 辅助数组，注意该数组取的是原数组[left, right]范围
        int[] temp = Arrays.copyOfRange(nums, left, right + 1);

        // 左数组 在辅助数组的范围
        int leftBegin = 0, leftEnd = mid - left;
        // 右数组 在辅助数组的范围
        int rightBegin = mid + 1 - left, rightEnd = right - left;

        // 在原数组的范围内直接借助左右数组覆盖
        for (int i = left; i <= right; i++) {
            // 左数组用完了直接赋值右数组
            if (leftBegin > leftEnd) {
                nums[i] = temp[rightBegin++];
            } else if (rightBegin > rightEnd || temp[leftBegin] <= temp[rightBegin]) {
                // 右数组用完了或者左数组数小
                nums[i] = temp[leftBegin++];
            } else {
                // 左右数组都没用完，但是左数组比较大
                nums[i] = temp[rightBegin++];
            }
        }
    }
```

观察归并排序划分阶段的代码，其实就是二叉树的后序遍历

- **空间复杂度**: 借助辅助数组实现合并，使用 $O(n)$ 的额外空间；递归深度为 $logn$，使用 $O(logn)$ 大小的栈帧空间。
  忽略低阶部分，所以空间复杂度为 $O(n)$
- **非原地排序**
- **稳定排序**
- **非自适应排序**

> 归并排序的时间复杂度一直是 $O(nlogn)$，而快速排序在最坏的情况下时间复杂度为 $O(n^2)$，为什么归并排序没有快速排序应用广泛呢？
答: 因为归并排序是非原地排序，在合并阶段需要借助非常量级的额外空间

#### 4.2.2 快速排序

- 核心思想: 快速排序同样基于分治算法的思想，与归并排序不同的是归并排序从上到下进行处理，先处理子问题，而快速排序处理过程有上到下，先分区，再处理子问题。
  快速排序的核心操作为 **哨兵划分**，选取数组中某个数为基准数，将小于基准数的排在左边，大于基准数的排在右边，之后再对两个小数组分别执行快速排序。

```java
    private static void quickSort(int[] nums, int left, int right) {
        if (left >= right) {
            return;
        }

        // 哨兵划分
        int partition = partition(nums, left, right);
        
        // 分别排序两个子数组
        quickSort(nums, left, partition - 1);
        quickSort(nums, partition + 1, right);
    }

    /**
     * 哨兵划分
     */
    private static int partition(int[] nums, int left, int right) {
        // 以 nums[left] 作为基准数，并记录基准数索引
        int base = nums[left];
        int baseIndex = left;

        while (left < right) {
            while (left < right && nums[right] >= base)
                right--;             // 从右向左找首个小于基准数的元素
            while (left < right && nums[left] <= base)
                left++;              // 从左向右找首个大于基准数的元素
            swap(nums, left, right); // 交换这两个元素
        }
        swap(nums, baseIndex, left); // 将基准数交换到两子数组的分界线
        return left;                 // 返回基准数索引
    }

    private static void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
```

- **平均时间复杂度**: $O(nlogn)$
- **最差时间复杂度**: $O(n^2)$
- **空间复杂度**: 最差情况下，递归深度为n，所以空间复杂度为 $O(n)$
- **原地排序**
- **非稳定排序**
- **自适应排序**

### 4.3 $O(n)$
桶排序、计数排序和基数排序，时间复杂度都为 $O(n)$，也称为线性排序，它们都不是基于比较的排序算法。

#### 4.3.1 桶排序

- 核心思想: 定义几个有序的"桶"，将要排序的数据分别放到这几个"桶"里，对每个桶里的数据单独进行排序，再把每个"桶"里的数据按照顺序依次取出，结果便是有序的。

桶排序的时间复杂度虽然为$O(n)$，但是它对数据是有要求的，待排序数组需要容易划分成m个"桶"，并且"桶"与"桶"之间有着"天然"的大小顺序，
这样在每个桶中的值排序完成后，取出结果便是有序的

- 桶排序非常适合用在外部排序中。比如，有10GB的订单需要按照金额排序，但是服务器的内存只有几百MB，那么该如何做？

采用桶排序的方法，假定金额为 $1 - 1000$ 且均匀分布，那么我们划分出100个桶，每个桶内的金额范围为10元，这样我们逐个将每个桶进行排序，
分别写到100个小文件中，这样按顺序读取这100个小文件写到大文件中，就完成了该排序。如果金额不是均匀分布的，那么我们对分到订单数量过多的桶再进行桶划分，
直到保证每个桶内的数据量满足内存大小要求。

## 5. 查找算法
### 4.1 二分查找

分 **双闭区间** 和 **左闭右开** 区间的两种写法
- **双闭区间**: 定义两指针时，左右指针都在数组范围内，那么查找的结束条件便是 left > right
- **左闭右开**: 右边开区间则表示右指针不在查找的数组范围内，那么查找的结束条件便是 left = right