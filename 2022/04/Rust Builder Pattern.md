# Rust 生成器模式
1. 生成器类一般与原始类有相同的属性
```
type ms = u32;

#[derive(Clone)]
struct TLSCert {
    key: String,
    cert: String,
}

pub struct Server {
    host: String,
    port: u16,
    tls: Option<TLSCert>,
    hot_reload: bool,
    timeout: ms,
}


pub struct ServerBuilder {
    host: String,
    port: u16,
    tls: Option<TLSCert>,
    hot_reload: bool,
    timeout: ms,
}
```

2. 生成器类构造器`new`方法的入参为最基本必要参数，其他属性附上默认值
```
impl ServerBuilder {
    fn new(host: String, port: u16) -> ServerBuilder {
        ServerBuilder {
            host,
            port,
            tls: None,
            hot_reload: false,
            timeout: 5000,
        }
    }
}
```

3. 添加生成器类其他属性赋值方法
```
impl ServerBuilder {
    fn tls(&mut self, tls: TLSCert) -> &mut Self {
        self.tls = Some(tls);
        self
    }

    fn hot_reload(&mut self, hot_reload: bool) -> &mut Self {
        self.hot_reload = hot_reload;
        self
    }

    fn timeout(&mut self, timeout: ms) -> &mut Self {
        self.timeout = timeout;
        self
    }
}
```
4. 生成器类`build`方法返回原始类
```
impl ServerBuilder {
    fn build(&mut self) -> Server {
        Server {
            host: self.host.clone(),
            port: self.port,
            tls: self.tls.clone(),
            hot_reload: self.hot_reload,
            timeout: self.timeout,
        }
    }
}
```

5. 通过生成器类的使用
```
fn main() {
    let host = "localhost".to_owned();
    let port = 8080;
    let cert = TLSCert {
        key: "...".to_owned(),
        cert: "...".to_owned(),
    };

    // Basic server
    let basic_server = ServerBuilder::new(host.clone(), port).build();

    // Server with TLS
    let tls_server = ServerBuilder::new(host.clone(), port)
        .tls(cert.clone())
        .build();

    // Fully configured server
    let server = ServerBuilder::new(host.clone(), port)
        .tls(cert.clone())
        .hot_reload(true)
        .timeout(5000)
        .build();
}
```

## 参考
* [Idiomatic Rust - Builder Pattern](https://www.youtube.com/watch?v=5DWU-56mjmg)
* [生成器](https://fomalhauthmj.github.io/patterns/patterns/creational/builder.html)