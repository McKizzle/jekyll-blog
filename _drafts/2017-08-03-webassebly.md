---
layout: pure
title: Build a Rust WebAssembly Module  
tags: [notes, webassembly, rust]
category: RO-Blog
date: 2017-08-03 00:00:00
---

# WebAssembly

## Setup Linux / OS X
### Installing and Configuring the Emscripten SDK

Setting up your machine for WebAssembly development is fairly straightforward. For more information please refer to the
[MDN WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly) and to 
[Emscripten - Getting Started](https://kripken.github.io/emscripten-site/docs/getting_started/index.html) for more detailed 
information regarding the emscripten SDK installation and system configuration.

Start out by either cloning the [Emscripten SDK Repository](https://github.com/kripken/emscripten) or downloading the provided tar on 
the [Emscripten download page](https://kripken.github.io/emscripten-site/docs/getting_started/downloads.html). Once fetched navigate 
to the Emscripten SDK directory in your favorate terminal emulator.

```sh
cd <path/to/emscripten-sdk/folder>
./emsdk update
./emsdk install latest
./emsdk activate latest 
```

The installation step will probably take a while since it requires fetching and compiling source directories. Once the activation is
complete you'll have the option to export env variables such that you can build your own WebAssembly modules.

```sh
# you can add this to your terminal profile startup script.
source <path/to/emscripten-sdk/folder>/emsdk_env.sh
```

### Installing Rust with Rustup
Developing WebAssembly modules requires compiling your rust source code with the latest version of the rust compiler (the nightly build)
The easiest way to do this is via the rustup tool which allows you to have multiple rust compilers installed on a single computer. 
Download the latest version of rustup at [https://www.rustup.rs](https://www.rustup.rs).

## Creating WebAssembly Modules with Rust Projects.
First create your cargo project by running `cargo new rust-webassembly`. Cargo is the default build tool that is provided with rust akin
to gnu-make and older compiled code (except simpler to use for rust programming). Unlike make, cargo allows you to specify dependencies 
to which it will fetch and compile so that they can be used within the project. This project is going to be simple and will not require
any external dependencies.

### Creating a cargo project
To create the cargo project run `cargo new rust-webassembly --bin`. This will create a directory of the following structure which should
also be git initialized.

```
rust-webassembly/
├── Cargo.toml
└── src
    └── main.rs
```

Why did I setup the project as a bin? The biggest reason to my insufficient knowledge of the emscripten sdk compiler. Using a rust
source file with a main method allows exporting the source functions that I'm interested in.

### Write the rust code.
Add the following code to the `main.rs` file. 

```rust

#[link(name = "-s EXPORTED_FUNCTIONS=['_ping']")]
#[link(name = "-s DEMANGLE_SUPPORT=1")]
extern {}

use std::os::raw::c_char;
use std::ffi::CString;

fn main() { } // the main function is required, it's used by the emcc compiler.

#[no_mangle]
pub fn ping() -> *mut c_char {
    CString::new("pong").unwrap().into_raw()
}
```

### Compiling your project to wasm
For simplicity add the makefile to your project

```make
SHELL := /bin/bash

all:
	cargo build --target=wasm32-unknown-emscripten --release
	mkdir -p site
	find target/wasm32-unknown-emscripten/release/deps -type f -name "*.wasm" | xargs -I {} cp {} site/site.wasm
	find target/wasm32-unknown-emscripten/release/deps -type f ! -name "*.asm.js" -name "*.js" | xargs -I {} cp {} site/site.js

serve: all
	pushd site; python -m SimpleHTTPServer; popd

clean:
	cargo clean
```

### Executing your wasm executable in javascript. 
Next add `index.html` to your site directory.

```html
<script type='text/javascript'>
  var Module = {};
  fetch('site.wasm')
    .then(response => response.arrayBuffer())
    .then(buffer => {
      Module.wasmBinary = buffer;
      Module._main = function() {
        var ping = Module.cwrap('ping', 'string', []);
        var pong = ping();
        console.log(pong);
      }
      var script = document.createElement("script");
      script.src = "site.js";
      script.onload = function() {
        console.log("Emscripten boilerplate loaded.");
      };
    });
   
</script>
<script type="text/javascript" src="site.js"></script>
<html>
  <body>
    <p> Open up your console to view the output. </p>
  </body>
</html>
```

## More advanced topics such as return types. And so forth. 
Under construction...
