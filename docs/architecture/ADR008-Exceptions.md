# ADR008: Exceptions

**Status**: Accepted and active.

## Context

Standard C++ supports exceptions. Exceptions (`try`, `catch`, `throw`) are often
used for error signaling and handling. Other techniques exist as well, such as
returning error codes and `setjmp`/`longjmp`.

Production C++ compilers allow compiling with exception support disabled. This
changes how code is generated, often by making it smaller and faster.

## Decision

C++ exceptions are disallowed. They are disabled for production builds. Other
techniques, such as returning error codes or `setjmp`/`longjmp`, are used
for error signalling and handling instead of exceptions.

## Consequences

If C++ exceptions were required, WebAssembly generated by Emscripten would need
non-trivial JavaScript support code. Because C++ exceptions are disabled,
WebAssembly generated by Emscripten is straightforward and needs little support
code. This makes embedding quick-lint-js in VS Code easier.

Enabling and using C++ exceptions increases file size by a significant amount
(e.g. 50 KiB for WebAssembly, excluding JavaScript support code) compared to
disabling C++ exceptions.

`setjmp`/`longjmp` is foreign to many C++ programmers and is harder to use than
C++ exceptions.