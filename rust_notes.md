### Implementation in webasm

the main bridging mechanism between rust and the context the js is running is through shared memory and the ability to invoke functions made visible through the exporting mechanism. 

```rust

//example code
#[wasm_bindgen]
extern "C"{
  //attach to the window.console object as 'log'
  #[wasm_bindgen(js_namespace = console, js_name = log)]
  fn log_u32(a: u32);
}

```

#### shared memory
to use Rust and shared memory, the server needs to serve the page with specific HTTP headers
```http
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

through [```window.crossOriginIsolated```](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) flag being set, it is possible to detect if shared memory across threads is supported or not. 

#### ```dyn&``` keyword
it is a lot like c++ vtables. when a type is decorated with the keyword, it loses its type information and the variable being passed consist of two pointers, one is a pointer to the instance and the other is to a table of available methods. this involves dynamic dispatch, and type erasure. 
 
#### other
b. Invoking a constructor of self type [SelfTy](https://doc.rust-lang.org/std/keyword.SelfTy.html)

c. [dyn keyword](https://doc.rust-lang.org/std/keyword.dyn.html)

d. [dynamic dispatch](https://doc.rust-lang.org/book/ch17-02-trait-objects.html)
