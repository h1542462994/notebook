# 文档规范[^1]

!!! info "更新版本"
    draft: 最后更新于 2019年10月17日

## 组织

### 文档的页面组织形式

目前的文档主要分为**笔记**，**想法**，**项目**，**杂项**，四个栏目。

**笔记**主要用于记录平时编程和科目学习时学到的知识，并对知识做一个简要的梳理。

**想法**主要用于记录日常的想法，此部分的内容并没有一定的组织形式。

**项目**主要用于记录打算进行和正在进行的项目，但以日常想法为主，具体的更新内容一般会在**github**中发布。

每个栏目下面的每篇文章应尽量保持在一级，推荐不超过两级，否则就可能因层次数太多而影响阅读。

## 内容

所有文档的内容应该长一些，因为其网页有自己的导航栏，所以一般不用担心文档内容太长导致找不到位置的情况。

### 关于链接

资源链接或者引用链接一般都是指向外部网页的链接，一般不推荐在文档内部使用相对链接，这回让文档看起来错综复杂。

要善于使用右侧的导航栏，合理使用h1~h6元素，使文章的层次比较清晰

### 关于脚注

脚注主要适合于防止参考链接或者对概念性的词语做相应的解释，不推荐和链接混用。

当一个知识点根据其他的文章来编写时，需要写脚注并指明其参考文献。或者，当一个专有词汇需要解释时，也可以用这种方式。

```
Text[^1]
[^1]:SomeText
```

## 提示信息

提示信息总共分为11种样式，每一种样式都支持多种关键字，比如note可以用seealso替代，summary可以用tldr替代。

但为了让文章的内容看起来简洁清晰，一般除了特定的场景外，不推荐使用这一功能。

### 语法

```
!!! note "note, seealse"
    input some text here.
```

!!! note "note, seealse"
    input some text here.

### 样式示例

??? summary "11种样式示例"

    !!! note "note, seealso"
        input some text here.

    !!! summary "summary, tldr"
        input some text here.

    !!! info "info, todo"
        input some text here.

    !!! tip "tip, hint, important"
        input some text here.

    !!! success "success, check, done"
        input some text here.

    !!! question "question, help, faq"
        input some text here.

    !!! warning "warning, caution, attention"
        input some text here.

    !!! failure "failure, fail, missing"
        input some text here.

    !!! danger "danger, error"
        input some text here.

    !!! bug "bug"
        input some text here.

    !!! quote "quote, cite"
        input some text here

### 用法

!!! note "note, seealso"
    note 主要用来写作者编写文档时要说的话。

!!! summary "summary, tldr"
    summary 主要用在需要折叠的内容时。

!!! info "info, todo"
    info 用于对文档的部分内容进行解释。

!!! tip "tip, hint, important"
    tip 用于写读者须知

!!! quote "quote, cite"
    quote 在引用其他文档时需要标注，推荐用于替代`>`块引用。

!!! warning "warning, caution, attention"
    warning 主要用于警告消极的信息

!!! success "success, check, done"
    success 与failure成对使用，用于指出正确的操作

!!! failure "failure, fail, missing"
    failure 与success成对使用，用于指出错误的操作

!!! question "question, help, faq"
    question 主要用来记录经常要问的问题

!!! debug "debug"
    debug 主要用于记录尚待确认的信息



[^1]: 此文所用到的扩展和样式根据[cyent的博客](https://cyent.github.io/markdown-with-mkdocs-material/)来编写。