### leetcode 算法记录

[toc]

![image-20220318223444726](/Users/alyx/Library/Application Support/typora-user-images/image-20220318223444726.png)





### 1.移动零

**思路：** 双指针&快排

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

**题解：**

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

**示例 1:**

~~~ 
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
~~~

**示例 2:**

~~~ 
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
~~~

**题解：**

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

**示例 1：**

~~~ 
输入：s = "Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
~~~

**示例 2:**

~~~ 
输入： s = "God Ding"
输出："doG gniD"
~~~

**题解**

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

**示例 1：**

~~~ 
输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba").
~~~

**示例 2：**

~~~ 
输入：s1= "ab" s2 = "eidboaoo"
输出：false
~~~

**题解：**

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



### 6.填充每一个节点的下一个右侧节点的指针

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



### 8.合并两个有序链表

**思路：** 递归



> #### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
>
> 将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```



**题解：**

~~~ java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if(list2 == null) return list1;
        if(list1 == null) return list2;

        if(list1.val >= list2.val){
            list2.next = mergeTwoLists(list1,list2.next);
            return list2;
        }else {
            list1.next = mergeTwoLists(list1.next,list2);
            return list1 ;
        }

    }
}
~~~



### 9.反转链表

**思路：**普通的遍历交换位置 & 递归



> #### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
>
> 给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

**示例 1：**

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

**题解：**

~~~ java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) return null;
        ListNode temp = null;
        ListNode cur = head;
        ListNode pre = null;
        while(cur != null){
            temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;

    }
}
~~~

递归方法解决：

~~~ java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
      //递归方法解决
      if(head == null || head.next == null) return head ;
      ListNode h = reverseList(head.next); //一直返回的都是一个节点 因为reverseList的返回值就是翻转后的头节点 所以最后返回它即可
      head.next.next = head ;//此时遍历到末尾 返回上一层 倒数第二个节点 head.next是最后一个节点 head.next.next表示最后一个节点指向倒数第二个
      head.next = null ;//断开了此时倒数第二个节点原本指向的最后一个节点的指针
      return h ;
    }
}
~~~



### 10.组合

**思路：** 回溯法



> #### [77. 组合](https://leetcode-cn.com/problems/combinations/)
>
> 给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。
>
> 你可以按 **任何顺序** 返回答案。

**示例 1：**

~~~ 
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
~~~

**示例 2：**

~~~ 
输入：n = 1, k = 1
输出：[[1]]
~~~



**题解：**



~~~ java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int n ,k;
    public List<List<Integer>> combine(int n, int k) {
        //回溯法
        this.n = n;
        this.k = k;
        backTracking(1);//回溯函数，传入起始数字
        return res;
    }
    private void backTracking(int index){
        if(list.size() == k){
            //退出条件
            res.add(new ArrayList<>(list));
            return ;
        }
        for(int i = index ; i <= n ;i++){
            list.add(i);
            backTracking(i+1);
            //回溯
            list.remove(list.size()-1);
        }
    }
}
~~~



### 11.全排列

**思路：** 回溯法



> #### [46. 全排列](https://leetcode-cn.com/problems/permutations/)
>
> 给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。



**示例 1：**

~~~ 
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
~~~

**示例 2：**

~~~ 
输入：nums = [0,1]
输出：[[0,1],[1,0]]
~~~

**示例 3：**

~~~ 
输入：nums = [1]
输出：[[1]]
~~~

**题解**

~~~ java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> list = new ArrayList<>();
    int size ;
    public List<List<Integer>> permute(int[] nums) {
        size = nums.length;
        dfs(nums,size);
        return res;
    }
    private void dfs(int[] nums , int size){
        if(list.size() == size){
            //跳出条件
            res.add(new ArrayList<>(list));
            return ;
        }
        for(int i = 0 ;i < size;i++){
            if(list.contains(nums[i])) continue;
            list.add(nums[i]);
            dfs(nums,size);
            list.remove(list.size() - 1);
        }
    }
}
~~~



### 12.字母大小写全排列

**思路：** 回溯法 & character的api



> #### [784. 字母大小写全排列](https://leetcode-cn.com/problems/letter-case-permutation/)
>
> 给定一个字符串 `s` ，通过将字符串 `s` 中的每个字母转变大小写，我们可以获得一个新的字符串。
>
> 返回 *所有可能得到的字符串集合* 。以 **任意顺序** 返回输出。

**示例 1:**

~~~ 
输入：s = "a1b2"
输出：["a1b2", "a1B2", "A1b2", "A1B2"]
~~~

**示例 2:**

~~~~
输入: s = "3z4"
输出: ["3z4","3Z4"]
~~~~



**题解：**



~~~ java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> letterCasePermutation(String s) {
        char[] chars = s.toCharArray();
        int size = s.length();
        dfs(chars,size,0);
        return res;
    }
    private void dfs(char[] chars,int size,int index){
        if(index == size) { //已经遍历完数组了 将修改了元素的数组变为string放入结果list中
            res.add(new String(chars));
            return ;
        }
        char c = chars[index];
        if(Character.isLetter(c)){ //注意这个api
            chars[index] = Character.toUpperCase(c);//此时有将元素变大变小的两种方案
            dfs(chars,size,index+1);
            chars[index] = Character.toLowerCase(c);
            dfs(chars,size,index+1);
        }else { //当不满足是字符时，直接继续往后遍历
            dfs(chars,size,index+1);
        }
        
    }
}
~~~

另一种模版化的写法

~~~ java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuilder path = new StringBuilder(); //多了一个sb来存放 
    public List<String> letterCasePermutation(String s) {
        backtrace(s, 0);
        return res;
    }

    void backtrace(String s, int index) {
        if ( path.length() == s.length() ) {
            res.add(path.toString());
            return;
        }
        char ch = s.charAt(index);
        // 数字，直接回溯
        if ( Character.isDigit(ch) ) {
            path.append(ch);
            backtrace(s, index+1);
            path.deleteCharAt(path.length()-1);
        } else {
            // 字母，两个回溯路线
            path.append(Character.toLowerCase(ch));
            backtrace(s, index+1);
            path.deleteCharAt(path.length()-1);

            path.append(Character.toUpperCase(ch));
            backtrace(s, index+1);
            path.deleteCharAt(path.length()-1);            
        }
    }
}
~~~



### 13.爬楼梯

**思路：** 动态规划



> #### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
>
> 假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。
>
> 每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？



**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

~~~ 
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。

1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
~~~



**题解：**



~~~ java
class Solution {
    public int climbStairs(int n) {
        //动态规划
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        if(n <= 1) return dp[n];
        for(int i = 2;i <= n ;i++){
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }

}
~~~



### 14.三角形最小路径和

**思路：** 动态规划（较难）



> #### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)
>
> 给定一个三角形 triangle ，找出自顶向下的最小路径和。
>
> 每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。



**示例 1：**

~~~
输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
~~~

**示例 2：**

```
输入：triangle = [[-10]]
输出：-10
```



**题解：**



~~~ java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if(triangle == null || triangle.size() == 0) return 0;
        //动态规划 看成是二维数组来计算
        int row = triangle.size();
        int col = triangle.get(row - 1).size();
        //从上往下
        int[][] dp = new int[row][col];

        dp[0][0] = triangle.get(0).get(0); //0行0列
        int res = Integer.MAX_VALUE; //用来做对比的 拿到数组中的最小值
        for(int i = 1 ; i < row ; i++){
            for(int j = 0; j < triangle.get(i).size() ; j++){
                //特殊情况 一个是最左边时
                if (j == 0){
                    dp[i][j] = dp[i-1][j] + triangle.get(i).get(j);
                }
                //最右边时 左上方 则坐标位置在现在的列位置-1
                else if(j == triangle.get(i).size() - 1){
                    dp[i][j] = dp[i-1][triangle.get(i-1).size() - 1] + triangle.get(i).get(j);
                }else {
                    //此时正上方和左前方都有可能是最小值
                    dp[i][j] = Math.min(dp[i-1][j],dp[i-1][j-1]) + triangle.get(i).get(j);
                }

            }
        }

        for(int k = 0 ; k < col ;k++){
            res = Math.min(dp[row - 1][k],res);
        }
        return res;


    }
}
~~~



### 15. 2的幂

**思路：** 一直除2



> #### [231. 2 的幂](https://leetcode-cn.com/problems/power-of-two/)
>
> 给你一个整数 n，请你判断该整数是否是 2 的幂次方。如果是，返回 true ；否则，返回 false 。
>
> 如果存在一个整数 x 使得 n == 2x ，则认为 n 是 2 的幂次方。



**示例 1：**

```
输入：n = 1
输出：true
解释：20 = 1
```

**示例 2：**

```
输入：n = 16
输出：true
解释：24 = 16
```

**示例 3：**

```
输入：n = 3
输出：false
```



**题解：**



~~~ java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n <= 0) return false ;
        if (n== 1) return true ;

        //一直除  2/2 =1  6 % 2 = 0 6/2= 3 3 % 2 != 0
        while(n != 1){
            if( n % 2 != 0) return false ;
            n = n / 2;
        }
        return true ;

    }
}
~~~



### 16.lru缓存 **

**思路：** 实现一个双向链表和map，链表用来记录插入的顺序



> #### [146. LRU 缓存](https://leetcode-cn.com/problems/lru-cache/)
>
> 请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
> 实现 LRUCache 类：
> LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
> int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
> void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
> 函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。



**示例：**

~~~ 
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
~~~



**题解**



1.记录一个超时的题解，容易理解



~~~ java
class LRUCache {
    int capacity;
    LinkedList<Integer> list = new LinkedList<>(); //用来按顺序存放key
    Map<Integer,Integer> map ;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>(capacity); //创建存放key-value的map结构
    }
    
    public int get(int key) {
        if(!list.contains(key)) return -1; //如果没有 直接返回-1
        //使用到了这个元素 所以list的这个元素顺序就要提前 先删除再加上
        //list.remove(Integer.valueOf(key)); //这里注意 删除有两个api 一个是remove(int index) 这会按照指针顺序删除链表元素 remove(object o) 这个会删除链表的某个元素 之所以不直接用key就是为了这问题
        //list.addLast(key);
        updateList(key);
        return map.get(key);
    }
    
    public void put(int key, int value) {
        //几种情况 一个是put原本存在的数据
        if(list.contains(key)) {
          updateList(key);
            map.put(key,value); //更新一下
        }else {
            //不是原本存在 需要判断是否超过容量大小
            //如果此时的容量满了 先删除 再put
            if(list.size() == capacity){
                int k = list.removeFirst();//删除最前面的元素 这肯定是最少使用的 返回的值就是删除的key
                map.remove(k);//在map中清空这个key-value
                map.put(key,value);
                list.add(key);
            }else{
                //没有超过容量 直接加入就行
                list.addLast(key);
                map.put(key,value);
            }
        }
    }
    public void updateList(int key){
            list.remove(Integer.valueOf(key));
            list.addLast(key);
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
~~~



2.大佬的解法



~~~ java
class LRUCache {
    
    int capacity ;
    Map<Integer,LinkedNode> map = new HashMap<>();
    LinkedNode head = new LinkedNode(0,0);
    LinkedNode tail = new LinkedNode(0,0);
    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail; //前后连接
        tail.front = head;
    }
    
    public int get(int key) {
        if(map.containsKey(key)){
            LinkedNode node = map.get(key);
            moveToHead(node);
            return node.val;
        }else {
            return -1;
        }
    }
    
    public void put(int key, int value) {
        //先判断是否map已存在该元素
        if(!map.containsKey(key)){ //如果不存在 
            if(map.size() == capacity){ //此时无法插入 需要去掉linkedNode的tail前面的一个节点 同时在linkedNode更新插入的节点
                deleteLastNode(); //删除最后的节点
            }
            //然后更新linkedNode 
            LinkedNode temp = head.next;
            LinkedNode newNode = new LinkedNode(key,value);
            head.next = newNode;
            newNode.front = head;
            newNode.next = temp; //将新的节点插到head的后面
            temp.front = newNode;
            map.put(key,newNode);
        }else {
            //如果存在
            LinkedNode tempNode = map.get(key);
            tempNode.val = value; //更新map中的linkedNode 的 value
            //更新节点位置到head 后面
            moveToHead(tempNode);

        }
    }
    private void deleteLastNode(){
        LinkedNode temp = tail.front;
        LinkedNode f = temp.front;
        f.next = tail;
        tail.front = f;
        map.remove(temp.key);
    }
    private void moveToHead(LinkedNode node){
        //先断掉现在这个node的前后连接
        node.front.next = node.next;
        node.next.front = node.front;
        //然后将这个node接在head后面
        LinkedNode temp = head.next;
        head.next = node;
        node.front = head;
        node.next = temp;
        temp.front = node;
    }
}

class LinkedNode{
    LinkedNode next; //后指针
    LinkedNode front; //前指针
    int key;
    int val;
    public LinkedNode(int key,int val){
        this.key = key;
        this.val = val;
    }

}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
~~~



### 17.只出现一次的数字

**思路：** 异或



> #### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)
>
> 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> 说明：
>
> 你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？//注意不能使用额外空间 还要有线性复杂度

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```



**题解：**



~~~ java
class Solution {
    public int singleNumber(int[] nums) {
        //参考大佬的异或 异或有几个性质：1.a ^ a = 0 2. a ^ 0 = a 3.异或满足结合律和交换律
        int res = nums[0];
        for(int i = 1; i < nums.length;i++){
            res = res ^ nums[i]; //^是异或符号
        }
        return res;
    }
}
~~~



### 18.k个一组翻转链表



**思路：** 递归 & 栈



> #### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
>
> 给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
>
> k 是一个正整数，它的值小于或等于链表的长度。
>
> 如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
>
> 进阶：
>
> 你可以设计一个只使用常数额外空间的算法来解决此问题吗？
> 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。



**示例 1：**

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```



**题解：**

第一种方式 便于理解 但是时间复杂度高

~~~ java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    Stack<ListNode> stack = new Stack<>(); //用来将遍历的节点压栈
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null || head.next == null) return head;//只有一个元素或者为空
        
        int localk = k;
        ListNode dummy = new ListNode(0);
        ListNode pre = dummy;
        ListNode cur = head;

        while(localk > 0 && cur != null){
            stack.push(cur);
            cur = cur.next;
            localk --;
        }
        if (localk > 0) return head; //此时说明都遍历完了 cur == null 还没有到到k个一组 直接原路返回

        while(!stack.isEmpty()){
            pre.next = stack.pop();
            pre = pre.next;
        }
        pre.next = reverseKGroup(cur,k); //此时的cur在下一次遍历的head处 
        return dummy.next;


    }
}
~~~



第二种解法，时间复杂度低，没有使用额外的空间 递归方式

~~~ java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null || head.next == null) return head;//只有一个元素或者为空
        ListNode index = head; 
        int localk = k;
        while(localk > 0){
            if(index == null) return head;//此时表示localk != 0 说明链表太短了 小于k 此时直接返回head 不翻转了
            index = index.next;//移动 跳出循环后 index在k个节点的的下一个
            localk --;
        }
        ListNode newHead = reverse(head,index); //头尾翻转 前闭后开
        head.next = reverseKGroup(index,k);//这里要注意 是head.next = xx
        return newHead;

    }
    private ListNode reverse(ListNode head,ListNode index){
        //翻转链表
        ListNode pre = null , next = null;
        while(head != index){
            next = head.next ; //暂存
            head.next = pre;//当前的head指向后面的pre节点
            pre = head;
            head = next;//pre head 都向前移动
        }
        return pre ;


    }
}
~~~



### 19.二叉树的锯齿形层序遍历

**思路：** 广度遍历



> #### [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
>
> 给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**示例 1：**

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
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
    List<List<Integer>> res = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<>();
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        if(root == null) return res;
    //广度遍历 偶数从左往右 奇数从右往左
        int level = 0;
        queue.add(root);
        dfs(root,level);
        return res;

    }
    private void dfs(TreeNode root,int level){
        while(queue.size() > 0){
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            for(int i = 0; i< size ; i++){
                TreeNode tempNode = queue.poll(); 
                if(level % 2 == 0){
                    list.add(tempNode.val); //偶数从左往右放入list 默认是append到list后面
                }else {
                    list.add(0,tempNode.val); //不断将新的value放到list的index=0的首位 实现倒序
                }
                if (tempNode.left != null) queue.add(tempNode.left);
                if (tempNode.right != null) queue.add(tempNode.right);
            }
            res.add(new ArrayList<>(list));
            level ++; //遍历完一层了 层数加一
        }

    }
}
~~~



### 20.排序数组



**思路**：一种是直接调用Arrays.sort()来排序，但面试肯定不会这么简单，使用快排来实现



> #### [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)
>
> 给你一个整数数组 `nums`，请你将该数组升序排列。



**示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

**示例 2：**

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```



**题解：**



~~~ java
class Solution {
    public int[] sortArray(int[] nums) {
        // int[] res = new int[nums.length];
        // PriorityQueue<Integer> queue = new PriorityQueue<>((o1,o2) -> o1 - o2); //小根堆
        // for(int i = 0;i< nums.length ; i++){
        //     queue.add(nums[i]);
        // }
        // for(int j = 0;j < res.length;j++){
        //     res[j] = queue.poll();
        // }
        // return res;
        //使用快排
        quickSort(nums,0,nums.length - 1);
        return nums;

    }
    private void quickSort(int[] nums,int start,int end){
        if(start > end) return ;
        int mid = partition(nums,start,end); //通过一个基准 分成两部分 mid左边的都比基准小，mid右边的都比基准大
        quickSort(nums,start,mid - 1); //然后递归调用 继续分治排序
        quickSort(nums,mid+1,end);
    }
    private int partition(int[] nums,int start ,int end){
        int pivot = nums[start];//基准
        int left = start + 1; //双指针遍历
        int right = end;
        while(left <= right){
            while(left <= right && nums[left] <= pivot) left ++; //如果本来左边的数就小于基准 直接不管 往后走
            while(left <= right && nums[right] >= pivot) right --; //如果本来右边的数大于基准 也直接不管 往前走
            if (left <= right){ //此时不满足
                swap(nums,left,right);//交换两个数位置
            }
        }
        swap(nums,start,right);//将基准和right指针交换 这样right左边全是比他小的 右边全是比他大的 
        return right;
    }
    private void swap(int[] nums,int left,int right){
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
~~~

