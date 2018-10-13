---
title: Functions
localeTitle: 功能
---
# C中的功能

有时您需要反复使用一段代码，但代码中的不同时间和地点。你可以一遍又一遍地复制和粘贴它，但这不是一个很好的解决方案 - 你的文件大小最终变得更大，你的代码更难调试，你的代码更难以阅读。相反，使用函数：函数就像代码中存在的迷你程序。您可以传递它们使用的变量，并且它们可以返回数据。

## 一个例子

这是一个分割两个数字的函数的简单示例。它不是很有用，因为我们有`/` ，但它显示了函数的不同部分。

```C
#include <stdio.h> 
 
 int divides(int a, int b) { 
    return a / b; 
 } 
 
 int main(void) { 
    int first = 5; 
    int second = 10; //MUST NOT BE ZERO; 
 
    int result = divides(first, second); 
 
    printf("first divided by second is %i\n", result); 
 
    return 0; 
 } 
```

请注意，像`main`一样， `divides`具有类似的格式。那是因为`main`也是一个函数 - 它只是特殊的，因为C首先寻找它。 `divides`也出现在`main`之前。这很重要，因为`main`呼叫是`divides` 。调用函数意味着正在使用其代码。代码必须在使用之前编译，并且C从顶部开始逐行编译，因此为了调用函数，必须在调用函数之前将其写出，就像在本例中一样。如果`divides`后，来到`main` ，它会失败，因为编译器不知道`divides`还不存在。您也可以在main之前使用函数原型，以便在main之后放置`divides` 。函数原型与具有相同变量和返回类型的函数相同，除了它们的大括号被省略并替换为分号，如下所示：

```C
int divides(int a, int b); 
```

另请注意， `divides`和`main`不是共享括号，并且不在彼此的括号中。即使一方呼叫另一方，它们也应该是分开的。

考虑到这一点，让我们在下一节中的函数的第一行，标题为：

## 打破功能声明

```C
int divides(int a, int b) 
```

函数声明以数据类型开头，在本例中为`int` 。无论您希望函数返回什么数据类型，都应放在此处。您可以返回任何数据类型，或者通过在此处放置`void` ，它可以不是数据类型。

接下来是函数的名称。每当您想要调用该函数时，这就是您将使用的名称。尽量使其成为描述性的东西，这样您就可以轻松识别它的作用。

名字后出现一对括号。在这些括号中输入我们的函数参数，这些函数将在代码运行时使用和使用。在这种情况下，有两个。它们都是`int`数据类型，它们将被命名为`a`和`b` 。理想情况下，这里会有更好的名称，但你会发现，对于简单快捷的方法，经常会使用临时变量名。

现在让我们来看看括号内的内容：

```C
return a / b; 
```

这非常简单，因为这是一个非常简单的功能。 `a`除以`b` ，返回该值。你已经在`main`函数中看到过`return` ，但现在它不是结束我们的程序，而是结束方法并将值赋给任何调用它的值。

所以要回顾一下这个函数的作用 - 它得到两个整数，将它们分开，并将它们返回给它所谓的任何东西。

### 函数的参数

参数用于将参数传递给函数。 它们有两种类型的参数： 在函数定义中写入的参数称为“形式参数”。 函数调用中写入的参数称为“实际参数”。它们也称为参数。它们被传递给函数定义，并以形式参数的形式创建副本。

## 一个更复杂的例子

那是一个单线功能。当有一个非常简单的操作需要反复执行，或者一个操作最终成为一条长线时，你会看到它们。通过使其成为一个函数，代码最终变得更易读和易于管理。

话虽这么说，大多数功能都不是一行代码。让我们来看看另一个稍微复杂的例子，它选择两个数字中较大的一个。

```C
int choose_bigger_int(int a, int b) { 
    if(a > b) 
        return a; 
 
    if(b > a) 
        return b; 
 
    return a; 
 } 
```

就像之前一样，函数将返回一个整数并取两个整数。没有什么新东西可以看到。

此代码以if语句开头，该语句检查`a`是否大于`b` 。如果它是，它将返回`a` 。如果这样做，函数在此结束 - 其余代码不会被评估。但是，如果未达到此返回语句，则将评估下一个if语句。如果为真，则返回`b`并且函数在此处结束。

由此，已经考虑了大于b且b大于a的条件。但是，如果a等于b，则该函数仍然不会返回任何内容。出于这个原因，我们返回a（a等于b，所以我们也可以返回）。

## 关于'范围'的一个词

范围是一个需要注意的事项。它指的是代码中可以访问变量的区域。将变量传递给函数时，该函数将获得自己的副本以供使用。这意味着调整函数中的变量不会在其他任何位置调整它。同样，如果您尚未将变量传递给函数，则它没有它并且无法使用它。

您可能已经观察到类似if语句和任何循环之类的问题。如果在括号内声明变量，则不能在这些括号之外访问它。这对于函数来说也是如此，但是有一些方法可以解决它：

*   通过在任何函数之外声明它来创建一个全局变量
*   这会使您的代码更加混乱，通常不建议使用。应尽可能避免
*   使用指针，您将在下面阅读
*   这可能会使您的代码更难以阅读和调试
*   像你应该的那样进入你的职能部门
*   如果这样做是一种选择，这是最好的方法

理想情况下，您将始终作为参数传递给您的函数，但您可能并不总是能够。选择最佳解决方案是您作为程序员的工作。

C中的递归 当函数在同一函数中被调用时，它被称为C中的递归。调用相同函数的函数称为递归函数。
```
int factorial (int n) 
 { 
    if ( n < 0) 
        return -1; /*Wrong value*/ 
    if (n == 0) 
        return 1; /*Terminating condition*/ 
    return (n * factorial (n -1)); 
 } 
```

# 在你继续之前......

## 回顾

*   函数很好用，因为它们使代码更清晰，更容易调试。
*   函数需要在被调用之前声明。
*   函数需要返回一个数据类型 - 如果没有返回任何内容，请使用`void` 。
*   函数接受参数使用 - 如果它们什么都不带，请使用`void` 。
*   `return`结束函数并返回一个值。你可以在一个函数中使用多个函数，但只要你找到一个函数，函数就会在那里结束。
*   将变量传递给函数时，它有自己的副本 - 更改函数中的某些内容不会在函数外部更改它。
*   在函数内声明的变量仅在该函数内可见，除非它们被声明为静态。