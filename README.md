WasmRPC
=======


WasmRPC is a standard format for [Web Assembly](https://webassembly.org/)
[modules](https://webassembly.org/docs/modules/) that allows them to be run
across multiple runtime implementations.

WasmRPC allows you to write the following rust code:

```
#[export]
fn hello(string: String) -> String {
    let mut new_string = string.clone();
    new_string.insert_str(0, "hello ");
    new_string
}
```

And call it from JavaScript like this:

```
import WasmRPC from 'wasm-rpc';

async function run() {
    const hello = new WasmRPC();
    await hello.loadFile('target/wasm32-unknown-unknown/debug/hello.wasm');
    let result = await hello.call("hello", "world");

    console.log(result);
    // -> "hello world"
}
run();
```

Read the [WasmRPC specification
here](https://github.com/wasm-rpc/wasm-rpc-spec/blob/master/SPEC.md).
