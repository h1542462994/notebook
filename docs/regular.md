# 文档编写规范[^1]

!!! note "作者插入"
    写这个文档主要是想对文档的格式做一个约束，从而提高可读性。

## 内容

文档编写的内容应该围绕一个主题来写，由于文档右边自带{==目录==}，因此，不推荐使用链接将一个主题的内容分开。必要时可以通过分页的方式进行处理。

## 提示信息

提示信息总共分为11种样式，每一种样式都支持多种关键字，比如note可以用seealso替代，summary可以用tldr替代

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

## 脚注

当一个知识点根据其他的文章来编写时，需要写脚注并指明其参考文献。或者，当一个专有词汇需要解释时，也可以用这种方式。

### 语法

```
Text[^1]
[^1]:SomeText
```


[^1]: 此文所用到的扩展和样式根据[cyent的博客](https://cyent.github.io/markdown-with-mkdocs-material/)来编写。