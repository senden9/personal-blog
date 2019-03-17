+++
title = "Lorem Ipsum - Test"
description = "Test entry for to see the formating."
date = 2019-02-16

[taxonomies]
tags = ["Lorem"]
+++
Test entry for to see the formating.

# Old Lorem Ipsum
*Lorem* ipsum dolor sit amet, consetetur [sadipscing](http://example.com/) elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Duis Autem
Duis **autem** vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi. Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat.

## Ut Wisi Enim
Ut wisi enim ad minim veniam, quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat. Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla facilisis at vero eros et accumsan et iusto odio dignissim qui blandit praesent luptatum zzril delenit augue duis dolore te feugait nulla facilisi.

Nam liber tempor cum soluta nobis eleifend option congue nihil imperdiet doming id quod mazim placerat facer possim assum. Lorem ipsum dolor sit amet, consectetuer adipiscing elit, sed diam nonummy nibh euismod tincidunt ut laoreet dolore magna aliquam erat volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo consequat.

Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse molestie consequat, vel illum dolore eu feugiat nulla facilisis.

# "What is an 'atomic' anyway?"

Atomics are data types on which operations are indivisible,
or the indivisible operations by themselves. Indivisible here means that
the side effects of the operations are only observable when the operation
is done. Generally, machines provide read and write operations over its
native integer both indivisible and divisible.

If you have ever heard of  "data races", divisible read and write causes them.
Suppose we are on a 64-bit word machine. Threads `A`, `B` and `C` have
access to address `p`. If `A` does a "divisible" write on `p` with `x`,
at the same time `C` does a "divisible" read, `C` might get half of previous
bits of `p` and half bits of `x`. Or a quarter of bits. Or the previous state.
Or the new state. It's actualy hard to predict. If thread `A` writes
"divisibily" again on `p` with `y` at the same time thread `B` writes
"divisibily" `z`, the resulting value of `p` might be a mix of both states or
something similar.

Because the behavior might vary between different machines, programming
languages such as Rust and C++ defines data races as undefined behavior. The
actual behavior might not be "leaving an intermediate value", but it could
be anything, from throwing an exception to setting the processor on fire.
You probably noticed that the problem here are the "divisible" operations.
If `A` had atomically written `x` on `p`, and `C` had atomically read from
`p`, `C` would see the bits of `p` either before of `A`'s write or after the
write, but not in the middle. The same apply for `A` and `B` writing
atomically; the final state would be either `y` or `z`, but not anything else.

## Atomics in Rust

Currently (rust nightly 1.28 and stable 1.26) these atomic data types were
stabilized: `AtomicPtr<T>`, `AtomicUsize`, `AtomicIsize` and `AtomicBool`.
They are all located in `core::sync::atomic` (or `std::sync::atomic`).
To explain, I will pick the simplest one: `AtomicBool`.

```rust
pub enum Ordering {
    Relaxed,
    Release,
    Acquire,
    AcqRel,
    SeqCst,
    // some variants omitted
}
```
