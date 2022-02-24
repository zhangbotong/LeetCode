![MergeSort-biTree](https://github.com/zhangbotong/LeetCode/blob/master/assets/MergeSort-biTree.png)

归并排序同二叉树后序遍历，只不过在根节点时进行操作（merge）

``` java
import java.util.Arrays;

/**
 * @author Kyrie
 * @date 2022/2/24 10:14 AM
 */
public class Sort {
    public static int[] tmp;
  
    public static int[] MergeSort(int[] arr) {
        tmp = new int[arr.length];
        Sort(arr, 0, arr.length - 1);
        return arr;
    }

    // arr[lo..hi]
    public static void Sort (int[] arr, int lo, int hi) {
        if (lo == hi){
            return;
        }
        int mid = lo + (hi - lo) / 2;
        Sort(arr, lo, mid); // 左子树
        Sort(arr, mid + 1, hi); // 右子树
        Merge(arr, lo, mid, hi); // 根节点
    }

    public static void Merge (int[] arr, int lo, int mid, int hi) {
        tmp = Arrays.copyOf(arr, arr.length);
        int i = lo, j = mid + 1;
        for (int k = lo; k <= hi; k++){
            if (i > mid){
                arr[k] = tmp[j++];
            } else if (j > hi){
                arr[k] = tmp[i++];
            } else if (tmp[i] < tmp[j]){
                arr[k] = tmp[i++];
            } else{
                arr[k] = tmp[j++];
            }
        }
    }

}
```


![MergeSort-mergeArray](https://github.com/zhangbotong/LeetCode/blob/master/assets/MergeSort-mergeArray.png)

时间复杂度：子问题个数 x 解决一个子问题复杂度

这里复杂度集中在 merge 函数，看图一，执行次数为二叉树节点个数，每次执行复杂度为每个节点子树组长度，即总数为【二叉树中所有数组元素个数】，每层 O(n)，共 logn 层，即 O(n*logn)
