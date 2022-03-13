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



### 5.腐烂的橘子

思路：图的bfs解法，注意有通用的bfs模版

> #### [994. 腐烂的橘子](https://leetcode-cn.com/problems/rotting-oranges/)
>
> 在给定的 m x n 网格 grid 中，每个单元格可以有以下三个值之一：
>
> 值 0 代表空单元格；
> 值 1 代表新鲜橘子；
> 值 2 代表腐烂的橘子。
> 每分钟，腐烂的橘子 周围 4 个方向上相邻 的新鲜橘子都会腐烂。
>
> 返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1 。



**示例 1：**

```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2：**

~~~ 
输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
~~~

**示例 3：**

```
输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```



**题解： **

~~~ java
class Solution {
    public int orangesRotting(int[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int newOrange = 0;
        //先统计2的位置放入queue中
        for(int i = 0; i< row ;i++){
            for(int j = 0;j< col ;j++){
                if(grid[i][j] == 2) {
                    queue.offer(new int[]{i,j}); //将腐败的橘子放到队列中，作为后面遍历的起点
                }
                if (grid[i][j] == 1){
                    grid[i][j] = -1; //将新鲜的橘子置为有区别的数 也可以不这么做 
                    newOrange ++; //统计信息惨的橘子
                }
            }
        }
        int[][] directions = {{1,0},{-1,0},{0,1},{0,-1}}; //方向矩阵
        int distance = 0; //记录遍历的次数 也就是腐烂的时间
        while (newOrange > 0 && !queue.isEmpty()){
            int size = queue.size();
            for(int i = 0;i < size ;i++){ //注意别忘了这个循环 循环一层 
                    int[] temp = queue.poll();
                    for(int[] direction : directions){
                    int x = direction[0] + temp[0];
                    int y = direction[1] + temp[1];
                    if(x < 0 || x >= row || y <0 || y>= col || grid[x][y] == 0) continue; //遇到边界情况直接跳过
                    if (grid[x][y] == -1) {
                        grid[x][y] = 2; //遇到新鲜的 直接腐烂掉 然后新鲜橘子数减一
                        newOrange --;
                        queue.offer(new int[] {x,y}); //还要将这个新腐败的橘子也放入队列中 后面将作为新的起点继续遍历
                    }
                }
            }
            distance ++; //每一轮遍历完就++
        }
        return newOrange == 0? distance : -1; //看是否还有新鲜的橘子 如果没有就是返回distance 如果有说明永远有不能腐败的橘子 返回-1
    }
}
~~~



### 6. 填充每一个节点的下一个右侧节点的指针

**思路：** 递归方法

> #### [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)
>
> 给定一个 完美二叉树 ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：
>
> struct Node {
>   int val;
>   Node *left;
>   Node *right;
>   Node *next;
> }
> 填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
>
> 初始状态下，所有 next 指针都被设置为 NULL。
>

**示例 1：**

~~~ 
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
~~~

**示例 2:**

```
输入：root = []
输出：[]
```



**题解：**

~~~ java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if (root == null) return null ;
        //递归方法
        //先排除叶子结点 遇到叶子结点 直接返回
        if(root.left == null && root.right == null) return root;
        //将根节点的左子树的next指针 指向右节点
        root.left.next = root.right;
        
        //这种情况是 root在中间的情况
        if(root.next != null ) root.right.next = root.next.left;
        //遍历往后的左右子树
        connect(root.left);
        connect(root.right);
        return root;

    }
}
~~~



### 7.合并二叉树

**思路：** 递归

> #### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)
>
> 给你两棵二叉树： root1 和 root2 。
>
> 想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。
>
> 返回合并后的二叉树。
>
> 注意: 合并过程必须从两个树的根节点开始。

**示例 1：**

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

**示例 2：**

```
输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```



**题解：**

~~~ java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
       public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        return mergeDFS(root1, root2);
    }
    private TreeNode mergeDFS(TreeNode root1, TreeNode root2) {
         // 当前两颗树对应的同一个位置节点都为null
        if (root1 == null || root2 == null) {
            return root1 == null ? root2 : root1;
        }
        root1.val = root1.val + root2.val;
        // 先深度遍历左节点
        root1.left = mergeDFS(root1.left, root2.left);
        // 在深度遍历右节点
        root1.right = mergeDFS(root1.right, root2.right);
        return root1;
    }
}
~~~

