```yaml
number: 7520
title: Extras contain whitespace
type: issue
state: closed
author: alexeagle
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-09-18T22:56:53Z
updated_at: 2024-09-25T17:41:56Z
url: https://github.com/astral-sh/uv/issues/7520
synced_at: 2026-01-10T04:45:10Z
```

# Extras contain whitespace

---

_Issue opened by @alexeagle on 2024-09-18 22:56_

Using https://github.com/astral-sh/uv/releases/download/0.4.12

with a requirements.in like

```
sentencepiece~=0.1.99
torch==2.1.0+cu118
transformers==4.35.2
```

pip-compile from https://github.com/jazzband/pip-tools produces a requirements.txt like

```
transformers[sentencepiece,torch]==4.35.2 \
    --hash=sha256:2d125e197d77b0cdb6c9201df9fa7e2101493272e448b9fba9341c695bee2f52 \
    --hash=sha256:9dfa76f8692379544ead84d98f537be01cd1070de75c74efb13abcbc938fbe2f
```

while `uv pip compile` produces one with an extra whitespace

```
transformers[sentencepiece, torch]==4.35.2 \
    --hash=sha256:2d125e197d77b0cdb6c9201df9fa7e2101493272e448b9fba9341c695bee2f52 \
    --hash=sha256:9dfa76f8692379544ead84d98f537be01cd1070de75c74efb13abcbc938fbe2f
```

My understanding of the grammar, and indeed the implementation from `pypa/packaging` seems to indicate that this difference should be inconsequential.

https://github.com/pypa/packaging/blob/cf2cbe2aec28f87c6228a6fb136c27931c9af407/src/packaging/_parser.py#L178

However in practice the `uv` file causes an error like

```
  File "/home/alex/.cache/bazel/_bazel_alex/c18c578b4ecea221790803eee605d790/sandbox/linux-sandbox/44/execroot/assemblyai/bazel-out/k8-opt-exec-ST-13d3ddad9198/bin/external/rules_python/tools/wheelmaker.runfiles/pypi__packaging/packaging/requirements.py", line 37, in __init__
    raise InvalidRequirement(str(e)) from e
packaging.requirements.InvalidRequirement: Expected extra name after comma
    transformers[sentencepiece,
                               ^
```

Technically you might say it's a bug in pypa/packaging but since `uv` intends to be 100% drop-in compatible with pip-tools I imagine you might want to fix it here.


---

_Comment by @zanieb on 2024-09-18 23:39_

The [official grammar](https://packaging.python.org/en/latest/specifications/dependency-specifiers/#complete-grammar) says that whitespace is allowed — this is a bug in the parser and should be reported upstream.

I'm not sure if we should change our representation here, but whatever we display should be consistent throughout uv.

---

_Label `compatibility` added by @zanieb on 2024-09-18 23:39_

---

_Label `needs-decision` added by @zanieb on 2024-09-18 23:39_

---

_Comment by @charliermarsh on 2024-09-19 02:19_

I prefer it with the space but we already do some things to accommodate pip here so it seems reasonable to change it.

---

_Comment by @zanieb on 2024-09-19 04:20_

It looks like the latest version of `packaging` is fine, these both pass

```
❯ uv run --with packaging --isolated --no-project -- python -c 'from packaging.requirements import Requirement; Requirement("pkg[foo,bar]")'
❯ uv run --with packaging --isolated --no-project -- python -c 'from packaging.requirements import Requirement; Requirement("pkg[foo, bar]")'
```

Are you using a really old version or something?

---

_Comment by @alexeagle on 2024-09-19 13:43_

This is using packaging version 23.1.

It's not trivial to repro since https://github.com/bazelbuild/rules_python/blob/0.32.2/tools/wheelmaker.py#L555 is on the callstack here. And packaging is fetched as a wheel here: https://github.com/bazelbuild/rules_python/blob/0.32.2/python/pip_install/repositories.bzl#L54

I agree that it's likely the bug is somewhere else, and I'll dig some more. Though regardless of where it is, any variability in the output runs into Hyrum's law, and is observable somewhere...

---

_Comment by @zanieb on 2024-09-19 14:05_

Hm interesting

```
❯ uv run --with packaging==23.1 --isolated --no-project -- python -c 'from packaging.requirements import Requirement; print(Requirement("pkg[foo, bar]"))'

pkg[bar,foo]
```



---

_Comment by @alexeagle on 2024-09-19 14:28_

Wow, the actual bug was in a patch my client applied on top of Bazel's rules_python - so only they could observe it...

```
diff --git a/python/pip_install/pip_repository_requirements.bzl.tmpl b/python/pip_install/pip_repository_requirements.bzl.tmpl
index 8e17720..fdeaae8 100644
--- a/python/pip_install/pip_repository_requirements.bzl.tmpl
+++ b/python/pip_install/pip_repository_requirements.bzl.tmpl
@@ -15,5 +15,11 @@ all_whl_requirements = all_whl_requirements_by_package.values()
 all_data_requirements = %%ALL_DATA_REQUIREMENTS%%

 _packages = %%PACKAGES%%
+
+all_packages_by_repository = {
+    package[0]: package[1].split(" ")[0]
+    for package in _packages
+}
+
 _config = %%CONFIG%%
 _annotations = %%ANNOTATIONS%%
```

I've fixed that to be less brittle, so I'm no longer affected by this bug. Up to you if you'd like to produce more pip-compile-compatible output in case someone else also has a broken parser.

---

_Comment by @charliermarsh on 2024-09-25 17:41_

In that case, I'm gonna leave it as-is for now. If we see another report, let's revisit. Thanks @alexeagle!

---

_Closed by @charliermarsh on 2024-09-25 17:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-25 17:41_

---
