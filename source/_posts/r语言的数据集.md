title: R语言的数据集
date: 2014-05-23 22:09:53
tags: R-Language
language: ch
categories: R-Language
---
<!--more-->
###向量
向量是用于存储数值型、字符型或逻辑型数据的一维数组。执行组合功能的函数c()可用来
创建向量。各类向量如下例所示：
```
a<- c(1,23,4,5,6,-2,4)
b<- c(“one”,”two”,”three”)
c<- c(TRUE,TRUE,FALSE,TRUE,FALSE)
```
注意 标量是只含一个元素的向量，例如f <- 3、g <- "US"和h <- TRUE。它们用于保存
常量。

###矩阵
矩阵是一个二维数组，只是每个元素都拥有相同的模式（数值型、字符型或逻辑型）。可通
过函数matrix创建矩阵。一般使用格式为：

```mymatrix <- matrix(vector,nrow=number_of_rows,ncol=number_of_columns,byrow=logical_value,
dimnames=list(char_vector_rownames,char_vector_colnames) )```

其中vector包含了矩阵的元素，nrow和ncol用以指定行和列的维数，dimnames包含了可选的、
以字符型向量表示的行名和列名。选项byrow则表明矩阵应当按行填充（byrow=TRUE）还是按
列填充（byrow=FALSE），默认情况下按列填充。

###数组
数组（array）与矩阵类似，但是维度可以大于2。数组可通过array函数创建，形式如下：

```
myarray <- array(vector,dimensions,dimnames)```

其中vector包含了数组中的数据，dimensions是一个数值型向量，给出了各个维度下标的最大
值，而dimnames是可选的、各维度名称标签的列表。
下面是一个（2×3×4）数值型数组的示例。

```
dim1 <- c(“A1”,”A2”)
dim2 <- c(“B1”,”B2”,”B3”)
dim3 <- c(“C1”,”C2”,”C3”,”C4”)
z <- array(1:24, c(2,3,4),dimnames=list(dim1,dim2,dim3))
z ```


###数据框
由于不同的列可以包含不同模式（数值型、字符型等）的数据，数据框的概念较矩阵来说更
为一般。它与你通常在SAS、SPSS和Stata中看到的数据集类似。数据框将是你在R中最常处理的
数据结构。
表2-1所示的病例数据集包含了数值型和字符型数据。由于数据有多种模式，无法将此数据
集放入一个矩阵。在这种情况下，使用数据框是最佳选择。
数据框可通过函数data.frame()创建：

```
mydata <- data.frame(col1,col2,col3,…)```

其中的列向量col1, col2, col3,… 可为任何类型（如字符型、数值型或逻辑型）。每一列的
名称可由函数names指定。代码清单2-4清晰地展示了相应用法。

![1](/img/r5.jpg)
函数attach()可将数据框添加到R的搜索路径中，。R在遇到一个变量名以后，将检查搜索路
径中的数据框，以定位到这个变量。函数detach()将数据框从搜索路径中移除。值得注意的是，detach()并不会对数据框本身做任何处理。这句是可以省略的，但其实它应当被例行地放入代码中，因为这是一个好的编程习惯。

###因子
类别（名义型）变量和有序类别（有序型）变量在R中称为因子（factor）。因子在R中非常重
要，因为它决定了数据的分析方式以及如何进行视觉呈现。你将在本书中通篇看到这样的例子。
函数factor()以一个整数向量的形式存储类别值，整数的取值范围是[1... k ]（其中k 是名义
型变量中唯一值的个数），同时一个由字符串（原始值）组成的内部向量将映射到这些整数上。
举例来说，假设有向量：

```
diabetes <- c(“T1”,”T2”,”T3”,”T4”)```

语句

```
diabetes <- factor(diabetes)```

将此向量存储为(1, 2, 1, 1)，并在内部将其关联为
1=Type1和2=Type2（具体赋值根据字母顺序而定）。针对向量diabetes进行的任何分析都会将
其作为名义型变量对待，并自动选择适合这一测量尺度的统计方法。
要表示有序型变量，需要为函数factor()指定参数ordered=TRUE。给定向量：

```
status <- c(“Poor”,”Improved”,”Excellent”,”Poor”)```

语句status <- factor(status, ordered=TRUE)会将向量编码为(3, 2, 1, 3)，并在内部将这
些值关联为1=Excellent、2=Improved以及3=Poor。另外，针对此向量进行的任何分析都会将其作为有序型变量对待，并自动选择合适的统计方法。对于字符型向量，因子的水平默认依字母顺序创建。这对于因子status是有意义的，因为
“Excellent”、“Improved”、“Poor”的排序方式恰好与逻辑顺序相一致。如果“Poor”被编码为“Ailing”，会有问题，因为顺序将为“Ailing”、“Excellent”、“Improved”。如果理想中的顺序是“Poor”、“Improved”、“Excellent”，则会出现类似的问题。按默认的字母顺序排序的因子很少能够让人满意。你可以通过指定levels选项来覆盖默认排序。例如：
各水平的赋值将为1=Poor、2=Improved、3=Excellent。请保证指定的水平与数据中的真实值
相匹配，因为任何在数据中出现而未在参数中列举的数据都将被设为缺失值。

![2](/img/r7.jpg)

###列表
列表（list）是R的数据类型中最为复杂的一种。一般来说，列表就是一些对象（或成分，
component）的有序集合。列表允许你整合若干（可能无关的）对象到单个对象名下。例如，某个列表中可能是若干向量、矩阵、数据框，甚至其他列表的组合。可以使用函数list()创建列表：

```
mylist <- list(object1,object2,…)```

其中的对象可以是目前为止讲到的任何结构。你还可以为列表中的对象命名：

```
mylist <- list(name1=object1,name2=object2,…)```


![3](/img/r8.jpg)

![4](/img/r9.jpg)

处理数据对象的实用函数

![5](/img/r10.jpg)

![6](/img/r11.jpg)