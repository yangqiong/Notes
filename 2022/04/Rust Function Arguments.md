# Rust 函数传参

## 形参`&str`
使用`&str`作为形参，入参可以为`&str`，或者`&String`。
```
fn print_animal_name(name: &str) {
    println!("{name}");
}

let oreo = "oreo".to_owned();
let jax = "jax";

print_animal_name(&oreo);
print_animal_name(jax);
```

## 形参引用类型
使用引用类型例如`&Dog`作为形参，入参可以为对象的引用`&Dog`，或者对象的智能指针引用`&Box<Dog>`
```
fn print_dog(dog: &Dog) {
    println!("{:?}", dog);
}

let dog = Box::new(Dog { name: oreo });
let dog2 = Dog {
    name: jax.to_owned(),
};

print_dog(&dog);
print_dog(&dog2);
```

## 形参数组引用
使用数组引用类型`&[]`作为形参，入参可以为数组对象引用或者vec引用。
```
fn animal_sounds(animals: &[Box<dyn Animal>]) {
    for a in animals {
        a.speak()
    }
}

let animals: Vec<Box<dyn Animal>> = vec![cat, dog];
let animals2: [Box<dyn Animal>; 2] = [cat, dog];

animal_sounds(&animals)
animal_sounds(&animals2)
```

## 引用
* [Idiomatic Rust - Function Arguments](https://www.youtube.com/watch?v=hBA00Tx_dIM&t=347s)