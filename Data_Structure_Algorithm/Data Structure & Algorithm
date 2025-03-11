# Concepts of Data Structure & Algorithm
---
資料結構是指相互之間存在著一種或多種關係的資料元素的集合和該集合中資料元素之間的關係組成。
- 常用的資料結構有：Array, Stack, Linked List, Graph, Hash, Queue, Tree, Heap 等，如圖所示：

![gh](https://raw.githubusercontent.com/SeanChenR/img_gif/main/myimage/1741575869000ufhrg6.png)

## Array

Array 是由相同類型的元素集合所組成是 Data Structure，分配一塊連續的記憶體來儲存。利用元素的索引可以計算出該元素對應的儲存位置。

| Advantage  | Disadvantage          |
| ---------- | --------------------- |
| 依索引查詢元素速度快 | Array 的大小固定後就無法擴容了    |
| 依索引遍歷數組方便  | Array 只能儲存一種 Type 的數據 |
> [!info] Extends
> 1. JS Array 的預設儲存值是 undefined，其他程式語言數組的預設儲存值是 0 或是垃圾數據
> 2. 與其他程式語言不同，JS 可以存取 Array 中不存在的索引，會傳回 undefined，而其他程式會噴錯或傳回垃圾數據
> 3. JS 可以儲存不同類型的數據，而其他程式語言只能儲存一種 Type 的 Data
> 4. 當 JS 的 Array 的儲存空間不夠用時，會自動擴張，而其他程式語言的 Array 大小是固定的，一旦定義了就無法改變
> 5. JS 中指派給 Array 的儲存空間是不連續的，而其他程式語言中指派給 Array 的儲存空間是連續的

> [!note] 適用場景
> 頻繁查詢，對儲存空間要求不大，很少增加和刪除的情況

## Stack

Stack 又被叫做 **棧** 或 **堆疊**，是一種特殊的線性表，僅能在其中一端操作，也就是 Stack 頂允許操作，Stack 底不允許操作。Stack 的特色是 **先進後出**，也可以說 **後進先出**。

![gh](https://raw.githubusercontent.com/SeanChenR/img_gif/main/myimage/17415772460000hlk5c.png)

> [!note] 適用場景
> Stack 的結構就像是一個貨櫃，越先放進去的東西就越晚才能拿出來。所以 Stack 常應用於實現 **遞歸** 功能方面的場景，例如：斐波那契數列、反轉列表順序、撤銷一個或一系列操作

> [!success] 用 Stack 實作一個撤銷的功能

```js
class Stack {
	data = []
	maxSize

	constructor(initialData, maxSize = -1) {
		this.data = Array.isArray(initialData) ? initialData : (typeof initialData == "undefined" ? [] : [initialData])
		this.maxSize = maxSize
	}

	isFull() {
		return this.maxSize != -1 ? (this.data.length == this.maxSize) : false
	}

	isEmpty() {
		return this.data.length == 0
	}

	add(item) {
		if(this.isFull()) {
		return false
		}
		this.data.push(item)
	}

	*generator() {
		while(!this.isEmpty()) {
		yield this.data.pop()
		}
	}

	pop() {
	const { value, done } = this.generator().next()
	if(done) return false
	return value
	}

	peek() {
		return [...this.data] // 返回 data 陣列的副本
	}
}

class Operation {
	constructor(val) {
		this.value = val
	}
}

class Add extends Operation {
	apply(value) {
		return value + this.value
	}
	undo(value) {
		return value - this.value
	}
}

class Multiple extends Operation {
	apply(value) {
		return value * this.value
	}

	undo(value) {
		return value / this.value
	}
}

class OpsStack {
	constructor() {
		this.value = 0
		this.operations = new Stack()
	}

	add(op) {
		this.value = op.apply(this.value)
		this.operations.add(op)
	}

	undo() {
		if(this.operations.isEmpty()) {
		return false
		}
		this.value = (this.operations.pop()).undo(this.value)
	}

	peekOperations() {
		return this.operations.peek()
	}
}


let s = new OpsStack()
s.add(new Add(10))
s.add(new Add(20))
console.log("Current Value: ", s.value)
console.log(s.peekOperations())
// Current Value: 30
// [ Add { value: 10 }, Add { value: 20 } ]
s.undo()
console.log("Current Value: ", s.value)
console.log(s.peekOperations())
// Current Value: 10
// [ Add { value: 10 } ]
s.add(new Multiple(2))
s.add(new Multiple(3))
console.log("Current Value: ", s.value)
console.log(s.peekOperations())
// Current Value: 60
// [ Add { value: 10 }, Multiple { value: 2 }, Multiple { value: 3 } ]
s.undo()
console.log("Current Value: ", s.value)
console.log(s.peekOperations())
// Current Value: 20
// [ Add { value: 10 }, Multiple { value: 2 } ]
```

操作類別（Add, Multiple）具有兩種方法：apply 使該操作生效（加乘），undo 表示相反的操作（減除）。
## Queue

Queue 跟 Stack 一樣是線性表，不同的是，Queue 可以在另一端添加元素，在另一端取出元素，也就是**先進先出** 的概念。

![gh](https://raw.githubusercontent.com/SeanChenR/img_gif/main/myimage/1741579805000fnw48w.png)

> [!note] 適用場景
> 因為 Queue 先進先出的特點，在多執行緒阻塞時，非常適合用 Queue 管理。

```js
class Queue {
	data = []
	maxSize

	constructor(initialData, maxSize = -1) {
		this.data = Array.isArray(initialData) ? initialData : (typeof initialData == "undefined" ? [] : [initialData])
		this.maxSize = maxSize
	}

	isFull() {
		return this.maxSize != -1 ? (this.data.length == this.maxSize) : false
	}

	isEmpty() {
		return this.data.length == 0
	}

	enqueue(item) {
		if(this.isFull()) {
			return false
		}
		this.data.push(item)
	}

	*generator() {
		while(!this.isEmpty()) {
			yield this.data.shift()
		}
	}

	dequeue() {
		const { value, done } = this.generator().next()
		if(done) return false
		return value
	}

	peek() {
		return [...this.data]
	}
}

  

let q = new Queue(3, 2)

q.enqueue(1)
q.enqueue(2)
console.log(q.peek())

let x = 0
while (x = q.dequeue()) {
	console.log(q.peek())
	console.log(x, "has been removed")
}

// [ 3, 1 ]
// [ 1 ]
// 3 has been removed
// []
// 1 has been removed
```

主要由 enqueue 和 dequeue 方法實作。透過enqueue可以將元素新增到佇列中，而使用後者則可以將其刪除。 這裡將數組用於基本資料結構，因為它極大地簡化了這兩種方法。入隊與將元素推送到數組相同，透過簡單的呼叫shift即可解決出隊問題，該操作將刪除第一個元素並返回它。
## Heap

Heap 是一種比較特殊的 Data Structure，可以被看作一棵樹的陣列物件堆，又被稱為 priority queue。但實際上跟 queue 不太一樣，因為 Queue 是允許先進先出。而 Map 雖然在堆底插入元素，在堆頂取出元素，但是 Map 裡面的元素排列並不是按照先後順序，而是按照一定的優先順序排列。在 Map 中最頂端的那個 Node (節點)，稱為 Root Node (根節點)，而 Root Node (根節點) 沒有 Parent Node (母節點)。

> [!summary] Summary -> 以 MinHeapify 為例
> - 取出要刪除的節點，也就是 Root Node
> - 用 Array 的最後一個元素取代 Root Node
> - 目前位置和其的 Children Node 比較
> - 刪除 Array 的最後一個元素
## Linked List

Linked List 是物理儲存單元上非連續、非順序的儲存結構，資料元素的邏輯順序是透過 Linked List 的指標位置實現，每個 Node 包含兩個元素，一個是儲存元素的資料域 (記憶體空間)，另一個是指向下一個 Node 位置的指標域。

| Advantage                                    | Disadvantage                  |
| -------------------------------------------- | ----------------------------- |
| Linked List 是一種常見的資料結構，不需要初始化容量，可以任意加減元素     | 因為含有大量的指標域，佔用空間較大             |
| 添加或刪除元素時只需要改變前後兩個元素 Node 的指標域指向地址即可，所以添加刪除很快 | 尋找元素需要遍歷 Linked List 來查找，非常耗時 |
|                                              |                               |
> [!note] 適用場景
> 資料量較小，需要頻繁增加、刪除的操作

![gh](https://raw.githubusercontent.com/SeanChenR/img_gif/main/myimage/1741591959000dy2ovw.png)

| Singly    | Circular                                            | Doubly                                                      |
| --------- | --------------------------------------------------- | ----------------------------------------------------------- |
| 結構簡單，實現容易 | 從尾到頭比較方便，當要處理的資料具有環型結構特徵時，適合採用 Circular Linked List | 需要多一個指針用於指向前後的 Node，因此如果存儲同樣多的數據，Doubly 要比 Singly 佔用更多的內存空間 |
|           |                                                     | Doubly 插入和刪除需要同時維護 next 和 prev 兩個指標                         |
|           |                                                     | 元素的存取需要透過順序訪問，支援雙向遍歷，這就是雙向鍊錶操作的靈活性根本                        |
> [!danger] 注意
> Singly 的 BigO 是 O(n)，而 Doubly 則是 O(1)，當要使用 Linked List 的時候，取決於時間複雜度和空間複雜度的取捨。

## Tree

Tree 是由 n(n>=1) 個有限 Node 組成，一個具有層次關係的集合。Tree 有以下特點：
1. 每個 Node 有 0 個或多個 Sub Node
2. 沒有 Parent Node 的稱為 Root Node
3. 每一個非 Root Node 的只會有一個 Parent Node
4. 除了 Root Node 之外，每個 Sub Node 可以分為多個不相交的 Sub Tree
5. 底下沒有 Children Node 的 Node 稱為 Leaf Node
6. Node 之間的連結稱為 Edges

> [!success] 實作一個 Binary Search Tree

```js
class Node {
	constructor(key) {
		this.key = key;
		this.left = null;
		this.right = null;
	}
}

class BinarySearchTree {
	constructor() {
		this.root = null;
		this.path = "";
		this.queue = [];
	}

	treeInsert(z) {
		let y = null;
		// this 代表的是 BinarySearchTree
		let x = this.root;

		while (x !== null) {
			y = x;
			if (z.key < x.key) {
				x = x.left;
			} else {
				x = x.right;
				}
			}

		if (y == null) {
			this.root = z;
		} else if (z.key < y.key) {
			y.left = z;
		} else {
			y.right = z;
		}
	}

	breadthFirstTraversal(n) {
		if (n != null) {
			this.queue.push(n);
			for (let i = 0; i < this.queue.length; i++) {
				let currentNode = this.queue[i];
				if (currentNode != null) {
					if (currentNode.left != null) {
						this.queue.push(currentNode.left);
					}
					if (currentNode.right != null) {
						this.queue.push(currentNode.right);
					}
				}
			}
		}
	}

	preOrder(n) {
		if (n != null) {
			this.path += n.key + " ";
			this.preOrder(n.left);
			this.preOrder(n.right);
		}
	}

	inOrder(n) {
		if (n != null) {
			this.inOrder(n.left);
			this.path += n.key + " ";
			this.inOrder(n.right);
		}
	}

  

	postOrder(n) {
		if (n != null) {
			this.postOrder(n.left);
			this.postOrder(n.right);
			this.path += n.key + " ";
		}
	}

  

	searchRecursively(n, key) {
		if (n == null || n.key == key) {
			return n;
		}

		else if (key < n.key) {
			return this.searchRecursively(n.left, key);
		} else {
			return this.searchRecursively(n.right, key);
		}
	}

	searchIteratively(n, key) {
		while (n != null && key != n.key) {
			if (key < n.key) {
				n = n.left;
			} else {
				n = n.right;
			}
		}
		return n;
	}
}

let BST = new BinarySearchTree();
BST.treeInsert(new Node(15));
BST.treeInsert(new Node(6));
BST.treeInsert(new Node(5));
BST.treeInsert(new Node(1));
BST.treeInsert(new Node(13));
BST.treeInsert(new Node(-7));
BST.treeInsert(new Node(3));

// BST.inOrder(BST.root);
// console.log(BST.path);

BST.breadthFirstTraversal(BST.root);
console.log(BST.queue);

// let result = BST.searchIteratively(BST.root, 5);
// console.log(result);
```

![gh](https://raw.githubusercontent.com/SeanChenR/img_gif/main/myimage/1741594035000w1895h.png)
## Graph

Graph 是由 Node 的有窮集合 V 和邊的集合 E 組成。其中，為了與樹狀結構加以區別，在圖結構中常將結點稱為頂點，邊是頂點的有序偶對，若兩個頂點之間存在一條邊，就表示這兩個頂點具有相鄰關係。Graph 是比較複雜的資料結構，在儲存資料上有著較複雜且有效率的演算法，分別有鄰接矩陣、鄰接表、十字鍊錶、鄰接多重資料表、邊集陣列等儲存結構。

> [!note] 適用場景
> Graph 是非常通用的，因為他們可用於表示實體相互連接的幾乎所有場景。我說的是用例，從網頁佈局到基於微服務的體系結構，再到真實世界的地圖，再到你可以想像的任何事物，以至於整個資料庫引擎喔都是基於 Graph 的概念 (如：Neo4j)。

## Hash

Hash 是根據 **key & value** 直接進行存取的資料結構，透過 **key & value** 來映射到集合中的位置，這樣很快解可以找到集合中的對應元素。

> [!note] 適用場景
> Hash 的應用場景很多，當然也有很多問題要考慮，例如 Hash 衝突的問題，如果處理的不好會浪費大量的時間，導致應用崩潰。如果實施得當，該結構將非常有效，以至於廣泛用於諸如資料庫索引（通常在需要快速查找操作時將欄位設為索引）等場景，甚至是快取實現，從而允許快速查找操作以進行檢索快取的內容。正如您可能已經猜到的，如果您希望進行快速且重複的查找，那麼這是一個很好的結構。

