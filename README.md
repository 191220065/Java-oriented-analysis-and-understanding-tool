# Java-oriented-analysis-and-understanding-tool

## 1.介绍

面向Java的分析与理解工具，该工具为应用 [Javaparser](https://javaparser.org)的一个简单的示例代码。新建maven项目，并添加javaparser依赖后的代码框架。

## 2.使用说明

* 环境变量：请自行配置好 [Java11](https://www.oracle.com/java/technologies/downloads/#java11) 环境
* data文件夹中存放了一些Java代码，作为本demo的演示。

## 3.Java Parser

javaparser是一个可以将java源码解析为一棵语法树，然后基于这棵树对java代码进行分析和修改的工具。

### 3.1  AST 打印

Java Parser AST打印有多种方式，当前支持三种打印方式：Yaml、XML和 Graphiz 可以识别的dot的图片生成的格式。

### 3.2 AST遍历

在Java Parser中，AST的遍历采用的是访问者模式，在访问者模式的基础上，增加了一个简单的包装器。重写visit方法。例如重写visit IFStmt的方法，与重写ForStmt方法：

```java
@Override
    public void visit(IfStmt n, Object arg){
        //TODO:Perfect this method
        strCFS+="If_start,";
        visit(n.getThenStmt(),arg);
        strCFS+="If_end,";

        if(n.hasElseBlock()||n.hasElseBranch()){
            if(n.getElseStmt().isPresent()){
                strCFS+="Else_start,";
                visit(n.getElseStmt().get(),arg);
                strCFS+="Else_end,";
            }
        }
    }
```

```java
 @Override
    public void visit(ForStmt n, Object arg){
        //TODO:Perfect this method
        strCFS+="For_start,";
        visit(n.getBody(),arg);
        strCFS+="For_end,";
    }
```

我们可以得到样例代码BigNum中方法getBigNum(int a, int b)的程序控制流为：

```
For_start,If_start,If_end,Else_start,Else_end,For_end
```

```java
public class BigNum{
    public void getBigNum(int a, int b){
        for(int i=0;i<10;i++) {
            if (a > b) {
                System.out.println(a);
            } else
                System.out.println(b);
        }
    }
}
```

## 4 实现

JavaParser还有许多应用等着你们去挖掘，大家可以先上手对更复杂的程序控制流结构进行提取来感受JavaParser，鼓励大家多多看JavaParser源码的实现以及JavaParser官方文档。
