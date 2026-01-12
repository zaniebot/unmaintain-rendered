```yaml
number: 17353
title: "[red-knot] Initial support for `dataclass`es"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dataclasses
created_at: 2025-04-11T14:28:57Z
updated_at: 2025-04-15T18:33:07Z
url: https://github.com/astral-sh/ruff/pull/17353
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Initial support for `dataclass`es

---

_@sharkdp_

## Summary

Add very early support for dataclasses. This is mostly to make sure that we do not emit false positives on dataclass construction, but it also lies some foundations for future extensions.

This seems like a good initial step to merge to me, as it basically removes all false positives on dataclass constructor calls. This allows us to use the ecosystem checks for making sure we don't introduce new false positives as we continue to work on dataclasses.

ticket: https://github.com/astral-sh/ruff/issues/16651

## Ecosystem analysis

I re-ran the mypy_primer evaluation of [the  `__init__` PR](https://github.com/astral-sh/ruff/pull/16512) locally with our current mypy_primer version and project selection. It introduced 1597 new diagnostics. Filtering those by searching for `__init__` and rejecting those that contain `invalid-argument-type` (those could not possibly be solved by this PR) leaves 1281 diagnostics. The current version of this PR removes 1171 diagnostics, which leaves 110 unaccounted for. I extracted the lint + file path for all of these diagnostics and generated a diff (of diffs), to see which `__init__`-diagnostics remain. I looked at a subset of these: There are a lot of `SomeClass(*args)` calls where we don't understand the unpacking yet (this is not even related to `__init__`). Some others are related to `NamedTuple`, which we also don't support yet. And then there are some errors related to `@attrs.define`-decorated classes, which would probably require support for `dataclass_transform`, which I made no attempt to include in this PR.

## Test Plan

New Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-04-11 14:29_

---

_Comment by @github-actions[bot] on 2025-04-11 14:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_tokenizer.py:133:37: Too many positional arguments to bound method `__init__`: expected 0, got 3
- Found 318 diagnostics
+ Found 317 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:169:34: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:176:38: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:204:39: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:216:31: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/speedscope.py:225:42: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:247:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:252:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:257:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:262:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:267:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:272:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:277:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:282:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:287:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:292:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- Found 290 diagnostics
+ Found 275 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:76:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:77:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:78:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:79:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:80:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:81:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:84:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:85:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:86:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:87:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:90:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:91:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:92:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:93:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:94:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:97:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:98:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:99:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:100:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:103:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:104:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:105:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:106:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:107:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:110:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:111:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:112:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:113:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:121:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:122:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:125:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:126:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:127:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:128:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:129:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:132:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:133:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:134:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:135:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:136:13: Argument `install_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:137:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:138:13: Argument `supported_platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:141:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:142:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:147:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:148:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:151:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:152:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:153:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:154:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:155:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:158:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:159:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:160:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:161:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:162:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:163:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:166:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:167:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:168:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:169:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:170:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:173:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:174:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:175:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:176:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:177:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:178:13: Argument `supported_platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:181:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:182:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:183:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:184:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:185:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:188:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:189:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:190:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:191:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:192:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:195:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:196:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:197:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:198:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:199:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:202:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:203:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:204:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:205:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:208:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:209:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:210:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:211:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:212:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:213:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:216:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:217:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:218:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:219:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:220:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:223:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:224:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:225:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:226:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:227:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:230:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:231:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:232:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:233:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:236:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:237:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:238:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:239:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:240:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:243:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:244:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:245:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:246:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:249:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:250:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:253:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:254:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:270:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:271:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:274:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:275:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:276:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:277:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:278:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:281:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:282:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:283:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:284:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:293:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:294:13: Argument `supported_platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:297:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:298:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:299:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:300:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:313:13: Argument `needs_mypy_plugins` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:314:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:315:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:318:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:319:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:320:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:321:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:322:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:325:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:326:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:327:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:328:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:331:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:332:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:333:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:334:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:337:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:338:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:339:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:340:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:343:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:344:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:345:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:346:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:347:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:350:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:351:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:352:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:353:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:356:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:357:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:358:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:359:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:362:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:363:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:364:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:365:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:366:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:369:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:370:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:371:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:372:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:381:13: Argument `needs_mypy_plugins` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:382:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:383:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:386:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:387:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:388:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:389:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:390:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:391:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:394:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:395:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:396:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:397:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:398:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:401:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:402:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:403:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:404:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:405:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:406:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:409:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:410:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:411:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:412:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:413:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:414:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:417:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:418:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:419:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:420:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:421:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:424:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:425:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:426:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:427:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:428:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:431:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:432:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:433:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:434:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:435:13: Argument `needs_mypy_plugins` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:436:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:439:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:440:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:441:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:442:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:443:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:444:13: Argument `supported_platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:447:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:448:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:449:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:450:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:451:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:454:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:455:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:456:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:457:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:460:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:461:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:462:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:463:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:466:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:467:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:468:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:469:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:470:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:473:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:474:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:475:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:476:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:479:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:480:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:481:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:482:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:483:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:486:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:487:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:488:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:489:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:490:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:493:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:494:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:495:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:496:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:499:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:500:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:501:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:502:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:503:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:506:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:507:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:508:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:509:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:510:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:513:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:514:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:515:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:516:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:517:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:520:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:521:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:522:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:523:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:526:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:527:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:528:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:529:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:532:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:533:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:534:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:535:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:536:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:539:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:540:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:541:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:542:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:545:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:546:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:547:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:548:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:549:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:552:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:553:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:554:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:555:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:556:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:557:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:560:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:561:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:562:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:563:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:566:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:567:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:568:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:569:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:572:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:573:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:574:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:575:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:576:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:579:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:580:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:581:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:582:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:583:13: Argument `cost` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:586:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:587:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:588:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:589:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:590:13: Argument `expected_mypy_success` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:593:13: Argument `location` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:594:13: Argument `mypy_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:595:13: Argument `pyright_cmd` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:596:13: Argument `deps` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/projects.py:597:13: Argument `expected_mypy_success` does not match any known par...*[Comment body truncated]*

---

_Renamed from "[red-knot] Skip constructor check for dataclasses" to "[red-knot] Initial support for `dataclass`es" by @sharkdp on 2025-04-14 19:11_

---

_Marked ready for review by @sharkdp on 2025-04-14 21:03_

---

_Review requested from @carljm by @sharkdp on 2025-04-14 21:03_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-14 21:03_

---

_Review requested from @dcreager by @sharkdp on 2025-04-14 21:03_

---

_@sharkdp reviewed on 2025-04-14 21:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:584 on 2025-04-14 21:24_

Mypy emits `"init" argument must be a True or False literal` in this case, Pyright just ignores it (falls back to the default value), which is what we also do at the moment. It does not seem particularly important.

```py
def _(flag: bool):
    @dataclass(order=flag)
    class A:
        x: int

    A(1) < A(2)  # supported or not?
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1695 on 2025-04-14 21:37_

This seems like it deserves a TODO.

There's already a similar clause for `Type::Callable` below with a TODO comment, we could place this one next to it (or even combine them).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:710 on 2025-04-14 21:45_

I guess this gets into handling inheritance, which is a TODO in the tests -- but I suspect this handling should go in `own_class_member` instead? Each separate class in an inheritance tree could have its own synthetic `__init__`, or regular `__init__`, and the right one should be discovered by MRO walk just like for any other attribute.

We should also ensure that this special handling only takes effect if there isn't an explicit `__init__` method present on the class already. Dataclasses won't override an explicit method with a synthetic one.

```py
>>> @dataclass
... class Person:
...     name: str
...     def __init__(self):
...         self.name = "Yo"
...
>>> p = Person("Foo")
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    p = Person("Foo")
TypeError: Person.__init__() takes 1 positional argument but 2 were given
>>> p = Person()
>>> p
Person(name='Yo')
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:159 on 2025-04-14 22:01_

This seems misleading, as this type does not represent the function `dataclasses.dataclass`, but rather some internal closure returned by it. The runtime says `<function dataclass.<locals>.wrap at 0x102892700>`.

I think the LSP folks would like us to avoid these angle-bracket names altogether, since they don't render nicely when syntax-highlighted as Python, but I'm not sure what alternative would work well here that would render nicely as Python. We may decide to just fallback to a generic callable signature type for this in the LSP?

For now maybe this would be less misleading:
```suggestion
            Type::DataclassDecorator(_) => f.write_str("<decorator produced by dataclasses.dataclass>"),
```



---

_@carljm approved on 2025-04-14 22:03_

Thank you!

---

_@sharkdp reviewed on 2025-04-15 07:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/display.rs`:159 on 2025-04-15 07:55_

> We may decide to just fallback to a generic callable signature type for this in the LSP?

That sounds reasonable, yes.

Using your suggestion for now.

---

_@sharkdp reviewed on 2025-04-15 08:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:710 on 2025-04-15 08:07_

> but I suspect this handling should go in own_class_member instead?

yes!

> We should also ensure that this special handling only takes effect if there isn't an explicit `__init__` method present on the class already. Dataclasses won't override an explicit method with a synthetic one.

Yes, I already have a test "Dataclass with custom `__init__` method" marked with To do.

---

_Merged by @sharkdp on 2025-04-15 08:39_

---

_Closed by @sharkdp on 2025-04-15 08:39_

---

_Branch deleted on 2025-04-15 08:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 17:04_

Rather than storing a field on `Class`, did you consider making `dataclass_metadata` a lazy Salsa-tracked method (and move the logic currently in `infer.rs` into that method), similar to our existing `Class::explicit_bases_query`? There might be lots of dataclasses in third-party modules that first-party code depends on, but I'm guessing we'd rarely see constructor calls to most of those classes in the first-party code we're actually checking. So most of the time we probably don't need to know whether a class is a dataclass or not. It's probably wasted computation (and memory) to check (and store) whether these are all dataclasses unless we actually see any constructor calls for these classes? (I wouldn't expect this to make a difference in our benchmarks because we don't have any dataclass-heavy benchmarks right now.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1743 on 2025-04-15 17:18_

interesting -- rather than creating a separate `Type` variant, I was assuming we'd do some more localised special-casing for `@dataclass` with arguments passed. Something like this:

```suggestion
            let decorator_ty = self.infer_decorator(decorator);
            if decorator_ty
                .into_function_literal()
                .is_some_and(|function| function.is_known(self.db(), KnownFunction::Dataclass))
            {
                dataclass_metadata = Some(DataclassMetadata::default());
                continue;
            }
            
            if let ast::Expr::Call(ast::ExprCall { func, arguments, range } = decorator {
                if self
                    .expression_ty(func)
                    .into_function_literal()
                    .is_some_and(|function| function.is_known(self.db(), KnownFunction::Dataclass)) {
                        dataclass_metadata = Some(DataclassMetadata::from_arguments(arguments));
                    }
                }
            }
```

The advantage would be that we wouldn't have to worry about all the questions everywhere such as "is this type a subtype of `DataclassMetadata`?", etc.

The disadvantage is that having a new `Type` variant means that we have a pretty good understanding of the `dataclass` function and its associated metadata even when the `dataclass()` call is long way removed from the class it's decorating. E.g. you can do things like this, and red-knot still understands that one of the classes has an autogenerated `__init__` while the other doesn't: https://playknot.ruff.rs/5876dcbd-aaa4-4dc7-a0f2-121052dcd277. And I think I _have_ seen issues at mypy where users have been upset that mypy doesn't support things like that. So it's pretty cool that we _do_ support this kind of thing!

It might be good to add some tests demonstrating this capability, though? 

---

_@AlexWaygood reviewed on 2025-04-15 17:18_

nice!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1743 on 2025-04-15 17:43_

I do think it's useful to support `my_dataclass = dataclasses.dataclass(...)` followed by later (even in a different module) use of `@my_dataclass`, and I agree we should add a test demonstrating that that works!

---

_@carljm reviewed on 2025-04-15 17:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 17:44_

I suspect that as we increase the accuracy of our modeling of dataclasses, we will have more and more cases where we do need to know whether a class is a dataclass or not (e.g. it affects inference of final attributes with an assignment on the class body, too), which may reduce the effectiveness of making that determination lazily?

---

_@carljm reviewed on 2025-04-15 17:44_

---

_@AlexWaygood reviewed on 2025-04-15 17:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 17:48_

Maybe! I'm not sure, which is why I was asking if it was considered ;)

I suppose another reason why `explicit_bases_query` is a lazy method -- which probably does not apply for dataclasses -- is to support self-referential class definitions such as `class str(Sequence[str]): ...` -- deferring inference of the bases helps avoid otherwise-unnecessary Salsa cycles.

---

_@sharkdp reviewed on 2025-04-15 18:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 18:05_

> which is why I was asking if it was considered

I did not consider this.

> So most of the time we probably don't need to know whether a class is a dataclass or not. It's probably wasted computation (and memory) to check (and store) whether these are all dataclasses unless we actually see any constructor calls for these classes?

We have to infer types for all class decorators anyway. So the additional work that we do here is to check if the inferred type is one of two possible options. I'm just guessing here as well, but I just can't imagine that this will ever be a performance-critical code path. You would have to have loads of different dataclasses that are never constructed anywhere for this to be relevant, and even then, there's so much more we have to do when type-checking a class definition.

Memory-wise, `MetaclassMetadata` is bitset with a size of 2 bytes. I think we could get that down to one byte potentially, as we might not need the information whether `repr` and `eq` are set to true (because those methods are defined in any case, it's just the implementation which changes?).

> we don't have any dataclass-heavy benchmarks

I'm happy to run a benchmark locally if you have any suggestions for codebases to check?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1743 on 2025-04-15 18:18_

> I was assuming we'd do some more localised special-casing for `@dataclass` with arguments passed.

This is something I briefly considered. I decided against it, mainly because I wanted to model it "correctly", and also because I saw that other type checkers support it.

> The advantage would be that we wouldn't have to worry about all the questions everywhere such as "is this type a subtype of `DataclassMetadata`?", etc.

Yes, I agree that this is annoying. I'm hopeful that we can merge some of the recently added special callable types into something more generally useful. Maybe that merged variant could have some methods like `.to_callable()` (which would return a corresponding `Callable` type) and `.to_instance_fallback()` (which would return somthing like `MethodWrapperType`), and then we could formulate a lot of those type properties in terms of these.

> It might be good to add some tests demonstrating this capability, though?

Yes :+1: 

The "Using `dataclass` as a function" test which I added in this PR was supposed to demonstrate something related: that you can also transform a class after-the-fact (`class C: ...; C = dataclass(order=True)(C)`). That doesn't work yet, but it would also require us to actually generate a new callable type for the `dataclass(order=True)` call, instead of just trying to detect the syntactic structure of `dataclass` being used as a decorator.

---

_@sharkdp reviewed on 2025-04-15 18:18_

---

_@AlexWaygood reviewed on 2025-04-15 18:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 18:18_

> We have to infer types for all class decorators anyway. So the additional work that we do here is to check if the inferred type is one of two possible options.

Hmm, good point. I suppose doing it lazily would only improve performance if you also changed our dataclass handling to be more localised, as per https://github.com/astral-sh/ruff/pull/17353#discussion_r2045113279 — but as discussed in that thread, that's probably not a good idea for other reasons.

The memory savings *might* still be worth it? Large codebases could have lots of class definitions; the new field on `Class` increases the amount of data we're storing in the salsa database even for non-dataclasses. As per your other question, though — no, I don't have a great suggestion for a codebase to run performance or memory benchmarks on.

> we might not need the information whether `repr` and `eq` are set to true (because those methods are defined in any case, it's just the implementation which changes?).

It's a bit of a stretch, but you could have something like this — we'd emit an error about attempting to instantiate an abstract class if we didn't synthesise the `__repr__` method on `Bar` (once we support ABCs and `@abstractmethod` properly):

```py
from abc import ABC, abstractmethod
from dataclasses import dataclass

class Foo(ABC):
    @abstractmethod
    def __repr__(self) -> str: ...

@dataclass
class Bar(Foo): ...

Bar()
```

For mypy, it's also important for it to synthesise the `__eq__` method for this error code: https://mypy.readthedocs.io/en/stable/error_code_list2.html#check-that-comparisons-are-overlapping-comparison-overlap

And there could be possible use cases from type-aware lints in the future, too? Not sure

---

_@sharkdp reviewed on 2025-04-15 18:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 18:30_

> The memory savings _might_ still be worth it? Large codebases could have lots of class definitions; the new field on `Class` increases the amount of data we're storing in the salsa database even for non-dataclasses.

Yes, by two bytes (or three, for the `Option`, if there are no niches) per class. Worst case eight bytes, due to alignment requirements. If you have 10 000 classes in a project, that's 80kB. Or am I missing something?

> It's a bit of a stretch, but you could have something like this

Thanks — I'll keep the `REPR` and `EQ` flags for now.

---

_@AlexWaygood reviewed on 2025-04-15 18:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:105 on 2025-04-15 18:33_

> Worst case eight bytes, due to alignment requirements. If you have 10 000 classes in a project, that's 80kB.

Alright, thanks, probably not worth worrying about then 😄

---
