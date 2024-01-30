# 链表

#### [138.随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/description) <a href="#id-138" id="id-138"></a>

[详解](https://leetcode.cn/problems/copy-list-with-random-pointer/solutions/2361362/138-fu-zhi-dai-sui-ji-zhi-zhen-de-lian-b-6jeo/?envType=study-plan-v2\&envId=selected-coding-interview)

**hash map实现**

```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    // hash map
    Node* copyRandomList(Node* head) {
        if(head == nullptr) return nullptr;
        Node* cur = head;
        unordered_map<Node*, Node*> map;
        // 复制链表
        while(cur != nullptr) {
            map[cur] = new Node(cur->val);
            cur = cur->next;
        }
        // 连接引用
        cur = head;
        while (cur != nullptr) {
            map[cur]->next = map[cur->next];
            map[cur]->random = map[cur->random];
            cur = cur->next;
        }
        return map[head];
    }
};
```

```Go
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    mapping := make(map[*Node]*Node)
    cur := head

   //创建新的链表，复制值
    for cur != nil {
        node := &Node{Val: cur.Val}
        mapping[cur] = node
        cur = cur.Next
    }
  // 复制原链表指针引用
    cur = head
    for cur != nil {
        mapping[cur].Next = mapping[cur.Next]
        mapping[cur].Random = mapping[cur.Random]
        cur = cur.Next
    }
    return mapping[head]
}
```

**拼接+拆分**

```Python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        if not head: return

        cur = head
        while cur:
            tmp = Node(cur.val)
            tmp.next = cur.next
            cur.next = tmp
            cur = tmp.next

        cur = head
        while cur:
            if cur.random:
                # cur.random.next才是新的节点
                cur.next.random = cur.random.next
            cur = cur.next.next

        # 拆分
        cur = res = head.next
        pre = head
        while cur.next:
            pre.next = pre.next.next
            cur.next = cur.next.next
            pre = pre.next
            cur = cur.next
        pre.next = None

        return res
```

```Go
func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }

    // 复制节点和next顺序
    cur := head
    for cur != nil {
        newNode := &Node{Val: cur.Val}
        // 插入
        newNode.Next = cur.Next
        cur.Next = newNode
        // 更新
        cur = newNode.Next
    }
    // 复制random指针
    cur = head
    for cur != nil {
        if cur.Random != nil{
            cur.Next.Random = cur.Random.Next
        }
        cur = cur.Next.Next
    }

    // 拆分
    res := head.Next
    pre := head
    cur = head.Next
    for cur.Next != nil {
        // 顺序不能乱，先pre,否则先修改cur，pre.Next.Next就变了第四个了
        pre.Next = pre.Next.Next
        cur.Next = cur.Next.Next

        pre = pre.Next
        cur = cur.Next
    }
    // 还原原链表结构，新链表结尾本就指向nil
    pre.Next = nil

    return res
}
```

#### 86. Partition List 🟡 <a href="#id-86partitionlist" id="id-86partitionlist"></a>

[力扣（LeetCode）官网 - 全球极客挚爱的技术成长平台](https://leetcode.cn/problems/partition-list)

```Go
func partition(head *ListNode, x int) *ListNode {
    if head == nil {return nil}
    dummy1 := &ListNode{-1, nil}
    dummy2 := &ListNode{-1, nil}
    small, large := dummy1, dummy2
    cur := head
    for cur != nil {
        if cur.Val < x {
            small.Next = cur
            small = small.Next
        }else {
            large.Next = cur
            large = large.Next
        }
        cur = cur.Next
    }
    // large链表的尾部可能连接着其他节点
    large.Next = nil
    small.Next = dummy2.Next
    return dummy1.Next
}
```
