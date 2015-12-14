---
layout: post
title: 链表特辑
date: 2015-12-10
categories: Leetcode
---

#链表#

链表题还是有很多的。三道hard，12道medium。在leetcode中给定的链表都是单向链表<br/>
Update: 2015-12-13

##Reverse Linked List## 
[传送门](https://leetcode.com/problems/reverse-linked-list/)

Two ways to implement:

1. iteratively

抓住要点就是将当前节点的后一个节点先保存，然后把当前的next指向之前的节点，pre用来维护当前节点的前面一个节点。

	public ListNode reverseList(ListNode head) {
       
        ListNode pre = null;
        ListNode cur = head;
        while(cur!=null){
            ListNode p = cur.next;
            cur.next = pre;
            pre = cur;
            cur = p;
        }
        return pre;
    }
	
2. recursively

这个做法比较tricky， 就是每次只看两个节点。吧已经递归好的后面的节点记到一个叫rest的节点上，然后从第二个节点开始递归，将当前的第二个节点的next指向当前的头节点head上。最后返回所有已经反好向的头rest。
	
	public ListNode reverseList(ListNode head) {
        
       if(head==null || head.next == null)
        return head;

	    ListNode second = head.next;//get second node   
	    head.next = null;//set first's next to be null

	    ListNode rest = reverseList(second);
	    second.next = head;
	    return rest;
    }

##Add Two Numbers##  
[传送门](https://leetcode.com/problems/add-two-numbers/)

给l1,l2 算和用链表形式输出。<br/>
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

这个题的思路非常简单：分3中情况，l1短，l2短，一样长。 然后新建链表l3 将和输出。这个题自己写了很久发现思路非常冗余。写了3个while来分别确认每种情况。其实应该用一个while就可以搞定，里面分别判断前面两种情况。然后while做完了判断是否还需要再进位。一下的写法是参考一个非常简洁的写法，感觉buf这个东西用的很妙啊，能用一个绝对不会用俩来维护个位数字和进位数字的。

	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	        ListNode p = l1;
	        ListNode q = l2;
	        ListNode l3 = new ListNode(0);
	        ListNode r = l3;
	        int buf = 0;
	        while(p!=null||q!=null){
	            if(p!=null){
	                buf += p.val;
	                p = p.next;
	            }
	            if(q!=null){
	                buf += q.val;
	                q = q.next;
	            }
	            r.next  = new ListNode(buf%10);
	            r = r.next; 
	            buf /= 10;
	        }
	        if(buf>0)
	            r.next  = new ListNode(buf);
	        return l3.next;
	    }






