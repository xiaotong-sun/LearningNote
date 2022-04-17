本文将以单链表和静态链表的初始化代码(c++)为例，具体分析了结构体中`typedef struct LNode{....} LNode, *LinkList, SLinkList[MaxSize];`的相关问题，并补充了C++中**引用类型**的一点知识。

#### 第一部分

首先给出单链表的初始化代码：

```c++
typedef struct LNode {
    ElemType data;
    struct LNode *next;
} LNode, *LinkList;

bool InitList(LinkList &L) {
    L = (LNode *) malloc(sizeof(LNode));
    if (L == NULL) {
        return false;
    }
    L -> next = NULL;
    return true;
}

int main() {
    LinkList L;
    InitList(L);
}
```



从以上代码中，我们看到在定义结构体时，将struct LNode重定义为两个名字：`LNode`和`*LinkList`

之所以这样命名，有助于我们后面对代码的理解。我们使用`LNode`来定义结构体变量并统一指代链表中的一个结点，使用`LinkList`定义指向结构体的指针并代表整个链表。

⭐上述表示定义了结构体`struct LNode`，并且`struct LNode`等价于`LNode`，`struct LNode *`等价于`LinkList`。所以定义结构体变量可以采用`struct LNode L`或者`LNode L`；定义结构体指针变量时可以采用`struct LNode *L`或者`LNode *L`或者`LinkList L`



#### 第二部分

现在有一个问题是为什么`InitList()`函数的形参为`LinkList &L`？这对于不了解c++的我来说确实引发了一阵困惑。

⭐其实，在c++中`LinkList &L`这种格式声明的是一种特殊的类型---->==引用类型==，它直接以按引用调用的方式调用变量（有点类似于指针，但并不相同）。

举个例子：

```c++
void swap1(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}

void swap2(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void swap3(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int a = 1, b = 2;
    swap1(a, b); // 按值调用 其结果仍然是a = 1, b = 2;
    swap2(&a, &b); // 通过指针模拟按引用调用，其结果为a = 2, b = 1;
    swap3(a, b); // 按引用调用，其结果为a = 2, b = 1;
}
```



通过以上的解释，我们就可以理解`InitList(LinkList &L)`实际上是按引用调用了链表指针，这样就可以将函数内对指针的修改带回到主函数中。

那么，如果我们不用这种方式，我们该怎么做呢？

此时，我们就需要使用==双指针==，代码如下：

```c++
typedef struct LNode {
    ElemType data;
    struct LNode *next;
} LNode, *LinkList;

bool InitList(LNode **L) { // 或者 InitList(LinkList *L)
    (*L) = (LNode *) malloc(sizeof(LNode));
    if ((*L) == NULL) {
        return false;
    }
    (*L) -> next = NULL;
    return true;
}

int main() {
    LinkList L;
    InitList(&L);
}
```

**到此为止，我们就解释了`LNode`和`LinkList`的具体使用情况（LNode声明结构体变量，LinkList声明结构体指针）以及C++中的引用类型。**



#### 第三部分

下面我们在举一个静态链表中的例子，深入理解一下。

先上代码：

```c++
#define MaxSize 10

typedef struct SNode{
    ElemType data;
    int next;
} SLinkList[MaxSize];

void testSLinkList() {
    SLinkList a;
    // 其他代码
}
---------------------------------
// 等价于
---------------------------------
# define MaxSize 10
struct SNode{
    ElemType data;
    int next;
};

void testSLinkList() {
    struct SNode a[MaxSize];
    // 后续代码
}


// 也就是说 struct Node a[MaxSize] <==> SLinkList a;
```



从上面我们总结出如下结论：

1. `typedef .... *LinkList;`可以使简化后续声明结构体指针的过程，`LinkList`就指代了一个结构体指针类型
2. `typedef ..... LinkList[MaxSize];`可以使简化后续声明结构体数组的过程，`LinkList`就指代了一个结构体数组类型

也就是说，我们通过`typedef`生成了一个简化版的新的数据类型。







