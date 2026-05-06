---
name: python
description: Python project conventions using modern tooling (uv, ruff, rye).
activation:
  - "*.py"
  - "pyproject.toml"
  - "python"
  - "uv"
  - "ruff"
---

# Python Project Conventions

## Target: Python 3.14 — Use Idiomatic Patterns

All code MUST use Python 3.14 idioms. Do NOT use legacy patterns.

### New in 3.14

- **Template strings (PEP 750):** Use `t"Hello {name}"` for safe string interpolation where injection matters (SQL, HTML, logging). Returns `Template` object for controlled rendering.
- **Deferred annotations (PEP 649/749):** Forward references no longer need string quotes. `def foo() -> MyClass` works even if `MyClass` is defined later. Use `annotationlib.get_annotations()` instead of accessing `__annotations__` directly.
- **Bracketless except (PEP 758):** `except ValueError, TypeError:` — parentheses no longer required around multiple exception types.
- **`functools.Placeholder`:** Use with `partial()` to reserve positional arg slots: `partial(f, Placeholder(), 5)`.
- **`pathlib.Path.copy()` / `.move()`:** Use instead of `shutil.copy2()` / `shutil.move()`.
- **`date.strptime()` / `time.strptime()`:** Parse directly on `date` and `time` classes.
- **`map(strict=True)`:** Like `zip(strict=True)` — raises if iterables differ in length.
- **`compression.zstd`:** Zstandard compression in stdlib.
- **`concurrent.interpreters`:** True multi-core parallelism without GIL constraints.

### Modern Python (3.10–3.13, still required)

- **Type param syntax (PEP 695):** `class Foo[T]:` — no `TypeVar`, no `Generic` imports.
- **`type` statement:** `type Alias = X | Y` — no `TypeAlias` annotation.
- **Union syntax:** `X | Y` — never `Union[X, Y]` or `Optional[X]`.
- **Built-in generics:** `list[int]`, `dict[str, Any]`, `tuple[int, ...]` — never import from `typing`.
- **`match`/`case`:** Prefer over if/elif chains when dispatching on type or structure.
- **`@override`** (from `typing`): On methods that override a base class method.
- **`Self`** (from `typing`): For methods returning the enclosing class.
- **`except*`:** For handling multiple concurrent exceptions (ExceptionGroups).
- **`tomllib`:** For reading TOML — no third-party toml readers.
- **`pathlib.Path`:** Over `os.path` for all path operations.
- **`from datetime import UTC`:** Never `timezone.utc`.
- **f-strings everywhere:** No `.format()` or `%` formatting.

### NEVER use (deprecated/removed patterns)

- `typing.Union`, `typing.Optional`, `typing.List`, `typing.Dict`, `typing.Tuple`, `typing.Set`
- `typing.TypeVar` + `Generic` (use PEP 695 syntax)
- `typing.TypeAlias` (use `type` statement)
- `typing.cast()` (use `# type: ignore[code]`)
- `TYPE_CHECKING` for circular deps (restructure modules instead)
- `fillna(method=...)` or `replace(value, method=...)` (use `.ffill()` / `.bfill()`)
- `__trunc__()` for int conversion (use `__int__()` or `__index__()`)
- `NotImplemented` in boolean context (raises TypeError in 3.14)
- `return`/`break`/`continue` in `finally` blocks (SyntaxWarning in 3.14)
- String-quoted forward references (deferred evaluation makes them unnecessary)

## Tooling
- Use `uv` for dependency management, virtual environments, and running python in projects
- Use `ruff` for linting and formatting
- Use `rye` when managing multiple Python versions

## Code Style
- Prefer Pydantic for validation schemas
- Use `pyproject.toml` over `setup.py`/`requirements.txt`
- Use code comments instead of strings for internal documentation
- Every Python module longer than 100 lines must have a header explaining the purpose of the module

## Circular Dependencies
- Never use `TYPE_CHECKING` to avoid circular dependency. If circular dependency is detected, STOP and suggest a principled fix:
  - Extract shared types to a separate module
  - Use Protocol classes for interface definitions
  - Restructure module hierarchy
