```yaml
number: 1219
title: "Enable configuration files to \"extend\" other configuration files"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/inherit
created_at: 2022-12-12T20:50:48Z
updated_at: 2022-12-13T01:28:23Z
url: https://github.com/astral-sh/ruff/pull/1219
synced_at: 2026-01-12T15:55:05Z
```

# Enable configuration files to "extend" other configuration files

---

_@charliermarsh_

This PR implements an `extends` keyword in the `pyproject.toml` schema. When provided, we resolve a `pyproject.toml` file by first loading the file pointed to by `extends`, then overriding it with any properties defined in the current file.

Configuration files are resolved recursively, such that the file you `extends` can also have an `extends` (and we error if you introduce a cycle).

In implementing this feature, I had to shift around some of the responsibilities that are split between `Options`, `Configuration`, and `Settings` (which are all bad names). As of this PR, they are as follows:

- `Options` is the simplest: it's the exact schema implemented in TOML. It's the thing we read from a `pyproject.toml` file. Every field is `Option`.
- `Settings` is the fully resolved, internal settings for the linter. It doesn't have to look like the user-facing representation in the TOML file. For example, it includes a single `FxHashSet` of enabled check codes, rather than an `ignore` and `select` list. None of the fields are `Option`, and `Settings` is responsible for translating each field from `None` to defaults.
- `Configuration` is the intermediate representation that results from taking `Options` and resolving them relative to a given file system path. For example, `Options` can include `excludes`, which are relative paths. When we transform `Options` to `Configuration`, we have to provide a "project root" relative to which it's resolved. From there, we transform any paths into absolute paths based on that project root. All of the fields on `Configuration` are also `Option`. (This changed in this PR.)

When creating a `Settings`, we load the final `pyproject.toml` file from disk into `Options`, then into `Configuration`. We then look at the `extends` keyword and iteratively collect all of the `Configuration` structs. When we're done, we go through and merge them into a single `Configuration`. (This is why I had to make all fields on `Configuration` optional -- so that we can tell what's defined, and what's not.) Finally, we take the `Configuration` and transform it into `Settings`.

Closes #1055. Closes #1215. #552 is also relevant.

---

_Comment by @charliermarsh on 2022-12-12 21:17_

@smackesey - This, combined with the changes on `main`, will let you apply this patch to https://github.com/dagster-io/dagster/pull/10901:

```diff
diff --git a/examples/docs_snippets/pyproject.toml b/examples/docs_snippets/pyproject.toml
index 13f6920c45..70bc2806fe 100644
--- a/examples/docs_snippets/pyproject.toml
+++ b/examples/docs_snippets/pyproject.toml
@@ -9,3 +9,14 @@ line-length = 88
 preview = true
 required-version = "22.10.0"
 target-version = ['py37', 'py38', 'py39', 'py310', 'py311']
+
+[tool.ruff]
+extend = "../../pyproject.toml"
+
+extend-ignore = [
+
+  # (local variable assigned but never used): This happens a lot in the docs snippets for didactic
+  # purposes.
+  "F841",
+
+]
diff --git a/pyproject.toml b/pyproject.toml
index 4012d47c3e..2c62a258d5 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -117,11 +117,6 @@ ignore = [
   "T201",  # (no print call)
   "T203",  # (no pprint call)

-  # (local variable assigned but never used): This happens a lot in the docs snippets for didactic
-  # purposes. We are putting it in the global rather than file-specific ignore pending resolution of
-  # a bug with file-specific ignores.
-  "F841",
-
 ]

 # Match black. Note that this also checks comment line length, but black does not format comments.
```


---

_Comment by @charliermarsh on 2022-12-12 21:24_

@andersk - Since you brought up overrides in #552, wondering what your stance is on how the "extends" semantics should work here. (If you're busy, all good, no worries.)

Right now, if you `extends`, we don't do any merging of any fields. It's effectively equivalent to `{**parent, **child}` (assuming that any fields omitted in the `pyproject.toml` are considered absent from the dicts rather than `None`, in the Python analogy).

I think ESLint merges the fields between parent and child. So, like, if one field is a list of plugins, I _believe_ ESLint will just concatenate the lists when merging a parent into a child.

I think I'd prefer something more explicit? I've considered using the `extend-select` (and related) fields for this purpose. So if the child sets a `select`, it would totally ignore the parent `select`. If a child sets `extend-select`, we actually wouldn't override the parent `extend-select`, and instead we'd effectively apply the parents settings, then the child's. This would enable children to _always_ tweak the parent configurations.


---

_Comment by @andersk on 2022-12-12 22:49_

One question with that strategy is what should happen if the parent sets `select = ["F4"]` and `ignore = ["F401"]` and the child sets `extend-select = ["F401"]`? Within the same configuration, `ignore` would ordinarily preside over `select`, but perhaps a child should always preside over its parent? What if the child sets `extend-select = ["F40"]` instead?

---

_Comment by @charliermarsh on 2022-12-12 23:24_

@andersk - Yeah, my instinct is that the child should always preside over the parent.

So the algorithm would be, roughly:

- Generate the base set of codes from `select` and `ignore`.
- Apply the parent's `extend-select` + `extend-ignore`.
- Apply the child's `extend-select` + `extend-ignore`.
- (Apply the command-line-provided `extend-select` and `extend-ignore`?)

On that last point: as an example, this today _doesn't_ enable `F407`, but I kind of think it should?

```
flake8 resources/test/fixtures/pyflakes/F407.py --ignore F407 --extend-select F407
```


---

_Comment by @charliermarsh on 2022-12-12 23:46_

(Ok, all `STOPSHIP` directives are removed. But this needs tests.)

---

_Merged by @charliermarsh on 2022-12-13 01:28_

---

_Closed by @charliermarsh on 2022-12-13 01:28_

---

_Branch deleted on 2022-12-13 01:28_

---
