## Easy

### Two Number Sum

> 求array数组中的两个数，它们的和等于target

解法1：两个for循环。时间复杂度O(n<sup>2</sup>)，空间复杂度(O1)

解法2：map遍历，存在返回结果，不存在加入map。时间复杂度O(n)，空间复杂度O(n)

解法3：先对数组排序，两个游标left和right相遇问题。时间复杂度O(nlogn)，空间复杂度O(1)

### Validate Subsequence

> 求sequence数组是否是array数组的subsequence，子序列即，是array的子集且出现顺序一致

解法1：两数组同时遍历，相同变量加一，看最后该变量是否等于sequence的长度

解法2：遍历array，如果和sequence[index]相等，则index++，最后看index是否等于len(sequence)

时间复杂度O(n)，空间复杂度O(1)

### Sorted Squared Array

> 求一个升序排列的数组各元素的平方的数组，结果也是升序排列

解法1：先计算平方，再把结果排序：sort.Ints(result)。时间复杂度O(nlogn)，空间复杂度O(n)

解法2：由于原数组是升序的，所以需要考虑较小数的绝对值（负数）大于较大值的情况。因此，搞两个指针，分别指向原数组头和尾，插入结果时会判断绝对值大小，绝对值大的插入到结果的头部。时间复杂度O(n)，空间复杂度O(n)

### Tournament Winner

> 一个二维数组，是两队两两对战的记录（前者主场后者客场），一个一维数组记录主客场胜负关系（0主场负，1主场胜），胜利加三分，求最终胜利的队

**解法**：一个map，key存队名，value存分数，currentBestTeam记录目前最高分的队，遍历队长记录表，同时也遍历了胜负关系表，更新map，更新currentBestTeam即可。时间复杂度O(n)，空间复杂度O(k)（n：记录数，k：队伍数）

### Non-Constructible Change

> 一个数组代表硬币数量，求用这些硬币不能凑出的最小零钱的值

**解法**：先将数组排序，遍历coins数组，初始化current变量为零，current为已遍历的coin的和，每次循环判断当前coin是否大于current+1，如果大于说明不可能凑出该零钱，返回current+1，每次循环结尾current += coin。时间复杂度O(nlogn)，空间复杂度O(1)

### Find Closest Value In BST

> 已知一个BST和一个数target，求BST中的节点值，它和这个target值最接近

**解法1**：递归。递归函数int findClosestValueInBst(BST tree, int target, int closest)，由于是BST，所以每次判断target-closest和target-tree.value()绝对值大小，tree更接近的话更新closest。由于是BST，所以target比value小（大），递归求左（右）子树的closest，最后返回closest。

平均时间复杂度O(logn)，平均空间复杂度O(logn)。最差时间、空间复杂度O(n)。

**解法2**：迭代。循环，当当前节点currentNode不为空，根据target-closest和closest-currentNode.value()依次找左（右）子树的节点，更新closest。

时间、空间复杂度解法1。

### Branch Sums

> 已知一个二叉树，求一个列表，元素是二叉树从左到右每一个分支的和















