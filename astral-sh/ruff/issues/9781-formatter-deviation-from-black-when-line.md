```yaml
number: 9781
title: "Formatter: deviation from Black when line splitting subscript assignment"
type: issue
state: closed
author: rhynix
labels:
  - formatter
  - style
assignees: []
created_at: 2024-02-02T10:43:18Z
updated_at: 2024-02-02T11:22:31Z
url: https://github.com/astral-sh/ruff/issues/9781
synced_at: 2026-01-12T15:54:49Z
```

# Formatter: deviation from Black when line splitting subscript assignment

---

_@rhynix_

Ruff splits lines differently when the line length exceeds the configured line length, and the line contains assignment using subscript notation. For instance, when assigning to a dict.

I had a look at the known deviations, and at other open issues, but could not find this deviation.

Input:

```python
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = "ccccccccccc"
```

Black output:

```python
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = (
    "ccccccccccc"
)
```

Ruff (`ruff format --isolated file.py`) output:

```python
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa[
    "bbbbbbbbbbbbbbbbbbbb"
] = "ccccccccccc"
```

Ruff version:

```
% ruff version
ruff 0.2.0
```

---

_Renamed from "Formatter: different line splitting on subscript assignment" to "Formatter: deviation from Black when line splitting subscript assignment" by @rhynix on 2024-02-02 10:43_

---

_Label `formatter` added by @AlexWaygood on 2024-02-02 11:00_

---

_Label `style` added by @AlexWaygood on 2024-02-02 11:00_

---

_Comment by @rhynix on 2024-02-02 11:03_

Black used to use the style ruff currently uses, but was changed late 2022.

This was the issue: https://github.com/psf/black/issues/1498
This was the PR: https://github.com/psf/black/pull/3368

---

_Comment by @MichaReiser on 2024-02-02 11:03_

Thanks for reporting this issue.

Is it possible that you use/used black 24? Or what black version are you using?

---

_Comment by @rhynix on 2024-02-02 11:05_

> Is it possible that you use/used black 24? Or what black version are you using?

That is correct.

```
% black --version
black, 24.1.1 (compiled: no)
Python (CPython) 3.8.18
```

---

_Comment by @rhynix on 2024-02-02 11:08_

I can confirm this change was introduced in Black 24.1:

https://github.com/psf/black/blob/main/CHANGES.md#2410

> If an assignment statement is too long, we now prefer splitting on the right-hand side (#3368)

Setting the format.preview setting to true changes this behaviour to match Black.

---

_Comment by @dhruvmanila on 2024-02-02 11:19_

So, Black 24.1 moved a lot of preview style rules into it's stable style (https://github.com/psf/black/issues/4042). We've implemented the same in our own preview style formatting which can be enabled through `--preview` flag. Refer to the [preview docs](https://docs.astral.sh/ruff/preview/) for more details.

If you run the mentioned code snippet under the preview style then you'd get the same output:
```console
ruff format --preview --diff
```

```diff
--- /Users/dhruv/playground/ruff/formatter/isolated.py
+++ /Users/dhruv/playground/ruff/formatter/isolated.py
@@ -1 +1,3 @@
-aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = "ccccccccccc"
+aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = (
+    "ccccccccccc"
+)
```

You can track the progress of the pull request which will promote the same preview style rules into stable style for Ruff here: https://github.com/astral-sh/ruff/pull/9639

I hope this helps :)

---

_Comment by @rhynix on 2024-02-02 11:22_

> So, Black 24.1 moved a lot of preview style rules into it's stable style ([psf/black#4042](https://github.com/psf/black/issues/4042)). We've implemented the same in our own preview style formatting which can be enabled through `--preview` flag. Refer to the [preview docs](https://docs.astral.sh/ruff/preview/) for more details.
> 
> If you run the mentioned code snippet under the preview style then you'd get the same output:
> 
> ```
> ruff format --preview --diff
> ```
> 
>  ```diff
> --- /Users/dhruv/playground/ruff/formatter/isolated.py
> +++ /Users/dhruv/playground/ruff/formatter/isolated.py
> @@ -1 +1,3 @@
> -aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = "ccccccccccc"
> +aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa["bbbbbbbbbbbbbbbbbbbb"] = (
> +    "ccccccccccc"
> +)
> ```
> 
> You can track the progress of the pull request which will promote the same preview style rules into stable style for Ruff here: #9639
> 
> I hope this helps :)

It sure does. Thank you for your help.

I'll close this issue, since I believe it does not provide any extra value at this point.

---

_Closed by @rhynix on 2024-02-02 11:22_

---
