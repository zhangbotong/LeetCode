![MergeSort-biTree](https://github.com/zhangbotong/LeetCode/blob/master/assets/MergeSort-biTree.png)

归并排序同二叉树后序遍历，只不过在根节点时进行操作（merge）

``` java
    public int[] sortArray(int[] nums) {
        mergeSort(nums, 0, nums.length - 1);
        return nums;
    }

    private void mergeSort (int[] array, int left, int right) {
        // 递归终止条件
        if (left >= right) return;
        // 单层处理逻辑，先分，再合
        int mid = left + (right - left) / 2;
        mergeSort(array, left, mid);
        mergeSort(array, mid + 1, right);
        merge(array, left, mid, right);
    }

    // 合并 2 个有序数组 [left, mid] 和 [mid + 1, right](入口保证 left <= mid < right)
    private void merge (int[] array, int left, int mid, int right) {
        int[] tmp = new int[right - left + 1];
        int i = left, j = mid + 1;
        for (int k = 0; k <= right - left; k++) {
            // i > mid 说明左边已经合并完了，直接把右边的元素放入 tmp
            if (i > mid){
                tmp[k] = array[j++];
            }else if (j > right) {// j > right 说明右边已经合并完了，直接把左边的元素放入 tmp
                tmp[k] = array[i++];
            }else {// 左右都没合并完，比较大小，小的放入 tmp
                tmp[k] = array[i] < array[j] ? array[i++] : array[j++];
            }
        }
        // 把 tmp 中的元素放回 array
        for (int k = left; k <= right; k++) {
            array[k] = tmp[k - left];
        }
    }
```


![MergeSort-mergeArray](https://github.com/zhangbotong/LeetCode/blob/master/assets/MergeSort-mergeArray.png)

时间复杂度：子问题个数 x 解决一个子问题复杂度

这里复杂度集中在 merge 函数，看图一，执行次数为二叉树节点个数，每次执行复杂度为每个节点子树组长度，即总数为【二叉树中所有数组元素个数】，每层 O(n)，共 logn 层，即 O(n*logn)
