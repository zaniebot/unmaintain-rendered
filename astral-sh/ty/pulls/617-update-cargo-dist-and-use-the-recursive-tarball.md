```yaml
number: 617
title: update cargo-dist and use the recursive-tarball feature
type: pull_request
state: merged
author: Gankra
labels:
  - release
assignees: []
merged: true
base: main
head: gankra/recursive-tarball
created_at: 2025-06-09T15:23:38Z
updated_at: 2025-06-12T15:23:12Z
url: https://github.com/astral-sh/ty/pull/617
synced_at: 2026-01-10T02:34:10Z
```

# update cargo-dist and use the recursive-tarball feature

---

_Pull request opened by @Gankra on 2025-06-09 15:23_

Fixes #372 

---

_Marked ready for review by @Gankra on 2025-06-09 16:42_

---

_Comment by @Gankra on 2025-06-09 16:44_

This current implementation i believe has the caveat that it will silently discard the broken symlinks in the ruff fuzzer. If you consider this an issue, the fuzzer should be changed to generate the symlinks on demand instead of using these checked in broken ones.

---

_Comment by @MichaReiser on 2025-06-09 16:49_

> This current implementation i believe has the caveat that it will silently discard the broken symlinks in the ruff fuzzer. If you consider this an issue, the fuzzer should be changed to generate the symlinks on demand instead of using these checked in broken ones.

I consider this an issue with the fuzzer :)

---

_@MichaReiser approved on 2025-06-09 16:49_

Nice, seems like a simple cargo dist upgrade to me. 

Thank you for looking into this

---

_Label `release` added by @MichaReiser on 2025-06-09 16:50_

---

_Comment by @Gankra on 2025-06-09 16:56_

Confirmed:

```
cargo dist build -aglobal
cp target/distrib/source.tar.gz ~/dev/tmp/ty2/
cd ~/dev/tmp/ty2
tar -xvf source.tar.gz
cd ruff
cargo test
```

builds and runs

Although on the ty checkout I'm on all these tests fail:

```
    mdtest__annotations_invalid
    mdtest__assignment_annotations
    mdtest__binary_instances
    mdtest__comparison_instances_membership_test
    mdtest__comparison_instances_rich_comparison
    mdtest__comparison_tuples
    mdtest__diagnostics_attribute_assignment
    mdtest__diagnostics_invalid_argument_type
    mdtest__diagnostics_no_matching_overload
    mdtest__diagnostics_semantic_syntax_errors
    mdtest__diagnostics_shadowing
    mdtest__diagnostics_union_call
    mdtest__diagnostics_unpacking
    mdtest__diagnostics_unresolved_import
    mdtest__diagnostics_unresolved_reference
    mdtest__diagnostics_unsupported_bool_conversion
    mdtest__diagnostics_version_related_syntax_errors
    mdtest__function_return_type
    mdtest__generics_legacy_functions
    mdtest__generics_pep695_functions
    mdtest__import_basic
    mdtest__loops_for
    mdtest__mro
    mdtest__overloads
    mdtest__protocols
    mdtest__unary_not
    mdtest__with_sync
```

I don't know if that's expected.


---

_Comment by @MichaReiser on 2025-06-09 17:02_

> Although on the ty checkout I'm on all these tests fail:

Not sure. How do you run those?

---

_Comment by @MichaReiser on 2025-06-09 17:05_

I just pulled `main` and ran `cargo nextest run` in the ruff directory and that seems to work fine. Any chance you have a venv activated? We only recently fixed the test isolation from externally set environment variables.

---

_Comment by @Gankra on 2025-06-09 17:43_

Interesting, `cargo nextest run` fairs far better, with only one failing test (ty_python_semantic::mdtest).


```
expression: snapshot
────────────────────────────────────────────────────────────────────────────────
+new results
────────────┬───────────────────────────────────────────────────────────────────
          0 │+---
          1 │+mdtest name: invalid.md - Tests for invalid types in type expressions - Diagnostics for common errors - Module-literal used when you meant to use a class from that module
          2 │+mdtest path: crates/ty_python_semantic/resources/mdtest/annotations/invalid.md
          3 │+---
          4 │+
          5 │+# Python source files
          6 │+
          7 │+## foo.py
          8 │+
          9 │+```
         10 │+1 | import datetime
         11 │+2 | 
         12 │+3 | def f(x: datetime): ...  # error: [invalid-type-form]
         13 │+```
         14 │+
         15 │+## PIL/Image.py
         16 │+
         17 │+```
         18 │+1 | class Image: ...
         19 │+```
         20 │+
         21 │+## bar.py
         22 │+
         23 │+```
         24 │+1 | from PIL import Image
         25 │+2 | 
         26 │+3 | def g(x: Image): ...  # error: [invalid-type-form]
         27 │+```
         28 │+
         29 │+# Diagnostics
         30 │+
         31 │+```
         32 │+error[invalid-type-form]: Variable of type `<module 'datetime'>` is not allowed in a type expression
         33 │+ --> src/foo.py:3:10
         34 │+  |
         35 │+1 | import datetime
         36 │+2 |
         37 │+3 | def f(x: datetime): ...  # error: [invalid-type-form]
         38 │+  |          ^^^^^^^^
         39 │+  |
         40 │+info: Did you mean to use the module's member `datetime.datetime` instead?
         41 │+info: rule `invalid-type-form` is enabled by default
         42 │+
         43 │+```
         44 │+
         45 │+```
         46 │+error[invalid-type-form]: Variable of type `<module 'PIL.Image'>` is not allowed in a type expression
         47 │+ --> src/bar.py:3:10
         48 │+  |
         49 │+1 | from PIL import Image
         50 │+2 |
         51 │+3 | def g(x: Image): ...  # error: [invalid-type-form]
         52 │+  |          ^^^^^
         53 │+  |
         54 │+info: Did you mean to use the module's member `Image.Image` instead?
         55 │+info: rule `invalid-type-form` is enabled by default
         56 │+
         57 │+```
```

---

_Comment by @MichaReiser on 2025-06-09 18:30_

Weird. Yeah, not sure why you're seeing those failures. Looking at the changes, it seems that you're missing entire snapshot files

---

_Comment by @Gankra on 2025-06-09 18:56_

Oh fun it looks like there's a freakout [around the snapshot files that contain `...` in their names](https://github.com/astral-sh/ruff/tree/main/crates/ty_python_semantic/resources/mdtest/snapshots)

---

_Comment by @Gankra on 2025-06-09 18:58_

Specifically the tarball does not contain them. I wonder if it's running afoul of `Path::parent()`...

---

_Comment by @MichaReiser on 2025-06-09 19:01_

😬 we should probably use a different truncation character (e.g the unicode one)

---

_Comment by @MichaReiser on 2025-06-10 10:42_

We actually do use the unicode `\u2026` and not a regular `...` 

---

_Comment by @Gankra on 2025-06-10 12:41_

Oh the bug is even more fun/silly than I thought

---

_Comment by @Gankra on 2025-06-10 12:42_

```
$ git ls-files --recurse-submodules | rg ty_python_semantic
...
"ruff/crates/ty_python_semantic/resources/mdtest/snapshots/unsupported_bool_con\342\200\246_-_Different_ways_that_\342\200\246_-_Part_of_a_union_wher\342\200\246_(7cca8063ea43c1a).snap"
"ruff/crates/ty_python_semantic/resources/mdtest/snapshots/version_related_synt\342\200\246_-_Version-related_synt\342\200\246_-_`match`_statement_-_Before_3.10_(2545eaa83b635b8b).snap"
ruff/crates/ty_python_semantic/resources/mdtest/statically_known_branches.md
ruff/crates/ty_python_semantic/resources/mdtest/stubs/class.md
ruff/crates/ty_python_semantic/resources/mdtest/stubs/ellipsis.md
...
```



---

_Comment by @Gankra on 2025-06-10 12:42_

git ls-files freaks out and wraps those paths in quotes with escaping, cool

---

_Comment by @Gankra on 2025-06-10 12:44_

Looks like the `-z` flag is the answer, will test that out.

---

_Comment by @Gankra on 2025-06-10 13:26_

Fixed in 

* https://github.com/astral-sh/cargo-dist/pull/43

Released and updated this PR to use it

`cargo test` now passes cleanly 🎊 

---

_Comment by @MichaReiser on 2025-06-12 13:59_

Is this ready to be merged?

---

_Comment by @Gankra on 2025-06-12 14:32_

Yep!

---

_Merged by @Gankra on 2025-06-12 14:32_

---

_Closed by @Gankra on 2025-06-12 14:32_

---

_Branch deleted on 2025-06-12 14:32_

---

_Comment by @WhyNotHugo on 2025-06-12 15:13_

Do I understand correctly that we (downstream) should use the workflow-generated tarballs, rather than the git-archive-generated tarballs?

Can these be built in a reproducible manner?

---

_Comment by @Gankra on 2025-06-12 15:20_

You can use the git-archive-generated tarballs if they work for you (they don't include the ruff submodule, this cannot be changed, that's how github works).

The workflow-generated tarballs can be generated hopefully-reproducibly with `cargo dist build -aglobal` which will produce `target/distrib/source.tar.gz` using the [astral-sh/cargo-dist](https://github.com/astral-sh/cargo-dist/) version in the dist-workspace.toml.

---

_Comment by @Gankra on 2025-06-12 15:23_

Morally the tarballs are just shoving the git-archive-generated ruff tarball into the ty tarball -- if there's significant divergence this isn't intentional.

---
