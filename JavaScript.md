## 检测类型

### typeof操作符  instanceof操作符

**typeof操作符：确定一个变量是字符串、数值、布尔值还是undefined**的最佳工具。

**如果变量的值是一个对象或null**，则typeof操作符会返回“**object**”。

**instanceof操作符：判断某个值是什么类型的对象。**

其语法如下所示：
>>  result = variable instanceof constructor

如果变量是给定引用类型的实例，那么instanceof操作符会返回true。  

例如：
>>  alert(person instanceof Object);
>>  alert(colors instanceof Array);
>>  alert(pattern instanceof RegExp);
>>  alert()

**注：使用typeof操作符检测函数时，返回值是function**

## 执行环境及作用域

>>  **执行环境**定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。
>>  在web浏览器中，全局执行环境被认为是window对象，因此所有全局变量和函数都是作为window对象的属性和方法创建的。某个执行环境中所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。
>>  每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行后，栈将其环境弹出，把控制权返回给之前的执行环境。
>>  当代码在一个环境中执行时，会创建变量对象的一个**作用域链**。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象作为变量对象。活动对象在最开始只包含一个变量，即arguments对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的的变量对象始终都是作用域中的最后一个对象。
>>  标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级地向后回溯，直至找到标识符为止（如果找不到标识符，通常会导致错误发生）。

```
var color = "blue"; 
 
function changeColor(){     
var anotherColor = "red"; 
 
    function swapColors(){         
    var tempColor = anotherColor;         
    anotherColor = color;         
    color = tempColor;            // 这里可以访问 color、anotherColor 和 tempColor     } 
 
    // 这里可以访问 color 和 anotherColor，但不能访问 tempColor             
    swapColors(); } 
 
// 这里只能访问 
color changeColor();
```
以上代码共涉及 3 个执行环境：全局环境、changeColor()的局部环境和 swapColors()的局部 环境。全局环境中有一个变量 color 和一个函数 changeColor()。changeColor()的局部环境中有 一个名为 anotherColor 的变量和一个名为 swapColors()的函数，但它也可以访问全局环境中的变 量 color。swapColors()的局部环境中有一个变量 tempColor，该变量只能在这个环境中访问到。 无论全局环境还是 changeColor()的局部环境都无权访问 tempColor。然而，在 swapColors()内部 则可以访问其他两个环境中的所有变量，因为那两个环境是它的父执行环境。图 4-3形象地展示了前面 这个例子的作用域链。 

### 延长作用域链

#### JavaScript中with的用法

with关键字的作用在于改变作用域，但是不推荐使用。

**with的基本用法**

with语句的原本用意是为逐级的对象访问提供命名空间式地速写方式，也就是在指定的代码区域，直接通过节点名称调用对象。

with通常被当做重复引用同一对象中的多个属性的快捷方式，可以不需要重复引用对象本身。

例如：
```
var obj={
  a:1,
  a:2,
  c:3
};

with(obj){
  a=3;
  b=4;
  c=5;
}
```
在这段代码中，使用了with语句关联了obj对象，这就意味着with代码块内部，每个变量首先被认为是一个局部变量，如果局部变量与obj对象的某个属性同名，则这个局部变量会指向obj对象属性。

**with的弊端**
- 导致数据泄露
```
function foo(obj) {
    with (obj) {
        a = 2;
    }
}

var o1 = {
    a: 3
};

var o2 = {
    b: 3
}

foo(o1);
console.log(o1.a);  //2

foo(o2);
console.log(o2.a);  //underfined
console.log(a);   //2
```
首先，我们来分析上面的代码。例子中创建了 o1 和 o2 两个对象。其中一个有 a 属性，另外一个没有。foo(obj) 函数接受一个 obj 的形参，该参数是一个对象引用，并对该对象医用执行了 with(obj) {...}。在 with 块内部，对 a 有一个词法引用，实际上是一个 LHS引用，将 2 赋值给了它。 
  当我们将 o1 传递进去，a = 2 赋值操作找到了 o1.a 并将 2 赋值给它。而当 o2 传递进去，o2 并没有 a 的属性，因此不会创建这个属性，o2.a 保持 undefined。
  
  
