---
mode: agent
description: Create a fully-typed Python class in selection file
---

Target file: `${file}`  
Class name: `${input:ClassName}`  
Key responsibilities: `${input:responsibilities}`

Tasks:
1. Inject a new class with:
   • `@dataclass(slots=True)` if the class is primarily data, else a
     regular class.  
   • Immutable design (`frozen=True`) when feasible.  
   • Rich comparison (`__lt__`, `__eq__`) and `__str__`.  
2. Add Google-style docstring incl. examples.  
3. Append a minimal pytest test stub in comment form (`# >>>`).

Only modify the regions wrapped by:

```python
# === CLASS START ===
# === CLASS END ===
