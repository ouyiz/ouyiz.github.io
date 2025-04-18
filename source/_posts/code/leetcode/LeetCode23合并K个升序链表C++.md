---
title: LeetCode23合并K个升序链表C++
categories:
  - 力扣
tags:
  - 代码
  - 链表
  - CPP
mathjax: true
date: 2025-04-10 17:25:13
---
### 题目

**题目描述：**
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入:lists = [[1,4,5],[1,3,4],[2,6]]
输出: [1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入: []
输出: []
```

**示例 3：**

```
输入: [ [] ]
输出: []
```



### 方法 1：逐对合并（分治法）
分治法的思路是将K个链表分成两部分，递归合并。

**时间复杂度：**
- O(N log K)，其中N是所有链表中元素的总数，K是链表的个数。
- 每次合并的时间复杂度是O(N)，但分治递归的深度是log K。

**代码实现：**
```cpp
class Solution {
public:
    // 递归分治合并 K 个链表
    ListNode* findRes1(vector<ListNode*>& lists, int left, int right) {
        if (right == left) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode* node1 = findRes1(lists, left, mid);
        ListNode* node2 = findRes1(lists, mid + 1, right);
        return findRes2(node1, node2);
    }

    // 合并两个已排序链表
    ListNode* findRes2(ListNode* node1, ListNode* node2) {
        ListNode* head = new ListNode(0);
        ListNode* cur = head;
        while (node1 != NULL && node2 != NULL) {
            if (node1->val < node2->val) {
                cur->next = node1;
                node1 = node1->next;
            } else {
                cur->next = node2;
                node2 = node2->next;
            }
            cur = cur->next;
        }
        if (node1 != NULL) cur->next = node1;
        if (node2 != NULL) cur->next = node2;
        return head->next;
    }

    // 合并 K 个链表
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) return NULL;
        return findRes1(lists, 0, lists.size() - 1);
    }
};

```



### 方法 2：最小堆（优先队列）
利用最小堆来合并所有链表。每次从所有链表中取出当前最小的节点，并将其添加到结果链表中。

**时间复杂度：**
- O(N log K)，其中N是所有链表中的节点总数，K是链表的数量。
- 堆的操作时间复杂度是log K，每次从堆中取出一个元素，最多会执行N次。
**代码实现：**
```cpp
class Solution {
public:
    struct Compare {
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val; // 小根堆
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*, vector<ListNode*>, Compare> pq;

        // 将每个链表的头节点加入堆
        for (auto list : lists) {
            if (list) pq.push(list);
        }

        ListNode* dummy = new ListNode(0);
        ListNode* current = dummy;

        // 从堆中取出最小节点，加入结果链表，并将该节点的下一个节点加入堆
        while (!pq.empty()) {
            ListNode* node = pq.top();
            pq.pop();
            current->next = node;
            current = current->next;

            if (node->next) {
                pq.push(node->next);
            }
        }

        return dummy->next;
    }
};
```



### 方法 3：迭代合并（超时）
通过逐个链表合并，先将第一个和第二个链表合并，再与第三个链表合并，以此类推。

**时间复杂度：**
- O(KN)，其中K是链表的个数，N是每个链表的平均长度。
- 这个方法每次合并两个链表，时间复杂度是O(N)，但是需要执行K-1次合并。

**代码实现：**
```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) return nullptr;
        ListNode* result = lists[0];

        for (int i = 1; i < lists.size(); ++i) {
            result = mergeTwoLists(result, lists[i]);
        }

        return result;
    }

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);
        ListNode* current = dummy;

        while (l1 && l2) {
            if (l1->val < l2->val) {
                current->next = l1;
                l1 = l1->next;
            } else {
                current->next = l2;
                l2 = l2->next;
            }
            current = current->next;
        }

        if (l1) current->next = l1;
        if (l2) current->next = l2;

        return dummy->next;
    }
};
```
