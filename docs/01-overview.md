# Python Binding Overview

`obix-binding-python` is a TypeScript binding that connects the **native FFI /
polyglot ABI bridge** to a Python runtime, for ML/AI integration and data-science workflows.

## What it does

The binding is a thin, typed control plane over one native entry point. It:

- Builds a structured **invocation envelope** for every call.
- Dispatches envelopes across the ABI boundary via `globalThis.__obixAbiInvoker`.
- Returns **typed error objects** instead of throwing at the FFI edge.
- Resolves interop capabilities from a **schema mode** (`monoglot` /
  `polyglot` / `hybrid`).
- Tracks Python-runtime state (memory / concurrency) through dedicated helpers.

It does **not** embed a Python runtime itself — it marshals calls to whatever
native library `ffiPath` points at and records what that library reports back.

## Capabilities

1. Lifecycle: `initialize` / `invoke` / `destroy` / `isInitialized`
2. FFI transport — envelope build + dispatch
3. Schema-mode resolution and validation
4. Typed error model at the ABI boundary
5. GIL, GC, asyncio, and Modules

## Module map

| Accessor | Type | Responsibility |
|----------|------|----------------|
| `ffiTransport` | `FFITransportAPI` | Envelope builder and ABI dispatcher |
| `gcTracker` | `GCTrackerAPI` | CPython GC generation counters |
| `gilManager` | `GILManagerAPI` | GIL acquire/release state and stats |
| `asyncioExecutor` | `AsyncioExecutorAPI` | Bounded task pool over the event loop |
| `moduleRegistry` | `ModuleRegistryAPI` | Imported-module registry |
| `schemaResolver` | `PythonSchemaResolverAPI` | Schema-mode resolver and validator |

See [03-binding-lifecycle.md](03-binding-lifecycle.md) for the full bridge API.
