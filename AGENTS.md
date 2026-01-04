# Agent Development Guide

A file for [guiding coding agents](https://agents.md/).

- The C API is defined in `quickjs.h`
- `quickjs.h` is in `.zig-cache`. If it isn't there, run `zig build` once.
- Write comprehensive unit tests for all new APIs.
- Mimic the style of the existing Zig codebase.

## Zig Guide

- Use Zig types in API parameters where possible.
- Never use `[*:0]const u8` in public APIs; use `[:0]const u8` and
  call `ptr` on it instead.
- For packed structs or enums porting the C API, always pair it
  with unit tests to ensure we match C constants
- For types that should match C layouts exactly (extern structs,
  enums, packed structs, etc.), always add a comptime assertion
  that the size and alignment matches the header
- For callbacks, use the `opaque.zig` helpers and make the callback
  use free (no conversion cost) Zig types wherever possible. See
  Runtime.addFinalizer or setPromiseHook for an example.
- Don't import other files within a test, use a top-level import.

## Testing Guide

- Tests should use real JavaScript wherever possible (use `eval`)
  and avoid simply twiddling internal state.
