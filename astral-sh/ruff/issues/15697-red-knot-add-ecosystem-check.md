```yaml
number: 15697
title: "[red-knot] add ecosystem check"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-23T17:18:10Z
updated_at: 2025-05-07T15:22:01Z
url: https://github.com/astral-sh/ruff/issues/15697
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] add ecosystem check

---

_Issue opened by @carljm on 2025-01-23 17:18_

### Description

We regularly observe the effects of changes to red-knot in the diagnostics emitted when running our (performance) benchmark against tomllib, and sometimes catch issues by seeing new false positives. But a) tomllib is one small codebase, not necessarily representative, and b) the ergonomics of catching these issues in the performance benchmark is poor.

We should select some larger real-world codebases and implement tooling to run red-knot on them and snapshot the diagnostics emitted, such that we can see (and easily update) this snapshot when we make changes to red-knot.

We should run this in CI to ensure our snapshot stays up to date.

(This is very similar to the ruff ecosystem check, and we can likely reuse some of that infrastructure?)

Bonus: if we implement https://github.com/astral-sh/ty/issues/213, we can also track changes to the prevalence of Todo types over time.

---

_Label `red-knot` added by @carljm on 2025-01-23 17:18_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-23 17:18_

---

_Renamed from "[red-knot] correctness "benchmark" against real codebases" to "[red-knot] add ecosystem check" by @carljm on 2025-01-23 23:32_

---

_Assigned to @sharkdp by @sharkdp on 2025-03-07 13:18_

---

_Comment by @sharkdp on 2025-03-07 13:37_

Just noting down some thoughts while working on this:

* It would help to have a concise (or structured) diagnostics diagnostics format. `mypy_primer` uses a simple line-based diff (without context) and it turns out that diffs between two rich-diagnostics red-knot outputs are … not great
  ![image](https://github.com/user-attachments/assets/e1aee9ac-41a9-43c8-b8cf-58ce902aa0d8)

  * This is planned in https://github.com/astral-sh/ruff/issues/14194
* We do still panic on a lot of the projects included in [`mypy_primer`'s list](https://github.com/hauntsaninja/mypy_primer/blob/7543da9b113a282219e43fe36f054219a4a61c38/mypy_primer/projects.py#L74)
  * This will likely be fixed by astral-sh/ruff#14029 
* Support for importing from `-stubs` packages is another thing that would make the ecosystem check more helpful.
  * This is planned in https://github.com/astral-sh/ruff/issues/11653#issuecomment-2446185608

---

_Comment by @sharkdp on 2025-03-10 11:23_

While selecting some projects from mypy_primer, I noted down some issues that appear very frequently across multiple codebases. If we want to work on reducing false positives diagnostics, those would likely be the candidates with the biggest impact:

* [x] Panics due to salsa cycles => https://github.com/astral-sh/ruff/pull/14029
* [ ] Importing from `-stubs` packages => https://github.com/astral-sh/ruff/issues/16612
* [ ] `lint:unresolved-import` and subsequent `lint:unresolved-attribute` due to `*`-imports from e.g. `collections.abc` or `asyncio` => https://github.com/astral-sh/ruff/issues/14169
* [ ] Consider `__all__` for re-export convention => https://github.com/astral-sh/ty/issues/199
* [x] Assignability of intersection types (results in e.g. `lint:invalid-argument-type`) => https://github.com/astral-sh/ruff/issues/14899
* [ ] Incorrect `lint:unresolved-reference` in the presence of unreachable code => https://github.com/astral-sh/ruff/issues/15797
* [x] `lint:unresolved-attribute` due to custom `__getattr__`, e.g. `argparse.Namespace` => https://github.com/astral-sh/ruff/issues/16614
* [ ] `lint:unused-ignore-comment` => Some of these could be a signal for false negatives? Others could just be an indication that we might need a feature to silence those unused *generic* `# type: ignore` comments
* [ ] `lint:unresolved-attribute` because we don't understand `super` => https://github.com/astral-sh/ruff/issues/16615
* [ ] Missing understanding of `@property` => https://github.com/astral-sh/ruff/issues/16616

---

_Comment by @sharkdp on 2025-03-14 21:29_

I think it's probably fine to close this. At the time of writing, we emit 1935 diagnostics on `main` when running `mypy_primer` with the following project selector (current setting in CI):
```regex
'/(mypy_primer|black|pyp|git-revise|zipp|arrow|isort|itsdangerous|rich|packaging|pybind11|pyinstrument)$'
```

I have merged all those diagnostics into a single text file for a quick analysis: [diagnostics.txt](https://github.com/user-attachments/files/19254794/diagnostics.txt)

We can group by lint, for example:
```
▶ cat diagnostics.txt | cut -d' ' -f1 | uniq -c | sort -nr
    494 error[lint:unresolved-import]
    386 error[lint:unresolved-attribute]
    237 warning[lint:possibly-unresolved-reference]
    178 warning[lint:possibly-unbound-attribute]
    162 warning[lint:unresolved-reference]
    120 error[lint:invalid-argument-type]
     84 warning[lint:unused-ignore-comment]
     62 error[lint:invalid-assignment]
     58 error[lint:invalid-return-type]
     38 error[lint:call-non-callable]
     24 error[lint:unsupported-operator]
     21 error[lint:division-by-zero]
     16 error[lint:invalid-base]
     13 error[lint:inconsistent-mro]
     10 error[lint:non-subscriptable]
      8 error[lint:missing-argument]
      7 error[lint:invalid-parameter-default]
      6 error[lint:not-iterable]
      3 warning[lint:call-possibly-unbound-method]
      3 error[lint:invalid-raise]
      2 error[lint:too-many-positional-arguments]
      1 warning[lint:possibly-unbound-import]
      1 error[lint:invalid-type-form]
      1 error[lint:invalid-exception-caught]
```

I have looked through most of the more obscure ones (starting this list from the bottom), and haven't found anything surprising. A lot of it comes down to known problems (no generics, no enums, …). The 21 `division-by-zero` are true positives because apparently `rich` likes to put `1 / 0` in their codebase to force a crash(?).

Apart from that, I think the list above is still accurate. The highest impact would still be working on imports, but note that some of these might be true positives (mypy_primer doesn't install all dependencies for a project. there's also no version requirements or similar, so even if a dependency is specified, it could be the wrong version).

---

_Closed by @sharkdp on 2025-03-14 21:29_

---
