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



