---
title: "Python - Locks, Acquisition, Release"
weight: 2
tags: ["python"]
---

_Date Created: 2025-07-27_

# Python - Locks, Acquisition, Release

## What is a Lock?

`Lock` is a synchronization primitive that ensures only one `coroutine/thread` can access a shared resource at a time.

## What is Acquisition and Release?

`Acquisition` is the process of obtaining exclusive access to a resource. `Release` frees the lock so other Lock can acquire it.

## Example

Here is an example where 3 workers take turn to acquire the lock.

````python
from asyncio import Lock

```python
import asyncio

async def worker(lock, worker_id):
    print(f"Worker {worker_id} is waiting for the lock")
    async with lock:
        print(f"Worker {worker_id} acquired the lock")
        await asyncio.sleep(2)  # Simulate work
        print(f"Worker {worker_id} is releasing the lock")

async def main():
    lock = asyncio.Lock()

    # Create 3 coroutines that will compete for the lock
    tasks = [
        asyncio.create_task(worker(lock, 1)),
        asyncio.create_task(worker(lock, 2)),
        asyncio.create_task(worker(lock, 3))
    ]

    await asyncio.gather(*tasks)

# Run the example
asyncio.run(main())
````

In this example `asyncio.Lock` is a context manager because it implemented `__aenter__` and `__aexit__` for async context manager. Here is a sample.

```python
import asyncio
from typing import Optional, Any

class CustomAsyncLock:

    def __init__(self):
        self._lock = asyncio.Lock()

    async def __aenter__(self):
        print("🔒 Context manager: Acquiring lock...")
        await self._lock.acquire()
        print("✅ Context manager: Lock acquired!")
        return self  # Return self so it can be used as 'as' variable

    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print(f"🔓 Context manager: Releasing lock (exception: {exc_type is not None})")
        self._lock.release()
        print("✅ Context manager: Lock released!")
        return False

    async def acquire(self):
        await self._lock.acquire()
        self._owner = "manual"

    def release(self):
        self._lock.release()

    def locked(self) -> bool:
        return self._lock.locked()

```

The `await self._lock.acquire()` will be blocking until the lock becomes available.

The parameters `exc_type`, `exc_val` and `exc_tb` in the `__aexit__` method are the exception information get passed when an exception occurs inside the `async with` block:

1. `exec_type`: The type (e.g., `ValueError`, `KeyError`). `None` if no exception.
2. `exec_val`: The actual exception instance. `None` if no exception.
3. `exc_tb`: The traceback object containing the stack trace information. `None` if no exception occurred.
