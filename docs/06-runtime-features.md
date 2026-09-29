# GIL, GC, asyncio, and Modules

## GIL manager

`binding.gilManager` tracks the Global Interpreter Lock state the native side
reports — `acquire()` / `release()` bookkeeping and a `GILStats` snapshot
(hold time, contention count). It does not itself hold a lock; it mirrors what
CPython does across the FFI boundary.

## GC tracker

`binding.getMemoryUsage()` returns `PythonGCStats` — per-generation collection
counts and tracked-object totals. `binding.gcTracker` exposes the raw counters.

## asyncio executor

`binding.asyncioExecutor` runs invocations as bounded concurrent tasks
(`executorPoolSize`) on the host event loop, so blocking native calls do not
starve other work. `AsyncioStats` reports queued / running / completed.

## Module registry

`binding.moduleRegistry` records which Python modules have been imported through
the bridge, so repeated `import` requests are deduplicated.
