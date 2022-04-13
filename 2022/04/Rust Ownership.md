# Rust 所有权

## 所有权规则
1. Rust 中的每一个值都有一个被称为其 所有者（owner）的变量。
2. 值在任一时刻有且只有一个所有者。
3. 当所有者（变量）离开作用域，这个值将被丢弃。

## 引用和`Copy`特性
赋值过程：包括变量赋值，函数传参，函数返回
* 如果类型实现了`Copy`特性（基本简单类型），传参过程相当于创建了一份新拷贝（克隆）
* 未实现`Copy`特性，传参过程会转移所有权，原变量将无法继续使用（移动）
* 使用引用，不获取值的所有权，但能访问变量。

## 引用的规则
* 在任意给定时间，要么 只能有一个可变引用，要么 只能有多个不可变引用。
* 引用必须总是有效的。

## slice
* slice是对一段连续元素的引用，所以没有所有权
* 字符串字面值是slice

## 引用
* [什么是所有权？](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html)