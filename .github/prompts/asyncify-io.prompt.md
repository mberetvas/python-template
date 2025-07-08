---
mode: agent
description: Convert blocking I/O code to async / await
---

File: `${file}`  
Selection: `${selection}`

Tasks:
1. Identify blocking calls (network, disk, sleep).  
2. Replace with `asyncio` or `aiofiles` / `httpx.AsyncClient`.  
3. Add an async wrapper `async def main_async()` plus
   sync shim `if __name__ == "__main__": asyncio.run(main_async())`.  
4. Update type hints `-> None` ➜ `-> Coroutine[Any, Any, None]`.  
5. Insert comment on event-loop best practices.

Return updated file only.
