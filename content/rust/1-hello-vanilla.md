+++
title = "Hello Vanilla"
description = "The rust journey"
date = 2025-02-24
updated = 2025-02-24
# draft = false
+++

These are my notes. The idea is having a database of all the problems I've solved in rust and being able to come back to them and reimplement them for new problems.

This is a post about vanilla how-to rust. 

# Rust

Rust is a "low level" language. It's supposed to be equivalent to C and C++, which I've always been fascinated with but never progressed in the skill tree for whatever reasons. 
Rust is a great opportunity to explore such depths. I like the idea of a compiler that comes with static analysis. Especially when applied in this domain. 
Plus the type system is amazing. I don't like inheritance too much, but traits are lovely. I also like the enum type rust has. 
What I don't like about rust is the whole async things. I like go's version much more. I wish rust figured out a way to not color all the functions with `async`. Maybe that boat has sailed, maybe not. 
I think zig wants to do it, but I guess it's just an unsolved problem at this point. So can't really blame rust. These things are very hard problems. I'll just say "It's incredible what we can already do today" and move on.  

Rust has been a hobby more than anything, and I wish to keep it like that, writing production code in such a language, I think I'll pass. I don't expect it to be anything more than a hobby. 
I've done a variety of things in it. But never to completion. Here's some of it for bragging's sake. 
 * I've used egui to make a local file browser and deploy it as wasm. 
 * I've used bevy to make a top down character move in 2d, collide with walls and such. 
 * I've used yahoo finance API to get stock quotes and plot them and stuff. I plan to extend this project much more as a research tool. 


## Basic
Well first we need to install rust. No. First we need to know about [The Rust Book](https://doc.rust-lang.org/stable/book/title-page.html). Everything in there should be read. Which includes how to install rust. 

The short version is this, as copy pasted from the book. Read. The. Book. 
```shell 
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh 
``` 

This will also install `cargo` which is a package manager for all things rust. You can manage your own projects and can also install other rust things like ripgrep. 

## Coding
Make a new directory, `cd` in it and do `cargo init`, that will set up a basic project for rust. 
Open `main.rs` and write code in it. 

### Hello, world!
This is the customary hello world in rust. It should just be there in `main.rs` when you first do `cargo init`, which you run with `cargo run`
```rust 
fn main() { 
    println!("Hello, world!"); 
} 
```

Here's another cool resource [Rust by Example](https://doc.rust-lang.org/stable/rust-by-example/), which has example code for everything: printing, variables and more. It's worth going there to check for syntax things rather than here. This post will be a strict subset of that resource for now. Only the things I need to lookup most often. 

### If-else
```rust
if n < 0 {
    print!("{} is negative", n);
} else if n > 0 {
    print!("{} is positive", n);
} else {
    print!("{} is zero", n);
}
```

### Loops
```rust
loop {
    println!("Infinite loop");
}
```
```rust
while true {
    println!("Another infinite loop");
}
```
```rust
println!("Printing 1 to 10");
for n in 1..=10 {
    println!("{}", n);
}
```

### Struct
```rust
struct Point {
    x: f32,
    y: f32,
}
```

### Enum
```rust
enum Color {
    Red,
    Blue,
    RGB(u32, u32, u32),
}
```

### Pattern matching
```rust
let color = Color::Red;
match color {
    Color::Red          => println!("The color is Red!"),
    Color::RGB(r, g, b) => println!("The color is r={}, g={}, b={}", r, g, b),
    ...                 => println!("The color is unknown"),
}

let a = Color::Red;
if let Color::Red = a {
    println!("a is red");
}
```

### Functions
```rust
pub async fn some_function<T: Debug>(t: T) -> Result<T> {
    Ok(t)
}
```

