```yaml
number: 22327
title: "[ty] Add support for functional `namedtuple` creation"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: charlie/dyn-members
head: charlie/functional-namedtuple
created_at: 2026-01-01T13:23:44Z
updated_at: 2026-01-12T02:22:57Z
url: https://github.com/astral-sh/ruff/pull/22327
synced_at: 2026-01-12T02:26:21Z
```

# [ty] Add support for functional `namedtuple` creation

---

_Pull request opened by @charliermarsh on 2026-01-01 13:23_

## Summary

This PR is intended to demonstrate how the pattern established in https://github.com/astral-sh/ruff/pull/22291 generalizes to other class "kinds".

Closes https://github.com/astral-sh/ty/issues/1049.


---

_Label `ty` added by @charliermarsh on 2026-01-01 13:23_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 13:25_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-01 13:26_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2103 diagnostics
+ Found 2102 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2026-01-02 16:46_

(The conformance changes are good, but I'm still working through the ecosystem changes.)

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-02 16:49_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 2 | 7 |
| `invalid-argument-type` | 1 | 0 | 4 |
| `invalid-assignment` | 0 | 0 | 5 |
| `possibly-missing-attribute` | 3 | 0 | 1 |
| `invalid-await` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **7** | **3** | **19** |


**[Full report with detailed diff](https://a9531c63.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://a9531c63.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @charliermarsh on 2026-01-02 18:05_

I believe the `ddtrace` issues are real. They have a fallback assignment:

https://github.com/DataDog/dd-trace-py/blob/6602420478b59b2d3df24ed27218ce12365b0cb7/ddtrace/vendor/psutil/_pslinux.py#L269-L274

So they create `scputimes = namedtuple('scputimes', 'user system idle')(0.0, 0.0, 0.0)`, then later treat that as if it's a class rather than instance:

https://github.com/DataDog/dd-trace-py/blob/6602420478b59b2d3df24ed27218ce12365b0cb7/ddtrace/vendor/psutil/_pslinux.py#L547

---

_Comment by @charliermarsh on 2026-01-02 18:08_

The `alerta` issues are false positives, but I believe mypy at least also suffers from them.

Specifically, they set deferred defaults [here](https://github.com/alerta/alerta/blob/b7111d23ae9aefb5064ef107976f62e3836217b6/alerta/database/backends/mongodb/utils.py#L18-L19):

```python
Query = namedtuple('Query', ['where', 'sort', 'group'])
Query.__new__.__defaults__ = ({}, [('_id', 1)], 'status')  # type: ignore
```


---

_Comment by @charliermarsh on 2026-01-02 18:19_

SciPy is similar -- deferred `__defaults__`: https://github.com/scipy/scipy/blob/b9ae1430d33a36c2e95b51932963278c255c100d/scipy/optimize/_linprog_util.py#L15-L17.

---

_Comment by @charliermarsh on 2026-01-02 18:20_

I think the Spack errors are true positives though? They're essentially creating a mock: https://github.com/spack/spack/blob/59b4e440391d5f58a665041ca365d9feb5caf472/lib/spack/spack/test/directives.py#L117-L119

---

_Marked ready for review by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @carljm by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-04 18:16_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-08 00:49_

---

_Converted to draft by @charliermarsh on 2026-01-10 18:53_

---

_Comment by @charliermarsh on 2026-01-12 02:22_

(I think this change got ruined in the rebase -- we lost most of the import pieces. I'm reviving from old commits.)

---
