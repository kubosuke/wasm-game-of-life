# Notes

## extern

https://doc.rust-lang.org/std/keyword.extern.html

`extern` keyword is used in 2 places in Rust:

1. as conjunction with the `crate` keyword - to aware of other Rust crates in the project
2. FFI(foreign function interface)

```rust
// **1**
extern crate pcre;
extern crate std; // equivalent to: extern crate std as std;
extern crate std as ruststd; // linking to 'std' under another name

// **2**
extern "C" {
    fn abs(input: i32) -> i32;
}

fn main() {
    unsafe {
        println!("Absolute value of -3 according to C: {}", abs(-3));
    }
}
```

## wasm_bindgen

attribute `wasm_bindgen` is used to interface with JS.

```rust

#![allow(unused_variables)]
fn main() {
mod utils;

use wasm_bindgen::prelude::*;

// When the `wee_alloc` feature is enabled, use `wee_alloc` as the global
// allocator.
#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

#[wasm_bindgen]
extern {
    fn alert(s: &str);
}

#[wasm_bindgen]
pub fn greet() {
    alert("Hello, wasm-game-of-life!");
}
}
```

this code imports `alert()` and export `greet()` .

## repr

`repr` attribute is used to represent that the layout is for the type

https://doc.rust-lang.org/reference/type-layout.html#representations

```rust

#[repr(C)]
struct ThreeInts {
    first: i16,
    second: i8,
    third: i32
}

#[repr(u8)]
pub enum Cell {
    Dead = 0,
    Alive = 1,
}
```

## difference between `use` and `extern`

https://stackoverflow.com/questions/29403920/whats-the-difference-between-use-and-extern

```
extern crate foo indicates that you want to link against an external library and brings the top-level crate name into scope (equivalent to use foo). As of Rust 2018, in most cases you won't need to use extern crate anymore because Cargo informs the compiler about what crates are present. (There are one or two exceptions)

use bar is a shorthand for referencing fully-qualified symbols.

Theoretically, the language doesn't need use — you could always just fully-qualify the names, but typing std::collections::HashMap.new(...) would get very tedious! Instead, you can just type use std::collections::HashMap once and then HashMap will refer to that.
```
