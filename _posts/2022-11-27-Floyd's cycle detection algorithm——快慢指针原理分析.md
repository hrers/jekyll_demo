---
layout: post
title:  "Floyd's cycle detection algorithm——快慢指针原理分析"
date:   2022-11-27 
categories: 算法学习
---
* 目录
{:toc #markdown-toc}

# Floyd's cycle detection algorithm——快慢指针原理分析

## 算法简介(wiki)

**Floyd判圈算法**( **Floyd Cycle Detection Algorithm** )，又称**龟兔赛跑算法**( **Tortoise and Hare Algorithm** )，是一个可以在[有限状态机](https://zh.m.wikipedia.org/wiki/有限状态机)、[迭代函数](https://zh.m.wikipedia.org/wiki/迭代函数)或者[链表](https://zh.m.wikipedia.org/wiki/链表)上判断是否存在[环](https://zh.m.wikipedia.org/wiki/環_(圖論))，求出该环的起点与长度的算法。

* 算法推论

  如果有限状态机、迭代函数或者链表上存在环，那么在某个环上以不同速度前进的2个[指针](https://zh.m.wikipedia.org/wiki/指针_(信息学))必定会在某个时刻相遇。同时显然地，如果从同一个起点(即使这个起点不在某个环上)同时开始以不同速度前进的2个指针最终相遇，那么可以判定存在一个环，且可以求出2者相遇处所在的环的起点与长度。

### 算法分析

* 前提条件

  一条单链表上存在环的情况 

* 算法图示

  <img src="https://raw.githubusercontent.com/hrers/hrers.github.io/gh-pages/_posts/%E7%AE%97%E6%B3%95%E5%AD%A6%E4%B9%A0/2022/11/27/image/FloydCircleDetetionAlgorithmAnalyse.png"/>

* 条件解释

  ```
  令p的速度为q的n倍，即 v_p = n * v_q ，soc作为圈的入口，当两者第一次在圈中相遇时，假设他们行进的距离为dist_p,dist_q ，p此时循环了x圈，q此时循环了y圈。
  dist_p = m + x*l + k 
  dist_q = m + y*l + k
  取值范围:
  速度倍数n:
  	在链表的这个语义下，指针行进的距离只能是整数的速度行进，因为不存在某个指针跑到链表两个节点中间的情况,故n为整数
  p,q行进的圈数x,y:
  	我们以soc为圈的入口，当p,q在meet相遇时，显然两个指针行进了整数倍的圈数加上k的距离才达到的meet点。故,x,y属于整数。
  ```

  

* 推论1：在一个存在环的单链表上，两个指针以不同的速度向前行进，那么当他们相遇的时候他们之间的行进距离之差为整数倍的圈长度

  ```latex
  当两个指针第一次在圈中meet相遇，两者的距离之差为
  dist_p - dist_q
  = m + x*l + k - (m + y*l + k)
  = (x-y)*l
  由于，x为整数，y为整数，故(x-y)为整数，即两者行进的距离之差为整数倍的圈周长
  ```

* 推论2：在相遇点meet,q回到sol点，p从meet点，两者以相同的速度行进，p和q下一个相遇点就在soc.

  ```latex
  当p,q相遇与meet时，两者的行进距离关系为
  dist_p = m + x*l + k 
  dist_q = m + y*l + k
  由于v_p=n*v_q 即
     (m + x*l + k)=n (m+y*l+k)
  -->l*(x-y)=(n-l)(m+k)
  -->l*(x-y)/(n-1)=m+k
  -->l*(int)=m+k   (x=int y=int n=int)-->m+k的长度为圆周长的整数倍 int是一个任意整数
  -->l*(int)-k=m   (int=(m+k)/l) 
  根据l*(int)-k=m 结合上述图示，现在将p,q速度调整为速度一致
  	当p从meet点出发，经过若干圈+(l-k)之后，就会回到soc点,即回到soc要走的距离为l*(int_p)-k
       而q从sol点出发，经过m之后就会到达soc点，而根据结论公式，m恰好长度也是 l*(int_q)-k
  虽然int_p,int_q不一定相同，int_p的取值范围时[1,∞),但int_q是一个定值即int_q=(m+k)/l
  即当l*(int_p)-k=l*(int_q)-k时
  即当int_p==int_q时，
  	q从sol走到soc,行进距离为m;
  	p从meet走到soc，行进距离为(m+k)/l圈-k的长度，k就是当前p所处的meet距soc的距离。当跑到(m+k)/l-1圈的时候回到meet点，再跑l-k就会到达soc点。
  ```

* 算法应用时间复杂度

  * 利用floyd's cycle detection得到带循环链表的循环入口节点的时间复杂度

    以上分析是按照任意速率进行得到相遇的必然性，但是算法设计时需要指定两者之间的速度倍率的。一般来讲，以p速度是q速度的两倍为例，q速度为1；则

    * 第一次相遇的时间复杂度

      由于p的速度是2，q的速度为1，假设最长进入圆圈中的初始相距为整个链表的最大子串长度，即链表的长度N.两个指针没行进一步，他们之间的距离就会缩小1，所以第一次相遇时间复杂度为O(N)

    * 第二次相遇的时间复杂度

      两个指针从第一次相遇meet点，到第二次相遇点soc，需要行进的距离为sol到soc的长度，p转多少圈之后到达soc点，p行进的长度和q是一致的，即m.而长度m是链表子串的长度，即m<N,那么第二次相遇的时间复杂度为O(N)

    所以，floyd's cycle detection获得圆圈入口节点的时间复杂度为O(N),空间复杂度为O(1)

  

## 参考资料

1. https://zh.wikipedia.org/wiki/Floyd%E5%88%A4%E5%9C%88%E7%AE%97%E6%B3%95
2. https://www.youtube.com/watch?v=LUm2ABqAs1w&list=PLs2RZqjweGSP3P6PuWzh82HViqjWknPv4&index=1
3. https://leetcode.cn/problems/c32eOV/solution/lian-biao-zhong-huan-de-ru-kou-jie-dian-vvofe/