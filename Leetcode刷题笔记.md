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



86.对链表分区
给定一个链表和一个值x，把链表中小于x的数都放到大于等于x的数的左边，并保持原始的相对位置
如给定 1->4->3->2->5->2   x=3      输出结果为 1->2->2->4->3->5
思路：1.new两个新链表，一个用来管理所有大于等于x的数，一个用来管理小于x的数。遍历原来的链表时，当当前node.val大于等于x就接在大链表上，当当前node.val小于x，就接在小链表上。
2.最后把小链表的最后一个接上大链表的第一个，大链表最后一个元数的next设置为null

class Solution {
    public ListNode partition(ListNode head, int x){
        //先判断异常条件
        if ( head == null || head.next == null) return head;
        
        //再判断正常条件
        ListNode smallNode = new ListNode(-1);
        ListNode bigNode = new ListNode(-1);
        ListNode originLessHead = smallNode;
        ListNode originBigHead = bigNode;
        while(head != null){
            if ( head.val < x ) {
                smallNode.next = head;
                smallNode = smallNode.next;
            }
            else{
                 bigNode.next = head;
                 bigNode = bigNode.next;
                }
            head = head.next;
        }
        //让小的队列连接上大的队列，同时让大的队列末尾是null
        smallNode.next = originBigHead.next;
        bigNode.next = null;
        return smallLessHead.next;
    }
}