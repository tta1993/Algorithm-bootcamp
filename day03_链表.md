# day03 链表理论基础, 203.移除链表元素,707.设计链表, 206.反转链表
## 链表理论基础
#### 链表属性
- 由指针串联
- 分数据域和指针域
- 内存空间储存地址散乱分布
- 由头节点定位，尾节点指向NULL
#### 链表类型
- 单链表：最常见的单向列表，指针域next指针指向后一个节点
- 双链表：双向列表，指针域有prev指针指向前一个节点和next指针指向后一个节点
- 循环链表：首尾相连的链表
#### 链表类型的定义
    class ListNode:
         def __init__(self, val=0, next=None):
             self.val = val
             self.next = next
  
##  203.移除链表元素
#### leetcode地址: https://leetcode.cn/problems/remove-linked-list-elements/
- 注意指针修改方向的顺序
- 创造虚拟头节点简化方法，或者对特殊节点的删除特殊处理
#### 代码
    class Solution:
        def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
            #删除值符合要求的节点，返回头节点
            dummy_head = ListNode(next = head) #加了一个虚拟头节点
            pointer = dummy_head
            while pointer.next != None:
                if pointer.next.val == val:
                    #如果下个节点的值是val，则删除下个节点
                    pointer.next = pointer.next.next
                else:
                    pointer = pointer.next
            return dummy_head.next

## 707.设计链表   :star:
#### leetcode地址: https://leetcode.cn/problems/design-linked-list/
- 定义一个带根据index提取数值，头添加，尾添加，index添加,index删除的链表
- 非常折磨，细节很多，要重点回顾
- index提取数值:有效index的范围
- 头添加，尾添加：注意没有节点的情况，并设置头尾节点指针，链表size跟着改变
- index添加，index删除：注意添加删除的节点位置，有效index的范围，链表size跟着改变
#### 代码
    class ListNode:
        def __init__(self, val = 0, prev = None, next = None):
            self.val = val
            self.prev = prev
            self.next = next

    class MyLinkedList:
        def __init__(self):
            self.head = None
            self.tail = None
            self.size = 0

        def get(self, index: int) -> int:

            if index < 0 or index >= self.size:
                return -1
            current = self.head
            for i in range(index):
                current = current.next
            return current.val


        def addAtHead(self, val: int) -> None:
            new_head = ListNode(val = val,prev = None, next = self.head)
            #一定要注意没有节点的时候,不光要设置加入的节点为头节点，还要设置为尾节点
            if self.head:
                self.head.prev = new_head
            else:
                self.tail = new_head
            self.head = new_head
            self.size += 1
   


        def addAtTail(self, val: int) -> None:
            new_tail = ListNode(val = val,prev = self.tail,next = None)
            if self.tail:
                self.tail.next = new_tail
            else:
                self.head = new_tail
            self.tail = new_tail
            self.size += 1
     

        def addAtIndex(self, index: int, val: int) -> None:
            if index == self.size:
                #addAtTail(val)
                self.addAtTail(val)
            elif index == 0:
                #addAtHead(val)
                self.addAtHead(val)
            elif index < self.size and index > 0:
                current = self.head
                for i in range(index-1):
                    current = current.next
                node = ListNode(val,current,current.next)
                current.next.prev = node
                current.next = node
                self.size += 1

        def deleteAtIndex(self, index: int) -> None:
            if index < 0 or index > self.size-1:
                return
            #注意self.head和self.tail是基础属性，不要顾头不顾尾
            if index == 0:
                #删除第一个元素
                self.head = self.head.next
                #删除元素后列表不为空和删除元素后列表为空的处理
                if self.head:
                    self.head.prev = None
                else:
                    self.tail = None

            elif index == self.size-1:
                #删除最后一个元素
                self.tail = self.tail.prev
                 #删除元素后列表不为空和删除元素后列表为空的处理
                if self.tail:
                    self.tail.next = None
                else:
                    self.head = None

            else:
                new = self.head
                for i in range(index):
                    new = new.next
                new_prev = new.prev
                new_next = new.next
                new_prev.next = new_next
                new_next.prev = new_prev
            self.size -= 1
            
## 206.反转链表
#### leetcode地址: https://leetcode.cn/problems/reverse-linked-list/solutions/2361282/206-fan-zhuan-lian-biao-shuang-zhi-zhen-r1jel/
- 一开始想着取出尾节点再插入头节点，但是太麻烦
- 其实只要改变链表的方向就可以
- 实际上有三个指针，两个用来锁定参与方向改变的两个节点，一个用来保证遍历方向按原链表方向
#### 代码
    class Solution:
        def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
            #思路是一步一步改变指针的方向，head-> tail 改为 head<-tail,而不是真的移动位置

            #需要三个指针
            cur_prev = None
            cur = head

        
            while cur:
                cue_next = cur.next
                cur.next = cur_prev
                cur_prev = cur
                cur = cue_next

            return cur_prev
            #最后一个节点反转过后头节点就不是cur了



