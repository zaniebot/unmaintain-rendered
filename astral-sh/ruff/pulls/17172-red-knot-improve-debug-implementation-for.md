```yaml
number: 17172
title: "[red-knot] Improve `Debug` implementation for `semantic_index::SymbolTable`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/symboltable-debug
created_at: 2025-04-03T12:15:20Z
updated_at: 2025-04-03T12:54:04Z
url: https://github.com/astral-sh/ruff/pull/17172
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Improve `Debug` implementation for `semantic_index::SymbolTable`

---

_@AlexWaygood_

## Summary

`dbg!`ing a `SymbolTable` is currently very noisy due to the `symbols_by_name` field, which doesn't tell you very much at all. The noisiness makes debugging difficult. This PR removes the `symbols_by_name` field from the `Debug` implementation.

## Test Plan

`dbg!` output before of the `builtins.pyi` global-scope symbol table:

<details>

```
[crates/red_knot_python_semantic/src/symbol.rs:633:5] symbol_table(db, scope) = SymbolTable {
    symbols: [
        Symbol {
            name: Name("_ast"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_sitebuiltins"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_typeshed"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("sys"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("types"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("dict_items"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("dict_keys"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("dict_values"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("AnyStr_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConvertibleToFloat"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConvertibleToInt"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FileDescriptorOrPath"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OpenBinaryMode"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OpenBinaryModeReading"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OpenBinaryModeUpdating"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OpenBinaryModeWriting"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OpenTextMode"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ReadableBuffer"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsAdd"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsAiter"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsAnext"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsDivMod"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsFlush"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsIter"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsKeysAndGetItem"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsLenAndGetItem"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsNext"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsRAdd"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsRDivMod"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsRichComparison"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsRichComparisonT"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsWrite"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Awaitable"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Callable"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Iterable"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Iterator"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("MutableSet"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Reversible"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("AbstractSet"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Sized"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BufferedRandom"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BufferedReader"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BufferedWriter"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FileIO"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TextIOWrapper"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("CellType"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("CodeType"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TracebackType"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("IO"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Any"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BinaryIO"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ClassVar"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Generic"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Mapping"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("MutableMapping"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("MutableSequence"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Protocol"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Sequence"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsAbs"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsBytes"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsComplex"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsFloat"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SupportsIndex"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeVar"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("final"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("overload"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("type_check_only"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Concatenate"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Literal"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("LiteralString"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ParamSpec"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Self"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeAlias"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeGuard"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeIs"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeVarTuple"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("deprecated"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("GenericAlias"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_T"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("int"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_I"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T_contra"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_R_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_KT"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_VT"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_S"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T1"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T2"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T3"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T4"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_T5"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_SupportsNextT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_SupportsAnextT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_AwaitableT"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_AwaitableT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_P"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_StartT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_StopT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_StepT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("object"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("staticmethod"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("classmethod"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("type"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("super"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_PositiveInteger"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_NegativeInteger"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_LiteralInteger"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("float"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("complex"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_FormatMapMapping"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_TranslateTable"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("str"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("bytes"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("bytearray"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_IntegerFormats"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("memoryview"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("bool"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("slice"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("tuple"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("function"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("list"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("dict"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("set"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("frozenset"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("enumerate"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("range"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("property"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_NotImplementedType"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("NotImplemented"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("abs"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("all"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("any"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ascii"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("bin"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("breakpoint"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("callable"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("chr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_PathLike"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("aiter"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsSynchronousAnext"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("anext"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("compile"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("copyright"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("credits"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("delattr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("dir"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("divmod"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("eval"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("exec"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("exit"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("filter"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("format"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("getattr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("globals"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("hasattr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("hash"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("help"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("hex"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("id"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("input"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_GetItemIterable"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("iter"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_ClassInfo"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("isinstance"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("issubclass"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("len"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("license"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("locals"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("map"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("max"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("min"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("next"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("oct"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_Opener"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("open"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ord"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsWriteAndFlush"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("print"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_E_contra"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_M_contra"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_SupportsPow2"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsPow3NoneOnly"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsPow3"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsSomeKindOfPow"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("pow"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("quit"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("reversed"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("repr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsRound1"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsRound2"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("round"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("setattr"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("sorted"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_AddableT1"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_AddableT2"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_SupportsSumWithNoDefaultGiven"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_SupportsSumNoDefaultT"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("sum"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("vars"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("zip"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("__import__"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("__build_class__"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("EllipsisType"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ellipsis"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Ellipsis"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BaseException"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("GeneratorExit"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("KeyboardInterrupt"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SystemExit"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Exception"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("StopIteration"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OSError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("EnvironmentError"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("IOError"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("WindowsError"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("ArithmeticError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("AssertionError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("AttributeError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BufferError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("EOFError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ImportError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("LookupError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("MemoryError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("NameError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ReferenceError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("RuntimeError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("StopAsyncIteration"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SyntaxError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SystemError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TypeError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ValueError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FloatingPointError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("OverflowError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ZeroDivisionError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ModuleNotFoundError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("IndexError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("KeyError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnboundLocalError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BlockingIOError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ChildProcessError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConnectionError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BrokenPipeError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConnectionAbortedError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConnectionRefusedError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ConnectionResetError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FileExistsError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FileNotFoundError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("InterruptedError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("IsADirectoryError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("NotADirectoryError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("PermissionError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ProcessLookupError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TimeoutError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("NotImplementedError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("RecursionError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("IndentationError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("TabError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnicodeError"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnicodeDecodeError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnicodeEncodeError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnicodeTranslateError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("Warning"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UserWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("DeprecationWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("SyntaxWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("RuntimeWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("FutureWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("PendingDeprecationWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ImportWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("UnicodeWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("BytesWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ResourceWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("EncodingWarning"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("_BaseExceptionT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_BaseExceptionT"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_ExceptionT_co"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND,
            ),
        },
        Symbol {
            name: Name("_ExceptionT"),
            flags: SymbolFlags(
                IS_BOUND,
            ),
        },
        Symbol {
            name: Name("BaseExceptionGroup"),
            flags: SymbolFlags(
                IS_USED | IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("ExceptionGroup"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
        Symbol {
            name: Name("PythonFinalizationError"),
            flags: SymbolFlags(
                IS_BOUND | IS_DECLARED,
            ),
        },
    ],
    symbols_by_name: {
        ScopedSymbolId(
            235,
        ): (),
        ScopedSymbolId(
            267,
        ): (),
        ScopedSymbolId(
            86,
        ): (),
        ScopedSymbolId(
            22,
        ): (),
        ScopedSymbolId(
            182,
        ): (),
        ScopedSymbolId(
            213,
        ): (),
        ScopedSymbolId(
            252,
        ): (),
        ScopedSymbolId(
            177,
        ): (),
        ScopedSymbolId(
            173,
        ): (),
        ScopedSymbolId(
            108,
        ): (),
        ScopedSymbolId(
            122,
        ): (),
        ScopedSymbolId(
            118,
        ): (),
        ScopedSymbolId(
            197,
        ): (),
        ScopedSymbolId(
            69,
        ): (),
        ScopedSymbolId(
            134,
        ): (),
        ScopedSymbolId(
            52,
        ): (),
        ScopedSymbolId(
            248,
        ): (),
        ScopedSymbolId(
            168,
        ): (),
        ScopedSymbolId(
            2,
        ): (),
        ScopedSymbolId(
            129,
        ): (),
        ScopedSymbolId(
            5,
        ): (),
        ScopedSymbolId(
            18,
        ): (),
        ScopedSymbolId(
            150,
        ): (),
        ScopedSymbolId(
            9,
        ): (),
        ScopedSymbolId(
            16,
        ): (),
        ScopedSymbolId(
            205,
        ): (),
        ScopedSymbolId(
            246,
        ): (),
        ScopedSymbolId(
            68,
        ): (),
        ScopedSymbolId(
            93,
        ): (),
        ScopedSymbolId(
            189,
        ): (),
        ScopedSymbolId(
            161,
        ): (),
        ScopedSymbolId(
            64,
        ): (),
        ScopedSymbolId(
            124,
        ): (),
        ScopedSymbolId(
            229,
        ): (),
        ScopedSymbolId(
            94,
        ): (),
        ScopedSymbolId(
            202,
        ): (),
        ScopedSymbolId(
            67,
        ): (),
        ScopedSymbolId(
            120,
        ): (),
        ScopedSymbolId(
            219,
        ): (),
        ScopedSymbolId(
            12,
        ): (),
        ScopedSymbolId(
            20,
        ): (),
        ScopedSymbolId(
            79,
        ): (),
        ScopedSymbolId(
            11,
        ): (),
        ScopedSymbolId(
            157,
        ): (),
        ScopedSymbolId(
            216,
        ): (),
        ScopedSymbolId(
            231,
        ): (),
        ScopedSymbolId(
            239,
        ): (),
        ScopedSymbolId(
            140,
        ): (),
        ScopedSymbolId(
            36,
        ): (),
        ScopedSymbolId(
            13,
        ): (),
        ScopedSymbolId(
            184,
        ): (),
        ScopedSymbolId(
            204,
        ): (),
        ScopedSymbolId(
            70,
        ): (),
        ScopedSymbolId(
            259,
        ): (),
        ScopedSymbolId(
            96,
        ): (),
        ScopedSymbolId(
            111,
        ): (),
        ScopedSymbolId(
            72,
        ): (),
        ScopedSymbolId(
            247,
        ): (),
        ScopedSymbolId(
            101,
        ): (),
        ScopedSymbolId(
            242,
        ): (),
        ScopedSymbolId(
            261,
        ): (),
        ScopedSymbolId(
            263,
        ): (),
        ScopedSymbolId(
            214,
        ): (),
        ScopedSymbolId(
            62,
        ): (),
        ScopedSymbolId(
            166,
        ): (),
        ScopedSymbolId(
            244,
        ): (),
        ScopedSymbolId(
            257,
        ): (),
        ScopedSymbolId(
            133,
        ): (),
        ScopedSymbolId(
            112,
        ): (),
        ScopedSymbolId(
            87,
        ): (),
        ScopedSymbolId(
            90,
        ): (),
        ScopedSymbolId(
            117,
        ): (),
        ScopedSymbolId(
            158,
        ): (),
        ScopedSymbolId(
            162,
        ): (),
        ScopedSymbolId(
            230,
        ): (),
        ScopedSymbolId(
            154,
        ): (),
        ScopedSymbolId(
            255,
        ): (),
        ScopedSymbolId(
            35,
        ): (),
        ScopedSymbolId(
            39,
        ): (),
        ScopedSymbolId(
            138,
        ): (),
        ScopedSymbolId(
            190,
        ): (),
        ScopedSymbolId(
            21,
        ): (),
        ScopedSymbolId(
            66,
        ): (),
        ScopedSymbolId(
            181,
        ): (),
        ScopedSymbolId(
            7,
        ): (),
        ScopedSymbolId(
            236,
        ): (),
        ScopedSymbolId(
            251,
        ): (),
        ScopedSymbolId(
            152,
        ): (),
        ScopedSymbolId(
            227,
        ): (),
        ScopedSymbolId(
            78,
        ): (),
        ScopedSymbolId(
            55,
        ): (),
        ScopedSymbolId(
            61,
        ): (),
        ScopedSymbolId(
            253,
        ): (),
        ScopedSymbolId(
            47,
        ): (),
        ScopedSymbolId(
            65,
        ): (),
        ScopedSymbolId(
            153,
        ): (),
        ScopedSymbolId(
            104,
        ): (),
        ScopedSymbolId(
            74,
        ): (),
        ScopedSymbolId(
            107,
        ): (),
        ScopedSymbolId(
            149,
        ): (),
        ScopedSymbolId(
            98,
        ): (),
        ScopedSymbolId(
            155,
        ): (),
        ScopedSymbolId(
            169,
        ): (),
        ScopedSymbolId(
            180,
        ): (),
        ScopedSymbolId(
            237,
        ): (),
        ScopedSymbolId(
            146,
        ): (),
        ScopedSymbolId(
            15,
        ): (),
        ScopedSymbolId(
            243,
        ): (),
        ScopedSymbolId(
            17,
        ): (),
        ScopedSymbolId(
            136,
        ): (),
        ScopedSymbolId(
            80,
        ): (),
        ScopedSymbolId(
            44,
        ): (),
        ScopedSymbolId(
            228,
        ): (),
        ScopedSymbolId(
            60,
        ): (),
        ScopedSymbolId(
            245,
        ): (),
        ScopedSymbolId(
            193,
        ): (),
        ScopedSymbolId(
            264,
        ): (),
        ScopedSymbolId(
            268,
        ): (),
        ScopedSymbolId(
            58,
        ): (),
        ScopedSymbolId(
            258,
        ): (),
        ScopedSymbolId(
            279,
        ): (),
        ScopedSymbolId(
            113,
        ): (),
        ScopedSymbolId(
            135,
        ): (),
        ScopedSymbolId(
            240,
        ): (),
        ScopedSymbolId(
            85,
        ): (),
        ScopedSymbolId(
            186,
        ): (),
        ScopedSymbolId(
            100,
        ): (),
        ScopedSymbolId(
            187,
        ): (),
        ScopedSymbolId(
            106,
        ): (),
        ScopedSymbolId(
            73,
        ): (),
        ScopedSymbolId(
            223,
        ): (),
        ScopedSymbolId(
            49,
        ): (),
        ScopedSymbolId(
            83,
        ): (),
        ScopedSymbolId(
            77,
        ): (),
        ScopedSymbolId(
            43,
        ): (),
        ScopedSymbolId(
            274,
        ): (),
        ScopedSymbolId(
            46,
        ): (),
        ScopedSymbolId(
            151,
        ): (),
        ScopedSymbolId(
            217,
        ): (),
        ScopedSymbolId(
            178,
        ): (),
        ScopedSymbolId(
            278,
        ): (),
        ScopedSymbolId(
            132,
        ): (),
        ScopedSymbolId(
            6,
        ): (),
        ScopedSymbolId(
            174,
        ): (),
        ScopedSymbolId(
            99,
        ): (),
        ScopedSymbolId(
            196,
        ): (),
        ScopedSymbolId(
            109,
        ): (),
        ScopedSymbolId(
            128,
        ): (),
        ScopedSymbolId(
            123,
        ): (),
        ScopedSymbolId(
            102,
        ): (),
        ScopedSymbolId(
            116,
        ): (),
        ScopedSymbolId(
            172,
        ): (),
        ScopedSymbolId(
            32,
        ): (),
        ScopedSymbolId(
            105,
        ): (),
        ScopedSymbolId(
            241,
        ): (),
        ScopedSymbolId(
            95,
        ): (),
        ScopedSymbolId(
            206,
        ): (),
        ScopedSymbolId(
            209,
        ): (),
        ScopedSymbolId(
            198,
        ): (),
        ScopedSymbolId(
            81,
        ): (),
        ScopedSymbolId(
            170,
        ): (),
        ScopedSymbolId(
            171,
        ): (),
        ScopedSymbolId(
            8,
        ): (),
        ScopedSymbolId(
            276,
        ): (),
        ScopedSymbolId(
            1,
        ): (),
        ScopedSymbolId(
            212,
        ): (),
        ScopedSymbolId(
            222,
        ): (),
        ScopedSymbolId(
            33,
        ): (),
        ScopedSymbolId(
            144,
        ): (),
        ScopedSymbolId(
            194,
        ): (),
        ScopedSymbolId(
            125,
        ): (),
        ScopedSymbolId(
            89,
        ): (),
        ScopedSymbolId(
            38,
        ): (),
        ScopedSymbolId(
            51,
        ): (),
        ScopedSymbolId(
            19,
        ): (),
        ScopedSymbolId(
            163,
        ): (),
        ScopedSymbolId(
            0,
        ): (),
        ScopedSymbolId(
            211,
        ): (),
        ScopedSymbolId(
            3,
        ): (),
        ScopedSymbolId(
            226,
        ): (),
        ScopedSymbolId(
            121,
        ): (),
        ScopedSymbolId(
            148,
        ): (),
        ScopedSymbolId(
            232,
        ): (),
        ScopedSymbolId(
            262,
        ): (),
        ScopedSymbolId(
            260,
        ): (),
        ScopedSymbolId(
            91,
        ): (),
        ScopedSymbolId(
            270,
        ): (),
        ScopedSymbolId(
            269,
        ): (),
        ScopedSymbolId(
            225,
        ): (),
        ScopedSymbolId(
            191,
        ): (),
        ScopedSymbolId(
            115,
        ): (),
        ScopedSymbolId(
            28,
        ): (),
        ScopedSymbolId(
            220,
        ): (),
        ScopedSymbolId(
            164,
        ): (),
        ScopedSymbolId(
            250,
        ): (),
        ScopedSymbolId(
            137,
        ): (),
        ScopedSymbolId(
            141,
        ): (),
        ScopedSymbolId(
            24,
        ): (),
        ScopedSymbolId(
            54,
        ): (),
        ScopedSymbolId(
            45,
        ): (),
        ScopedSymbolId(
            188,
        ): (),
        ScopedSymbolId(
            75,
        ): (),
        ScopedSymbolId(
            40,
        ): (),
        ScopedSymbolId(
            234,
        ): (),
        ScopedSymbolId(
            30,
        ): (),
        ScopedSymbolId(
            41,
        ): (),
        ScopedSymbolId(
            127,
        ): (),
        ScopedSymbolId(
            185,
        ): (),
        ScopedSymbolId(
            145,
        ): (),
        ScopedSymbolId(
            23,
        ): (),
        ScopedSymbolId(
            238,
        ): (),
        ScopedSymbolId(
            143,
        ): (),
        ScopedSymbolId(
            167,
        ): (),
        ScopedSymbolId(
            110,
        ): (),
        ScopedSymbolId(
            25,
        ): (),
        ScopedSymbolId(
            31,
        ): (),
        ScopedSymbolId(
            57,
        ): (),
        ScopedSymbolId(
            195,
        ): (),
        ScopedSymbolId(
            221,
        ): (),
        ScopedSymbolId(
            218,
        ): (),
        ScopedSymbolId(
            37,
        ): (),
        ScopedSymbolId(
            71,
        ): (),
        ScopedSymbolId(
            50,
        ): (),
        ScopedSymbolId(
            176,
        ): (),
        ScopedSymbolId(
            179,
        ): (),
        ScopedSymbolId(
            200,
        ): (),
        ScopedSymbolId(
            266,
        ): (),
        ScopedSymbolId(
            277,
        ): (),
        ScopedSymbolId(
            119,
        ): (),
        ScopedSymbolId(
            84,
        ): (),
        ScopedSymbolId(
            114,
        ): (),
        ScopedSymbolId(
            165,
        ): (),
        ScopedSymbolId(
            271,
        ): (),
        ScopedSymbolId(
            280,
        ): (),
        ScopedSymbolId(
            256,
        ): (),
        ScopedSymbolId(
            249,
        ): (),
        ScopedSymbolId(
            88,
        ): (),
        ScopedSymbolId(
            27,
        ): (),
        ScopedSymbolId(
            139,
        ): (),
        ScopedSymbolId(
            265,
        ): (),
        ScopedSymbolId(
            4,
        ): (),
        ScopedSymbolId(
            53,
        ): (),
        ScopedSymbolId(
            29,
        ): (),
        ScopedSymbolId(
            130,
        ): (),
        ScopedSymbolId(
            42,
        ): (),
        ScopedSymbolId(
            76,
        ): (),
        ScopedSymbolId(
            147,
        ): (),
        ScopedSymbolId(
            156,
        ): (),
        ScopedSymbolId(
            208,
        ): (),
        ScopedSymbolId(
            273,
        ): (),
        ScopedSymbolId(
            183,
        ): (),
        ScopedSymbolId(
            224,
        ): (),
        ScopedSymbolId(
            97,
        ): (),
        ScopedSymbolId(
            233,
        ): (),
        ScopedSymbolId(
            82,
        ): (),
        ScopedSymbolId(
            142,
        ): (),
        ScopedSymbolId(
            254,
        ): (),
        ScopedSymbolId(
            131,
        ): (),
        ScopedSymbolId(
            63,
        ): (),
        ScopedSymbolId(
            48,
        ): (),
        ScopedSymbolId(
            215,
        ): (),
        ScopedSymbolId(
            103,
        ): (),
        ScopedSymbolId(
            14,
        ): (),
        ScopedSymbolId(
            92,
        ): (),
        ScopedSymbolId(
            207,
        ): (),
        ScopedSymbolId(
            275,
        ): (),
        ScopedSymbolId(
            160,
        ): (),
        ScopedSymbolId(
            26,
        ): (),
        ScopedSymbolId(
            56,
        ): (),
        ScopedSymbolId(
            34,
        ): (),
        ScopedSymbolId(
            272,
        ): (),
        ScopedSymbolId(
            59,
        ): (),
        ScopedSymbolId(
            126,
        ): (),
        ScopedSymbolId(
            159,
        ): (),
        ScopedSymbolId(
            199,
        ): (),
        ScopedSymbolId(
            175,
        ): (),
        ScopedSymbolId(
            192,
        ): (),
        ScopedSymbolId(
            201,
        ): (),
        ScopedSymbolId(
            203,
        ): (),
        ScopedSymbolId(
            210,
        ): (),
        ScopedSymbolId(
            10,
        ): (),
    },
}
```

</details>


I checked that with this PR, the second field is gone from the debug output (I'd paste it in, but it goes over the GitHub comment length maximum).

---

_Label `red-knot` added by @AlexWaygood on 2025-04-03 12:15_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-03 12:15_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-03 12:15_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-03 12:15_

---

_Comment by @github-actions[bot] on 2025-04-03 12:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-04-03 12:36_

---

_Merged by @AlexWaygood on 2025-04-03 12:37_

---

_Closed by @AlexWaygood on 2025-04-03 12:37_

---

_Branch deleted on 2025-04-03 12:37_

---

_Comment by @MichaReiser on 2025-04-03 12:45_

Alex couldn't wait haha... @ntBre started the release. You may want to hold off merging other PRs

---

_Comment by @AlexWaygood on 2025-04-03 12:50_

Oh, sorry! Didn't see the Discord message until after I merged

---

_Comment by @ntBre on 2025-04-03 12:54_

:laughing: No worries, I think it's not as big of a deal for red-knot PRs anyway

---
