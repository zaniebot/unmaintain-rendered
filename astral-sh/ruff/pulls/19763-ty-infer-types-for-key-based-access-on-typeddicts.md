```yaml
number: 19763
title: "[ty] Infer types for key-based access on TypedDicts"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/typeddict-getitem
created_at: 2025-08-05T13:02:06Z
updated_at: 2025-08-06T11:32:14Z
url: https://github.com/astral-sh/ruff/pull/19763
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Infer types for key-based access on TypedDicts

---

_@sharkdp_

## Summary

This PR adds type inference for key-based access on `TypedDict`s and a new diagnostic for invalid subscript accesses:

```py
class Person(TypedDict):
    name: str
    age: int | None

alice = Person(name="Alice", age=25)

reveal_type(alice["name"])  # revealed: str
reveal_type(alice["age"])  # revealed: int | None
```

And when you try to access a non-existing key:

<img width="655" height="257" alt="image" src="https://github.com/user-attachments/assets/98086090-a277-483a-b742-cf4b5ab9b513" />

part of https://github.com/astral-sh/ty/issues/154

## Test Plan

Updated Markdown tests

---

_Label `ty` added by @sharkdp on 2025-08-05 13:02_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-05 13:02_

---

_Comment by @github-actions[bot] on 2025-08-05 13:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 13:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
discord.py (https://github.com/Rapptz/discord.py)
- discord/soundboard.py:217:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/state.py:1800:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/state.py:1806:93: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/webhook/async_.py:764:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/webhook/async_.py:766:100: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 531 diagnostics
+ Found 526 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_config.py:160:44: error[invalid-key] TypedDict `ConfigDict` cannot be indexed with a key of type `str`
+ pydantic/_internal/_generate_schema.py:2762:21: error[unresolved-attribute] Type `DefinitionReferenceSchema & ~None` has no attribute `clear`
+ pydantic/_internal/_generate_schema.py:2769:21: error[unresolved-attribute] Type `DefinitionReferenceSchema & ~None` has no attribute `clear`
+ pydantic/deprecated/config.py:22:43: error[invalid-key] TypedDict `ConfigDict` cannot be indexed with a key of type `str`
+ pydantic/json_schema.py:2054:47: error[invalid-key] Invalid key access on TypedDict `IncExSeqSerSchema`: Unknown key "schema"
+ pydantic/json_schema.py:2054:47: error[invalid-key] Invalid key access on TypedDict `IncExDictSerSchema`: Unknown key "schema"
- Found 766 diagnostics
+ Found 772 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/netinfo.py:598:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | @Todo(Support for `TypedDict`)` is possibly unbound
+ cloudinit/netinfo.py:598:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | Unknown` is possibly unbound
- cloudinit/netinfo.py:611:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | @Todo(Support for `TypedDict`)` is possibly unbound
+ cloudinit/netinfo.py:611:25: warning[possibly-unbound-attribute] Attribute `get` on type `str | Unknown` is possibly unbound
+ cloudinit/sources/DataSourceVMware.py:1000:48: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1001:26: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1008:48: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1009:26: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1018:53: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
+ cloudinit/sources/DataSourceVMware.py:1028:53: error[invalid-key] TypedDict `Interface` cannot be indexed with a key of type `Literal[0]`
- Found 607 diagnostics
+ Found 613 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/optimize/optimize_reports/optimize_reports.py:503:9: error[invalid-argument-type] Argument to function `generate_pair_metrics` is incorrect: Expected `DataFrame`, found `Series[Any] | Unknown`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:514:91: error[invalid-argument-type] Argument to function `generate_all_periodic_breakdown_stats` is incorrect: Expected `list[Unknown]`, found `DataFrame`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:552:48: error[invalid-argument-type] Argument to function `calculate_trade_volume` is incorrect: Expected `list[dict[str, Any]]`, found `list[dict[Hashable, Any]]`
- Found 375 diagnostics
+ Found 378 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/setup_prometheus_adapter_config.py:734:8: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["seriesQuery"]` with `dict[Unknown, Unknown] | None`
+ paasta_tools/setup_prometheus_adapter_config.py:737:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:738:25: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:753:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ paasta_tools/setup_prometheus_adapter_config.py:772:22: warning[possibly-unbound-attribute] Attribute `get` on type `dict[Unknown, Unknown] | None` is possibly unbound
- Found 883 diagnostics
+ Found 888 diagnostics

altair (https://github.com/vega/altair)
+ tests/vegalite/v6/test_api.py:558:17: error[invalid-key] Invalid key access on TypedDict `_Value`: Unknown key "condition"
+ tests/vegalite/v6/test_api.py:604:32: error[invalid-key] Invalid key access on TypedDict `_Value`: Unknown key "condition"
- Found 1304 diagnostics
+ Found 1306 diagnostics

rclip (https://github.com/yurijmikhalevich/rclip)
+ rclip/main.py:38:13: error[invalid-key] TypedDict `ImageMeta` cannot be indexed with a key of type `str`
+ rclip/main.py:38:27: error[invalid-key] TypedDict `Image` cannot be indexed with a key of type `str`
- Found 12 diagnostics
+ Found 14 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/dependencies/dev.py:589:75: error[invalid-argument-type] Argument to function `version_compare_many` is incorrect: Expected `Iterable[str]`, found `str | None`
+ mesonbuild/interpreter/interpreter.py:846:65: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `bool | None`
+ mesonbuild/interpreter/interpreter.py:1779:98: error[invalid-key] Invalid key access on TypedDict `FindProgram`: Unknown key "version_argument"
+ mesonbuild/interpreter/interpreter.py:1820:29: error[invalid-key] Invalid key access on TypedDict `FuncDependency`: Unknown key "include_type"
+ mesonbuild/interpreter/interpreter.py:2286:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "args"
+ mesonbuild/interpreter/interpreter.py:2291:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "protocol"
+ mesonbuild/interpreter/interpreter.py:2293:29: error[invalid-key] Invalid key access on TypedDict `BaseTest`: Unknown key "verbose"
+ mesonbuild/interpreter/interpreter.py:2336:19: error[invalid-key] Invalid key access on TypedDict `FuncInstallHeaders`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2521:23: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2527:90: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "install_tag" - did you mean "install_dir"?
+ mesonbuild/interpreter/interpreter.py:2528:60: error[invalid-key] Invalid key access on TypedDict `FuncInstallData`: Unknown key "preserve_path"
+ mesonbuild/interpreter/interpreter.py:2595:32: error[invalid-key] Invalid key access on TypedDict `FuncInstallSubdir`: Unknown key "install_tag" - did you mean "install_dir"?
- mesonbuild/interpreter/interpreter.py:2704:47: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `@Todo(Support for `TypedDict`) | str | ExternalProgram`
+ mesonbuild/interpreter/interpreter.py:2704:47: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `str | ExternalProgram`
+ mesonbuild/interpreter/interpreter.py:2711:69: error[invalid-argument-type] Argument to function `do_conf_file` is incorrect: Expected `ConfigurationData`, found `(dict[str, str | int] & ~dict[Unknown, Unknown]) | ConfigurationData`
+ mesonbuild/interpreter/interpreter.py:2728:54: error[invalid-argument-type] Argument to function `dump_conf_header` is incorrect: Expected `ConfigurationData`, found `(dict[str, str | int] & ~dict[Unknown, Unknown]) | ConfigurationData`
+ mesonbuild/interpreter/interpreter.py:2729:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `used` on type `(dict[str, str | int] & ~dict[Unknown, Unknown]) | ConfigurationData`
+ mesonbuild/interpreter/interpreter.py:2741:47: error[invalid-argument-type] Argument to function `substitute_values` is incorrect: Expected `list[str | ExternalProgram]`, found `list[Executable | ExternalProgram | Compiler | File | str]`
- mesonbuild/interpreter/interpreter.py:2742:47: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `@Todo(Support for `TypedDict`) | str | ExternalProgram`
+ mesonbuild/interpreter/interpreter.py:2742:47: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `str | ExternalProgram`
- mesonbuild/interpreter/interpreter.py:2756:56: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `(@Todo(Support for `TypedDict`) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | (ExternalProgram & ~AlwaysTruthy & ~AlwaysFalsy)`
+ mesonbuild/interpreter/interpreter.py:2756:56: error[invalid-argument-type] Argument to function `bold` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy) | (ExternalProgram & ~AlwaysTruthy & ~AlwaysFalsy)`
+ mesonbuild/interpreter/interpreter.py:2763:21: error[invalid-key] Invalid key access on TypedDict `ConfigureFile`: Unknown key "copy"
+ mesonbuild/interpreter/interpreter.py:3418:24: error[invalid-key] Invalid key access on TypedDict `Executable`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3418:24: error[invalid-key] Invalid key access on TypedDict `StaticLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3418:24: error[invalid-key] Invalid key access on TypedDict `SharedLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3418:24: error[invalid-key] Invalid key access on TypedDict `SharedModule`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3418:24: error[invalid-key] Invalid key access on TypedDict `Jar`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3423:24: error[invalid-key] Invalid key access on TypedDict `Executable`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3423:24: error[invalid-key] Invalid key access on TypedDict `StaticLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3423:24: error[invalid-key] Invalid key access on TypedDict `SharedLibrary`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3423:24: error[invalid-key] Invalid key access on TypedDict `SharedModule`: Unknown key "language_args"
+ mesonbuild/interpreter/interpreter.py:3423:24: error[invalid-key] Invalid key access on TypedDict `Jar`: Unknown key "language_args"
+ mesonbuild/modules/cmake.py:331:109: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
+ mesonbuild/modules/cmake.py:409:9: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `used` on type `ConfigurationData | (dict[Unknown, Unknown] & ~dict[Unknown, Unknown])`
- mesonbuild/modules/gnome.py:1554:55: warning[possibly-unbound-attribute] Attribute `get_target_dir` on type `Unknown | Backend | None` is possibly unbound
- mesonbuild/modules/gnome.py:1600:24: warning[possibly-unbound-attribute] Attribute `get_executable_serialisation` on type `Unknown | Backend | None` is possibly unbound
- mesonbuild/modules/pkgconfig.py:733:103: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `(@Todo(Support for `TypedDict`) & ~AlwaysFalsy) | None | Unknown | str`
+ mesonbuild/modules/pkgconfig.py:733:103: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None | Unknown`
+ mesonbuild/modules/python.py:291:34: error[invalid-key] Invalid key access on TypedDict `PyInstallKw`: Unknown key "preserve_path"
- Found 776 diagnostics
+ Found 804 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/code.py:406:37: error[invalid-argument-type] Argument to function `urlsafe` is incorrect: Expected `str`, found `str | None`
+ openlibrary/plugins/worksearch/code.py:410:13: warning[possibly-unbound-attribute] Attribute `split` on type `str | None` is possibly unbound
+ openlibrary/plugins/worksearch/code.py:454:33: error[invalid-key] TypedDict `SolrDocument` cannot be indexed with a key of type `str`
- openlibrary/solr/updater/edition.py:346:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- openlibrary/solr/updater/edition.py:348:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 697 diagnostics
+ Found 698 diagnostics

zulip (https://github.com/zulip/zulip)
+ corporate/views/support.py:582:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Realm | None`, found `Self@Model`
+ corporate/views/support.py:894:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteRealm`, found `Self@Model`
+ corporate/views/support.py:894:65: warning[possibly-unresolved-reference] Name `remote_realm` used when possibly not defined
+ corporate/views/support.py:902:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RemoteZulipServer`, found `Self@Model`
+ corporate/views/support.py:902:66: warning[possibly-unresolved-reference] Name `remote_server` used when possibly not defined
- Found 7420 diagnostics
+ Found 7425 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~76MB
+     memo metadata = ~80MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-08-05 13:12_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 37 | 0 | 0 |
| `invalid-argument-type` | 13 | 0 | 4 |
| `unused-ignore-comment` | 0 | 7 | 0 |
| `possibly-unbound-attribute` | 2 | 2 | 2 |
| `non-subscriptable` | 3 | 0 | 0 |
| `invalid-assignment` | 2 | 0 | 0 |
| `possibly-unresolved-reference` | 2 | 0 | 0 |
| `unresolved-attribute` | 2 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **62** | **9** | **6** |

**[Full report with detailed diff](https://david-typeddict-getitem.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-05 14:36_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-05 14:36_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-05 19:37_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-05 19:37_

---

_@sharkdp reviewed on 2025-08-05 19:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/Cargo.toml`:51 on 2025-08-05 19:39_

I know that we had a custom Levenshtein implementation in https://github.com/astral-sh/ruff/pull/18705, but that was removed again. And `strsim` is already a transitive dependency for the CLI version of `ty` at least — via `clap`.

Happy to replace that with something else though.

---

_@sharkdp reviewed on 2025-08-05 19:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:2622 on 2025-08-05 19:41_

I copied this function more or less 1:1 from another project of mine. It's not like I have a huge amount of experience with the heuristic here, but I did some iterations on it and it usually gives decent results.

---

_@sharkdp reviewed on 2025-08-05 19:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:166 on 2025-08-05 19:43_

We can see a few ecosystem hits due to this behavior. But it's relatively clear in the spec that this should be disallowed:

> A key that is not a literal should generally be rejected, since its value is unknown during type checking, and thus can cause some of the above violations.

> Type checkers should allow [final names](https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final) with string values to be used instead of string literals in operations on TypedDict objects.

> Similarly, an expression with a suitable [literal type](https://typing.python.org/en/latest/spec/literal.html#literal) can be used instead of a literal value

---

_@sharkdp reviewed on 2025-08-05 19:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:169 on 2025-08-05 19:45_

This is less clear. Maybe this should also be an error? If so, it would violate the gradual guarantee. So I'm inclined to say that this is fine?

---

_@sharkdp reviewed on 2025-08-05 19:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:41 on 2025-08-05 19:46_

I'm not 100% satisfied with the "concise" error message here, but they are constructed from the main message and the message on the primary annotation, so the design space is somewhat limited.

---

_@sharkdp reviewed on 2025-08-05 19:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8944 on 2025-08-05 19:47_

We could consider making this a query in the future. But right now, it's only used when constructing the diagnostic message.

---

_Comment by @github-actions[bot] on 2025-08-05 19:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1098 on 2025-08-05 19:48_

This is a bit unfortunate. It's extremely useful to reuse the dataclass-fields machinery, but obviously the naming fit is not great. And `Field` has a few attributes that are not relevant for `TypedDict` items. Will think about this... not really related to this PR.

---

_@sharkdp reviewed on 2025-08-05 19:48_

---

_@sharkdp reviewed on 2025-08-05 19:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1102 on 2025-08-05 19:48_

clippy forced me to rename this attribute, as it would otherwise start with the struct name..

---

_Marked ready for review by @sharkdp on 2025-08-05 19:52_

---

_Review requested from @carljm by @sharkdp on 2025-08-05 19:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-05 19:52_

---

_Review requested from @dcreager by @sharkdp on 2025-08-05 19:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-08-05 19:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:30 on 2025-08-05 19:59_

So.. all of these still don't work, because the type of `alice` here is simply `dict[Unknown, Unknown]`. See below for the real tests for this feature.

---

_@sharkdp reviewed on 2025-08-05 19:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/Cargo.toml`:51 on 2025-08-05 20:45_

I have no opinions here -- I'm in favor of whatever works and is implemented :)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:41 on 2025-08-05 20:48_

The message doesn't look too bad to me

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:248 on 2025-08-05 20:50_

I'm curious why this didn't automatically fall out from our existing Place support -- I'd have thought we'd see the assignment to `td["x"]` as a `Definition`. Did you happen to look at that at all yet? It's fine for it to be added later, just curious if you know more.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:166 on 2025-08-05 20:55_

Yeah, [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=c2518c2bc32d41fbbe72afb2d5a7669a) follows the strict spec here; [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgCYAiSAxjADRQBiqAhgDb0AySMxIb9AQRRwAsAChxVVswDOMqAAVeMsCgAURBGUo0AlAC5xUY1BTMIxfVBkwQRk8zSXMKfAB8oAOVXFx4zwIAsgCiAPoMAJIBHFZMZqxQALxQAERmFiniAgDiYZHRsSysANpcPHwlKY7EKQC6tUmp1ZkSYqTEwFChalogKihWSn2q9KzcvGyhANbEcFZlE5XNtfQArihIqqFgwNOzMvPjFcVVTin0aeY1K9a2e3O3IGsoUyhgAO4o91ZCcLpQAFoAHxeHyGMQmKAgYgAN2Ik3gWh6ylUJ3S13%2BUAAxFDYfDWGQrDY7BCTNC4QiSMjhigTstMTjyfjCS53KCUL5WpCmZSkb1%2BsUAiFwlEBBxagzcRSCaQibZ7MYeaxQojiNSBTk8qLxZKlSzUGzvBy-KTFXjeWr%2BaixuVJjM4BLjIzzTKrAaoB4jZyFVL8SqqVbaetNl8dvcZI7sb62PrXB72cR48STZCcbxwCArMVUDC2EhSAD7Q0UhEULmxqQoPaoMwqFRiHIoKpCCQKNR8AADIb9DtWACqLzenyrs1Sby%2BxAAHkgbKg0CkWtyXf6%2BSjaWktlOZzA53VdS6WQPXh8UCmTGmQBmszm8wWi6lNNp21Au2uO1AqMwUKYwPgAEaJqg7STmQUDvNwAAWNYjnATadKqL7Eh2i5ksuqrqqixL3JGzrSoeg4nkAA) made the decision to be more lenient. I think it makes sense to start with what the spec says, and see how much pushback we get in practice. Looks like [pyrefly](https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgCYAiSAxjADRQBiqAhgDb0AySMxIb9AQRRx6IYgDdibAPrwExAFAKqrZgGc1UAAq81YFAAoi8itRgBKAFwKotqCmYRilqGpggbd5mmeYU+AB8oADl9RQVggQBZAFFpBgBJSI4XJgdWKABeKAAiBycchQEAcTjE5NSWVgBtLh4+GpzvYhyAXVas3ObChVJiYChpA3kQPRQXHVH9elZuXhkAa2I4Fzr5xu7W+gBXFCR9aTBgaSW4NVW5huqmnxz6PMcWrdd3E+WXNxAdlAWUMAB3FBvFZQIRwcxQAC0AD4QmFrHYoGJJDI5MRhrp9Nd8k8IVAAMRIiRSVhkD7uTy2ZEk2QkDFTFDXTZ4wnUthkvyBOEocKItmsWnyeljaqRWLxJICDitFlElGk0jkjx84moukjEUlMqS6Wy-kc1Bc0I8pQq+WC9EarGzeqLZYy2ys1UKlyGqBBY28uz8i3CrG7fZAo7AtQOglykkG-zu7nEGOfU12Qm8cAgFzVVDiNhIUiQ04dHIJFBZ2akKCnKDMKhUYgaKD6QgkUw0KAAA0mY1bLgAqj8-oDy8tcn8gcQAB5INyoNA5Qpmmlov2MvIHceTmDTtp650c3u-AEoROOqAp3DpzPZ3P53LGMiUFvtzEoVtQKjMFD2MD4ABGcdQfTHMgoH+bgAAtK0HOB6wGNE20+Vs529Z1fStRlPmBMMnXlXc+wPBQgA) is doing the same.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:169 on 2025-08-05 20:56_

I agree this should not be an error; the `Any` could materialize to a valid literal key.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:382 on 2025-08-05 20:57_

The suggestions feature is great!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:59 on 2025-08-05 21:00_

nit: if this will be `pub(crate)` and used elsewhere in the crate, then it probably belongs up in `src/lib.rs` alongside `FxOrderSet`, `FxIndexMap`, and `FxIndexSet`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1098 on 2025-08-05 21:01_

The name seems tolerable to me; the wasted fields are not ideal but seems reasonable to follow up on this separately.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2054 on 2025-08-05 21:05_

I like the synthesized-overloads approach a lot. I seem to remember @erictraut mentioning before that neither pyright nor mypy implement TypedDict `__getitem__` or `__setitem__` by synthesizing overloads. I'm curious if you've looked at the more advanced TypedDict features we have yet to implement, and considered whether this approach will be able to handle those features also?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:2642 on 2025-08-05 21:08_

nit: couldn't we filter out very short `existing_names` earlier, before we actually do the Levenshtein calculation for them?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8897 on 2025-08-05 21:12_

```suggestion
                                        "TypedDict `{}` cannot be indexed with a key of type `{}`",
```

---

_@carljm approved on 2025-08-05 21:13_

Looks great, thank you!!

---

_@sharkdp reviewed on 2025-08-06 06:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:248 on 2025-08-06 06:42_

oh! I somehow assumed this is just a pre-existing limitation and didn't really look at the nicely written text at the beginning of this section. Fixed now. We just need to consider `Type::TypedDict` to be "safely mutable".

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2054 on 2025-08-06 06:59_

I think that the other methods here (`get`, `update`, etc.) can be handled in a similar way to `__getitem__`.

For validating writes to attributes, we should be able to synthesize a similar overload set for `__setitem__`.

I do not yet have a good understanding of all `TypedDict` features to come (I merely read the docs/spec, and wrote a [list of all tasks](https://github.com/astral-sh/ty/issues/154#issue-3046358712)). So there might very well be things that can not be handled this way. But I currently don't see why that would be a reason not to solve those basic features with synthesized overloads.

My biggest worry is that this approach here does not scale well to large `TypedDict`s. Synthesizing N overloads is probably not that bad, but resolving calls to these overloads probably scales superlinear in N?

---

_@sharkdp reviewed on 2025-08-06 06:59_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-06 07:00_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-06 07:00_

---

_Comment by @sharkdp on 2025-08-06 07:35_

Looked again at almost all ecosystem changes and they're basically all true positives (a few known limitations unrelated to TypedDicts)!

---

_Merged by @sharkdp on 2025-08-06 07:36_

---

_Closed by @sharkdp on 2025-08-06 07:36_

---

_Branch deleted on 2025-08-06 07:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:382 on 2025-08-06 10:28_

+1!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2054 on 2025-08-06 10:28_

> My biggest worry is that this approach here does not scale well to large `TypedDict`s. Synthesizing N overloads is probably not that bad, but resolving calls to these overloads probably scales superlinear in N?

I think we just need to see how this goes. We can always change the approach later if it proves a problem in practice.

For `tuple.__getitem__`, I attempted to mitigate this problem by synthesizing the minimum number of overloads. This is done by combining them where possible. E.g. for `tuple[int, str, int]`, we "only" synthesize these overloads -- the overloads for the first element and the third element are combined:

```py
@overload
def __getitem__(self, index: Literal[0, 2, -1, -3], /) -> int: ...
@overload
def __getitem__(self, index: Literal[1, -2], /) -> str: ...
@overload
def __getitem__(self, index: SupportsIndex, /) -> int | str: ...
@overload
def __getitem__(self, index: slice, /) -> tuple[int | str, ...]: ...
```

You could do a similar thing for `TypedDict` `__getitem__` methods: for `Foo` in the following example, it looks like we synthesize two overloads, but really only one is required (they can be combined, since the value type is the same for both keys:

```py
from typing import TypedDict

class Foo(TypedDict):
    x: str
    y: str
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/Cargo.toml`:51 on 2025-08-06 10:34_

The advantage of going with the custom implementation in #18705 (which I think it would be fairly easy to bring back -- it's quite isolated as a module) is that Brent and I based it directly on the CPython implementation of this feature, and we brought in most of CPython's tests for this feature. CPython's implementation of this feature is very battle-tested at this point: it's been present in several stable releases of Python and initially received a large number of bug reports (which have since been fixed) regarding bad "Did you mean?" suggestions. So at this point I think we can be pretty confident that CPython's implementation is very well tuned for giving good suggestions for typos in Python code specifically.

Having said that, it's obviously nice for us to have to maintain less code, and exactly which Levenshtein implementation we go with probably isn't the most important issue for us right now :-)

---

_@AlexWaygood reviewed on 2025-08-06 10:34_

Nice!!

---

_@sharkdp reviewed on 2025-08-06 11:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2054 on 2025-08-06 11:32_

Yeah, that's a great optimization idea. And potentially more impactful than for tuples (as I would assume `TypedDict`s to have more items, on average). There's probably also a lot of `TypedDict`s out there where the majority of value types are just `int` or `str`.

What's not immediately clear to me is if this is an optimization at all? We'll spend more time when synthesizing the overloads, and we have to allocate an additional hashmap to group the items. And it's not obvious to me that this will even lead to faster overload resolution? It might be that creating / iterating over these union types is more expensive than a larger number of overloads with simple `Literal["key"]` argument types? I guess we'll need a micro-benchmark with a large `TypedDict`.

I wrote down two tasks in https://github.com/astral-sh/ty/issues/154

---
