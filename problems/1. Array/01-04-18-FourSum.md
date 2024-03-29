### 题目
[18. 4Sum](https://leetcode.com/problems/4sum/)

### 题目描述
```
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:

The solution set must not contain duplicate quadruplets.

Example:

Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

### 思路
对于**kSum**，转化为**twoSum**，增加k-2层循环即可，其时间复杂度为O(n^k-1)。

**twoSum：**数组有序，返回所有解，不含重复元组；对撞指针实现O(n)。

### 关键点分析
* 加法可能超出 int 范围，因此要用减法。
* 减治 至 2Sum
* 去除重复元组的实现
	* 1~2(2Sum)：找到一组解后，其left和right身边相等元素均skip；因为[1，2,2,3,3，4]，[2,3]只需出现一次；
	* 3~k: 若nums[i] == nums[i-1]，则skip i；因为[nums[i], x, y]与[nums[i-1], x, y]是重复元组。

### 代码
```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {

        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            //skip duplicate of k
            if (i > 0 && nums[i] == nums[i-1]) continue;
            int complement1 = target - nums[i];
            for (int j = i+1; j < nums.length; j++){
                //skip  duplicate of k-1
                if (j > i+1 && nums[j] == nums[j-1]) continue;
                int complement2 = complement1 - nums[j];
                List<List<Integer>> tmp = twoSum
                        (nums, j+1, nums.length-1, complement2);
                if (tmp != null){
                    //每个list加上i和j
                    for (List<Integer> list: tmp){
                        list.add(0,nums[j]);
                        list.add(0,nums[i]);
                    }
                    res.addAll(tmp);
                }
            }
        }
        return res;
    }

        public List<List<Integer>> twoSum (int[] nums, int left, int right, int target){
        List<List<Integer>> res = new ArrayList<>();
        //检查数据合法性
        if (left < 0 || right >= nums.length)
            throw new IllegalStateException("input is no solution");
        while (left < right){
            int sum = nums[left] + nums[right];
            if (sum == target){
                //left,right是一组解，不可能有相等的left或right
                //但可能有left+1, right-1的另一组解
                List<Integer> tmp = new ArrayList<>();
                tmp.add(nums[left]);
                tmp.add(nums[right]);
                res.add(tmp);
                //skip duplicate of 2
                while (left < right && nums[left] == nums[left+1]){
                    left++;
                }
                while (left < right && nums[right] == nums[right-1]){
                    right--;
                }
                left++;right--;
            }
            else if (sum < target){
                left++;
            }
            else {
                right--;
            }
        }
        return res;
    }

}
```