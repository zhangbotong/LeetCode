### 题目
[15. 3Sum](https://leetcode.com/problems/3sum/)

### 题目描述

给定数组，找出三元组之和为 0 的所有三元组。

单个三元组内，每个元素只使用一次；去除相同数值三元组；

限制：3 <= nums.length <= 3000；-10^5 <= nums[i] <= 10^5

```
Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

不能有重复三元组，指三元组值不重复。eg. 
nums = {-1, -1, -1, 2}
solution:[[-1, -1, 2]]
```

### 思路

时间复杂度肯定高于 O(n*logn)，所以先排序，不增加时间复杂度; 

然后遍历数组，每次遍历时，将当前元素作为第一个元素，然后在剩余元素中使用双指针法，找到正确的两个元素。

 排除重复三元组：外层遍历时，如果当前元素与前一个元素相同，则跳过。双指针法时，如果指针指向的元素与前一个元素相同，则跳过。

**前后指针思想**expand到三数之和；

* 两数之和：有序O(1) + 前后指针O(n) = O(n)；
* 三数之和：多加一层遍历，排序O(nlog n) + **遍历O(n)**  X  前后指针O(n) = O(n^2);

![picture](https://github.com/zhangbotong/LeetCode/blob/master/assets/15.png)

图片来源大佬：[azl397985856](https://github.com/azl397985856)

### 关键点分析
* 去除重复三元组。不能有[[-1,-1,2], [-1,-1,2]]。解决： 遍历时，遇之前有过重复元素，skip；（**不能往后遇重复skip，eg. -1,-1,2，skip后无解）**
* 前后指针找到一组解时，记得也要移动 left 和 right。

### 代码
```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.List;

public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < nums.length; i++) {
            // 排除重复三元组
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                // 排除重复三元组
                if (left > i + 1 && nums[left] == nums[left - 1]) {
                    left++;
                    continue;
                }
                if (right < nums.length - 1 && nums[right] == nums[right + 1]) {
                    right--;
                    continue;
                }
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0){
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                    right--;
                }else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        return res;
    }
}

```