# Rust Vec与迭代器以及Option

## 遍历Vec
* 直接遍历Vec
```
let v = vec![1, 2, 3];
for i in &v {
    println!("{}", i);
}

let mut v = vec![1, 2, 3];
for i in &mut v {
    *i += 50;
}
```

* 使用迭代器
```
let v1 = vec![1, 2, 3];
let v1_iter = v1.iter();

for val in v1_iter {
    println!("Got: {}", val);
}

let mut v1 = vec![1, 2, 3];
let v1_iter = v1.iter_mut();
for val in v1_iter {
    *val += 50;
}
```

## 向Vec中添加元素
```
let grade = "A+";
let mut grades = vec!["B-", "C+", "D"];
grades.push(grade)
```

## 向Vec中添加可选原色
```
let grade = Some("A+");
let mut grades = vec!["B-", "C+", "D"];

// 方式一
if let Some(grade) = grade {
  grades.push(grade)
}

// 方式二
grades.extend(grade);
```

## 迭代器的一些应用
```
// Extend an iterator
let grade = Some("A+");
let grades = vec!["B-", "C+", "D"];
for grade in grades.iter().chain(grade.iter()) {
    println!("{grade}");
}

// Filter out non variants
let grades = vec![Some("A+"), None, Some("B+"), None];
let grades: Vec<&str> = grades.into_iter().flatten().collect();
println!("{grades:?}");

// Map and filter out non variants
let grades = ["3.8", "B+", "4.0", "A", "2.7"];
let grades: Vec<f32> = grades.iter().filter_map(|s| s.parse().ok()).collect();
println!("{grades:?}");
```

## 迭代器区别
* `iter`: 普通迭代器
* `iter_mut`: 可变迭代器，用于修改迭代器元素
* `into_iter`: 获取所有权迭代器，用于构建新迭代器

## 参考
* [遍历 vector 中的元素](https://kaisery.github.io/trpl-zh-cn/ch08-01-vectors.html#%E9%81%8D%E5%8E%86-vector-%E4%B8%AD%E7%9A%84%E5%85%83%E7%B4%A0)
* [使用迭代器处理元素序列](https://kaisery.github.io/trpl-zh-cn/ch13-02-iterators.html)
* [Idiomatic Rust - Iterating over Option](https://www.youtube.com/watch?v=xAkcLcYuuWg)