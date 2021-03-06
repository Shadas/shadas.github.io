---
layout: post
title: 动态规划的一些笔记 
category: algorithm
tag : algorithm
---

最近几个星期一直在面试, 从小公司到一些有知名度的大厂, 最大的收获就是发现了自己对动归的理解很不到位, 所以仔细学习一下   

#### 动态规划
动态规划是什么, 通过拆分问题, 定义问题状态和状态之间的关系, 按顺序求解子问题, 使问题能够以递推或者分治的方式得以解决, 动态规划更强调状态定义以及状态在阶段之间的转移, 包括转移的策略  
一个问题是否能用动态规划处理, 往往会围绕状态转移来判断, 如果能把问题与子问题的状态定义出, 就可以推导出他们的状态转移方程(通常是一个带条件的递推式), 当然我们给出的状态转移方程, 而我们寻找的一般是一个满足无后效性跟最优子结构的条件状态转移方程  

`无后效性, 这是某一类问题拆分状态时具有的一种独特状态, 可以描述为状态转移到某一个状态时, 后续的状态是不受这个状态前的决策影响的, 在考虑这个状态后的策略时, 跟之前的状态没有关系, 有个例子表示, 比如在求最长公共子序列问题中, 输10个字符, 求到第5个字符的最长子序列结果和只输前5个字符, 求到第5个字符的最长子序列结果是一样的`    

这里说一下我自己理解的递推, 递推是一个由小到大的过程, 或者说是由简单到复杂, 其实递推的过程也是问题状态, 状态在阶段中转移, 这只不过往往递推的状态t1,...,tn, 都是简单独立的, t2单纯由t2推导, t3单纯由t2推导, 各个状态独立。 举个例子就比如求斐波那契数列, 我们只需要每次确定前两个状态, 就可以求出下一个状态(下一个数值), 所以以递推的思路可以做到时间复杂度O(n), 空间复杂度O(1), 其实这个过程也可以用动归来描述, 因为他确实符合动归的特性要求  

继续无效性的话题, 一个具备无后效性的问题, 如果他的递推式或者叫做状态转移方程可以表示为每个状态的最优都是从前一个状态的最优中得到的, 那么其实这种问题可以直接由贪心算法求得, 只需要一直递推最优的选择就可以了。 而往往如果出现了一个不具备无后效性的问题, 搜索就变成唯一方案了, 比如一个迷宫的问题, 必须搜索所有的状态保存起来, 才有可能在终点找到最优的那个方案  

到底选择什么方法, 其实都是由状态以及状态转移的方式决定的, 引用一个别人的[回答](https://www.zhihu.com/question/23995189/answer/35429905):  
>每个阶段只有一个状态->递推  
>每个阶段的最优状态都是由上一个阶段的最优状态得到的(强制要求无后效性)->贪心  
>每个阶段的最优状态是由之前所有阶段的状态的组合得到的->搜索  
>每个阶段的最优状态可以从之前某个阶段的某个或某些状态直接得到而不管之前这个状态是如何得到的->动态规划  

`可以看的出贪心跟动归的显著区别, 贪心每个阶段的最优都是从上个阶段的最优推到的, 所以要求每个状态必定包含上一个状态的最优子结构, 对无后效性与最优子结构是强要求的, 而动归的问题最优解可能包含在之前的多个阶段的多个状态中, 所以需要保存多个前置状态`  

关于贪心跟动归, 找到有这样一段话总结, 他把对于状态的理解放到了一棵树来描述: 
`求最优解的问题, 从根本上说是一种对解空间的遍历, 最直接的就是暴力搜索, 然而最优解的解空间通常都是以指数阶增长。最优解问题大都拆分成一个个的子问题, 把解空间的遍历视作对子问题树的遍历, 完全遍历即为暴力搜索(因此暴力搜索往往也只能采取一些局部搜索的策略, 以时间做限制找到一次近似最优解), 而贪心跟动态规划本质上是对子问题树的一种修剪, 两种算法要求问题都具有的一个性质就是"最优子结构", 即, 组成最优解的每一个子问题的解, 对于这个子问题本身肯定也是最优的, 如果以自顶向下的方向看问题树(原问题看作根), 我们每次只需要向下遍历代表最优解的子树就可以保证会得到整体的最优解, 也就是说可以简单的用一个值(最优值)代表整个子树, 而不用去求出这个子树所可能代表的所有值。 动态规划方法代表了这一类问题的一般解法, 我们自底向上(从叶子向根)构造子问题的解, 对每一个子树的根, 求出下面每一个叶子的值, 并且以其中的最优值作为自身的值, 其它的值舍弃, 动态规划的代价就取决于可选择的数目(树的叉数), 子问题的的数目(树的高度), 每个子问题中影响因素的数目(树的节点数)。 贪心算法是动态规划方法的一个特例, 其特殊在, 不需要知道一个节点所有子树的情况, 就可以求出这个节点的值, 通常这个值都是对于当前的问题情况下, 显而易见的"最优"情况, 因此用"贪心"来描述这个算法的本质。由于贪心算法的这个特性, 它对解空间树的遍历不需要自底向上, 而只需要自根开始, 选择最优的路, 一直走到底就可以了, 这样, 与动态规划相比, 它的代价只取决于子问题的数目, 而选择数目总为1`

对这两个概念体现最好的一个题就是, 所谓面额问题, 一个国家发行的法币应该是能满足贪心算法的, 就是比如需要一个特定的数值A, 那么可选货币范围内, 用贪心就可以选出数量最少的最优解。 比如我们有1, 2, 5, 10这些面额, 所以凑足11就可以用一个10加一个1, 两张纸币就可以满足。 但是如果货币设计成1, 5, 8就会出现问题, 一个11如果按照贪心的算法， 会先选一个8, 之后三个1, 一共四张纸币, 这显然并不是最优解, 因为两个5加一个1, 只需要三张  

所以其实动态规划真正考验我们的地方, 就是准确的找到这种状态的定义, 以及状态的转移方式, 把问题从无序细化到有序, 使之具有无后效性与最优子结构, 把问题的处理方法从搜索推进到动归乃至贪心, 至于到动态规划的细节, 是自顶而下(递归)还是自低而上(递推), 或者利用重叠子, 还是利用所谓备忘录法, 只要我们能找到合适的动态转移方程, 这些方法都是可选的  

#### lcs  
从一到最简单的动归题分析一下, lcs, 有最长公共子序列(Longest Common Subsequence)跟最长公共子串(Longest Common Substring)两个问题可以描述  

#### 最长公共子序列  
比较x=abccabe,y=abcdabf这两个字符串, abb, abca都是x与y的一个子序列, 而abcab是x与y的一个lcs, 也就是最长公共子序列, 可以看出如果暴力搜索去找lcs, 时间复杂度是指数级别的  

设x为s1的下标, y为s2的下标, 我们可以定义lcs(x,y)为问题中的一个状态, 其含义表示以s1[x]与s2[y]结尾的最长公共子串, 可以看出lcs(x-1,y)就是lcs(x,y)之前的一个状态, 如果s1[x] == s2[y] 那么lcs(x,y) = lcs(x-1, y-1) + 1, 这个状态并不难找  

所以我们要分别按照两个字符串以从头到的每个字符的位置来结尾的情况比较, 用一个二维数组来表示matrix[i][j], 状态转移方程为:  

>1. dp[i][j] = 0, 如果i == 0 or j == 0, 当某个字符串为空时, lcs为0  
>2. dp[i][j] = dp[i-1][j-1] + 1, if s1[i] == s2[j]  
>3. dp[i][j] = max{d[i][j-1], dp[i-1][j]}, if s1[i] != s2[j]  

```
int lcsec(char* s1, char* s2)
{
    int len1 = strlen(s1);
    int len2 = strlen(s2);

    int matrix[len1+1][len2+1];
    for(int i = 0; i <= len1; i++)
    {
        for(int j = 0; j <= len2; j++)
        {
            if(i == 0 || j == 0)
                matrix[i][j] = 0;
            else if(s1[i-1] == s2[j-1])
                matrix[i][j] = matrix[i-1][j-1] + 1;
            else
                matrix[i][j] = matrix[i-1][j] < matrix[i][j-1] ? matrix[i][j-1] : matrix[i-1][j];
        }
    }

    for(int i = 0; i<= len1; i++){
        for(int j = 0; j <= len2; j++){
            printf("%d  ", matrix[i][j]);
        }
        printf("\n");
    }
    int i = len1 - 1;
    int j = len2 - 1;                                                                            
    int index = matrix[len1][len2];                                                              
    char s[index+1];                                                                             
    s[index] = '\0';                                                                             
    while( i >= 0 && j >= 0){                                                                    
        if(s1[i] == s2[j]){                                                                      
            s[--index] = s1[i];                                                                  
            i--;                                                                                 
            j--;                                                                                 
        }                                                                                        
        else{                                                                                    
            if(matrix[i+1][j] > matrix[i][j+1]){                                                 
                j--;                                                                             
            }                                                                                    
            else{                                                                                
                i--;                                                                             
            }                                                                                    
        }                                                                                        
    }                                                                                            
    printf("%s\n", s);                                                                           
                                                                                                 
    return matrix[len1][len2];                                                                   
}     
```  

可以看出这里使用了一个matrix[len1+1][len2+1]的二维数组存储每一对可能的字符串的比较结果, 因为我们要打印出最终结果, 所以存储了之前的每个状态, 但是单单看这个问题, 动归并不真正需要存储之前的每一个位置, 严格的说, 只需要记录上一行的每个位置, 就足够完成每次的递推计算了    




  




