#### 1.两两交换链表中的节点

**题目链接**：https://leetcode.cn/problems/swap-nodes-in-pairs/

**解题思路**：创建虚拟头节点 ，判断虚拟头结点的后面二个节点是否为空。

```java
/**
 * 自己写的版本
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode preHead = new ListNode();
        preHead.next = head;
        ListNode cur = preHead;
        ListNode temp;
        while(cur.next != null && cur.next.next != null){
            temp = cur.next.next;
            cur.next.next = temp.next;
            temp.next = cur.next;
            cur.next = temp;
            cur = cur.next.next;
        }
        return preHead.next;
    }
}

// 递归版本
class Solution {
    public ListNode swapPairs(ListNode head) {
        // base case 退出提交
        if(head == null || head.next == null) return head;
        // 获取当前节点的下一个节点
        ListNode next = head.next;
        // 进行递归
        ListNode newNode = swapPairs(next.next);
        // 这里进行交换
        next.next = head;
        head.next = newNode;

        return next;
    }
} 
// 卡尔
class Solution {
  public ListNode swapPairs(ListNode head) {
        ListNode dumyhead = new ListNode(-1); // 设置一个虚拟头结点
        dumyhead.next = head; // 将虚拟头结点指向head，这样方面后面做删除操作
        ListNode cur = dumyhead;
        ListNode temp; // 临时节点，保存两个节点后面的节点
        ListNode firstnode; // 临时节点，保存两个节点之中的第一个节点
        ListNode secondnode; // 临时节点，保存两个节点之中的第二个节点
        while (cur.next != null && cur.next.next != null) {
            temp = cur.next.next.next;
            firstnode = cur.next;
            secondnode = cur.next.next;
            cur.next = secondnode;       // 步骤一
            secondnode.next = firstnode; // 步骤二
            firstnode.next = temp;      // 步骤三
            cur = firstnode; // cur移动，准备下一轮交换
        }
        return dumyhead.next;  
    }
}
```

#### 2.删除链表的倒数第N个节点

**题目链接**：https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

**解题思路**：我：通过得到链表长度减去n得到倒数的节点，实现正向转换。卡尔：快慢指针，相距n个长度实现删除。

```java
// 自己写的
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int size = 0;
        ListNode preHead = new ListNode();
        preHead.next = head;
        ListNode temp = preHead;
        while(temp.next != null){
            size++;
            temp = temp.next;
        }
        temp = preHead;
        int step = size - n;
        while(step != 0){
            temp = temp.next;
            step--;
        }
        temp.next = temp.next.next;
        return preHead.next;

    }
}

// 卡尔
public ListNode removeNthFromEnd(ListNode head, int n){
    ListNode dummyNode = new ListNode(0);
    dummyNode.next = head;

    ListNode fastIndex = dummyNode;
    ListNode slowIndex = dummyNode;

    //只要快慢指针相差 n 个结点即可
    for (int i = 0; i < n  ; i++){
        fastIndex = fastIndex.next;
    }

    while (fastIndex.next != null){
        fastIndex = fastIndex.next;
        slowIndex = slowIndex.next;
    }

    //此时 slowIndex 的位置就是待删除元素的前一个位置。
    //具体情况可自己画一个链表长度为 3 的图来模拟代码来理解
    slowIndex.next = slowIndex.next.next;
    return dummyNode.next;
}
```



#### 3.链表相交

**题目链接**：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/

**解题思路**：主要是同步二个链表的长度，使链表从相交（如果有）的节点，向前相同数量的顶点开始向后面遍历。

```java
// 自己写的
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = 0;
        int lenB = 0;
        int len = 0;
        ListNode tempA = headA;
        ListNode tempB = headB;
        while(tempA != null){
            lenA++;
            tempA = tempA.next;
        }
        while(tempB != null){
            lenB++;
            tempB = tempB.next;
        }
        len = lenA > lenB ? (lenA-lenB):(lenB-lenA);
        if(lenA > lenB){
            while(len != 0){
                headA = headA.next;
                len--;
            }
        }
        if(lenA < lenB){
            while(len != 0){
                headB = headB.next;
                len--;
            }
        }
        while(headA != null && headB != null){
            if(headA == headB){
                return headA;
            }else{
                headA = headA.next;
                headB = headB.next;
            }
        }
        return null;
    }
}
// 卡尔写的
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        int lenA = 0, lenB = 0;
        while (curA != null) { // 求链表A的长度
            lenA++;
            curA = curA.next;
        }
        while (curB != null) { // 求链表B的长度
            lenB++;
            curB = curB.next;
        }
        curA = headA;
        curB = headB;
        // 让curA为最长链表的头，lenA为其长度
        if (lenB > lenA) {
            //1. swap (lenA, lenB);
            int tmpLen = lenA;
            lenA = lenB;
            lenB = tmpLen;
            //2. swap (curA, curB);
            ListNode tmpNode = curA;
            curA = curB;
            curB = tmpNode;
        }
        // 求长度差
        int gap = lenA - lenB;
        // 让curA和curB在同一起点上（末尾位置对齐）
        while (gap-- > 0) {
            curA = curA.next;
        }
        // 遍历curA 和 curB，遇到相同则直接返回
        while (curA != null) {
            if (curA == curB) {
                return curA;
            }
            curA = curA.next;
            curB = curB.next;
        }
        return null;
    }

}
```

#### 4.环形链表II

**题目链接**：https://leetcode.cn/problems/linked-list-cycle-ii/

**解题思路**：公式推导。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```

