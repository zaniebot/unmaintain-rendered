```yaml
number: 18888
title: "[formatter] Fix missing blank lines before decorated classes in .pyi files"
type: pull_request
state: merged
author: krikera
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-23T10:20:38Z
updated_at: 2025-06-24T14:25:45Z
url: https://github.com/astral-sh/ruff/pull/18888
synced_at: 2026-01-10T18:39:09Z
```

# [formatter] Fix missing blank lines before decorated classes in .pyi files

---

_Pull request opened by @krikera on 2025-06-23 10:20_

## Summary

Fixes missing blank lines before decorated classes that follow functions in `.pyi` (stub) files.

**Problem**: When a decorated class follows a function definition in a stub file, the formatter was incorrectly omitting the blank line before the class, resulting in formatting like:

```python
def hello(): ...
@decorator
class A: ...
```

**Solution**: Removed the `class_decorator_instead_of_empty_line` condition that was preventing blank lines from being inserted in this scenario. This condition was based on a Black quirk that was actually a bug and shouldn't be replicated.

The fix is gated behind preview mode to ensure backward compatibility. When preview mode is enabled, decorated classes following functions now correctly get blank lines:

```python
def hello(): ...

@decorator
class A: ...
```

This brings Ruff's behavior in line with proper Python style guidelines and fixes the inconsistency where blank lines were added for other statement types but not decorated classes.

## Test Plan

**Manual Testing:**
- Created test files with functions followed by decorated classes
- Verified fix works with `--preview` flag: `cargo run --bin ruff_python_formatter -- --emit stdout --preview test.pyi`
- Verified backward compatibility without `--preview`: old behavior preserved
- Tested with various decorator types (`@lambda`, `@final`, multiple decorators)

**Automated Testing:**
- Added new test case `decorated_class_after_function.pyi` with preview options
- Ran `cargo test -p ruff_python_formatter` - all tests pass
- Snapshot tests show correct formatting changes for both new and existing test cases
- Existing functionality unaffected (no regressions)

**Preview Mode Integration:**
- Added `is_blank_line_before_decorated_class_in_stub_enabled()` preview function
- Verified condition is only applied when preview mode is disabled
- Confirmed proper gating follows established preview patterns

Closes #18865

---

_Review requested from @MichaReiser by @krikera on 2025-06-23 10:20_

---

_Label `formatter` added by @MichaReiser on 2025-06-23 10:52_

---

_Label `preview` added by @MichaReiser on 2025-06-23 10:52_

---

_Comment by @MichaReiser on 2025-06-23 10:59_

The code changes look good. Can you run `cargo fmt` and have a look through the existing tests that changed.

---

_Renamed from "[formatter] Fix missing blank lines before decorated classes in .pyi files (#18865)" to "[formatter] Fix missing blank lines before decorated classes in .pyi files" by @MichaReiser on 2025-06-24 14:09_

---

_@MichaReiser approved on 2025-06-24 14:11_

---

_Comment by @MichaReiser on 2025-06-24 14:15_

Thank you, this is great

---

_Comment by @github-actions[bot] on 2025-06-24 14:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+14 -0 lines in 5 files in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+14 -0 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/_frozen_importlib_external.pyi#L36'>stdlib/_frozen_importlib_external.pyi~L36</a>
```diff
     loader: LoaderProtocol | None = None,
     submodule_search_locations: list[str] | None = ...,
 ) -> importlib.machinery.ModuleSpec | None: ...
+
 @deprecated(
     "Deprecated as of Python 3.6: Use site configuration instead. "
     "Future versions of Python may not enable this finder by default."
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/abc.pyi#L29'>stdlib/abc.pyi~L29</a>
```diff
     def register(cls: ABCMeta, subclass: type[_T]) -> type[_T]: ...
 
 def abstractmethod(funcobj: _FuncT) -> _FuncT: ...
+
 @deprecated("Use 'classmethod' with 'abstractmethod' instead")
 class abstractclassmethod(classmethod[_T, _P, _R_co]):
     __isabstractmethod__: Literal[True]
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/os/__init__.pyi#L872'>stdlib/os/__init__.pyi~L872</a>
```diff
 def listdir(path: BytesPath) -> list[bytes]: ...
 @overload
 def listdir(path: int) -> list[str]: ...
+
 @final
 class DirEntry(Generic[AnyStr]):
     # This is what the scandir iterator yields
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/os/__init__.pyi#L945'>stdlib/os/__init__.pyi~L945</a>
```diff
 def getppid() -> int: ...
 def strerror(code: int, /) -> str: ...
 def umask(mask: int, /) -> int: ...
+
 @final
 class uname_result(structseq[str], tuple[str, str, str, str, str]):
     if sys.version_info >= (3, 10):
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/os/__init__.pyi#L1243'>stdlib/os/__init__.pyi~L1243</a>
```diff
     src: StrOrBytesPath, dst: StrOrBytesPath, *, src_dir_fd: int | None = None, dst_dir_fd: int | None = None
 ) -> None: ...
 def rmdir(path: StrOrBytesPath, *, dir_fd: int | None = None) -> None: ...
+
 @final
 class _ScandirIterator(Generic[AnyStr]):
     def __del__(self) -> None: ...
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/os/__init__.pyi#L1403'>stdlib/os/__init__.pyi~L1403</a>
```diff
     def spawnve(mode: int, path: StrOrBytesPath, argv: _ExecVArgs, env: _ExecEnv, /) -> int: ...
 
 def system(command: StrOrBytesPath) -> int: ...
+
 @final
 class times_result(structseq[float], tuple[float, float, float, float, float]):
     if sys.version_info >= (3, 10):
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/typing.pyi#L150'>stdlib/typing.pyi~L150</a>
```diff
 class _Final: ...
 
 def final(f: _T) -> _T: ...
+
 @final
 class TypeVar:
     @property
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stdlib/typing.pyi#L458'>stdlib/typing.pyi~L458</a>
```diff
 # Abstract base classes.
 
 def runtime_checkable(cls: _TC) -> _TC: ...
+
 @runtime_checkable
 class SupportsInt(Protocol, metaclass=ABCMeta):
     @abstractmethod
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L142'>stubs/gdb/gdb/__init__.pyi~L142</a>
```diff
 # Types
 
 def lookup_type(name: str, block: Block = ...) -> Type: ...
+
 @final
 class Type(Mapping[str, Field]):
     alignof: int
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L333'>stubs/gdb/gdb/__init__.pyi~L333</a>
```diff
 class Thread(threading.Thread): ...
 
 def selected_thread() -> InferiorThread: ...
+
 @final
 class InferiorThread:
     name: str | None
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L466'>stubs/gdb/gdb/__init__.pyi~L466</a>
```diff
 
 def current_progspace() -> Progspace | None: ...
 def progspaces() -> Sequence[Progspace]: ...
+
 @final
 class Progspace:
     executable_filename: str | None
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L489'>stubs/gdb/gdb/__init__.pyi~L489</a>
```diff
 def current_objfile() -> Objfile | None: ...
 def objfiles() -> list[Objfile]: ...
 def lookup_objfile(name: str, by_build_id: bool = ...) -> Objfile | None: ...
+
 @final
 class Objfile:
     filename: str | None
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L554'>stubs/gdb/gdb/__init__.pyi~L554</a>
```diff
 # Blocks
 
 def block_for_pc(pc: int) -> Block | None: ...
+
 @final
 class Block:
     start: int
```
<a href='https://github.com/python/typeshed/blob/14337eba537140fc449509daf4f0d32b8d00fb8a/stubs/gdb/gdb/__init__.pyi#L580'>stubs/gdb/gdb/__init__.pyi~L580</a>
```diff
 def lookup_global_symbol(name: str, domain: int = ...) -> Symbol | None: ...
 def lookup_static_symbol(name: str, domain: int = ...) -> Symbol | None: ...
 def lookup_static_symbols(name: str, domain: int = ...) -> list[Symbol]: ...
+
 @final
 class Symbol:
     type: Type | None
```

</p>
</details>




---

_Merged by @MichaReiser on 2025-06-24 14:25_

---

_Closed by @MichaReiser on 2025-06-24 14:25_

---
