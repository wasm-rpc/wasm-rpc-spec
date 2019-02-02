WasmRPC
=======


WasmRpc is a standard format for [Web Assembly](https://webassembly.org/)
[modules](https://webassembly.org/docs/modules/) that allows modules to be run
across multiple Web Assembly runtime implementations.

Goals
----
* The spec should be as simple as possible without being any simpler


Types
----
WasmRpc specifies a super set of the existing [WebAssembly
types](https://webassembly.github.io/spec/core/syntax/types.html) (i32, i64,
f32, f64)).


### Pointer

A pointer to a [Concise Binary Object](CBOR) encoded value. The first 4 bytes
represent the pointer length and rest represent a CBOR encoded value. 




Function parameters
--------

Function parameters are encoded as CBOR and then passed to the function via
`Pointer`s.


Return codes and values
------------

The first 4 bytes of the return value return a 32 bit integer representing the
error code. If the function returns and error code of `0` the function executed
successfully. Any other value represents an error code. The remaining bytes are
a `Pointer` to a return value. Both successful and failed responses can return
error values. I the case of an error that value can be an error message or any
other object representing the error.


Required Exports
--------

### Memory allocation

Memory allocation must be handled by the  

| Function | Arguments | Returns | Description |
|----------|-----------|---------|-------------| 
|alloc | size: usize | pointer: *mut u8| Allocates `size` bytes of memory and returns the
location at `pointer` |
|dealloc | pointer: *mut u8, old_size: usize |void| Deallocates `old_size` bytes of memory located at `pointer`

Optional Imports
----------------

### Memory imports

Memory implementations should be fast and cheap. Memory should be restricted to
smaller payloads but can be read and written to more often.

| Function | Arguments | Returns Description |
|----------|-----------|---------------------|
|get_memory| key: Pointer| pointer: Pointer| Gets a CBOR encoded value located at `pointer` by `key`
| set_memory | key: Pointer, value: Pointer | N/A | Sets a CBOR encoded value `value` by `key`

### Memory imports

Memory implementations should be fast and cheap. Memory should be restricted to
smaller payloads but can be read and written at a higher frequency.

| Function | Arguments | Returns Description |
|----------|-----------|---------------------|
| get_memory | key: Pointer | pointer: Pointer| Gets a CBOR encoded value located at `pointer` by `key` |
| set_memory | key: Pointer, value: Pointer | N/A | Sets a CBOR encoded value `value` by `key`

### Storage imports

Storage should be thought of as a filesystem or Amazon S3. Large blobs of data
can be stored and loaded but  

| Function | Arguments | Returns Description |
|----------|-----------|---------------------|
| get_storage | key: Pointer | pointer: Pointer| Gets a CBOR encoded value located at `pointer` by `key`
| set_storage | key: Pointer, value: Pointer | N/A | Sets a CBOR encoded value `value` by `key`
