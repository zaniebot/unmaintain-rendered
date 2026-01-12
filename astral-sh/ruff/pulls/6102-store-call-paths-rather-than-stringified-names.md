```yaml
number: 6102
title: Store call paths rather than stringified names
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/store-call-path
created_at: 2023-07-26T19:19:18Z
updated_at: 2023-08-05T15:40:32Z
url: https://github.com/astral-sh/ruff/pull/6102
synced_at: 2026-01-12T02:52:03Z
```

# Store call paths rather than stringified names

---

_Pull request opened by @charliermarsh on 2023-07-26 19:19_

## Summary

Historically, we've stored "qualified names" on our `BindingKind::Import`, `BindingKind::SubmoduleImport`, and `BindingKind::ImportFrom` structs. In Ruff, a "qualified name" is a dot-separated path to a symbol. For example, given `import foo.bar`, the "qualified name" would be `"foo.bar"`; and given `from foo.bar import baz`, the "qualified name" would be `foo.bar.baz`.

This PR modifies the `BindingKind` structs to instead store _call paths_ rather than qualified names. So in the examples above, we'd store `["foo", "bar"]` and `["foo", "bar", "baz"]`. It turns out that this more efficient given our data access patterns. Namely, we frequently need to convert the qualified name to a call path (whenever we call `resolve_call_path`), and it turns out that we do this operation enough that those conversations show up on benchmarks.

There are a few other advantages to using call paths, rather than qualified names:

1. The size of `BindingKind` is reduced from 32 to 24 bytes, since we no longer need to store a `String` (only a boxed slice).
2. All three import types are more consistent, since they now all store a boxed slice, rather than some storing an `&str` and some storing a `String` (for `BindingKind::ImportFrom`, we needed to allocate a `String` to create the qualified name, but the call path is a slice of static elements that don't require that allocation).
3. A lot of code gets simpler, in part because we now do call path resolution "earlier". Most notably, for relative imports (`from .foo import bar`), we store the _resolved_ call path rather than the relative call path, so the semantic model doesn't have to deal with that resolution. (See that `resolve_call_path` is simpler, fewer branches, etc.)

In my testing, this change improves the all-rules benchmark by another 4-5% on top of the improvements mentioned in #6047.


---

_Converted to draft by @charliermarsh on 2023-07-26 19:19_

---

_Marked ready for review by @charliermarsh on 2023-07-28 02:09_

---

_@charliermarsh reviewed on 2023-07-28 02:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_6.py.snap`:23 on 2023-07-28 02:09_

I don't know if this is good or bad. We now show the fully resolved module path, rather than the relative path.

---

_@charliermarsh reviewed on 2023-07-28 02:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:403 on 2023-07-28 02:11_

I'm not thrilled about this. Basically, if you have `from ...foo import bar`, and the triple dots means you've extended past the module root that we detected for the file, then we can't resolve the import. In the past, we dealt with this in `resolve_call_path` (we'd just return `None` for such imports), but now we need a way to track these in the semantic model, to know that they're unused. So we need _some_ call path for these. The fallback here is that we create a call path like `[".", ".", ".", "foo", "bar"]` in such cases.


---

_Comment by @github-actions[bot] on 2023-07-28 02:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      9.4±0.40ms     4.3 MB/sec    1.00      8.9±0.41ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.04  1818.7±85.78µs     9.2 MB/sec    1.00  1751.3±98.12µs     9.5 MB/sec
formatter/numpy/globals.py                 1.02    205.0±8.51µs    14.4 MB/sec    1.00    200.6±9.21µs    14.7 MB/sec
formatter/pydantic/types.py                1.06      3.9±0.18ms     6.6 MB/sec    1.00      3.7±0.14ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.01     11.6±0.38ms     3.5 MB/sec    1.00     11.5±0.39ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.1±0.12ms     5.4 MB/sec    1.00      3.0±0.09ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.04   465.4±31.02µs     6.3 MB/sec    1.00   446.4±22.81µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.3±0.15ms     4.8 MB/sec    1.00      5.2±0.13ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.05      6.2±0.34ms     6.6 MB/sec    1.00      5.9±0.20ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1272.6±34.24µs    13.1 MB/sec    1.00  1255.2±36.64µs    13.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.6±7.10µs    18.8 MB/sec    1.00    155.1±8.30µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      2.7±0.11ms     9.4 MB/sec    1.00      2.6±0.12ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.04ms     5.1 MB/sec    1.01      8.1±0.05ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1529.9±9.60µs    10.9 MB/sec    1.01  1543.8±13.10µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    154.9±1.23µs    19.1 MB/sec    1.02    157.6±7.91µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.03ms     7.6 MB/sec    1.01      3.4±0.04ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.01     10.1±0.12ms     4.0 MB/sec    1.00     10.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    294.8±3.30µs    10.0 MB/sec    1.01    296.9±3.19µs     9.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      4.6±0.04ms     5.6 MB/sec    1.00      4.6±0.02ms     5.6 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.04ms     7.5 MB/sec    1.00      5.4±0.03ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1071.1±6.65µs    15.5 MB/sec    1.00  1069.6±10.25µs    15.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    112.7±1.16µs    26.2 MB/sec    1.00    111.8±1.18µs    26.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.3±0.01ms    10.9 MB/sec    1.00      2.3±0.02ms    10.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_6.py.snap`:23 on 2023-07-28 06:07_

Hmm. I would prefer keeping the import the same as it is in the source document. It can otherwise be confusing... You search for `pyflakes` and you then end up removing the wrong import. 

---

_@MichaReiser reviewed on 2023-07-28 06:11_

I haven't fully reviewed this yet, mainly because I can't say that I understand what a `CallPath`. What I find interesting is that the documentation uses an entirely different terminology:

```
/// A representation of a qualified name, like `typing.List`.
```

Does call path represent any qualified name? If so, should we rename `CallPath` to `FullQualifiedName or `QualifiedName` (I would still love it if it is a new type wrapper rather than a type alias)? 

I'm bringing this up because I would find it strange to store a `CallPath` in an `Import`... because a `CallPath` gives me the impression to be call specific. However, I think storing `FullQualifiedName`s on an import sounds like a good idea. 

---

_Comment by @charliermarsh on 2023-07-28 12:55_

Yes, sorry, we are storing the fully qualified name on the import, represented as a slice of dot-separated components. The "thing we are storing" hasn't changed meaningfully, apart from that we now store the resolved paths for relative imports. Instead, it's the representation that has changed (instead of a single string, it's now a slice of components).

I can look into a more thorough refactor that introduces some structs for clarity, but I think it should probably be separate. What would you find clearest for now? Renaming the _attribute_ back to `qualified_name` or `qualified_path`?

---

_@charliermarsh reviewed on 2023-07-28 13:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_6.py.snap`:23 on 2023-07-28 13:10_

I don't actually have a great way to solve this. I may have to revert to storing relative paths for relative imports, and doing the relative path resolution in `resolve_call_path` as before. Or, when we create the name for the diagnostic, I could format the name based on the statement. The crux of the issue is that we don't store whether the import was relative on the binding right now, and adding that information will increase the size of the struct.


---

_Comment by @zanieb on 2023-07-28 13:34_

@charliermarsh I agree a return to `qualified_name` makes sense — `qualified_path` makes it a bit clearer that we're storing the name as components but I'm not sure if it's worth the possible confusion with file paths.

---

_Comment by @MichaReiser on 2023-07-28 16:14_

> I can look into a more thorough refactor that introduces some structs for clarity, but I think it should probably be separate. What would you find clearest for now? Renaming the attribute back to qualified_name or qualified_path?

That sounds good to me. Would that mean we rename `CallPath` to `QualifiedName/Path`? 

---

_Comment by @charliermarsh on 2023-07-28 16:15_

We may want two different wrapper structs, similar to how `Path` can be absolute or relative.

---

_@charliermarsh reviewed on 2023-07-28 18:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__F401_F401_6.py.snap`:23 on 2023-07-28 18:54_

Alright, this behavior is restored. I moved relative path resolution back into the semantic model, so we now store relative call paths (like `[".", "foo", "bar"]` for `from .foo import bar`).

---

_Comment by @charliermarsh on 2023-07-28 18:55_

I'm not sure that I want to tackle the nomenclature changes in this PR -- I'd rather do a dedicated pass over them separately. Is there any change you'd want to see that would make _this_ PR more palatable to merge?

---

_Comment by @charliermarsh on 2023-07-28 19:49_

(I am working on a change to update the nomenclature, unify the documentation, and introduce standalone structs for these concepts.)

---

_@zanieb approved on 2023-07-28 20:18_

---

_Review requested from @zanieb by @zanieb on 2023-07-31 05:14_

---

_Comment by @MichaReiser on 2023-08-01 05:56_

I'm not very opinionated on this. I always prefer to store more specific types than strings. 

---

_@MichaReiser approved on 2023-08-05 08:17_

---

_Merged by @charliermarsh on 2023-08-05 15:21_

---

_Closed by @charliermarsh on 2023-08-05 15:21_

---

_Branch deleted on 2023-08-05 15:21_

---
