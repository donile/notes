# Ownership

## Box

Memory on the heap.

```rust
let box = Box::new("5");
```

## Box Deallocation Principle

If a variable owns a box, when Rust deallocates the variable's frame, then Rust deallocates the box's heap memory.

## Ownership

Only exists at compile-time

All heap data must be owned by exactly one variable

Rust deallocates heap data once it's owner goes out of scope

Ownership can be transfered by moves, which happen on assignments and function calls

Heap data may only be accessed through its current owner, not a previous owner

References cannot transfer ownership of non-copyable types.

If two variables owned the same object/data, they both would attempt to deallocate the memory, causing a double-free error.

## Moving

Transfers ownership of a box (memory on the heap) to a new variable.

A variable may not be used after it's ownership has been transfered to a new variable.

## Borrowing

Use the `&` character to create a reference

References are non-owning pointers

## Dereferencing

```rust
let x: i32 = 5;
let x_ref = &x;
let y = *x;
*x += 1;
```

## Immutaable References

Create an immutable reference with the `&` operator.

Provides aliasing (ability to read data) but disallows mutation (ability to write data).

```rust
let num: i32 = 5;
let num_ref = &num;
println!("num: {}", *num_ref);
```

## Mutable References

Create a mutable reference with the `&mut` operator.

A mutable reference may change the underlying data.

```rust
let mut num: i32 = 5;
let num_ref = &mut num;
*num_ref = 6;
println!("num: {}", *num_ref);
```


