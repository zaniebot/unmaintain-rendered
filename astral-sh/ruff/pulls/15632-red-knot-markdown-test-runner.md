```yaml
number: 15632
title: "[red-knot] Markdown test runner"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-runner
created_at: 2025-01-21T09:33:53Z
updated_at: 2025-02-05T16:14:36Z
url: https://github.com/astral-sh/ruff/pull/15632
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Markdown test runner

---

_Pull request opened by @sharkdp on 2025-01-21 09:33_

## Summary

As more and more tests move to Markdown, running the mdtest suite becomes one of the most common tasks for developers working on Red Knot. There are a few pain points when doing so, however:

- The `build.rs` script enforces recompilation (~five seconds) whenever something changes in the `resource/mdtest` folder. This is strictly necessary, because whenever files are added or removed, the test harness needs to be updated. But this is very rarely the case! The most common scenario is that a Markdown file has *changed*, and in this case, no recompilation is necessary. It is currently not possible to distinguish these two cases using `cargo::rerun-if-changed`. One can work around this by running the test executable manually, but it requires finding the path to the correct `mdtest-<random-hash>` executable.
- All Markdown tests are run by default. This is needed whenever Rust code changes, but while working on the tests themselves, it is often much more convenient to only run the tests for a single file. This can be done by using a `mdtest__path_to_file` filter, but this needs to be manually spelled out or copied from the test output.
- `cargo`s test output for a failing Markdown test is often unnecessarily verbose. Unless there is an *actual* panic somewhere in the code, mdtests usually fail with the explicit [*"Some tests failed"* panic](https://github.com/astral-sh/ruff/blob/c616650dfa514c58ac86c5c60648d4d2dcf17d6d/crates/red_knot_test/src/lib.rs#L96) in the mdtest suite. But in those cases, we are not interested in the pointer to the source of this panic, but only in the mdtest suite output.

This PR adds a Markdown test runner tool that attempts to make the developer experience better.

Once it is started using
```bash
uv run crates/red_knot_python_semantic/mdtest.py
```
it will first recompile the tests once (if cargo requires it), find the path to the `mdtest` executable, and then enter into a mode where it watches for changes in the `red_knot_python_semantic` crate. Whenever …
* … a Markdown file changes, it will rerun the mdtest for this specific file automatically (no recompilation!). 
* … a Markdown file is added, it will recompile the tests and then run the mdtest for the new file
* … Rust code is changed, it will recompile the tests and run all of them

The tool also trims down `cargo test` output and only shows the actual mdtest errors.

The tool will certainly require a few more iterations before it becomes mature, but I'm curious to hear if there is any interest for something like this.

## Preview

![mdtest](https://github.com/user-attachments/assets/ebdb20a0-c12d-4657-94b1-ac0f38d79fe9)

## Test Plan

- Tested the new runner under various scenarios.

---

_Label `testing` added by @sharkdp on 2025-01-21 09:33_

---

_Label `red-knot` added by @sharkdp on 2025-01-21 09:33_

---

_Review requested from @carljm by @sharkdp on 2025-01-21 09:33_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-21 09:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-21 09:33_

---

_Comment by @github-actions[bot] on 2025-01-21 09:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/mdtest.py`:145 on 2025-01-21 10:04_

Curious Python typing scenario. The `Never`/`NoReturn` here is technically correct, I believe, but apparently not possible for type-checkers to infer. `watch` returns an infinite generator, but even if the return type of `watch` would be `Generator[…, …, Never]`, that doesn't seem to help. Both mypy and pyright have problems with this:
```py
from typing import Never, Generator

def generator() -> Generator[int, None, Never]:
    while True:
        yield 1

def f() -> Never:
    for _ in generator():
        pass
```

---

_@sharkdp reviewed on 2025-01-21 10:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:1 on 2025-01-21 11:04_

Should we document this script in https://github.com/astral-sh/ruff/blob/main/crates/red_knot_test/README.md?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:5 on 2025-01-21 11:07_

I nearly suggested that we use `termcolor` instead of `rich`, since it's a much smaller library to have as a dependency, but I suppose your use of `console.status` is good enough reason to stick with `rich` 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:8 on 2025-01-21 11:10_

Should we add a `tool.uv.exclude-newer` value in this inline config, to ensure new versions of these dependencies don't unexpectedly break the script? I doubt that they will -- `rich` and `watchfiles` are both very widely used and fairly dependable... but there's still a chance that one of them might do a breaking major release at some point?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:23 on 2025-01-21 11:11_

minor: you could annotate these with `Final`:

```suggestion
from typing import Final, Literal, Never

from rich.console import Console
from watchfiles import Change, watch

CRATE_NAME: Final = "red_knot_python_semantic"
CRATE_ROOT: Final = Path(__file__).resolve().parent
MDTEST_DIR: Final = CRATE_ROOT / "resources" / "mdtest"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:52 on 2025-01-21 11:17_

nit: I generally prefer to make most `bool` flags keyword-only; it makes callsites more readable

```suggestion
        self, status_message: str, *, message_on_success: bool = True
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:180 on 2025-01-21 11:23_

is this an exhaustive match? Could we be explicit by adding a fallback branch with either `pass` (if it's not exhaustive) or an assertion that it's unreachable (if it's exhaustive)? We can use `typing.assert_never()` if this match is meant to be exhaustive (though pyright [has some issues](https://github.com/microsoft/pyright/issues/7559#issuecomment-2108198425) with the function)

```suggestion
                match change:
                    case Change.added:
                        # When saving a file, some editors (looking at you, Vim) might first
                        # save the file with a temporary name (e.g. `file.md~`) and then rename
                        # it to the final name. This creates a `deleted` and `added` change.
                        # We treat those files as `changed` here.
                        if (Change.deleted, path_str) in changes:
                            changed_md_files.add(relative_path)
                        else:
                            new_md_files.add(relative_path)
                    case Change.modified:
                        changed_md_files.add(relative_path)
                    case _:
                        pass
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:97 on 2025-01-21 11:26_

I'm generally not a _massive_ fan of instance attributes that aren't initialized in `__init__`, because they can lead to `AttributeError`s at unexpected moments. I'd prefer to either add an assertion that the attribute has been initialized here (the suggestion below), or (better) have the attribute declared as `Path | None` in the class body, initialize it to `None` in `__init__`, and add an assertion in this method that `self.mdtest_executable is not None` before using it. 

I'd also make `capture_output` keyword-only

```suggestion
    def _run_mdtest(
        self, arguments: list[str] | None = None, *, capture_output: bool = False
    ) -> subprocess.CompletedProcess:
        arguments = arguments or []
        assert hasattr(self, "mdtest_executable"), "Must call `self._recompile_tests()` before calling `self._run_mdtest()`"
        return subprocess.run(
            [self.mdtest_executable, *arguments],
            cwd=CRATE_ROOT,
            env=dict(os.environ, CLICOLOR_FORCE="1"),
            capture_output=capture_output,
            text=True,
            check=False,
        )
```

---

_@AlexWaygood approved on 2025-01-21 11:27_

This is fantastic!

---

_@sharkdp reviewed on 2025-01-21 11:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/mdtest.py`:5 on 2025-01-21 11:33_

Right... I have never used `rich` before and it seems a bit bloated, but on the other hand, `uv` is fast :smile: 

---

_@sharkdp reviewed on 2025-01-21 11:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/mdtest.py`:8 on 2025-01-21 11:40_

We could also use the brand new locking for scripts: https://docs.astral.sh/uv/guides/scripts/#locking-dependencies.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:32 on 2025-01-21 11:57_

minor: you could consider using a dataclass for the `MDTestRunner`. It doesn't help with our `__init__` method much, but it would autogenerate a reasonable `__repr__` method for the class

```suggestion
from dataclasses import dataclass
from pathlib import Path
from typing import Final, Literal, Never, assert_never

from rich.console import Console
from watchfiles import Change, watch

CRATE_NAME: Final = "red_knot_python_semantic"
CRATE_ROOT: Final = Path(__file__).resolve().parent
MDTEST_DIR: Final = CRATE_ROOT / "resources" / "mdtest"


@dataclass
class MDTestRunner:
    mdtest_executable: Path | None = None
    console: Console

    def __init__(self) -> None:
        self.console = Console()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:34 on 2025-01-21 11:59_

this parameter seems to always be passed using keyword arguments, which I think is more readable; you could possibly enforce it in the signature
```suggestion
    def _run_cargo_test(self, *, message_format: Literal["human", "json"]) -> str:
```

---

_@AlexWaygood reviewed on 2025-01-21 11:59_

---

_@sharkdp reviewed on 2025-01-21 12:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/mdtest.py`:32 on 2025-01-21 12:11_

Hm :smile:. I love dataclasses, but this class feels very much *not* like a data class. It has a custom `__init__`, I don't want/need auto-generated comparison methods, and I probably don't want to debug this class by inspecting it's `repr` (I previously had a logging output that showed the `mdtest_executable` path, but removed that because it didn't add any value). Change my mind.

---

_@AlexWaygood reviewed on 2025-01-21 12:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:32 on 2025-01-21 12:14_

Aha! I see you're one of the dataclass "purists" ;)

I think there's two perspectives on `@dataclass`: you either think that it's a convenient code generator for a class, or you think that it should only be used for simple classes that wrap data, similar to Rust structs. I generally think of them the first way, but the name of the module obviously favours the second interpretation ;)

I also agree that the gains here are very minimal, so I'm happy to leave it as-is!

---

_@AlexWaygood approved on 2025-01-21 12:15_

---

_@AlexWaygood reviewed on 2025-01-21 12:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/mdtest.py`:145 on 2025-01-21 12:15_

I'm sure there was a discussion at some point about how best to annotate infinitely looping generator functions in such a way that type checkers would understand that they never return. I can't find it now, though :(

---

_@MichaReiser approved on 2025-01-21 12:22_

This is amazing! 

---

_Comment by @sharkdp on 2025-01-21 13:05_

Thank you for the reviews! I'm going to merge this, so that everyone interested can easily test it.

---

_Merged by @sharkdp on 2025-01-21 13:06_

---

_Closed by @sharkdp on 2025-01-21 13:06_

---

_Branch deleted on 2025-01-21 13:06_

---
