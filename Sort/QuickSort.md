[TOC]



### 思路

时间复杂度分析：T(n) = 2T(n/2) + n, 2T(n/2) = 4T(n/4) + n, ..., n/2T(2) = nT(1) + N，共 logn 个等式，替换得:T(n) = nlogn

### 关键点分析

* 内层 while 循环，先由后左，保证最后 left=right 时，nums[left] < pivot，用于和 begin 交换。若先左后右，则最后 left=right 时，nums[left] > pivot，因为 begin 在左边所以不能直接交换。
* 内层 while 循环，之所以加 “left < right”，是为防止数组越界。

### 代码

```java
private int partition (int[] nums, int left, int right) {
  int begin = left;
  int pivot = nums[left];
  while (left < right) {
    // 先右后左，保证最后 left=right 时，nums[left] <= pivot，用于和 begin 交换
    // 加 left < right，防止数组越界
    while (left < right && nums[right] >= pivot) {
      right--;
    }
    while (left < right && nums[left] <= pivot) {
      left++;
    }
    if (left < right) swap(nums, left, right);
  }
  swap(nums, begin, right);
  return right;
}

private void quickSort(int[] nums, int left, int right) {
  if (left >= right) return;
  int partition = partition(nums, left, right);
  quickSort(nums, 0, partition - 1);
  quickSort(nums, partition + 1, right);
}

public int[] sortArray(int[] nums) {
  quickSort(nums, 0, nums.length - 1);
  return nums;
}

private void swap (int[] nums, int i, int j) {
  int tmp = nums[i];
  nums[i] = nums[j];
  nums[j] = tmp;
}

```

### partition举一反三（在其他地方应用）

#### 求数组里大小 N 中第 3 大的数

|        | 思路                                                         | 时间复杂度 | 空间复杂度 |
| ------ | ------------------------------------------------------------ | ---------- | ---------- |
| 思路一 | 遍历数组加到容量 n 的大顶堆，然后依次取出 N 个数。           | O(nlogn)   | O(n)       |
| 思路二 | 遍历数组加到容量 3 的小顶堆，若比堆顶大则替换堆顶，最终堆顶即为结果 | O(nlog3)   | O(1)       |
| 思路三 | partition 方法每执行一次找到一个对的位置，执行一次后，依据此位置缩小范围为一侧。 | O(n)       | O(1)       |



##### 思路一（大顶堆/小顶堆，O(nlogn)）

大顶堆：遍历数组加到大顶堆，然后依次取出 N 个数。时间复杂度：建堆O(nlogn)，取数 logn，合计 O(nlogn)；空间复杂度：O(n)

小顶堆：排除最小的 n-3 个元素。因为堆容量为3，因此堆插入删除时间均为常数，时间复杂度为 O(n)，空间 O(1)

##### 思路二（partition，O(n)）

partition 方法每执行一次找到一个对的位置，执行一次后，依据此位置缩小范围为一侧。

时间复杂度：T(n) = T(n/2) + n, T(n/2) = T(n/4) + n/2, ..., T(2) = T(1) + 2, 替换得：T(n) = 2+4+8+...+n = (q^n-1)/(q-1)*a1 = O(n)

##### 代码

```java
  // partition 思路  
	private int thirdMax(int[] nums, int left, int right) {
        // 递归终止条件
        if (left > right || left < 0 || right > nums.length - 1) return -1;
        // 单层递归逻辑
        int targetIndex = nums.length - 3;
        int partition = partition(nums, left, right);
        if (targetIndex > partition) {
            return thirdMax(nums, partition + 1, right);
        }
        if (targetIndex < partition) {
            return thirdMax(nums, left, partition - 1);
        }
        return nums[targetIndex];
    }

		// 数组中第 3 大的数 -- 堆排序
    // 优先级队列就是按某一优先级排序的队列，默认排序是最小堆
    public int thirdMaxByMinHeap(int[] nums) {
        int resut = Integer.MIN_VALUE;
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        for (int i = 0; i < nums.length; i++) {
            if (minHeap.size() < 3) {
                minHeap.offer(nums[i]);
            }else {
                if (nums[i] > minHeap.peek()){
                    minHeap.poll();
                    minHeap.offer(nums[i]);
                }
            }
        }
        resut = minHeap.poll();
        return resut;
    }
		// 大顶堆实现
    public int thirdMaxByMaxHeap (int[] nums) {
        int resut = Integer.MIN_VALUE;
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        for (int i = 0; i < nums.length; i++) {
            maxHeap.offer(nums[i]);
        }
        maxHeap.poll();
        maxHeap.poll();
        resut = maxHeap.poll();
        return resut;
    }
```



