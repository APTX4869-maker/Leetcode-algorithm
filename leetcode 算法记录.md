### leetcode 算法记录



### 1.

###### 思路 ： 双指针&快排

> #### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)
>
> 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> **请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

###### 题解：

~~~ java
class Solution {
    public void moveZeroes(int[] nums) {
        int size = nums.length;
        if(size <= 1 || nums == null) return ;
        //使用双指针方法 快排的思路 j指向的位置为0 左边是不为0的数 i 不断移动 当遇到不为0的数时 和j交换位置 然后j ++
        int j = 0;
        for(int i = 0; i < size ; i++){
            if(nums[i] != 0){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j++] = temp ;
            }
        }
    }
}
~~~



### 2 

思路：翻转数组

> #### [189. 轮转数组](https://leetcode-cn.com/problems/rotate-array/)
>
> 给你一个数组，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

###### 示例 1:

~~~ 
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
~~~

###### 示例 2:

~~~ 
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
~~~

###### 题解：

~~~ java
class Solution {
    public void rotate(int[] nums, int k) {
    int size = nums.length;
    if(size < 2) return;
    k = k % size; //注意有可能k远大于数组长度 需要取模缩小范围
    reverse(nums,0,size-1); //整个翻转
    reverse(nums,0,k-1);
    reverse(nums,k,size-1);
       
    }
    //翻转数组范围内的内容
    public int[] reverse(int[] nums,int start,int end){
        int temp = 0;
        while(start <= end){
            temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start ++ ;
            end --;
        }
        return nums;
    }
}
~~~



