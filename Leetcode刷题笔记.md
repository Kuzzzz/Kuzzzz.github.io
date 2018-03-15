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



83.删除已排序的链表中重复的元素
思路：用前后两个指针指向链表，如果两个指针指向的值相等，那么第二个指针就往后挪，挪到与第一个指针不同的值为止，然后让第一个指针的next指向第二个指针，之后两个指针同时向后挪一个，一直到结束。
特殊情况：当list的结尾几个node是重复的时候,第二个指针会指向Null,需要特殊处理，令pre.next=null,这样list的尾部就不会丢

class Solution{
  public ListNode RemoveDuplicates(ListNode head){
    if( head == null || head.next ==null) return null;
    
    ListNode pre = new ListNode(-1);
    ListNode cur = null;
    
    pre.next = head;
    cur = head;
    
    while( cur != null){
      if (pre.val == cur.val){
        cur = cur.next;
        if ( cur == null){
          pre.next = null；
        }
      }
      else{
        pre.next = cur;
        pre = pre.next;
        cur = cur.next;
      }
    }
    return head;
  }
}


82.删除已排序的链表中重复的元素②
与上一题相似，不同之处在于遇到相同的元素就把该元素全部删掉，因此需要三个指针，且增加一个伪头结点
思路：设置一个flag变量来判断是否需要删除元素，当没有遇到重复的元素时，三个指针同时向后移动。如果遇到重复元素，设置flag为true，并让ptr2一直往后找找到第一个与ptr1值不等的位置时停止，这时，ptr1指向的node的值是一个重复值，需要删除，所以这时就需要让ptr0的next连上当前的ptr2，这样就把所有重复值略过了。然后，让ptr1和ptr2往后挪动继续查找。
      当ptr2一直往后找的过程中，是有可能ptr2==null（这种情况就是list的最后几个元素是重复的，例如1->2->3->3->null)，这时ptr1指向的值肯定是需要被删除的，所以要特殊处理，令ptr0的next等于null，把重复值删掉。其他情况说明最后几个元素不重复，不需要处理结尾，遍历就够了

class Solution{
  public ListNode RemoveDuplicates2(ListNode head){
    if( head == null || head.next == null ) return head;
    
    ListNode fakehead = new ListNode(-1);
    fakehead.next = head;
    ListNode pre = fakehead;
    ListNode cur = fakehead.next;
    ListNode pas = fakehead.next.next;
    Boolean flag = false;
    while( pas != null){
      if(cur.val == pas.val){
          flag = true;
          pas = pas.next;
       }
       else{
          if(flag){
             pre.next = pas;
             flag = false;
           }
           else{
             pre.next = cur;
            }  
           cur = pas;
           pas = pas.next;
        }
     }
    return fakehead.next;
  }
}


61.轮转列表 
给定一个列表，倒着数k个node，从那开始到结尾的所有元素和前面的部分位置对调
如；1->2->3->4->5  k=2   变成  4->5->1->2->3
思路：特殊：k可以是大于整个List长度的数，这时要对len取模，如果取模之后是0 就不用rorate 直接返回
看到这种倒着数数的直接使用双指针，pre和cur一前一后同时向后移动。两个指针之间的间隔为k，当ptr2到达最后一个元素后，ptr1就是倒数第k个的位置。移动结束后，newhead = pre.next;  cur.next= head;  pre.next = null;  


class Solution{
  public ListNode rotateRight(ListNode head,int k){
    if(head == null || head.next == null) return head;
        ListNode fakehead = new ListNode(-1);
        fakehead.next = head;
        ListNode pre = head;
        ListNode cur = head;
        int length = 0;
        //计算链表长度
        while(head != null){
            length++;
            head = head.next;
        }
        k = k%length;
        int i = 0;
        //cur到达指定位置
        while(i<k){
            i++;
            cur = cur.next;
        }
        //pre cur同时向后移动，直到cur到达最后
        while(cur.next != null){
            pre=pre.next;
            cur=cur.next;
        }
        cur.next = fakehead.next;
        fakehead.next = pre.next;
        pre.next = null;
        
        return fakehead.next;
  }
}


19.删除从链表末尾开始向前的第n个元素
思路：同上一题，需要两个指针，一前一后 间隔为n，当cur指向链表尾节点时pre指向的就是要删除的元素。
特殊情况：n的值和链表长度相同时，要删除第一个点，fakehead.next=fakehead.next.next; 返回fakehead.next

class Solution{
  public ListNode removeNthFromEnd(ListNode head,int n){
    if(head == null) return head;
        
        ListNode fakehead = new ListNode(-1);
        fakehead.next =head;
        ListNode pre = fakehead;
        ListNode cur = fakehead;
        int length = 0;
        int i = 0;
        
        while(head != null){
            length++;
            head = head.next;
        }
        //处理length == n时的情况
        if(n == length){
            fakehead.next = fakehead.next.next;
            return fakehead.next;
        }

        //让pre和cur之间相差n
        while(i<n){
            i++;
            cur = cur.next;
        }
        //让pre和cur同时向后移动，知道cur.next == null
        while(cur.next != null){
            pre = pre.next;
            cur = cur.next;
        }
        //pre.next 就是需要删除的元素
        pre.next = pre.next.next;
        
        return fakehead.next;  
   }
}

24.交换链表中相邻两个元素的位置   
如：1->2->3->4  得到2->1->4->3
思路：用两个指针分别指向相邻元素，然后交换两个相邻元素的值，之后分别同时向后移动，重复之前操作

class Solution{
  public ListNode swapPairs(ListNode head){
    if(head == null || head.next == null) return head;
        
        ListNode fakehead = new ListNode(-1);
        fakehead.next = head;
        ListNode pre = head;
        ListNode cur = head.next;
        int value = 0;
        while(pre != null && cur != null){
            value = pre.val;
            pre.val = cur.val;
            cur.val = value;
            if(cur.next != null){
                pre.next = cur.next;
                if(pre.next != null){
                    cur.next = pre.next;
                }
                else break;
            }else break;
        }
        return fakehead.next;
  }
}

