# Rust 避免内存分配

## 交互两个变量值
```
use std::mem;

let mut x = 5;
let mut y = 42;

mem::swap(&mut x, &mut y);
```

## 获取变量所有权，原值置空
```
use std::mem;

let mut v: Vec<i32> = vec![1, 2];

let old_v = mem::take(&mut v);
assert_eq!(vec![1, 2], old_v);
assert!(v.is_empty());
```
`mem::take`等价于`mem::replace(name, String::new())`

## 获取变量所有权，原值替换
```
use std::mem;

let mut v: Vec<i32> = vec![1, 2];

let old_v = mem::replace(&mut v, vec![3, 4, 5]);
assert_eq!(vec![1, 2], old_v);
assert_eq!(vec![3, 4, 5], v);
```

## 其他
优点：没有内存分配
实际例子：
```
use std::mem;

enum MultiVariateEnum {
    A { name: String },
    B { name: String },
    C,
    D
}

fn swizzle(e: &mut MultiVariateEnum) {
    use MultiVariateEnum::*;
    *e = match e {
        // Ownership rules do not allow taking `name` by value, but we cannot
        // take the value out of a mutable reference, unless we replace it:
        A { name } => B { name: mem::take(name) },
        B { name } => A { name: mem::take(name) },
        C => D,
        D => C
    }
}
```

## 参考
* [Idiomatic Rust - Avoiding Allocations](https://www.youtube.com/watch?v=eEDKc7u4uwg)
* [Function std::mem::take](https://doc.rust-lang.org/std/mem/fn.take.html)
* [make:{take(), place()}](https://fomalhauthmj.github.io/patterns/idioms/mem-replace.html)