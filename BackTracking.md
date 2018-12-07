# Java Data Structure and Backtracking Algorithm 
Tianyuan Deng
[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This file is about How we apply BackTracking in Java; And also the description of Stiring , Char, Array, Tree; 

The target audience are the CS student or peopl who like coding and algorithm have basic knowledge in this area.(Chinese reader)

 
> Backtracking is a general algorithm for finding all (or some) solutions to some computational problems, notably constraint satisfaction problems, that incrementally builds candidates to the solutions, and abandons each partial candidate c (“backtracks”) as soon as it determines that c cannot possibly be completed to a valid solution

### 基本概念
回溯算法说白了就是穷举法。不过回溯算法使用剪枝函数，剪去一些不可能到达最终状态（即答案状态）的节点，从而减少状态空间树节点的生成。 
回溯法是一个既带有系统性又带有跳跃性的的搜索算法。它在包含问题的所有解的解空间树中，按照深度优先的策略，从根结点出发搜索解空间树。算法搜索至解空间树的任一结点时，总是先判断该结点是否肯定不包含问题的解。如果肯定不包含，则跳过对以该结点为根的子树的系统搜索，逐层向其祖先结点回溯。否则，进入该子树，继续按深度优先的策略进行搜索。

回溯法在用来求问题的所有解时，要回溯到根，且根结点的所有子树都已被搜索遍才结束。
而回溯法在用来求问题的任一解时，只要搜索到问题的一个解就可以结束。
这种以深度优先的方式系统地搜索问题的解的算法称为回溯法，它适用于解一些组合数较大的问题
--------------------- 
### 解题思路：
在leetcode中比较经典的backtracking问题有以下几个： 
51. N-Queens 
52. N-Queens II 
39. Combination Sum 
40. Combination Sum II 
216. Combination Sum III 
46. Permutations 
47. Permutations II 
78. Subsets 
90. Subsets II 
131. Palindrome Partitioning
--------------------- 
### permutations
首先看 permutations 这个问题，是求一个数组的全排列，思路就是将数组当做一个池子，第一次取出一个数，然后在池子里剩下的数中再任意取一个数此时组成两个数，然后再在池子中剩下的数里取数，直到无数可取，即取完一次，形成一个排列。

```sh
public class Solution {
    private List<List<Integer>> result = new ArrayList<List<Integer>>();
    private int length;
    public List<List<Integer>> permute(int[] nums) {
        length = nums.length;
        List<Integer> select = new ArrayList<Integer>();
        helper(select,nums,0);
        return result;
    }
    //s代表已取出的数，nums则是有所有数的池子，pos代表要取第几个位置的数
    public void helper(List<Integer> s,int[] nums,int pos){
        //跳出条件是已取了池子里所有的数，完成一次排列
        if(pos == length){
            result.add(new ArrayList<Integer>(s));
            return;
        }
        for(int i=0;i<nums.length;i++){
            int num = nums[i];
            //取过的数不再取    
            if(s.contains(num)){
                continue;
            }
            s.add(num);
            helper(s,nums,pos+1);
            //重要！！遍历过此节点后，要回溯到上一步，因此要把加入到结果中的此点去除掉！
            s.remove(s.size()-1);
        }
    }
}
```
--------------------- 
我总结其中最重要的有几点：

递归函数的开头写好跳出条件，满足条件才将当前结果加入总结果中
已经拿过的数不再拿 if(s.contains(num)){continue;}
遍历过当前节点后，为了回溯到上一步，要去掉已经加入到结果list中的当前节点。
因此我们可以总结出一个递归函数的模板：

```sh
//list s是已取出的数，nums是原始数组，pos是当前取第几个位置的数
public void helper(List<Integer> s,int[] nums,int pos){
        //跳出条件
        if(……){
            ……
            return;
        }
        //遍历池子中的数
        for(int i=0;i<nums.length;i++){
            int num = nums[i];
            //取过的数不再取    
            if(s.contains(num)){
                continue;
            }
            //取出一个数
            s.add(num);
            //进行下一个位置的取数，pos+1
            helper(s,nums,pos+1);
            //重要！！遍历过此节点后，要回溯到上一步，因此要把加入到结果中的此点去除掉！
            s.remove(s.size()-1);
        }
    }
}

这是最原始的模板，根据题要求的不同会增加参数或各种判断条件，但都离不开这个框架。
```
--------------------- 
### subsets
其次再来看Subsets问题，是取一个数组的组合而不是全排列，基本代码结构都很相似，不同的有：

不是在结果长度等于数组长度时才将结果加入总结果中，而是在每次递归中都将当前组合加入结果中，因为求的是子集而不是全排列。
每次递归不是在池子中随便取一个数加入当前结果，因为此题要求的是子集，[1,3]和[3,1]是相同的，要求的是[1,3]，因此每次在取数时，都要从其位置开始取后面的数，防止取到[3,1]这样的结果。
```sh
public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> s = new ArrayList<Integer>();
        helper(result,s,nums,0,0);
        return result;
    }

    public void helper(List<List<Integer>> result,List<Integer> s,int[] nums,int pos,int iter){
  //不是在结果长度等于数组长度时才将结果加入总结果中，而是在每次helper中都将当前组合加入结果中，因为求的是子集而不是全排列。
        result.add(new ArrayList<Integer>(s));

        if(pos == nums.length){
            return;
        }
        //增加iter参数，用来代表当前数的位置，只能取此数后面的数
        for(int i=iter;i<nums.length;i++){
            if(s.contains(nums[i])){
                continue;
            }
            s.add(nums[i]);
            helper(result,s,nums,pos+1,i);
            s.remove(s.size()-1);
        }
    }
```
--------------------- 
从这两道题可以有许多变种，但都可以归为permutations问题或subsets问题。

### follow-up
像上面这种permutations和subsets问题的II问题一般都是说数组中有重复的数。 
如Permutations II，[1,1,2] have the following unique permutations: 
[ 
[1,1,2], 
[1,2,1], 
[2,1,1] 
] 
这之中有两个重复的1，一次第一次选取了第一个1作为全排列的第一个数，得到112和121。但若选择第二个1作为全排列的第一个数，依旧能得到112和121，此时就会有重复的全排列出现，因此，在每一次选择同一位置的数时（如选择全排列第一个位置的数），只选择重复的数中的第一个。 
为了保证重复的数在一起，一般这种问题都要对数组进行一个排序处理： 

```sh
Arrays.sort(nums);
```

然后我再另外用一个数组来记录每个数在当前位置处有没有被取过：

```sh
int[] visited = new int[nums.length];
```

没有取过的话默认为0，取了则设为1；

因此比如当我们在全排列的第一个位置取了第二个1，而此时发现第一个1并没有被取过，这就说明第一个1还没有被作为全排列的第一个位置，因此此时就应该放弃取第二个1作为全排列的第一个位置。

因此递归函数应为：
```sh
public void helper(List<Integer> s,int[] nums,int[] visited,int pos){
        if(pos == length){
            List<Integer> list = new ArrayList<Integer>(s);
            result.add(list);
            return;
        }
        for(int i=0;i<nums.length;i++){
//如果此数取过了则不再取||此数和前面的数相同而前面的数没有取，则此数也不能取
            if(visited[i]==1 || (i>0 && nums[i] == nums[i-1]&& visited[i-1] == 0)){
                continue;
            }
            s.add(nums[i]);
            visited[i] = 1;
            helper(s,nums,visited,pos+1);
            //注意要回溯到上一步的话也要把visited当前位置还原
            s.remove(s.size()-1);
            visited[i] = 0;
        }
    }
   ```
--------------------- 
subsets问题在处理重复数时同理。

## 总结
backtracking类型题其实思路都是一致的，连解决follow-up都是一致的，只要掌握了思路，做同一类型的题都是手到擒来。

## Java 数组
数组对于每一门编程语言来说都是重要的数据结构之一，当然不同语言对数组的实现及处理也不尽相同。
Java 语言中提供的数组是用来存储固定大小的同类型元素。
你可以声明一个数组变量，如 numbers[100] 来代替直接声明 100 个独立变量 number0，number1，....，number99。

### 声明数组变量
首先必须声明数组变量，才能在程序中使用数组。下面是声明数组变量的语法：

```sh
dataType[] arrayRefVar;   // 首选的方法
 
或
 
dataType arrayRefVar[];  // 效果相同，但不是首选方法
```
注意: 建议使用 dataType[] arrayRefVar 的声明风格声明数组变量。 dataType arrayRefVar[] 风格是来自 C/C++ 语言 ，在Java中采用是为了让 C/C++ 程序员能够快速理解java语言。

### 实例
下面是这两种语法的代码示例：
```sh
double[] myList;         // 首选的方法
 
或
 
double myList[];         //  效果相同，但不是首选方法
```

创建数组
Java语言使用new操作符来创建数组，语法如下：
```sh
arrayRefVar = new dataType[arraySize];
```
上面的语法语句做了两件事：

>一、使用 dataType[arraySize] 创建了一个数组。
>二、把新创建的数组的引用赋值给变量 arrayRefVar。
数组变量的声明，和创建数组可以用一条语句完成，如下所示：

```sh
dataType[] arrayRefVar = new dataType[arraySize];
```
另外，你还可以使用如下的方式创建数组。

```sh
dataType[] arrayRefVar = {value0, value1, ..., valuek};
```

数组的元素是通过索引访问的。数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1。

### 实例
下面的语句首先声明了一个数组变量 myList，接着创建了一个包含 10 个 double 类型元素的数组，并且把它的引用赋值给 myList 变量。

TestArray.java 文件代码：
```sh
public class TestArray {
   public static void main(String[] args) {
      // 数组大小
      int size = 10;
      // 定义数组
      double[] myList = new double[size];
      myList[0] = 5.6;
      myList[1] = 4.5;
      myList[2] = 3.3;
      myList[3] = 13.2;
      myList[4] = 4.0;
      myList[5] = 34.33;
      myList[6] = 34.0;
      myList[7] = 45.45;
      myList[8] = 99.993;
      myList[9] = 11123;
      // 计算所有元素的总和
      double total = 0;
      for (int i = 0; i < size; i++) {
         total += myList[i];
      }
      System.out.println("总和为： " + total);
   }
}
```
以上实例输出结果为：
总和为： 11367.373

### Arrays 类
java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。

具有以下功能：

>给数组赋值：通过 fill 方法。
对数组排序：通过 sort 方法,按升序。
比较数组：通过 equals 方法比较数组中元素值是否相等。
查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

具体说明请查看下表：

|序号|方法和说明|
| -- | -------- |
1|	public static int binarySearch(Object[] a, Object key)|
- |用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1)。|
2|	public static boolean equals(long[] a, long[] a2)
- |如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。
3|	public static void fill(int[] a, int val)
-|将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。
4|	public static void sort(Object[] a)
-|对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。


---------------------------END--------------------------------






