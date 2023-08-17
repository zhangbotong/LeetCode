### 题目
[167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### 题目描述

给定有序数组，找到两数之和等于 target，返回下标。

限制：有且仅有唯一解

```
Note:

Your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution and you may not use the same element twice.
Example:

Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

### 思路
特点分析： 有序。  
Solution：**对撞指针**。  
逻辑：

* nums[l] + nums[r] < target,则l右移
* nums[l] = nums[r] > target,则r左移

![](https://blog-1257126549.cos.ap-guangzhou.myqcloud.com/blog/59rnm.gif)

### 关键点分析
* 对撞指针思想

### 代码
```java
import java.util.Arrays;

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0, right = numbers.length - 1;
        while (left < right) {
            if (numbers[left] + numbers[right] == target){
                return new int[] {left + 1, right + 1};
            }
            if (numbers[left] + numbers[right] < target) {
                left++;
            } else {
                right--;
            }
        }
        return new int[2];
    }
}
```

### 相关问题
**对撞指针：** [125. Valid Palindrome](https://github.com/zhangbotong/LeetCode/blob/master/problems/125.%20Valid%20Palindrome%20.md)