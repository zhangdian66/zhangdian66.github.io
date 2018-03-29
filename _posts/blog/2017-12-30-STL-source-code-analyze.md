---
layout: post
title: "STL source code analyze"
description: STL分析
category: blog
---
##目的
学习stl源码，最近对于模板编程很有兴趣。STL应该是很经典的代码了，好好研究一下代码
架构。中间有很多内容参考侯捷《STL源码分析》，极为推荐大家详细研读。
这将是一个很长的内容，不断更新中。

##多线程
to do

##内存管理
内存管理的目的：
1：减少内存碎片化
2：减少内存分配时间

```
template <class  T, class Alloc>
class simple_alloc {
public:
    static T* allocate(size_t n) 
        { return 0 == n? 0: (T*)Alloc::allocate(n * sizeof(T)); }
    static T* allocate(void)
        { return (Tp*)Alloc::allocate(sizeof(T)); }
    static void deallocate(T* p, size_t n)
        { if(n != 0) Alloc::deallocate(p, n*sizeof(T)); }
    static void deallocate(T* p)
        { Alloc::deallocate(p, sizeof(T)); }
};
```
simple_alloc只是封装了Alloc类的响应函数，作为统一接口。

分析vector函数，可以看到Alloc类是调用alloc类

```
template <class _Tp, class _Alloc = __STL_DEFAULT_ALLOCATOR(_Tp) >
class vector {  }
#define __STL_DEFAULT_ALLOCATOR(T) alloc
typedef __default_alloc_template<__NODE_ALLOCATOR_THREADS, 0> alloc
```
根据以上代码调用可以发现，最终为vector类分配空间的是default_alloc_template类。

如图所示，内存分配分为两个部分，如果大于128B以上，由malloc直接分配，否则由一系列
链表分配。这种分配方式和scst，slub极为相似。只是该类只需维护一个free list，
已分配内存由客户负责释放，或者等进程结束由系统负责收回。这里多插一句，
操作系统使用brk或者mmap分配内存，然后标准C库提供了malloc，STL又提供更精细的
内存分配。

default_alloc_template类基本设计如下，16个间隔8字节的链表，一个heap空间和相应的
分配函数。

```
static void* allocate(size_t n)
{
    void* result= NULL;
    if(n > 128){
        result = malloc_alloc::allocate(n);
    }else{
        Obj* my_free_list = S_free_list + _S_freelist_index(n);
        _lock _lock_instance;   //多线程处理

    }
}
```



