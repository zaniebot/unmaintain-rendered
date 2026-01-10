```yaml
number: 2156
title: Log build output
type: pull_request
state: closed
author: konstin
labels:
  - tracing
assignees: []
base: main
head: konsti/log-build-output
created_at: 2024-03-04T14:46:04Z
updated_at: 2024-09-03T01:25:02Z
url: https://github.com/astral-sh/uv/pull/2156
synced_at: 2026-01-10T12:53:31Z
```

# Log build output

---

_Pull request opened by @konstin on 2024-03-04 14:46_

Show source distribution build output in debug (`-v`) logging.

Example output for `uv pip install --no-cache-dir --no-binary :all: -v tqdm 2> stderr.txt`: [gist](https://gist.github.com/konstin/61758c6bd5f4294cfc048fea18d5cca3).

I'd suggest taking hierarchical layer out of the default `-v`, i find the indentation makes it hard to read and the spans are generally not that informative compare to the actual log messages.

I've also added anstream filtering to the log output after seeing ansi codes in my log files.

Fixes #2146
Partially addresses #1567 - It doesn't show output while compiling but only after. We can consider i meanwhile but this is a much larger project due to complexity around pipes.

---

_Comment by @charliermarsh on 2024-03-08 21:33_

My gut is that this needs to be streamed in order to be useful, even though it makes it much more complex. Otherwise, it still feels like uv is hanging and you don't have clarity into what's going on or why.

---

_Comment by @alex on 2024-03-08 21:37_

For the interactive use case, yes, 99% of the value is in streaming it. I do think there are some benefits to having it available for later review in contexts like CI logs, even if it's printed in batch.

---

_Label `tracing` added by @konstin on 2024-03-10 14:05_

---

_Comment by @henryiii on 2024-04-02 11:55_

Can't this forward the TTY info? At least when streaming is supported, but it seems like it might make sense to do it correctly now? Scikit-build-core is careful to produce nicely colored output if printing to a console, and it works with `build`. If you direct to file, then it doesn't produce the ansi codes. `pip` processes the output and messes with it (not a fan), so it doesn't work there, it can't detect that it's a terminal or get the width. Also GHA supports ANSI codes.

---

_Comment by @konstin on 2024-04-02 12:56_

The main challenge is that we build in parallel. We can't plain forward the output as a single tenant builder like scikit-build-core can, we need to somehow wrap them with context. I'd love to have something like `docker buildx` which gives tmux-like windows into the build process, but that seems out of scope. I agree that i would be nice to preserve colours, especially for stderr.

---

_Comment by @henryiii on 2024-04-02 14:33_

IMO, `uv pip install .` and `uv pip install -e .` could be treated specially - you usually want the output for those. Not sure if that would help, and it wouldn't solve the general case, but just a thought. (You also don't want to cache them, which I've run into `uv pip install .` doing).

---

_Comment by @pradyunsg on 2024-04-05 22:33_

FWIW, pip's model for local package builds is that it doesn't cache anything about them -- they get rebuilt + reinstalled on every run.

---

_Closed by @charliermarsh on 2024-09-03 01:24_

---

_Comment by @charliermarsh on 2024-09-03 01:25_

Should be handled by #6903.

---
