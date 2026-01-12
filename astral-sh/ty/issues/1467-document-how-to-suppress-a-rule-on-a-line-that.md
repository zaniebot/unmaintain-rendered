```yaml
number: 1467
title: Document how to suppress a rule on a line that already has a pragma comment
type: issue
state: closed
author: hynek
labels:
  - documentation
  - suppression
assignees: []
created_at: 2025-11-03T06:58:14Z
updated_at: 2025-11-21T09:24:11Z
url: https://github.com/astral-sh/ty/issues/1467
synced_at: 2026-01-12T15:54:25Z
```

# Document how to suppress a rule on a line that already has a pragma comment

---

_@hynek_

### Question

I have [code](https://github.com/hynek/svcs/blob/1d6efae1a517eeabc3d283212e6e4bb058a10d85/src/svcs/_core.py#L484
) in _svcs_ that needs to be a bit clever about it treats callables in order to support both sync and async.

I have to suppress PLW2901 (redefinition of loop variable), but with some recent-ish ty release I now also have to suppress `call-non-callable`.

The docs [say](https://docs.astral.sh/ty/suppression/#ty-suppression-comments):

> To suppress a rule violation inline add a `# ty: ignore[<rule>]` comment **at the end of the line**

So I hoped I could just write:

```python
                if iscoroutinefunction(oc):
                    oc = oc()  # noqa: PLW2901 ty: ignore[call-non-callable]

```

but just like Ruff, ty ignores the second part. I can only suppress one: the first one.

What am I missing? I'd expect the case of where I have to silence both the linter and the type checker is quite common, but I haven't found anything on the bug tracker.

I literally don't know what to do, so I've disabled ty for now again.

### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Label `question` added by @hynek on 2025-11-03 06:58_

---

_Label `suppression` added by @AlexWaygood on 2025-11-03 12:08_

---

_Comment by @MichaReiser on 2025-11-03 12:41_

Thanks for the nice write up. 

Either of the following two should work:

```py
                if iscoroutinefunction(oc):
                    oc = oc()  # noqa: PLW2901 # ty: ignore[call-non-callable]
```

```py
                if iscoroutinefunction(oc):
                    oc = oc()  # ty: ignore[call-non-callable] # noqa: PLW2901 
```

The key is to start each `noqa` or `ty: ignore` comment with its own `#`. 

[Here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/suppressions/type_ignore.md#nested-comments) a few examples from our test suite.

Given that this has come up before on the ruff side, I think it does make sense to add an example like this to the documentation

---

_Label `question` removed by @MichaReiser on 2025-11-03 12:44_

---

_Label `documentation` added by @MichaReiser on 2025-11-03 12:44_

---

_Renamed from "How to combine ty's suppression with Ruff's?" to "Document how to suppress a rule on a line that already has a pragma comment" by @MichaReiser on 2025-11-03 12:45_

---

_Comment by @hynek on 2025-11-03 14:56_

Ah wow yes, that works and with pyrefly too – and nobody has documented it. :D  Thanks!

---

_Closed by @MichaReiser on 2025-11-21 09:24_

---
