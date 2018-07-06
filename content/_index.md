<!-- vim: tw=80
+++
title = "Intercom"
description = "Framework for writing Rust components for use from other languages"
tree = false
+++
-->

Intercom is a low-overhead framework designed for fast in-process communication.
The primary goal is to enable easy use of Rust in existing projects that are
written in other languages to allow implementing individual features in Rust
without the need to rewrite the whole project.

```rust
#[intercom_struct]
struct Library;

#[intercom_impl]
impl Library {
    pub fn get_greeting(&self, name: &str) -> Result<String>
    {
        Ok(format!("Hello {}!", name));
    }
}
```

