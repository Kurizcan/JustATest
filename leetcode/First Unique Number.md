# First Unique Number

[First Unique Number](https://leetcode.com/explore/challenge/card/30-day-leetcoding-challenge/531/week-4/3313/)

找到一组队列中第一个出现一次的数字。

需要完成以下函数：FirstUnique、showFirstUnique、add

```
Input: 
["FirstUnique","showFirstUnique","add","showFirstUnique","add","showFirstUnique","add","showFirstUnique"]
[[[2,3,5]],[],[5],[],[2],[],[3],[]]
Output: 
[null,2,null,2,null,3,null,-1]

Explanation: 
FirstUnique firstUnique = new FirstUnique([2,3,5]);
firstUnique.showFirstUnique(); // return 2
firstUnique.add(5);            // the queue is now [2,3,5,5]
firstUnique.showFirstUnique(); // return 2
firstUnique.add(2);            // the queue is now [2,3,5,5,2]
firstUnique.showFirstUnique(); // return 3
firstUnique.add(3);            // the queue is now [2,3,5,5,2,3]
firstUnique.showFirstUnique(); // return -1
```

## 分析

类似 LFU，使用双向链表 + 哈希表

双向链表：记录只出现一次的元素，首部为最早进入链表的
哈希表：记录元素以及对应的出现次数。

```java
class FirstUnique {

    class Node {
        int key;
        int cnt;
        Node pre;
        Node next;
        public Node(int key, int cnt) {
            this.key = key;
            this.cnt = cnt;
        }
    }

    Node head = null;
    Node tail = null;
    private void insert(Node node) {
        if (head == null) {
            head = node;
            tail = node;
            return;
        }
        tail.next = node;
        node.pre = tail;
        tail = node;
    }

    private void remove(Node node) {
        if (head == node) {
            head = node.next;
            if (head != null) {
                head.pre = null;
            }
            return;
        }
        if (tail == node) {
            tail = node.pre;
            tail.next = null;
            return;
        }
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }

    HashMap<Integer, Node> map = new HashMap();

    public FirstUnique(int[] nums) {
        for (int num : nums) {
           add(num);
        }
    }

    public int showFirstUnique() {
        if (head == null) {
            return -1;
        }
        return head.key;
    }

    public void add(int value) {
        Node node = map.get(value);
        if (node == null) {
            node = new Node(value, 1);
            insert(node);
        } else {
            if (node.cnt == 1) {
                remove(node);
            }
            node.cnt++;
        }
        map.put(value, node);
    }
}
```

