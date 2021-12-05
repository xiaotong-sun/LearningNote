## protege使用笔记

#### 1. Object properties 对象属性

简介：描述两个 对象之间的关系

特性：

- **Functional Properties（函数性）**
  A能推导出B
  对于任一给定的x，能推导出与之相对应的y

- **Inverse Function Properties（反函数性）**
  B能推导出A
  England isLivedBy Matthew

  当个体与类之间的关系具有函数型和反函数型时，可以做出如下推导：
  A的生母是B（函数性）
  A的生母是C（函数性）
  结论：B就是C（反函数性）

- **Transitive Properties（传递性）**
  A的祖先是B
  B的祖先是C
  结论：C是A的祖先

- **Symmetric Properties（对称性）**
  A是B的兄弟
  B是A的兄弟

- **Asymmetric Properties（不对称性）**
  A是B的兄弟
  B是A的爸爸

- **Reflexive Properties（自反性）**
  A可以了解自己（A）
  A也可以了解B

- **Irreflexive Properties（反自反性）**
  A可以是B的爸爸
  但A不能是自己（A）的爸爸