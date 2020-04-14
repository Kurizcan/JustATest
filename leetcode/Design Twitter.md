# 355. Design Twitter

[Design Twitter](https://leetcode.com/problems/design-twitter/)

## 分析

由于题目需要找出最近 10 条推特，包括自己发的以及自己所关注的人发的，可以考虑这么做：

1. hashmap<userId, tweet> : 存储某个用户发送的推特
2. hashmap<userId, followeeSet> : 存储某个用户关注的用户
3. heap：使用堆进行 10 条推特的检索，需要先遍历用户的关注列表，将多个推特链表按照时间先后顺序归并，也就是插入堆中。
4. tweet 构成一个链表的形式，每次用户发送推特，使用头插法进行插入该链表

