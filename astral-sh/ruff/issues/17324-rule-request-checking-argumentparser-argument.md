```yaml
number: 17324
title: "Rule request: checking `ArgumentParser` argument names"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-04-09T19:52:51Z
updated_at: 2025-04-10T06:23:41Z
url: https://github.com/astral-sh/ruff/issues/17324
synced_at: 2026-01-12T15:54:55Z
```

# Rule request: checking `ArgumentParser` argument names

---

_@jamesbraza_

### Summary

`AttributeError`s can arise from typos in `ArgumentParser`'s args after `parse_args`.

```python
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--arg", type=str)
args = parser.parse_args()
print(args.not_an_arg)
```

This snippet will blow up with `AttributeError: 'Namespace' object has no attribute 'not_an_arg'`, as `not_an_arg` is not an argument.

It would be cool if Ruff could detect this type of error.

---

_Label `rule` added by @ntBre on 2025-04-09 20:48_

---

_Label `type-inference` added by @ntBre on 2025-04-09 20:48_

---

_Comment by @ntBre on 2025-04-09 20:52_

This seems like the kind of thing that would benefit from type inference. I tested [mypy] and [pyright], and they don't currently detect this, though. Maybe red-knot will!

[mypy]: https://mypy-play.net/?mypy=latest&python=3.12&gist=3f37523417f908f8a6a489d9c144f87b
[pyright]: https://pyright-play.net/?pythonVersion=3.8&strict=true&code=JYWwDg9gTgLgBAQygczEgzgUwFDbVLKOAXkRXywDoBBFAVxEwDsYAFDTKACgEo8OolBABNhAfSTIGzGFwBEAWgWS5AGjgwAnmEzF0MKH0noScCp0rmJKdLzxRgLLscpMIMCU2vIeQA

---

_Comment by @MichaReiser on 2025-04-10 06:23_

I think that would even be hard for a type checker, and would require a feature similar to TypeScript's [template literal types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html) that allows defining "dynamic" members (`not_an_arg`). What makes this worse is that `argparse` API allows a lot of customization which impacts the argument names etc, making this hard to represent. It might as well be that Python needs a new argument parser with a typing friendlier API

I'll close this issue because it isn't actionable for us in the short term (even mid-term)

---

_Closed by @MichaReiser on 2025-04-10 06:23_

---
