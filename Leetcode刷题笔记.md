# Leetcode 刷题笔记

## 链表

2.  (2 ->4->3)+(5->6->4) 其中第一位是百位，第二位是十位，第三位是个位

   思路：递归的对应相加，并判断是否要进位，要进位的话就进位

   Class Solution{

   ​    public ListNode addTwoNumbers(ListNode l1,Listcode l2){

   ​        if(l1==null) return l2;

   ​        if(l2==null) return l1;

   ​        ListNode l3 = new ListNode(( l1.val + l2.val)%10);

   ​        l3.next = addTwoNumbers(l1.next,l2.next);

   ​        if((l1.val+l2.val)/10>0){

   ​            l3.next = addTwoNumbers(new ListNode((l1.val+l2.val)/10),l3.next);

   ​         }

   ​      }

   }

   ​

   206.翻转链表

    思路：因为避免链表在某处断开，所以需要三个指针，分别指向当前遍历到的节点、它的前后节点 

   public class Solution{

   ​    public ListNode reversList(ListNode head){

   ​        if (head == null) return null;

   ​        ListNode pNode = head;

   ​        ListNode pReverseHead = null;

   ​        ListNode pPre = null;

   ​        while( pNode != null ){

   ​            ListNode pNext = pNode.next;

   ​            if ( pNext == null ){

   ​                 pReverseHead = pNode;

   ​             }

   ​             pNode.next = pPre;

   ​             pPre = pNode;

   ​             pNode = pNext;

   ​        }

   ​        return pReverseHead;

   ​    }

   }



92.翻转列表2：翻转一个链表中从m到n的位置的元素，

​     如：1->2->3->4->5->null ,m=2  n=4

  return 1->4->3->2->5 ->null

​      思路：与前一题一样，三个指针，startNode,node1,node2

​                 startNode永远指向需要开始reverse的点的前一个位置

​                 node1指向开始reverse的那个node,node2指向第二个需要翻转的node

class Solution {

​    public ListNode reverseBetween(ListNode head , int m , int n){

​        ListNode newHead = new ListNode(-1);

​        newHead.next = head;

​        if( head == null || head.next = null ) return newHead.next;

​        ListNode startNode = newHead;

​        ListNode node1 = null;

​        ListNode node2 = null;

​        for(int i=0;i<n;i++){

​            if( i<m-1 ){

​                startNode = startNode.next;

​             }

​              else if ( i == m-1){

​                          node1=startNode.next;

​                          node2=node1.next;

​                       }

​                       else{

​                             node1.next = node2.next;

​                             node2.next = startNode.next;

​                             startNode.next = node2;

​                             node2 = node1.next;

​                         }

​        }

​        return newHead.next;

​    }

}



