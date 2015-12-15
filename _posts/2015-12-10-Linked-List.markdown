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

##Reverse Linked List II## 
[传送门](https://leetcode.com/problems/reverse-linked-list-ii/)

reverse 升级版！只转链表中的某一段。思路和上题一样一样的，可以先数到m，然后从m开始到n截止去做reverse。
Remember to use fakehead, add it to the real head of linklist. Then move pointer to find prenode of m, then use 3 pointers do the same reverve thing as first question. At last, just link the m node (now is the tail of reversed node) to the rest of linklist, and link the previous node (use cur pointer to index it) to the n node.

	public ListNode reverseBetween(ListNode head, int m, int n) {
        if(head==null)
            return head;
        ListNode fakehead = new ListNode(0);
        fakehead.next = head;
        head = fakehead;//////////////
        for(int i = 1;i<m;i++){
            if(head == null)
                return null;
            head = head.next;
        }
        ListNode pre = head;
        ListNode nodem = head.next;
        ListNode cur = nodem;
        ListNode nt = cur.next;
        for(int i = m;i<n;i++){
            if(nt==null)
                return null;
            ListNode temp = nt.next;
            nt.next = cur;
            cur = nt;
            nt = temp;
        }
        nodem.next = nt;
        pre.next = cur;
        return fakehead.next;
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

##MMerge Two Sorted Lists## 
[传送门](https://leetcode.com/problems/merge-two-sorted-lists/)

此题是基础而且相当简单，记得搞个fakehead做新的链表头，然后俩list里小的连在后面，同时更新次链表的指针直到俩都空。如果俩链表长度不同，则将链表的next指向长的那个链表。最后返回fakehead的下一个节点。

	public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode p1 = l1;//pointer to l1
        ListNode p2 = l2;//pointer to l2
        ListNode fake = new ListNode(0);
        ListNode L  = fake;// new pointer
        while(p1!=null&&p2!=null){
            if(p1.val<=p2.val){
                L.next = p1;
                p1 = p1.next;
            }else{
                L.next = p2;
                p2 = p2.next;
            }
            L = L.next;//
        }
        if(p1==null)
            L.next = p2;
        else
            L.next = p1;
        return fake.next;
    }

##Merge k Sorted Lists##
[传送门](https://leetcode.com/problems/merge-k-sorted-lists/)

此题是上一个的延伸版。有三种解法：1.用上面的方法俩俩merge，一只merge到底。因为太傻了，而且时间复杂度很高，所以不用，总的时间复杂度是O(kn^2).因为是一个一个merge到里面的，所以一共有k个list，每个list访问要O(n)。2.第二种我第一反应是divide and conquer， 就是merge sort 的思想。 时间复杂度根据主定理是O(nklogk). T(k) = 2T(k/2)+T(nk) 3.最简单的方法，建最小堆。时间复杂度一样是O(nklogk),但是java PriorityQueue 好方便啊。

	merge sort：

	heap solution：
	public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0)
            return null;
        //init heap
        PriorityQueue<ListNode> minheap = new PriorityQueue(lists.length,
            new Comparator<ListNode>(){
                public int compare(ListNode a,ListNode b){
                    if(a.val > b.val)
                        return 1;
                    if(b.val>a.val)
                        return -1;
                    else
                        return 0;
                    
                }
            }
        );
        for(ListNode list :lists){//吧链表的节点加到堆里
            if(list!=null)
                minheap.add(list);
        }
        //pull出最小的加到链表中直到堆空
        ListNode fake = new ListNode (0);
        ListNode pointer = fake;
        while(minheap.size()>0){
            ListNode temp = minheap.poll();
            pointer.next = temp;
            if(temp.next!=null)
                minheap.add(temp.next);//剩下的node不断加到堆里
            pointer = pointer.next;
        }
    return fake.next;
    }

##Remove Nth Node From End of List##
[传送门](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

##Remove Duplicates from Sorted List II##
[传送门](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

##Rotate List##
[传送门](https://leetcode.com/problems/rotate-list/)

##Swap Nodes in Pairs##
[传送门](https://leetcode.com/problems/swap-nodes-in-pairs/)

##Reverse Nodes in k-Group##
[传送门](https://leetcode.com/problems/reverse-nodes-in-k-group/)

##Reorder List##
[传送门](https://leetcode.com/problems/reorder-list/)

	public void reorderList(ListNode head) {
        if(head!=null&&head.next!=null){
           
            ListNode slow = head;
            ListNode fast = head;
            while(fast!=null&&fast.next!=null&&fast.next.next!=null){
                slow = slow.next;
                fast = fast.next.next;
            }
            ListNode mid = slow.next;
            slow.next = null;
            mid = reverse(mid);
            ListNode p = head;
            ListNode q = mid;
            while(q!=null){
                ListNode t1 = p.next;
                ListNode t2 = q.next;
                p.next = q;
                q.next = t1;
                p = t1;
                q = t2;
            }
        }
    }
    public ListNode reverse(ListNode head){
        if(head==null||head.next==null)
            return head;
        ListNode pre = head;
        ListNode p = head.next;
        while(p!=null){
            ListNode temp = p.next;
            p.next = pre;
            pre = p;
            p = temp;
        }
        head.next = null;
    return pre;
    }

##总结##

1. DummyNode的使用。
做链表题目时，如果我们需要返回头部，一般情况我们可以创建一个虚拟节点，叫DummyNode，把头部挂在它的后面。这样就算头部变化了之后，只要返回DummyNode.next就能轻松得到新头部，而不用纠结新的头部到底 在哪里。
2. Merge LinkedList是相当基础的题目，这么多题目是基于它的，必须写熟。
3. Reverse linkedList最简单的写法就是创建DummyNode，然后把旧的链表不断插入到DummyNode的后面，就能轻松地返回链表了。
4. 操作链表的时候，我们经常会改变某些Node的下一个节点。如果你希望待一下会再用到被改变掉的下一个节点，一定记得用tmp先把它保存起来。
5. 查找链表的中间节点：使用2个快慢指针，一个进2步，一个进1步，快指针到达终点时，慢指针就会停留在链表的中间位置了。



