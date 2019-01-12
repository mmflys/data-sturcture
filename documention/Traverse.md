## **遍历**

### 根,左右儿子访问先后顺序
1. **先序遍历**: 根 左 右
2. **中序遍历**: 左 根 右
3. **后续遍历**: 左 右 根

### **层序遍历**
所有深度为α的节点要在深度为α+1的节点之前进行处理

### **深度优先遍历**
Depth-First-Search(DFS)是一种用于遍历或搜索树或图的算法.
沿着树的深度遍历树的节点,尽可能深的搜索树的分支.
当节点v的所在边都己被探寻过,搜索将回溯到发现节点v的那条边的起始节点.
这一过程一直进行到已发现从源节点可达的所有节点为止.
如果还存在未被发现的节点，则选择其中一个作为源节点并重复以上过程，整个进程反复进行直到所有节点都被访问为止.属于盲目搜索.
ps: 树的深度优先搜索在二叉树的特例下，就是二叉树的先序遍历操作
[wiki](https://zh.wikipedia.org/wiki/%E6%B7%B1%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2)

### **广度优先遍历**
Breadth-First-Search(BFS)又译作宽度优先搜索,或横向优先搜索,是一种图形搜索算法.
简单的说,BFS是从根节点开始,沿着树的宽度遍历树的节点.
如果所有节点均被访问,则算法中止.广度优先搜索的实现一般采用open-closed表.
[wiki](https://zh.wikipedia.org/wiki/%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2)