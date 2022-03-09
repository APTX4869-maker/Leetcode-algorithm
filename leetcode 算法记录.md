### leetcode 算法记录



### 1.移动零

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



### 2.轮转数组

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



### 3.反转字符串中的单词III

思路：双指针，遇到空格反转

> #### [557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)
>
> 给定一个字符串 `s` ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

##### 示例 1：

~~~ 
输入：s = "Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
~~~

##### 示例 2:

~~~ 
输入： s = "God Ding"
输出："doG gniD"
~~~



##### 题解

~~~ java
class Solution {
    public String reverseWords(String s) {
        //双指针方法解决
        char[] chars = s.toCharArray(); //注意这个方法 直接转为char数组
        int size = s.length();
        int left = 0;
        for(int i = 0; i < size ; i++){
            char current = chars[i];
            if(current == ' '){
                reverse(chars,left,i-1); //遇到空格 说明前面的一段可以翻转了
                left = i+1;
            }else if(i == size - 1){
               reverse(chars,left,i) ; //到最后了 直接翻转
            }
        }
        return String.valueOf(chars); //注意这个方法 直接转为string类型
    }
    public void reverse(char[] chars ,int start,int end){ //翻转的通用函数
        while(start < end){
            char temp = chars[start];
            chars[start]=chars[end];
            chars[end] = temp;
            start ++ ;
            end --;
        }
    }
}
~~~



### 4.字符串的排列

思路：滑动窗口，包含26个字母的数组

> #### [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
>
> 给你两个字符串 s1 和 s2 ，写一个函数来判断 s2 是否包含 s1 的排列。如果是，返回 true ；否则，返回 false 。
>
> 换句话说，s1 的排列之一是 s2 的 子串 



##### 示例 1：

~~~ 
输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba").
~~~

##### 示例 2：

~~~ 
输入：s1= "ab" s2 = "eidboaoo"
输出：false
~~~



##### 题解：

~~~ java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        //滑动窗口方法 同时利用了这是26个小写字母的题目
        int len1 = s1.length(),len2 = s2.length();
        if(len1 > len2) return false; //如果s1长度大于s2 说明s2不可能包含s1 直接返回false
        int[] arr1 = new int[26],arr2 = new int[26];

        //两个数组一直保持s1的字符个数 滑动窗口来比较
        for(int i = 0; i< len1 ; i++){
            arr1[s1.charAt(i) - 'a'] ++; //相当于是有26个字符对应的数组 每次出现哪个字符 就在该字符的数组位置加一
            arr2[s2.charAt(i) - 'a'] ++;
        }
        int left = 0,right = len1 - 1;//s2字符串的指针
        while(right < len2){
            if(Arrays.equals(arr1,arr2)) return true; //遍历前先判断两个数组是否相同
            right ++;
            if(right != len2){ //防止right移动后超出范围
                arr2[s2.charAt(right) - 'a'] ++;
            }
            arr2[s2.charAt(left) - 'a'] --; //之前的left 位置因为需要右移动一位 所以之前位置的字母的数组位置减一
            left ++;//左边也移动一位
        }
        return false;   
    }
}
~~~



