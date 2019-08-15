算法的概念

algorithms

https://en.wikipedia.org/wiki/Algorithm

[https://zh.wikipedia.org/wiki/%E7%AE%97%E6%B3%95](https://zh.wikipedia.org/wiki/算法)

**如何实现一个高效的单向链表逆序输出？**

时间复杂度和空间复杂度的概念



## 二叉树、出现翻转





常见的时间复杂度



| 操作数                 | 时间复杂度 | 名称  |
| ---------------------- | ---------- | ----- |
| 3                      | O(1)       | 常数  |
| 105log2                | O(logn)    | 对数  |
| 2n+4                   | O(n)       | 线性  |
| 7n+9nlogn+39           | O(nlogn)   | nlogn |
| 3n²+2n+5               | O(n²)      | 平方  |
| 3n³+2n+5               | O(n³)      | 立方  |
| 2^n                    | O(2^n)     | 指数  |
| n*(n-1)*(n-2)....3*2*1 | O(n!)      | 阶乘  |
| n^n                    | O(n^n)     |       |

https://blog.csdn.net/jw903/article/details/37558187

O(1)>O(log2n)>O(n)>O(nlog2n)>O(n^2)>O(n^3)>O(2^n)>O(n!)>O(n^n)

最坏情况

最好情况

平均情况



空间复杂度

​	算法需要消耗的内存空间

​	包括程序代码所占的空间、输入数据所占用的空间和辅助变量所占用的空间





常见的查找算法



常见的数据结构

​	数组

​	list 链表 双链表

​	map

​	stack 先进后出

​	heap 堆 二叉堆 

​	queue

​	set

​	graph 图 通常使用邻接矩阵和邻接表标识，前者容易实现但是对于稀疏矩阵会浪费较多空间，后者使用链表的方式存储信息但是对于图搜索时间复杂度较高





php实现双向链表

​	array_shift  array_unshift

​	array_pop array_push



斐波那契数列





写出常见的排序算法

冒泡排序

快速排序





深入理解HashMap和HashTable

<https://blog.csdn.net/jesse_wuguangzu/article/details/39807691>



简单说下快速排序算法



互联网公司最常见的面试算法题有哪些？

<https://www.zhihu.com/question/24964987>







基本思想：通过一趟排序将待排序列分割成两部分，其中一部分比另一部分记录小，再分别对这两部分继续快速排序，以达到有序。

算法实现：设有两个指针low和high，初值为low=1，high=n，设基准值为key(通常选第一个)，则首先从high位置开始向前搜索，找到第一个比key小的记录与key交换，然后从low位置向后搜索，找到第一个比key大的记录与基准值交换，重复直至low=high为止。





第一趟排序结果，key之前的记录值比key之后的记录值小。





11 25 9  3  16 2   //选择11为key

2  25 9  3  16 11

2  11 9  3  16 25

2  3  9  11 16 25











对快速排序算法理解



排序算法看了很多遍，可还是看完以后当时懂了，过后就又忘了。排序算法是很多算法的基础，也是找工作必考内容。今年微软的实习生笔试考到哪几种排序算法是稳定的，前几天的一个摩根的电话面试，对方让我讲讲快速排序算法的思想（完全不记得，只能说不会了）。今天早上起来看了一会《算法》 ，看到基本的排序算法，突然想到了应该怎么理解快速排序算法，用博客记录下来，加深印象。

首先说说最基本的排序算法：选择排序。

选择排序是最简单的排序算法，也是速度最慢的排序算法，时间复杂度为O(n^2)，需要进行～n^2次比较。算法的基本思想是：进行n次循环，每次选取剩余元素中最小的元素。代码如下：

0102030405060708091011121314151617 public class Selection{  public static void sort(Comparable[] a)  {    int N = a.length;    for(int i = 0; i < N; i++)    {      // 将a[i]与a[i+1...N]中最小的元素交换      int min = i;      for(int j = i+1; j < N; j++)      {        if(less(a[j], a[min])) min = j;      }      exch(a, i, min);    }  }}

首先，为什么选择排序性能低？因为它要进行～n^2次比较，而这些比较有很多是没必要的。比如，对于数组{4, 3, 5, 2, 6, 1}，其中有6个元素，使用选择排序，第一次循环，第一个元素4要和后面的五个元素分别进行比较，在这次比较中有一个重要的信息没有利用到，那就是3、2、1三个元素比4小，而5、6两个元素比4小，也就是说，在后续的循环中对于3、2、1和5、6分别进行的比较是多余的。于是我想到如果能利用好这个重要的信息不就能减少比较次数，提高排序算法的性能了，而这正是快速排序算法的思想了。

以上是我对快速排序算法思想的理解，关于快排具体怎么实现的就不多说了。

另外，对于如何提高算法性能，想法也应该是类似的。对于一个问题，先想一个naive的算法，然后分析这个算法，看有没有重复的操作，有哪些操作是不必要的，然后改进算法。如果实在没有多余的操作，那说不定使用什么特殊的数据结构能够提高算法效率，也就是空间换时间的问题。还有很多其他办法提高算法效率，比如排序，有时间专门写一篇博客总结吧。





使用PHP描述冒泡排序和快速排序算法，对象可以是一个数组



//对数组冒泡排序

时间换空间

原理是临近的数字两两进行比较,按照从小到大或者从大到小的顺序进行交换,

因此冒泡排序总的平均时间复杂度为O(n^2) 。



function bubbleSort($numbers) {

​    $cnt = count($numbers);

​    for ($i = 0; $i < $cnt; $i++) {

​        for ($j = 0; $j < $cnt - $i - 1; $j++) {

​            if ($numbers[$j] > $numbers[$j + 1]) {

​                $temp = $numbers[$j];

​                $numbers[$j] = $numbers[$j + 1];

​                $numbers[$j + 1] = $temp;

​            }

​        }

​    }

 

​    return $numbers;

}



快速排序：不稳定，时间复杂度 最理想 O(nlogn) 最差时间O(n^2)

<?php

function quickSort(&$arr){

​    if(count($arr)>1){

​        $k=$arr[0];

​        $x=array();

​        $y=array();

​        $_size=count($arr);

​        for($i=1;$i<$_size;$i++){

​            if($arr[$i]<=$k){

​                $x[]=$arr[$i];

​            }elseif($arr[$i]>$k){

​                $y[]=$arr[$i];

​            }

​        }

​        $x=quickSort($x);

​        $y=quickSort($y);

​        return array_merge($x,array($k),$y);

​    }else{

​        return$arr;

​    }

}

?>

使用PHP描述顺序查找和二分查找算法，顺序查找必须考虑效率，对象可以是一个有序数组



//使用二分查找数组中某个元素function bin_sch($array, $low, $high, $k){

​    if ($low <= $high){

​        $mid = intval(($low+$high)/2);

​        if ($array[$mid] == $k){

​            return $mid;

​        }elseif ($k < $array[$mid]){

​        return bin_sch($array, $low, $mid-1, $k);

​    }else{

​        return bin_sch($array, $mid+1, $high, $k);

​    }

​    }

​    return -1;

}





写一个二维数组排序算法函数，可以调用php内置函数，能够具有通用性



function array_sort($arr, $keys, $order=0) {

​    if (!is_array($arr)) {

​        return false;

​    }

​    $keysvalue = array();

​    foreach($arr as $key => $val) {

​        $keysvalue[$key] = $val[$keys];

​    }

​    if($order == 0){

​        asort($keysvalue);

​    }else {

​        arsort($keysvalue);

​    }

​    reset($keysvalue);

​    foreach($keysvalue as $key => $vals) {

​        $keysort[$key] = $key;

​    }

​    $new_array = array();

​    foreach($keysort as $key => $val) {

​        $new_array[$key] = $arr[$val];

​    }

​    return $new_array;

}







