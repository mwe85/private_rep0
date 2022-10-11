### Implementation in webasm

1. the main bridging mechanism between rust and the context the js is running is through shared memory and the ability to invoke functions made visible through the exporting mechanism. 

```rust

//example code
#[wasm_bindgen]
extern "C"{
  //attach to the window.console object as 'log'
  #[wasm_bindgen(js_namespace = console, js_name = log)]
  fn log_u32(a: u32);
}

```

2. to use Rust and shared memory, the server needs to serve the page with specific HTTP headers
```http
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

through ```window.crossOriginIsolated``` flag being set, it is possible to detect if shared memory across threads is supported or not. 
