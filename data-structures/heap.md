### 优先队列（堆）

优先队列是允许至少下列两种操作的数据结构：
- Insert：插入，等价于 Enqueue(入队)
- DeleteMin：找出、返回和删除优先队列中最小的元素，等价于出队(Dequeue)

### 优先队列几种简单的实现
##### 使用链表（有两种）
1. 在表头以 O(1) 执行插入操作，并遍历该链表以删除最小元，这需要 O(N) 时间
2. 始终让表保持排序状态，这使得插入代价高昂（O(N)）而 DeleteMin 花费低廉 (O(1))

##### 使用二叉查找树
它对两种操作的平均运行时间都是 O(log N)
基于 **二叉查找树** 的特性，插入是随机的，但删除则不是，会反复除去左子树中的节点

### 二叉堆（堆）
一般把二叉堆称为堆，它有两个性质，即结构性和堆序性

##### 结构性
堆是一棵被完全填满的二叉树，有可能的例外是在底层，底层上的元素从左到右填入。这样的树称为 **完全二叉树**，它的高是 [Log N]

我们可以使用 **数组** 实现 **完全二叉树**

**对于数组中任一位置 i 上的元素，其左儿子在位置 2i 上，右儿子在左二子后的单元 (2i + 1)中，它的父亲则在位置 [i / 2]上**

##### 堆序性
使操作被快速执行的性质是 **堆序性**

- 由于我们需要快速地找出最小元，因此最小元应该在根上
- 如果我们考虑任意子树也应该是一个堆，那么任意节点就应该小于它的所有后裔

根据堆序性，最小元总可以在根处找到

### 堆的基本操作
##### Insert(插入) O(log N)

新元素插入时通过**上滤**找出正确的位置

*上滤*：
- 为将一个元素 X 插入到堆中，我们在下一个空闲位置创建一个空穴
- 如果 X 可以放在该空穴而不破坏堆的序，那么插入完成
- 否则，我们把空穴的父节点上的元素移入该空穴中，这样，空穴就朝着根的方向上行一步
- 继续该过程直到 X 能被放入空穴中为止

<img src="./assets/上滤.png" width="377" height="326"/>

##### DeleteMin(删除最小元) O(log N)

DeleteMin 以类似于插入的方式处理，不过使用的是 **下滤**

*下滤*：
- 当删除一个最小元时，在根节点处产生了一个空穴
- 由于现在堆少了一个元素，因此堆中最后一个元素 X 必须移动到该堆的某个地方
- 如果 X 可以被放到空穴中，那么 DeleteMin 完成
- 如果不可以放入，则将空穴的两个儿子中较小者移入空穴，这样就把空穴向下推了一层，重复该步骤直到 X 可以被放入空穴中

<img src="./assets/下滤.png" width="380" height="510"/>

##### DecreaseKey(降低关键字的值)

DecreaseKey(P,∆H) 降低在位置 P 处的关键字的值，降值的幅度为正的量 ∆
- 由于可能破坏堆的序，因此必须通过上滤对堆进行调整

##### IncreaseKey(增加关键字的值)

IncreaseKey(P,∆H) 增加位置 P 处的关键字的值，增加的幅度为正的量 ∆
- 同 DecreaseKey，需要通过下滤对堆进行调整

*案例：在操作系统对系统程序进行管理，通过 `DecreaseKey` 和 `IncreaseKey` 降低或者增加关键字的优先级*

##### Delete(删除)
Delete(P,H) 操作通过删除堆中位置P的节点，它分为两步，DecreaseKey => DeleteMin

*案例：当一个进程被用户中止时，它必须从优先队列中除去*

##### BuildHeap(构建堆)
- BuildHeap(H)把  N 个关键字作为输入并把它们放入空堆中。显然，这可以使用 N 个相继的 Insert(插入)操作来完成，由于每个 Insert 将花费 O(1) 平均时间，所以总的运行时间为 O(N)
- 当执行完 Insert 后，已经保证了树的结构性，为了堆序性，我们通过 percolateDown(i) 遍历对节点进行下钻，最终创建一个堆

```c
  for(i = N/2; i>0; i--) {
    percolateDown(i)
  }
```

### 优先队列的应用

1. 优先级的调度
在一需要根据单位优先级进行操作的系统，可以使用优先队列，如 操作系统对进程的调度

2. 以一个选择问题为例，（当输入是 N 个元素以及一个整数 k,找出第 K 个最大的的元素）
- 解法1：将这 N 个元素排序后放入数组，取出第 K 个元素，时间复杂度为 **O(N^2)**
- 解法2：将 K 个元素读入一个数组并将其排序，这些元素中的最小者在第 K 个位置上，再一个个处理其他元素，拿一个元素与数组第K个元素进行比较，如果小的话，则跳过，大的话，则将数组第 K 个元素除去，而这个元素则放在其余 k-1个元素间正确的位置上。当遍历结束时，数组第 K 个元素就是所要的，该算法时间复杂度为 **O(N˙k)**
- 解法3：通过 **优先队列**，我们将 N 个元素通过 BuildHeap 放入一个数组，然后执行 K 次 DeleteMax ，从该堆最后提取的元素就是答案，该算法的时间复杂度为 **O(N + k logN)**
- 解法4：在解法2的基础上，我们对放入 K 的数组用**堆**实现，前 k 个元素通过调用一次 BuildHeap 以 总时间 **O(k)** 被置入堆中。处理每个其余的元素时间为 **O((N - k)log k)**,总的时间复杂度为O(K + (N - k)log k)

### d-堆
d-堆是二叉堆的简单推广，它恰像一个二叉堆，只是所有的节点都有 d 个儿子，二叉堆是2-堆

- d-堆比二叉堆要浅的多，它将 insert 操作的运行时间改进为 O(log d N)
- 相反，由于每个节点的子节点变多了，在进行 deleteMin 时，会花费 d-1 次比较，于是 deleteMin 的用时 提高到 O(d logd N)
- d-堆用数组表示要困难的多，不能再像二叉堆可以用2乘除来移位

*案例：当堆太大不能完全装入主存的时候，可以通过 d-堆来实现*

### 合并堆
将两个堆合并成一个堆是困难的操作，存在许多实现堆的方法使得 Merge 操作的运行时间为 O(log N)，我们这里有3种基本的数据结构：

1. 左式堆
2. 斜堆
3. 二项队列

由于将一个数组拷贝到另一个数组开销非常的大，正因为如此，所有支持高效合并的高级数据结构都需要使用指针

##### 左式堆
左式堆类似于二叉堆，不过，它不是理想平衡的，而实际上是趋向于非常不平衡

- 对于堆中的每一个节点 X，左儿子的零路径长至少与右儿子的零路径长一样大

**零路径长：我们把任一节点 X 的零路径长 Npl(x) 定义为从 X 到一个没有两个儿子的节点的最短路径长**

##### Merge O(log N)
用递归实现:

```c++
Merge(PriorityQueue H1,PriorityQueue H2) {
  if(H1 == NULL) {
    return H2
  }
  if(H2 == NULL) {
    return H1
  }
  if(H1->Element < H2->ELement) 
    return Merge1(H1, H2)
  else 
    return Merge1(H2, H1)
}

Merge1(PriorityQueue H1, PriorityQueue H2) {
  if(h1->Left == NULL) {
    H1->Left == H2
  } else {
    H1->Right = Merge(H1->Right, H2)
    if(H1->Left->Npl < H1->Right->Npl) {
      SwapChildren(H1) // 对换两树
    }
    H1->Npl = H->Right->Npl + 1
  }
  return H1
}
```

遍历实现：
需要遍历两趟，
1. 通过合并两个堆的右路径建立一棵新的树，为此，以排序的顺序安排 H1 和 H2 右路径上的节点，保持它们各自的左儿子不变
2. 在遍历一次，对左式堆性质被破坏的那些节点进行儿子的交换

##### Insert
可以把被插入项看成单节点堆并执行一次 Merge 来完成插入

```c++
Insert(Element X, PriorityQueue H) {
  PriorityQueue SingleNode

  SingleNode = malloc(sizeOf(struct TreeNode))
  if(SingleNode == NULL) {
    FatalError('Out of space !!!')
  } else {
    // 构建单个节点堆
    SingleNode->Element = X
    SingleNode->Npl = 0
    SingleNode->Left = SingleNode->Right = NULL

    H = Merge(SingleNode, H)
  }

  return H
}     
```

##### Delete O(log N)
先除掉根而得到两个堆，然后再将这两个堆合并

```c++
DeleteMin(PriorityQueue H) {
  PriorityQueue LeftHeap, RightHeap

  if(isEmpty(H)) {
    return H
  }

  LeftHeap = H->Left
  RightHeap = H->Right

  free(H)
  return Merge(LeftHeap, RightHeap)
}
```

### 斜堆
斜堆是左式堆的自调节形式，它不存在对树的结构限制，也不会保留**零路径长**，它的 Merge 与左式堆相同，只不过每次过后都会无条件交换左右子树，通过这种方式来使其摊还时间复杂度达到 O(log N)

### 二项队列

左式堆与斜堆每次操作花费 O(log N)时间，而二叉堆每次插入是常数平均时间，二项队列支持合并、插入、删除三种操作，合并、插入的最坏情形运行时间为 O(log N)，而插入操作平均花费常数时间

<img src="./assets/二项队列.png" width="445" height="301"/>

##### 二项队列结构
- 一个二项树不是一棵堆序的树，而是堆序树的集合，称为**森林**
- 堆序树中的每一棵都是有约束的形式，叫做**二项树**
- 每一个高度上至多存在一棵二项树，高度为 0 的二项树是一棵单节点树
- 高度为 K 的二项树 Bk 通过将一棵二项树 Bk-1 附接到另一棵二项树 Bk-1的根上而构成

##### 二项队列合并（O(log N)）
合并操作是通过将两个队列加到一起来完成的

<img src="./assets/二项队列合并.png" width="653" height="341"/>

##### 二项队列插入，最坏运行时间是（O(log N)）
插入就是特殊情形的合并，只要创建一棵单节点树并执行一次合并
如果元素就要插入的那个优先队列中不存在的最小的二项树是 Bi,那么运行时间与 i+1 成正比

如图例子：

<img src="./assets/二项队列插入.png" width="661" height="238"/>

##### 二项队列删除 （O(log N)）
- 通过首先找到具有最小根的二项树，令该树为 Bk
- 设原始的优先队列为 H，我们从 H 的树的森林中除去二项树 Bk，形成新的二项树队列 H'
- 除去 Bk 的根，得到一些二项树 B0，B1,...,BK-1，它们共同形成优先队列 H''
- 合并 H' 和 H''

<img src="./assets/二项队列删除.png" width="663" height="261"/>

##### 二项队列实现
实现二项树分为两步，**二项树** 和 **二项树的集合**

*二项树*
由于需要合并，所以二项树通过链表来表示
- 每个节点都有一个指向它的第一个儿子的指针，儿子按照它们的子树的大小排序

*二项树的队列*
二项树的队列采用数组结构

<img src="./assets/二项队列实现.png" width="" height=""/>

**合并的C实现**
```c++
  CombineTrees(BinTree T1, BinTree T2) {
    if(T1->Element > T2->Element) {
      return CombineTrees(T2, T1)
    }

    T2->NextSibling = T1->LeftChild
    T1->LeftChild = T2
    return T1
  }
```




