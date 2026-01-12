```yaml
number: 13002
title: "[`flake8-pyi`] Skip type annotations in `string-or-bytes-too-long` (`PYI053`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: pyi053-ignore-annotation
created_at: 2024-08-20T03:31:26Z
updated_at: 2024-08-20T10:54:02Z
url: https://github.com/astral-sh/ruff/pull/13002
synced_at: 2026-01-12T15:55:42Z
```

# [`flake8-pyi`] Skip type annotations in `string-or-bytes-too-long` (`PYI053`)

---

_@dylwil3_

PYI053 is meant to protect against default arguments being too lengthy, not, e.g., type annotations with long literal strings.

Closes #12995


---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-20 03:31_

---

_Comment by @github-actions[bot] on 2024-08-20 03:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -14 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11052:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11057:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11062:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11067:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:3422:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5868:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5873:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8355:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8360:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8689:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8694:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:918:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:923:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:928:</a> PYI062 Duplicate literal member `...`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 14 | 0 | 14 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -14 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -14 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11052:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11057:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11062:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11067:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:3422:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5868:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5873:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8355:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8360:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8689:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8694:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:918:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:923:</a> PYI062 Duplicate literal member `...`
- <a href='https://github.com/python/typeshed/blob/a7ff1be4394cef089dd62090a2e2ad10cc77cfe6/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:928:</a> PYI062 Duplicate literal member `...`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 14 | 0 | 14 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:62 on 2024-08-20 09:52_

I wondered if it might be better to be more targeted in our exclusions here, since there are relatively few valid annotations in stub files that would involve long string literals -- basically just `Literal` and `Annotated`, I think? We can detect if we're in a `typing.Literal` slice like this:

```suggestion
    if semantic.in_typing_literal() {
```

But we don't currently track in the semantic model whether we're in an `Annotated` slice in the same way, and I'm not sure if it's worth it to add that tracking just for this, as I'm not sure it would practically get us much.

TL;DR -- this seems good; thanks!

---

_@AlexWaygood approved on 2024-08-20 09:52_

Thanks!

---

_Label `bug` added by @AlexWaygood on 2024-08-20 09:53_

---

_Label `rule` added by @AlexWaygood on 2024-08-20 09:53_

---

_Renamed from "[flake8-pyi] Skip type annotations in `string-or-bytes-too-long (PYI053)`" to "[`flake8-pyi`] Skip type annotations in `string-or-bytes-too-long` (`PYI053`)" by @AlexWaygood on 2024-08-20 09:53_

---

_Merged by @AlexWaygood on 2024-08-20 09:53_

---

_Closed by @AlexWaygood on 2024-08-20 09:53_

---

_Branch deleted on 2024-08-20 10:54_

---
