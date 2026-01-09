---
number: 16046
title: B023 rule incorrectly flags loop variables in cases where closures intentionally and correctly capture loop variables
type: issue
state: closed
author: gweidart
labels:
  - needs-info
assignees: []
created_at: 2025-02-08T22:54:03Z
updated_at: 2025-03-06T07:19:16Z
url: https://github.com/astral-sh/ruff/issues/16046
synced_at: 2026-01-07T13:12:16-06:00
---

# B023 rule incorrectly flags loop variables in cases where closures intentionally and correctly capture loop variables

---

_Issue opened by @gweidart on 2025-02-08 22:54_

### Description

## Keywords Searched
- "B023"
- "closure in loop"
- "function definition does not bind loop variable"
- "loop variable capture"
- "dynamic function creation loop"

## Bug Description
Ruff's B023 rule incorrectly flags loop variables in cases where closures intentionally and correctly capture loop variables. This is a false positive as the loop variable binding works as expected in Python's scoping rules.

## Minimal Reproduction
```python
# Example 1: Dynamic callback creation (common in UI frameworks, event handlers, etc.)
def create_callbacks(items: dict) -> list:
    callbacks = []
    
    for name, data in items.items():
        def callback():  # Ruff[B023] incorrectly flags 'name' here
            print(f"Processing {name}: {data}")  # This is the flagged line
        
        callbacks.append(callback)
    
    return callbacks

# Example 2: Real-world usage with Click (common CLI pattern)
def create_cli(command_map: dict) -> click.Group:
    cli_group = click.Group()
    
    for name, command in command_map.items():
        def command_fn(**kwargs):  # Ruff[B023] incorrectly flags 'name' here
            context = CommandContext(
                command=name,  # This is the flagged line
                query=kwargs.get("query"),
                options=parse_options(kwargs),
            )
            exit_code = asyncio.run(run_command(command_map, context))
                if exit_code != 0:
                    click.get_current_context().exit(exit_code)
            except Exception as e:
                click.echo(f"Error: {e!s}", err=True)
                click.get_current_context().exit(1)
        
        cmd = click.Command(
            name=name,
            callback=command_fn,
            help=getattr(command, "help", None)
        )
        cli_group.add_command(cmd)
    
    return cli_group
```

## Command Invoked
```bash
ruff check . --isolated
```

```bash

B023 Function definition does not bind loop variable `name`
    |
139 |             try:
140 |                 context = CommandContext(
141 |                     command=name,
    |                             ^^^^ B023
142 |                     query=kwargs.get("query"),
143 |                     options=parse_options(kwargs),
    |

Found 1 error.

```
## Ruff Settings (pyproject.toml)
```toml
[tool.ruff]
select = ["E", "F", "B", "I"]
ignore = []
line-length = 88
target-version = "py39"
fix = true
unfixable = []
exclude = [
    ".git",
    ".ruff_cache",
    ".venv",
    "venv",
    "__pycache__",
    "build",
    "dist",
]

```
## Ruff Version
```bash
$ ruff --version
ruff 0.9.5
```

> [!NOTE]
> This pattern is common in many Python applications where dynamic function creation is needed:
>   - Event handlers and callbacks
>   - CLI command creation
>   - UI framework event bindings
>   - Plugin systems 
>   - Decorator factories
>

1. The loop variable is correctly captured in the closure according to Python's scoping rules

2. The code works correctly at runtime - each function correctly captures and uses its corresponding loop variable

3. This is a legitimate and common Python pattern, not a bug or anti-pattern

Similar to the confirmed false positive with : 
#7847 #15716 

## Expected Behavior
Ruff should not flag B023 for loop variables that are intentionally captured by closures, as this is a valid and common Python pattern that works correctly according to Python's scoping rules. 

---

_Comment by @InSyncWithFoo on 2025-02-09 02:46_

If I'm not mistaken, the example boils down to this:

```python
class A: ...

def g():
    d = {}

    for i in range(10):
        def f():
            print(i)

        a = A()
        a.f = f
        d[i] = a

    return d
```

```python
# Somewhere else
d = g()
d[0].f()  # 9
```

...which proves that the diagnostic is a true positive. Am I missing something?

---

_Comment by @dylwil3 on 2025-02-09 02:52_

I agree with @InSyncWithFoo , even directly copying your Example 1, I get:

```pycon
>>> def create_callbacks(items: dict) -> list:
...     callbacks = []
...
...     for name, data in items.items():
...         def callback():  # Ruff[B023] incorrectly flags 'name' here
...             print(f"Processing {name}: {data}")  # This is the flagged line
...
...         callbacks.append(callback)
...
...     return callbacks
...
>>> d = {"a":1,"b":2}
>>> create_callbacks(d)[0]()
Processing b: 2
```
which seems like not what one wants, right?

---

_Label `needs-info` added by @MichaReiser on 2025-02-09 13:59_

---

_Closed by @MichaReiser on 2025-03-06 07:19_

---
