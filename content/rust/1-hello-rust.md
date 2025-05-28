+++
title = "Hello Rust"
description = "The rust problem"
date = 2025-02-24
updated = 2025-05-28
# draft = false
+++

This is a post about my experience reasoning around and learning rust. I have been at it on and off for many months now. And I am dissatisfied with my progress. I was able to click with python in a day, and golang in about a month. Why is rust testing my patience like this? 
I "know" rust. But I don't "know" rust.

# Rust

## Introduction
Rust is a "low level" language. It's supposed to be equivalent to C and C++, which I've always been fascinated with but never progressed in the skill tree for whatever reasons. 
Rust is a great opportunity to explore such depths. I like the idea of a compiler that comes with static analysis. Especially when applied in this domain. 
Plus the type system is amazing. I don't like inheritance too much, but traits are lovely. I also like the enum type rust has. 
What I don't like about rust is the whole async things. I like go's version much more. I wish rust figured out a way to not color all the functions with `async`. Maybe that boat has sailed, maybe not. 
I guess it's just an unsolved problem at this point in this domain. Go comes inbuilt with a garbage collector and a runtime. So can't really blame rust. These things are very hard problems. I'll just say "It's incredible what we can already do today" and move on. 

Rust has been a hobby more than anything, and I wish to keep it like that, writing production code in such a language, I think I'll pass. I don't expect it to be anything more than a hobby. 
I've done a variety of things in it. But never to completion. Here's some of it for bragging's sake. 
 * I've used egui to make a local file browser and deploy it as wasm. 
 * I've used bevy to make a top down character move in 2d, collide with walls and such. 
 * I've used yahoo finance API to get stock quotes and plot them and stuff. I plan to extend this project much more as a research tool. 

## Resources
Before installing rust we need to know the scriptures: 
 - [The Rust Book](https://doc.rust-lang.org/stable/book/title-page.html).
 - [Rust By Example](https://doc.rust-lang.org/stable/rust-by-example/).
 - [jonhoo](https://www.youtube.com/@jonhoo)

## Learn and Teach
How would you teach someone the basics? It's much easier if you just memorize the syntax. I've always had a problem with it and I've forever been stuck in this loop of just opening 'rust by example' to copy-paste some example and make tweaks to it. 
A more advanced version of this is relying too heavily on AI. If you have a problem, ask AI to produce minimum working example and tweak it to your liking. Sure you "understand", but the solution doesn't stick with you.
Memorizing maybe isn't the best approach here. Look at this function signature:
```Rust
fn get(&self) -> Pin<Box<dyn Future<Output = Response> + Send + '_>>
```
Would I be able to tell just by looking at it why it is the way it is? Nope. I had a specific problem. Chatgpt gave me some explanation about how: 
 - Tokio moves stuff around different threads for efficiency reasons, so we need the `Pin`.
 - Size of `dyn Future` isn't known at compile time, so we need the `Box`; its size is known to the compiler. 
 - `dyn Future` is needed for dynamic dispatch of the future, so we can have a future of unknown size at compile time. 
 - This future returns a `Response`, which is a type of unknown size. It comes from doing an API call, so no way of knowing how big it is. 
 - `Send` is a trait bound which makes the `Future` thread safe. Tokio shares data among threads by default. 
 - `_'` is a lifetime bound indicating that the `Future` can borrow data with same lifetime as `&self`. 

I went through many iterations to come up with this. It is perfect for now. But if I have a different problem with slightly different solution, I won't be able to just pull this out of my head. 
I will need chatgpt to tell me again what the problem is, and what the solution is. At the very least, I will need to reference this code. 

Well this was just an example. After having this adventure I discovered a crate called [async-trait](https://docs.rs/async-trait/latest/async_trait/) that does exactly this. The headbangingonwall wasn't useless though since it had learning value. 

One of the criticisms of rust is its learning curve. You need to know everything about everything. So I'm not taking the blame, nor am I blaming rust or chatgpt. 
This is a problem that will be solved by lots more practice. 
