# How to create a function for Sat and use it

## Generate a library skeleton

```bash
cargo new --lib hello-echo
```

## Update `Cargo.toml`

Update the `Cargo.toml` by adding the 2 sections below:

```toml
[lib]
crate-type = ["cdylib", "rlib"]

[dependencies]
suborbital = "0.12.0"
# add yourother dependencies here
```

## Change `src/lib.rs`

```rust
use suborbital::runnable::*;

struct HelloEcho{}

impl Runnable for HelloEcho {
    fn run(&self, input: Vec<u8>) -> Result<Vec<u8>, RunErr> {
        let in_string = String::from_utf8(input).unwrap();
        Ok(format!("hello {}", in_string).as_bytes().to_vec())
    }
}

// initialize the runner, do not edit below //
static RUNNABLE: &HelloEcho = &HelloEcho{};

#[no_mangle]
pub extern fn _start() {
    use_runnable(RUNNABLE);
}
```

## Build the wasm file

```bash
cd hello-world
cargo build --target wasm32-unknown-unknown
cp target/wasm32-unknown-unknown/debug/hello-world.wasm ./
```


## Use the function with Sat

