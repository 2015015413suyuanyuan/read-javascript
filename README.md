# read-javascript  

# 你不知道的JavaScript （上卷）

## 第一章  作用域  

### 1.3

LHS查询：试图找到变量的容器本身

RHS查询：查找某个变量的值

### 1.4异常

如果RHS查询在所有嵌套的作用域中遍寻不到所需的变量，引擎就会抛出ReferenceError异常。

如果RHS查询找到了一个变量，但是你尝试对这个变量的值进行不合理的操作，比如试图对一个非函数类型的值进行函数调用，或者null或undefined类型的值中的属性，那么引擎会抛出另外一种类型的异常，叫作TypeError。

ReferenceError同作用域判别失败相关，而TypeError则代表作用域判别成功了，但是对结果的操作是非法或不合理的。

