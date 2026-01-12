```yaml
number: 6134
title: Implement PYI047
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI047
created_at: 2023-07-27T21:02:02Z
updated_at: 2023-07-29T00:43:44Z
url: https://github.com/astral-sh/ruff/pull/6134
synced_at: 2026-01-12T15:55:20Z
```

# Implement PYI047

---

_@LaBatata101_

## Summary

Checks for the presence of unused private `typing.TypeAlias` definitions.

ref #848 

## Test Plan

Snapshots and manual runs of flake8


---

_Comment by @github-actions[bot] on 2023-07-27 21:38_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+22, -0, 0 error(s))

<details><summary>bokeh (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/typings/selenium/webdriver/common/by.pyi#L7'>src/typings/selenium/webdriver/common/by.pyi:7:1:</a> PYI047 Private TypeAlias `_ByType` is never used
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/src/typings/selenium/webdriver/common/keys.pyi#L6'>src/typings/selenium/webdriver/common/keys.pyi:6:1:</a> PYI047 Private TypeAlias `_KeySeq` is never used
</pre>

</p>
</details>
<details><summary>typeshed (+20, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/_typeshed/__init__.pyi#L252'>stdlib/_typeshed/__init__.pyi:252:1:</a> PYI047 Private TypeAlias `_BufferWithLen` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/asyncio/__init__.pyi#L42'>stdlib/asyncio/__init__.pyi:42:5:</a> PYI047 Private TypeAlias `_AwaitableLike` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/asyncio/__init__.pyi#L43'>stdlib/asyncio/__init__.pyi:43:5:</a> PYI047 Private TypeAlias `_CoroutineLike` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/email/__init__.pyi#L8'>stdlib/email/__init__.pyi:8:1:</a> PYI047 Private TypeAlias `_ParamType` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/email/__init__.pyi#L9'>stdlib/email/__init__.pyi:9:1:</a> PYI047 Private TypeAlias `_ParamsType` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/lib2to3/pgen2/__init__.pyi#L9'>stdlib/lib2to3/pgen2/__init__.pyi:9:1:</a> PYI047 Private TypeAlias `_Convert` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stdlib/sys.pyi#L16'>stdlib/sys.pyi:16:1:</a> PYI047 Private TypeAlias `_OptExcInfo` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/PyYAML/yaml/cyaml.pyi#L14'>stubs/PyYAML/yaml/cyaml.pyi:14:1:</a> PYI047 Private TypeAlias `_CLoader` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/PyYAML/yaml/loader.pyi#L12'>stubs/PyYAML/yaml/loader.pyi:12:1:</a> PYI047 Private TypeAlias `_Loader` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/bleach/bleach/__init__.pyi#L20'>stubs/bleach/bleach/__init__.pyi:20:1:</a> PYI047 Private TypeAlias `_HTMLAttrKey` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/influxdb-client/influxdb_client/domain/write_precision.pyi#L5'>stubs/influxdb-client/influxdb_client/domain/write_precision.pyi:5:1:</a> PYI047 Private TypeAlias `_WritePrecision` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/keyboard/keyboard/_mouse_event.pyi#L5'>stubs/keyboard/keyboard/_mouse_event.pyi:5:1:</a> PYI047 Private TypeAlias `_MouseEvent` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/pika/pika/credentials.pyi#L9'>stubs/pika/pika/credentials.pyi:9:1:</a> PYI047 Private TypeAlias `_Credentials` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/pyinstaller/PyInstaller/building/__init__.pyi#L7'>stubs/pyinstaller/PyInstaller/building/__init__.pyi:7:1:</a> PYI047 Private TypeAlias `_PyiBlockCipher` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/python-xlib/Xlib/protocol/structs.pyi#L10'>stubs/python-xlib/Xlib/protocol/structs.pyi:10:1:</a> PYI047 Private TypeAlias `_Arc6IntSequence` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/python-xlib/Xlib/protocol/structs.pyi#L7'>stubs/python-xlib/Xlib/protocol/structs.pyi:7:1:</a> PYI047 Private TypeAlias `_RGB3IntIterable` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/python-xlib/Xlib/protocol/structs.pyi#L8'>stubs/python-xlib/Xlib/protocol/structs.pyi:8:1:</a> PYI047 Private TypeAlias `_Rectangle4IntSequence` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/python-xlib/Xlib/protocol/structs.pyi#L9'>stubs/python-xlib/Xlib/protocol/structs.pyi:9:1:</a> PYI047 Private TypeAlias `_Segment4IntSequence` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/tensorflow/tensorflow/keras/initializers.pyi#L40'>stubs/tensorflow/tensorflow/keras/initializers.pyi:40:1:</a> PYI047 Private TypeAlias `_Initializer` is never used
+ <a href='https://github.com/python/typeshed/blob/0d8a6bc2004b7b6d8335e35b017438125c305264/stubs/tensorflow/tensorflow/keras/regularizers.pyi#L13'>stubs/tensorflow/tensorflow/keras/regularizers.pyi:13:1:</a> PYI047 Private TypeAlias `_Regularizer` is never used
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI047 | 22 | 22 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.6±0.44ms     3.8 MB/sec    1.00     10.3±0.50ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.11ms     8.1 MB/sec    1.02      2.1±0.23ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   234.2±12.34µs    12.6 MB/sec    1.04   244.4±42.34µs    12.1 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.23ms     5.6 MB/sec    1.00      4.4±0.22ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.49ms     2.9 MB/sec    1.00     14.1±0.65ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.17ms     4.7 MB/sec    1.03      3.7±0.11ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   497.1±30.89µs     5.9 MB/sec    1.00   486.7±23.36µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.25ms     4.0 MB/sec    1.00      6.3±0.31ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.7±0.23ms     5.3 MB/sec    1.00      7.6±0.24ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1612.0±64.26µs    10.3 MB/sec    1.00  1565.1±86.35µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.7±8.45µs    16.1 MB/sec    1.01   184.7±12.29µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.15ms     7.6 MB/sec    1.03      3.5±0.15ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.06ms     4.0 MB/sec    1.08     11.0±0.06ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1949.1±18.11µs     8.5 MB/sec    1.07      2.1±0.05ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    201.9±1.87µs    14.6 MB/sec    1.04    210.8±2.52µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.03ms     5.9 MB/sec    1.07      4.6±0.05ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.10ms     3.0 MB/sec    1.00     13.6±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.01      3.7±0.02ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    362.8±4.55µs     8.1 MB/sec    1.01    364.9±6.26µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.04ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.03ms     5.3 MB/sec    1.00      7.7±0.06ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1514.1±19.24µs    11.0 MB/sec    1.00  1508.0±11.32µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    151.5±1.50µs    19.5 MB/sec    1.00    149.5±1.37µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.6 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-27 21:50_

Thank you!

Since all the patterns were reviewed in https://github.com/astral-sh/ruff/pull/6098 this LGTM

---

_@zanieb requested changes on 2023-07-27 21:52_

Unfortunately it looks like those typeshed ecoystem changes are false positives e.g. https://github.com/python/typeshed/blob/852882b8bfe9e1792448b94809351a392e83ce9e/stdlib/_ast.pyi#L381-L385

cc @charliermarsh 

---

_Renamed from "Implement PYI047" to "[`flake8-pyi`] Implement PYI047" by @LaBatata101 on 2023-07-27 22:00_

---

_Renamed from "[`flake8-pyi`] Implement PYI047" to "Implement PYI047" by @LaBatata101 on 2023-07-27 22:00_

---

_Comment by @charliermarsh on 2023-07-27 22:28_

Should be straightforward to fix. Will chime in on how later.

---

_Comment by @charliermarsh on 2023-07-28 02:08_

@LaBatata101 - Can you take a look at the refactor I did to the existing variants of these rules in https://github.com/astral-sh/ruff/pull/6142? In short, we should move them from the `bindings.rs` analyzer to the `deferred_scopes.rs` analyzer, so that they don't run on shadowed bindings.

---

_Label `rule` added by @MichaReiser on 2023-07-28 06:12_

---

_Comment by @LaBatata101 on 2023-07-28 21:02_

> @LaBatata101 - Can you take a look at the refactor I did to the existing variants of these rules in #6142? In short, we should move them from the `bindings.rs` analyzer to the `deferred_scopes.rs` analyzer, so that they don't run on shadowed bindings.

Looks great! Very good explanation too. 

I've already updated this PR and #6136 

---

_Comment by @zanieb on 2023-07-28 22:27_

It looks like the history is a little messed up here causing conflicts.

---

_Comment by @LaBatata101 on 2023-07-28 23:16_

> It looks like the history is a little messed up here causing conflicts.

Yeah... I don't really know what I did when I was pulling from upstream, everything went fine when I pulled on #6136.

I don't really know how to fix the history, I hope @charliermarsh can fix my mess 😅 

---

_Comment by @charliermarsh on 2023-07-28 23:43_

Haha. All good, I can figure out the history :)

---

_@charliermarsh approved on 2023-07-29 00:11_

---

_Comment by @charliermarsh on 2023-07-29 00:18_

Checked typeshed locally:

```console
/Users/crmarsh/workspace/typeshed/stubs/Pillow/PIL/_imaging.pyi:12:7: PYI046 Private protocol `_PixelAccessor` is never used
/Users/crmarsh/workspace/typeshed/stubs/bleach/bleach/callbacks.pyi:9:7: PYI046 Private protocol `_Callback` is never used
/Users/crmarsh/workspace/typeshed/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:26:7: PYI046 Private protocol `_HasTagAndText` is never used
/Users/crmarsh/workspace/typeshed/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:28:7: PYI046 Private protocol `_HasTagAndTextAndAttrib` is never used
Found 4 errors.
```

These all have `# noqa: Y046` on them, so they seem correct (since we use a different code for this rule).

---

_Merged by @charliermarsh on 2023-07-29 00:21_

---

_Closed by @charliermarsh on 2023-07-29 00:21_

---

_Branch deleted on 2023-07-29 00:33_

---
