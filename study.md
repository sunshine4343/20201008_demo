### 常见的数据结构
- 数组（Aarray）
- 栈（Queue）
- 链表（Linked List）
- 图（Graph）
- 散列表（Hash）
- 队列（Queue）
- 树（Tree）
- 堆（Heap）
#### 栈结构实现
- 特点：后进先出。(LIFO：Last In First Out)
- 仅允许在表的一端进行插入和删除运算。这一端被称为栈顶，相对地，把另一端称为栈底。
- 栈常见的操作
```
push() 添加一个新元素到栈顶位置。
pop() 移除栈顶的元素，同时返回被移除的元素。
peek() 返回栈顶的元素，不对栈做任何修改（该方法不会移除栈顶的元素，仅仅返回它）。
isEmpty() 如果栈里没有任何元素就返回 true，否则返回 false。
size() 返回栈里的元素个数。这个方法和数组的 length 属性类似。
toString() 将栈结构的内容以字符串的形式返回。
```

- JavaScript 代码实现栈结构
```
// 栈结构的封装
class Map {

  constructor() {
    this.items = [];
  }

  // push(item) 压栈操作，往栈里面添加元素
  push(item) {
    this.items.push(item);
  }

  // pop() 出栈操作，从栈中取出元素，并返回取出的那个元素
  pop() {
    return this.items.pop();
  }

  // peek() 查看栈顶元素
  peek() {
    return this.items[this.items.length - 1];
  }

  // isEmpty() 判断栈是否为空
  isEmpty() {
    return this.items.length === 0;
  }

  // size() 获取栈中元素个数
  size() {
    return this.items.length;
  }

  // toString() 返回以字符串形式的栈内元素数据
  toString() {
    let result = '';
    for (let item of this.items) {
      result += item + ' ';
    }
    return result;
  }
}
```
- 测试封装的栈结构
```
// push() 测试
stack.push(1);
stack.push(2);
stack.push(3);
console.log(stack.items); //--> [1, 2, 3]

// pop() 测试
console.log(stack.pop()); //--> 3

// peek() 测试
console.log(stack.peek()); //--> 2

// isEmpty() 测试
console.log(stack.isEmpty()); //--> false

// size() 测试
console.log(stack.size()); //--> 2

// toString() 测试
console.log(stack.toString()); //--> 1 2
```
#### 堆结构实现
- 只允许在表的前端（front）进行删除操作。
- 只允许在表的后端（rear）进行插入操作。
- 特点：先进先出。(FIFO：First In First Out)
- 队列常见的操作
```
enqueue(element) 向队列尾部添加一个（或多个）新的项。
dequeue() 移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。
front() 返回队列中的第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息与 Map 类的 peek 方法非常类似）。
isEmpty() 如果队列中不包含任何元素，返回 true，否则返回 false。
size() 返回队列包含的元素个数，与数组的 length 属性类似。
toString() 将队列中的内容，转成字符串形式。
```
- 代码实现
```
class Queue {

  constructor() {
    this.items = [];
  }

  // enqueue(item) 入队，将元素加入到队列中
  enqueue(item) {
    this.items.push(item);
  }

  // dequeue() 出队，从队列中删除队头元素，返回删除的那个元素
  dequeue() {
    return this.items.shift();
  }

  // front() 查看队列的队头元素
  front() {
    return this.items[0];
  }

  // isEmpty() 查看队列是否为空
  isEmpty() {
    return this.items.length === 0;
  }

  // size() 查看队列中元素的个数
  size() {
    return this.items.length;
  }

  // toString() 将队列中的元素以字符串形式返回
  toString() {
    let result = '';
    for (let item of this.items) {
      result += item + ' ';
    }
    return result;
  }
}
```
- 测试代码
```
const queue = new Queue();

// enqueue() 测试
queue.enqueue('a');
queue.enqueue('b');
queue.enqueue('c');
queue.enqueue('d');
console.log(queue.items); //--> ["a", "b", "c", "d"]

// dequeue() 测试
queue.dequeue();
queue.dequeue();
console.log(queue.items); //--> ["c", "d"]

// front() 测试
console.log(queue.front()); //--> c

// isEmpty() 测试
console.log(queue.isEmpty()); //--> false

// size() 测试
console.log(queue.size()); //--> 2

// toString() 测试
console.log(queue.toString()); //--> c d
```
#### 优先队列
- 优先级队列主要考虑的问题：
每个元素不再只是一个数据，还包含优先级。
在添加元素过程中，根据优先级放入到正确位置。
- 优先队列的实现
```
// 优先队列内部的元素类
class QueueElement {
  constructor(element, priority) {
    this.element = element;
    this.priority = priority;
  }
}

// 优先队列类（继承 Queue 类）
export class PriorityQueue extends Queue {

  constructor() {
    super();
  }

  // enqueue(element, priority) 入队，将元素按优先级加入到队列中
  // 重写 enqueue()
  enqueue(element, priority) {
    // 根据传入的元素，创建 QueueElement 对象
    const queueElement = new QueueElement(element, priority);

    // 判断队列是否为空
    if (this.isEmpty()) {
      // 如果为空，不用判断优先级，直接添加
      this.items.push(queueElement);
    } else {
      // 定义一个变量记录是否成功添加了新元素
      let added = false;

      for (let i = 0; i < this.items.length; i++) {
        // 让新插入的元素进行优先级比较，priority 值越小，优先级越大
        if (queueElement.priority < this.items[i].priority) {
          // 在指定的位置插入元素
          this.items.splice(i, 0, queueElement);
          added = true;
          break;
        }
      }

      // 如果遍历完所有元素，优先级都大于新插入的元素，就将新插入的元素插入到最后
      if (!added) {
        this.items.push(queueElement);
      }
    }
  }

  // dequeue() 出队，从队列中删除前端元素，返回删除的元素
  // 继承 Queue 类的 dequeue()
  dequeue() {
    return super.dequeue();
  }

  // front() 查看队列的前端元素
  // 继承 Queue 类的 front()
  front() {
    return super.front();
  }

  // isEmpty() 查看队列是否为空
  // 继承 Queue 类的 isEmpty()
  isEmpty() {
    return super.isEmpty();
  }

  // size() 查看队列中元素的个数
  // 继承 Queue 类的 size()
  size() {
    return super.size();
  }

  // toString() 将队列中元素以字符串形式返回
  // 重写 toString()
  toString() {
    let result = '';
    for (let item of this.items) {
      result += item.element + '-' + item.priority + ' ';
    }
    return result;
  }
}
```
- 测试代码
```
const priorityQueue = new PriorityQueue();

// 入队 enqueue() 测试
priorityQueue.enqueue('A', 10);
priorityQueue.enqueue('B', 15);
priorityQueue.enqueue('C', 11);
priorityQueue.enqueue('D', 20);
priorityQueue.enqueue('E', 18);
console.log(priorityQueue.items);
//--> output:
// QueueElement {element: "A", priority: 10}
// QueueElement {element: "C", priority: 11}
// QueueElement {element: "B", priority: 15}
// QueueElement {element: "E", priority: 18}
// QueueElement {element: "D", priority: 20}

// 出队 dequeue() 测试
priorityQueue.dequeue();
priorityQueue.dequeue();
console.log(priorityQueue.items);
//--> output:
// QueueElement {element: "B", priority: 15}
// QueueElement {element: "E", priority: 18}
// QueueElement {element: "D", priority: 20}

// isEmpty() 测试
console.log(priorityQueue.isEmpty()); //--> false

// size() 测试
console.log(priorityQueue.size()); //--> 3

// toString() 测试
console.log(priorityQueue.toString()); //--> B-15 E-18 D-20
```

#### 链表
- 链表和数组
链表和数组一样，可以用于存储一系列的元素，但是链表和数组的实现机制完全不同。
- 数组
存储多个元素，数组（或列表）可能是最常用的数据结构。
几乎每一种编程语言都有默认实现数组结构，提供了一个便利的 [] 语法来访问数组元素。
缺点：
数组的创建需要申请一段连续的内存空间(一整块内存)，并且大小是固定的，当前数组不能满足容量需求时，需要扩容。 (一般情况下是申请一个更大的数组，比如 2 倍，然后将原数组中的元素复制过去)
在数组开头或中间位置插入数据的成本很高，需要进行大量元素的位移。

- 链表
存储多个元素，另外一个选择就是使用链表。
不同于数组，链表中的元素在内存中不必是连续的空间。
链表的每个元素由一个存储元素本身的节点和一个指向下一个元素的引用(有些语言称为指针)组成。
链表优点：
内存空间不必是连续的，可以充分利用计算机的内存，实现灵活的内存动态管理。
链表不必在创建时就确定大小，并且大小可以无限延伸下去。
链表在插入和删除数据时，时间复杂度可以达到 O(1)，相对数组效率高很多。
链表缺点：
访问任何一个位置的元素时，需要从头开始访问。(无法跳过第一个元素访问任何一个元素)
无法通过下标值直接访问元素，需要从头开始一个个访问，直到找到对应的元素。
虽然可以轻松地到达下一个节点，但是回到前一个节点是很难的。
- 链表中的常见操作
```
append(element) 向链表尾部添加一个新的项。
insert(position, element) 向链表的特定位置插入一个新的项。
get(position) 获取对应位置的元素。
indexOf(element) 返回元素在链表中的索引。如果链表中没有该元素就返回-1。
update(position, element) 修改某个位置的元素。
removeAt(position) 从链表的特定位置移除一项。
remove(element) 从链表中移除一项。
isEmpty() 如果链表中不包含任何元素，返回 trun，如果链表长度大于 0 则返回 false。
size() 返回链表包含的元素个数，与数组的 length 属性类似。
toString() 由于链表项使用了 Node 类，就需要重写继承自 JavaScript 对象默认的 toString 方法，让其只输出元素的值。
```
- 链表封装
```
class Node {
    constructor(element) {
        this.element = element;
        this.next = null;
    }
}
// 封装链表
export class LinkedList {
    constructor() {
        // head 指向链表的第一个节点,初始 head 为null 
        this.head = null;
        // 记录链表长度，初始链表长度为0
        this.length = 0;
    }
    // append(element) 向链表尾部添加一个新的项。
    append(element) {
        // 1.根据element创建Node对象
        const newNode = new Node(element);
        // 2.追加到最后
        // 当head为null就追加到后边
        if (!this.head) {
            this.head = newNode;
        } else {
            // current指向第一个节点
            let current = this.head;
            // 当current.next不为空时，让它指向下一个节点
            while (current.next) {
                current = current.next;
            }
            // 当current.next为空时，让它指向新节点，就是成功添加节点到链表中
            current = newNode;
        }
        // 每添加进链表中一个元素，length要加一
        this.length++;
    }
    // insert(position, element) 向链表的特定位置插入一个新的项。
    /*
          思路
            设置index为计数器  
            设置一个current记录当前元素的位置
            设置一个previous记录当前元素的前一个元素的位置
            在 while 循环中，每当index小于position时，current就指向他的下一个元素(current.next)
            直到index >= position时，previous指向新添加的元素，新添加的元素指向current
            插入成功后，length要加一
     */
    insert(position, element) {
        // 1.判断越界问题
        if (position < 0 || position > this.length) return false;
        // 2.创建新的节点
        const newNode = new Node(element);
        // 3.插入元素
        if (position === 0) {
            // 在链表头部加元素
            // 插入元素的next指向head，然后让插入元素成为head
            newNode.next = this.head;
            this.head = newNode;
        } else {
            let index = 0;
            let current = this.head;
            let previous = null;
            while (index++ < position) {
                previous = current;
                current = current.next;
            }
            previous.next = newNode;
            newNode.next = current;
        }
        this.length++;
        return true;
    }
    // get(position) 获取对应位置的元素。
    /*
          思路
            设置index为计数器  
            设置一个current记录当前元素的位置
            在 while 循环中，每当index小于position时，current就指向他的下一个元素(current.next)
            直到index >= position时，current指向position位置的元素
            最后返回该位置的元素
     */
    get(position) {
        // 1.判断越界问题
        if (position < 0 || position > this.length - 1) return null;
        // 2.查找该位置的元素
        let index = 0;
        let current = this.head;

        while (index++ < position) {
            current = current.next;
        }
        return current.element;
    }
    // indexOf(element) 返回元素在链表中的索引。如果链表中没有该元素就返回-1。
    /*
            思路
                在循环中，判断current指向的元素和传过来的element元素是否完全相等
                相等则返回当前位置的索引值
                不相等继续进行循环，让计数器index加一，current指向他的下一个元素
                如果在链表中没有找到element，则返回-1
     */
    indexOf(element) {
        // 获取第一个元素
        let current = this.head;
        let index = 0;
        // 开始查找
        while (current) {
            if (current.element === element) {
                return index;
            }
            index++;
            current = current.next;
        };
        return -1;
    }
    // update(position, element) 修改某个位置的元素。
    update(position, element) {
        // 1.刪除position位置的元素
        const result = this.removeAt(position);
        // 2.插入position位置element元素
        this.insert(position,element);
        return result;
    }
    // removeAt(position) 从链表的特定位置移除一项。
    removeAt(position) {
        // 1.判断是否越界
        if (position < 0 || position > this.length - 1) return null;
        // 2.删除元素
        let current = this.head;
        let previous = null;
        let index = 0;
        if (position === 0) {
            this.head = current.next;
        } else {
            while (index++ < position) {
                previous = current;
                current = current.next;
            }
            previous.next = current.next;
        }
        this.length--;
        return current.element;
    }
    // remove(element) 从链表中移除一项。
    remove(element){
        // 1.获取元素的位置
        const index=this.indexOf(element);
        if(index===-1)return;
        // 2.删除该位置的元素
        this.removeAt(index);
    }
    // isEmpty() 如果链表中不包含任何元素，返回 trun，如果链表长度大于 0 则返回 false。
    isEmpty(){
        return this.length === 0;
    }
    // size() 返回链表包含的元素个数，与数组的 length 属性类似。
    size(){
        return this.length;
    }
}
```
#### 双向链表
- 单向链表和双向链表
    单向链表
        只能从头遍历到尾或者从尾遍历到头（一般从头到尾）。
        链表相连的过程是单向的，实现原理是上一个节点中有指向下一个节点的引用。
        单向链表有一个比较明显的缺点：可以轻松到达下一个节点，但回到前一个节点很难，在实际开发中, 经常会遇到需要回到上一个节点的情况。
    双向链表
        既可以从头遍历到尾，也可以从尾遍历到头。
        链表相连的过程是双向的。实现原理是一个节点既有向前连接的引用，也有一个向后连接的引用。
        双向链表可以有效的解决单向链表存在的问题。
- 双向链表缺点：
每次在插入或删除某个节点时，都需要处理四个引用，而不是两个，实现起来会困难些。
相对于单向链表，所占内存空间更大一些。
但是，相对于双向链表的便利性而言，这些缺点微不足道。
- 双向链表结构
双向链表不仅有 head 指针指向第一个节点，而且有 tail 指针指向最后一个节点。
每一个节点由三部分组成：item 储存数据、prev 指向前一个节点、next 指向后一个节点。
双向链表的第一个节点的 prev 指向 null。
双向链表的最后一个节点的 next 指向 null。
- 双向链表常见的操作
```
append(element) 向链表尾部追加一个新元素。
insert(position, element) 向链表的指定位置插入一个新元素。
getElement(position) 获取指定位置的元素。
indexOf(element) 返回元素在链表中的索引。如果链表中没有该元素就返回 -1。
update(position, element) 修改指定位置上的元素。
removeAt(position) 从链表中的删除指定位置的元素。
remove(element) 从链表删除指定的元素。
isEmpty() 如果链表中不包含任何元素，返回 trun，如果链表长度大于 0 则返回 false。
size() 返回链表包含的元素个数，与数组的 length 属性类似。
toString() 由于链表项使用了 Node 类，就需要重写继承自 JavaScript 对象默认的 toString 方法，让其只输出元素的值。
forwardString() 返回正向遍历节点字符串形式。
backwordString() 返回反向遍历的节点的字符串形式。
```