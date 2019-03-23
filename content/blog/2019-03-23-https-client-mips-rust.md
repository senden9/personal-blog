+++
title = "(edit title) HTTPS Client MIPS Rust"
date=2019-03-23

[taxonomies]
tags = ["Rust", "MIPS"]

[extra]
author = "Stefano Probst"
+++
Want to access an HTTPS endpoint of an API via reqwest. Problem TLS/SSL.

Default backend not working (openssl cross compiling). Try different backend via https://github.com/seanmonstar/reqwest/blob/d2eee8591a2ae3dac5d3bc33c0d7a25c7b01f9bf/Cargo.toml#L48-L58.
```toml
[dependencies.reqwest]
version = "0.9.12"
default-features = false # do not include the default features
features = ["rustls-tls"]
```

Also not working because ring does not support mips at the moment of writing. https://github.com/briansmith/ring/issues/562

Possible solution: Try to build in Docker with Cross! https://github.com/rust-embedded/cross
