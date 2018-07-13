---
layout: post
title: partition算法
description: 深入研究一点点
category: blog
---

##Partiton算法##
partition算法只要应用于quicksort排序中，其实还有更多其他的应用，一起总结学习。
[参考文档][link1]  

##Partition 算法实现##
比较高效的算法是使用双指针进行分组，代码如下：
```c++
int partition(vector<int>& arr, int begin, int end)
{
    int ref = arr[begin];
    while(end > begin){
        while(end > begin && arr[end] >= ref)
            end--;
        arr[begin] = arr[end];
        while(end > begin && arr[begin] < ref)
            begin++;
        arr[end] = arr[begin];
    }
    arr[begin] = ref;
    return begin;
}

```
基本过程： 
1. 选择参考值（这儿可以优化）  
2. 将大于参考值的丢后面，将小于参考值的丢前面
3. 最后将参考值归位并返回参考值的问题

```
void quicksort(vector<int>& arr, int begin, int end)
{
    if(begin >= end)
        return;
    int pos = partition(arr, begin ,end);
    quicksort(arr, begin, pos);
    quicksort(arr, pos+1, end);
    return;
}
```

###寻找K-th数字###
在一个无序数组中查找第k-th的数字  
基本思路如下:
1. if(pos == k - 1), bingo
2. if(pos < k - 1), the left part
3. if(pos > k - 1), the right part  

```
int find-kth-number(vector<int>& arr, int k){
    int begin = 0;
    int end = arr.size() - 1;
    if(k < 0 || k > end) return -1;
    while(begin < end){
        int pos = partition(arr, begin, end);
        if(pos == k - 1) return arr[pos];
        (pos > k - 1) ? ( end = pos ) : ( start = pos - 1 );
    }
}
```
分析一下算法的时间复杂度，最差时间复杂度为O(n^2), 平均时间复杂度为O(nlgn);

###Three Way Partition###
假如一次性排序可以将数组的元素分为大于，等于，小于三组。
一次扫描，多次交换，最终实现平衡。
```
void three-way-partition(vector<int>& arr, int ref){
    int less_pos = 0;
    int bigger_pos = arr.size() - 1;
    int scan_pos = 0;
    while(scan_pos < bigger_pos){
        if(arr[scan_pos] > target){
            swap(arr[scan_pos], arr[bigger_pos--]);
        }else(arr[scan_pos] < target){
            swap(arr[scan_pos], arr[less_pos++]);
        }else
            scan_pos++;
    }
}
```







[link1]:    http://selfboot.cn/2016/09/01/lost_partition/
