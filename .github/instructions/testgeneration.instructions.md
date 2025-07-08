---
description: Guidelines for generating pytest unit & integration tests
applyTo: "**/*.py"
---

# 🧪  Pytest Test-Generation Rules

## Naming & Location
1. Test modules **must** live under `tests/` and be named  
   `test_<src_module_name>.py`.
2. When generating a class of tests, name it `Test<SrcClassName>`
   (CamelCase).

## Structure & Style
1. Each test follows **Arrange – Act – Assert** commented sections.  
2. Use plain `assert` statements—not `self.assert*`.  
3. Keep one logical assertion per test unless `pytest.mark.parametrize`
   is used.

## Fixtures
1. Prefer **function-scoped fixtures**; name them with a trailing
   `_fx`.  
2. Place shared fixtures in `conftest.py`.  
3. For external resources (DB, API), provide a
   `@pytest.fixture(scope="session")` that spins up a lightweight
   dockerised service if feasible.

## Parametrisation
1. Use `pytest.mark.parametrize` for input matrices; list parameters
   vertically for readability.  
2. Always include an edge-case row (e.g., empty string, `0`, `None`).

## Async & IO
1. Tag async tests with `@pytest.mark.asyncio`.  
2. Use `pytest.raises` for exception paths; include **exact** message
   match.  
3. Mock blocking IO with `monkeypatch` *or* `pytest-mock`’s
   `mocker.patch`.

## Coverage & Quality
1. Auto-insert a `# pragma: no cover` comment for intentionally
   unreachable branches.  
2. Target ≥ 90 % line coverage; if that’s not possible, explain why in a
   `# noqa: E501` comment above the exclusion.  
3. Add type hints to every generated test function.

## Output Conventions
1. Return **one** fenced code block per file, using the filename as the
   info string:  
    filename=tests/test_math_utils.py
   # content…

2. End each file with a blank line (\n).
