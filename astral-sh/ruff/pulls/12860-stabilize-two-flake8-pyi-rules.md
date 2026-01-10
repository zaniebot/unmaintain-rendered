```yaml
number: 12860
title: "Stabilize two `flake8-pyi` rules"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.6
head: stabilize-two-flake8-pyi-rules
created_at: 2024-08-13T12:59:26Z
updated_at: 2024-08-13T15:14:31Z
url: https://github.com/astral-sh/ruff/pull/12860
synced_at: 2026-01-10T21:38:32Z
```

# Stabilize two `flake8-pyi` rules

---

_Pull request opened by @MichaReiser on 2024-08-13 12:59_

## Summary

Stabilize `PYI057` and `PYI62`.

The rules have been added more than 90 days ago and there are no outstanding issues (searching by rule name or code). 

It is a bit funny that the rules apply to both `pyi` and `py` files, considering that they are part of `flake8-pyi`.
They do match the behavior of the upstream rules and we can move the rules into a more appropriate group when doing the rule categorization.


## Test Plan

See ecosystem check



---

_Label `rule` added by @MichaReiser on 2024-08-13 12:59_

---

_Comment by @github-actions[bot] on 2024-08-13 13:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+16 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 Duplicate literal member `-1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stdlib/_collections_abc.pyi#L63'>stdlib/_collections_abc.pyi:63:24:</a> PYI057 Do not use `typing.ByteString`, which has unclear semantics and is deprecated
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11052:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11057:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11062:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:11067:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:3422:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5868:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:5873:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8355:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8360:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8689:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L1188'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:1188:8694:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:918:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:923:</a> PYI062 Duplicate literal member `...`
+ <a href='https://github.com/python/typeshed/blob/89bb3c148cc72884092eb1da51f34eb6f671f58b/stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi#L496'>stubs/tensorflow/tensorflow/core/protobuf/rewriter_config_pb2.pyi:496:928:</a> PYI062 Duplicate literal member `...`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 15 | 15 | 0 | 0 | 0 |
| PYI057 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/codes.rs`:782 on 2024-08-13 13:29_

I think this one has been around for a little less than 90 days -- it was merged on May 29 and released on May 31. But it's also a very simple and uncontroversial rule (IMO), so I'm okay with stabilising it a couple of weeks before we normally would 😄 it's not in our versioning policy that we have to wait 90 days

---

_@AlexWaygood approved on 2024-08-13 13:29_

---

_Comment by @AlexWaygood on 2024-08-13 13:31_

I think the ecoystem report has gone haywire because you changed the base branch of the PR after it had been opened. Could you try rebasing it? Or maybe closing and reopening it?

---

_Closed by @MichaReiser on 2024-08-13 13:32_

---

_Reopened by @MichaReiser on 2024-08-13 13:32_

---

_@MichaReiser reviewed on 2024-08-13 13:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:782 on 2024-08-13 13:33_

Lol, that's correct. I only saw *May* and was like, that's a long time ago. I did do the proper math for the other rules.

---

_Comment by @MichaReiser on 2024-08-13 14:14_

Hmm, I'm trying to reproduce the typeshed errors where the error message looks very off but without success so far

---

_Comment by @AlexWaygood on 2024-08-13 14:15_

> Hmm, I'm trying to reproduce the typeshed errors where the error message looks very off but without success so far

If you can't repro them locally, it's probably another case of https://github.com/astral-sh/ruff/issues/11305

---

_Comment by @MichaReiser on 2024-08-13 15:14_

Ohh, thanks for pointing that out. I did paste that code snippet into the test and I can't reproduce locally.

---

_Merged by @MichaReiser on 2024-08-13 15:14_

---

_Closed by @MichaReiser on 2024-08-13 15:14_

---

_Branch deleted on 2024-08-13 15:14_

---
