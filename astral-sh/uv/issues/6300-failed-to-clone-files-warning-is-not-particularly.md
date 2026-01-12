```yaml
number: 6300
title: "\"Failed to clone files\" warning is not particularly useful"
type: issue
state: closed
author: dimaqq
labels:
  - error messages
  - cli
assignees: []
created_at: 2024-08-21T05:20:21Z
updated_at: 2024-08-25T16:02:09Z
url: https://github.com/astral-sh/uv/issues/6300
synced_at: 2026-01-12T15:59:02Z
```

# "Failed to clone files" warning is not particularly useful

---

_@dimaqq_

I'm getting this warning every time in these 2 circumstances:
- host, macos, separate code-sensitive fs volume for code
- vm, linux, "folder" shared from host

Frankly, if I use `uv`, I use it for the result (functional vevn) first, and speed only second. Copying files is reasonably fast too at 265 milliseconds (warm run).

I would argue that this message belongs to debug, rather than a prominent warning.

```command
> uv venv -p python3.13 --seed .host-3.13
Using Python 3.13.0rc1 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtualenv with seed packages at: .host-3.13
warning: Failed to clone files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, reflinking may not be supported.
         If this is intentional, set `UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
 + pip==24.2
Activate with: source .host-3.13/bin/activate.fish
```

`uv 0.3.0 (Homebrew 2024-08-20)`


---

_Comment by @zanieb on 2024-08-21 12:41_

See also discussion at https://github.com/astral-sh/uv/issues/6101

---

_Label `error messages` added by @zanieb on 2024-08-21 12:42_

---

_Label `cli` added by @zanieb on 2024-08-21 12:42_

---

_Comment by @charliermarsh on 2024-08-21 13:18_

I actually think this is somewhat useful since people tend not to realize that it's happening. I'd suggest setting `export UV_LINK_MODE=copy` or similar to make the behavior explicit on your end.

---

_Comment by @dimaqq on 2024-08-22 08:49_

Not useful for me as a user (N=1 statistic and personal bias).

I don't know how common my use case is, let's say, hypothetically it affects 5% of uses and the tool becomes popular. Is it fair to ask all these users to add an env var globally?

> I actually think this is somewhat useful since people tend not to realize that it's happening. 

Why is it important to know?

What does it mean for an end user, in practice?

---

_Comment by @konstin on 2024-08-22 09:26_

It was added because copying instead of hardlinking means a major performance degradation, for example in a transformers test case:

```
$ hyperfine --warmup 1 --prepare "rm -rf .venv" "uv sync --extra torch --extra onnx --extra audio" "uv sync --extra torch --extra onnx --extra audio --link-mode=copy" 
Benchmark 1: uv sync --extra torch --extra onnx --extra audio
  Time (mean ± σ):     114.5 ms ±   8.8 ms    [User: 76.5 ms, System: 350.1 ms]
  Range (min … max):   100.1 ms … 133.1 ms    13 runs
 
Benchmark 2: uv sync --extra torch --extra onnx --extra audio --link-mode=copy
  Time (mean ± σ):     607.5 ms ±   9.4 ms    [User: 87.6 ms, System: 2618.9 ms]
  Range (min … max):   590.0 ms … 627.8 ms    10 runs
 
Summary
  uv sync --extra torch --extra onnx --extra audio ran
    5.31 ± 0.42 times faster than uv sync --extra torch --extra onnx --extra audio --link-mode=copy
```

---

_Comment by @dimaqq on 2024-08-22 12:30_

On one hand, I surly want modern tools to be fast, especially in use cases like `pre-commit` and one day `uvx`.

On the other hand, if someone installs PyTorch or onx, I would presume that they use these packages, and if that’s to train a model we’re taking about hours of use time, perhaps 400ms extra venv install time doesn’t matter that much?

---

_Comment by @charliermarsh on 2024-08-25 16:02_

I hear you, but I think it's reasonable to show this if the default isn't working as it's supposed to. A lot of users didn't know this was happening. You can set `UV_LINK_MODE=copy` everywhere if you'd rather not see it.

---

_Closed by @charliermarsh on 2024-08-25 16:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 16:02_

---
