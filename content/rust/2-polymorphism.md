+++
title = "Polymorphism"
description = "Traits"
date = 2025-03-28
updated = 2025-03-28
# draft = false
+++ 

Traits is an incredible feature rust has. I want to avoid dissing on other languages no matter how much I enjoy it. So I'll keep it civil. Let's go on a different journey. Imagine you want to build some typical desktop software. It will go on the internet, it will dance with databases, it will do some heavy processing. All packaged together. 
But internet isn't reliable. Choice of a particular database isn't written in stone. Processing too can be done by variety of libraries. This is the standard use case of polymorphism. You want to call the same api functions, but different implementations will be called in the backend, depending on whatever. 

Traits allows you to do that in rust. I've never used java, but it's supposed to be similar to interfaces from that world. Let's look at code examples. 

```rust
trait TraitThing: Debug {
    fn show(&self);
}
```
This is a simple trait that dictates that that function must exist in structs that implement this trait. These mandatory methods serve as definition for our API. In the end, we will initialize a trait object, and without caring what struct is being initialized in the background, call this show method without fear. The `Debug` you see is another trait. What this line means is that structs that implement our `TraitThing` trait must also implement the `Debug` trait. There are certain limitations on what sort of trait bounds we can add in our trait definitions. We can't add `Default` trait bound for example. Because the signature `default() -> Self` returns an object who's size isn't known at compile time. [More Information](https://doc.rust-lang.org/reference/items/traits.html). But that doesn't stop us from actually implementing the `Default` trait or using the `default()` method on our structs. But we can't call it on trait objects. We shouldn't need to call `default()` on trait objects, but it might be a limitation for other traits.

```rust
#[derive(Debug)]
struct StructThing1;

impl Default for StructThing1 {
    #[instrument]
    fn default() -> Self {
        info!("Called default() on StructThing1");
        StructThing1
    }
}

impl TraitThing for StructThing1 {
    #[instrument]
    fn show(&self) {
        info!("Called show() StructThing1");
    }
}
```
This is first implementation for our trait. The `#[derive(Debug)]` line automatically implements the `Debug` trait for our struct. The `#[instrument]` macro prints a log line whenever that function is called. `info!()` is also a logging thing that logs at info level. As we saw before, we couldn't add `Default` trait bound, but we implemented the trait anyway. 

```rust
#[derive(Debug)]
struct StructThing2;

impl Default for StructThing2 {
    #[instrument]
    fn default() -> Self {
        info!("Called default() on StructThing2");
        StructThing2
    }
}

impl TraitThing for StructThing2 {
    #[instrument]
    fn show(&self) {
        info!("Called show() StructThing2");
    }
}
```
This is another implementation for our trait. This is done just to demonstrate later that we can use trait object to call show on any of the implementing structs.
```rust
#[derive(Debug)]
enum EnumThing {
    EnumThing1,
    EnumThing2
}
```
This is an enum. We can set which implementation of our trait we want to "select". This can be done in non enum, in a sort of if-else way if there's any other logic to set it. We do it manually. 

```rust
fn main() {
    tracing_subscriber::fmt::init();

    let mut enum_thing = EnumThing::EnumThing1;
    let mut trait_object: Box<dyn TraitThing> = match enum_thing {
        EnumThing::EnumThing1 => Box::new(StructThing1::default()),
        EnumThing::EnumThing2 => Box::new(StructThing2::default())
    };

    trait_object.show();

    enum_thing = EnumThing::EnumThing2;
    trait_object = match enum_thing {
        EnumThing::EnumThing1 => Box::new(StructThing1::default()),
        EnumThing::EnumThing2 => Box::new(StructThing2::default())
    };

    trait_object.show();
}
```
The `tracing_subscriber::fmt::init();` line is there to capture all the logs we've been writing till now. There are 2 important blocks in this main function. We initialize the enum, and instantiate our trait object. This has the type `Box<dyn TraitThing>`. The `dyn TraitThing` part tells us that it's a dynamically defined object: any object that implements the `TraitThing` trait. Calling methods on trait object is achieved with dynamic dispatch, which is where the `dyn` keyword comes from. `Box<>` is a pointer to say that it's allocated on heap. Any object can implement this trait, so we don't really know the size of object when we make it. That's why it needs to be on heap. In the first block, we created a trait object and called show on it. In the next block, we're just modifying the same object and not creating a new one, yet the implementation changes to 2nd struct. This is the output.
```log
2025-03-28T03:24:26.453717Z  INFO default: crate_thing: Called default() on StructThing1
2025-03-28T03:24:26.453761Z  INFO show{self=StructThing1}: crate_thing: Called show() StructThing1
2025-03-28T03:24:26.453769Z  INFO crate_thing: Called default() on StructThing2
2025-03-28T03:24:26.453774Z  INFO show{self=StructThing2}: crate_thing: Called show() StructThing2
```

This behavior allows us for example to write a scrapper that scraps the same information from different websites. Or interact with different databases using the same API. Or use multiple libraries for processing our data. There is of course not infinite flexibility. If it's the same application there are certain assumptions that go with that, like the read / write operations you're doing on database and the data are similar, similar processing is being done by all the different libraries. It is only as flexible as you can make your trait methods. And this is a good thing. Infinite flexibility also means infinite complexity. 

