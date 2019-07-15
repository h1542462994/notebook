# 深度学习[^a]

监督学习和优化

## 介绍

这个教程将会介绍一些{==无监督特征学习==}[^b]和深度学习的主要思想。通过学习，你也会掌握几个特征学习和深度学习的算法及其应用，观察它们是如何自动化解决问题，以及学会将这些思想应用到新的问题上。

!!! tip "读者须知"
    在这里假设你已经对机器学习(特别是熟悉监督学习，Logistic回归，梯度下降算法[^c])，如果你还对这些概念不熟悉，我们推荐你先学习机器学习的课程并完成Ⅱ、Ⅲ、Ⅳ三小节[^k]。
    所有学习需要的代码都可以从[Github Repository](https://github.com/amaas/stanford_dl_ex)中找到。

!!! quote "贡献者"
    材料由Andrew Ng, Jiquan Ngiam, Chuan Yu Foo, Yifan Mai, Caroline Suen, Adam Coated, Andrew Maas, Awni Hannun, Brody Huval, Tao Wang, Sameep Tandon


## 线性回归[^d]

### 预备知识[^l]

作为一个学者，我们从掌握线性回归开始。主要是熟悉{==目标函数==}[^m]，计算梯度，然后根据一系列参数优化目标函数。这些知识将会为以后掌握更加复杂[^n]的算法提供基础。

!!! info "提示"
    读者想要了解更多细节可以参考{==监督学习==}的教材笔记 [点击下载](http://cs229.stanford.edu/notes/cs229-notes1.pdf)。

线性回归的目标是从一个输入向量$x$

## Logistic回归[^e]

## 向量化[^f]

## 调试-梯度检测[^g]

## Softmax回归[^h]

## 调试-偏差与方差[^i]

## 调试-优化器和目标[^j]

[^a]: 该文档由[英文文档](http://deeplearning.stanford.edu/tutorial/)翻译而来，当前正在翻译

[^b]: 无监督特征学习:Upsupervised feature learning.

[^c]: 梯度下降算法:Gradient Descent

[^k]: 原文没有给出链接，[教程地址](http://openclassroom.stanford.edu/MainFolder/CoursePage.php?course=MachineLearning) - 译者注

[^d]: 线性回归:Linear Regression

[^l]: 预备知识: Problem Formulatin，字面意思为问题公式化，但根据上下文应该翻译为预备知识 - 译者注

[^m]: 目标函数: objective function.

[^n]: 复杂的: sophisticated

[^e]: Logistic回归:Logistic Regression

[^f]: 向量化:Vectorization

[^g]: 调试-梯度检测: Debugging:Gradient Checking

[^h]: Softmax回归: Softmax Regression

[^i]: 调试-偏差和方差:Debugging: Bias and Variance

[^j]: 调试-优化器和目标:Debugging:Optimizers and Objectives