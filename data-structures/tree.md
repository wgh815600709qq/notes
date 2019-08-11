对于大量的输入数据，链表的线性访问时间太慢，此时，可以使用树的数据结构

### 定义
- tree：树。一些节点的集合
- root：根节点
- 子树：一棵完整的但不包含根节点的树
- edge：节点相互连接的单位
- child：子节点
- parent：父节点
- leaf：树叶。没有 child 的节点
- sibling：兄弟节点。具有相同 parent 的节点
- path：路径。从n1(开始节点),n2,...,nk(结束节点)的一个序列，并且上一个节点是下一个节点的 parent
- length：路径的长。该 path 上的边的条数，每个节点到自身的 length 为0，在一棵树中从根到每个节点恰好存在一条路径