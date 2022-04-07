# Rust 构造器

## 创建对象
rust语言中struct没O有构造器。所以一般使用[关联函数](https://kaisery.github.io/trpl-zh-cn/ch05-03-method-syntax.html#%E5%85%B3%E8%81%94%E5%87%BD%E6%95%B0)`new`创建一个对象

```
pub struct User {
    id: u32,
    pub username: String,
    pub role: Role,
}

impl User {
  pub fn new(username: String) -> Self {
    Self {
      id: thread_rng().gen_range(0..9999999),
      username,
      role: Role::Creator
    }
  }
}
```

## 默认构造器
如果struct中所有字段都支持[Default](https://doc.rust-lang.org/stable/std/default/trait.Default.html)trait，则可以通过`#[derive(Default)]`实现默认构造器。
```
#[derive(Default)]
pub struct Post {
    content: String,
    tags: Vec<String>,
    likes: u32
}

let post1 = Post::default();
```

实现`Default`的好处是可以用于标准库任何使用[*or_default函数](https://doc.rust-lang.org/stable/std/?search=or_default)
```
let result: Result<Post, String> = Err("error".to_owned());
let post3 = result.unwrap_or_default();
```

## derive-new
使用[derive-new](https://crates.io/crates/derive-new/)创建了一个`new`构造器，并且可设置默认初始值。
```
use derive_new::new;

#[derive(new)]
pub struct Post {
    content: String,
    #[new(value = "vec![\"rusty\".to_owned()]")] 
    tags: Vec<String>,
    #[new(default)]
    likes: u32
}
```