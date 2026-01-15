```yaml
number: 2518
title: Incorrect Type Inference for dict.get() with Non-Matching Default Value
type: issue
state: open
author: shuangluoxss
labels: []
assignees: []
created_at: 2026-01-15T14:44:13Z
updated_at: 2026-01-15T14:44:13Z
url: https://github.com/astral-sh/ty/issues/2518
synced_at: 2026-01-15T14:51:03Z
```

# Incorrect Type Inference for dict.get() with Non-Matching Default Value

---

_@shuangluoxss_

### Summary

## Description 

When using dict.get(key, default) and `default` have different type with the dict value, the type checker incorrectly infers the return type

## Minimal Reproduction 
```python
res_dict = {k: [0] for k in ["x", "y", "z"]}
res = res_dict.get("a", 1)
res.append(3)
```
playground link: https://play.ty.dev/2a34c232-5be9-4321-ae0b-cce8683258be
## Expected Behavior
`res` should be inferred as `list[int] | int` and `res.append` should raise type error, like `basedpyright` did

<img width="683" height="83" alt="Image" src="https://github.com/user-attachments/assets/2c479ee5-047f-4b82-a688-5c16f6e1fee6" />

## Actual Behavior
`res` was inferred as `list[Unknown | list[int]] | Unknown` and do not give any error

<img width="990" height="79" alt="Image" src="https://github.com/user-attachments/assets/53cc328c-7515-4157-86fe-39459a397404" />

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---
