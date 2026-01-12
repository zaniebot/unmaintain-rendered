```yaml
number: 4753
title: Allow specifying different index strategies for different extra indexes
type: issue
state: open
author: nathanjmcdougall
labels:
  - wish
assignees: []
created_at: 2024-07-03T00:51:41Z
updated_at: 2024-07-28T00:40:54Z
url: https://github.com/astral-sh/uv/issues/4753
synced_at: 2026-01-12T15:58:51Z
```

# Allow specifying different index strategies for different extra indexes

---

_@nathanjmcdougall_

### Summary

This is a request for an enhancement.

I would like to be able to specify a different index strategy for each index specified in `--extra-index-url`.

### Rationale

I have a situation where I have three indexes I want to use:

- `index-url` is set to PyPI
- `extra-index-url` is set to a Pytorch index at https://download.pytorch.org/whl/cu118 followed by another private index.

I would like to use the `first-index` strategy when resolving on the first index (the private one), to protect from dependency confusion attacks in the other two.

But the Pytorch index has out-of-date packages like `packaging==22.0`, so I would like to use the `unsafe-first-match` strategy to allow the dependency resolver to find a compatible version of packages in PyPI even if the package name is found in the Pytorch index first.

### Possible option

I think one way to handle this would be to allow `index-strategy` to be a comma-separated list of options with the same length as `extra-index` e.g. in this case perhaps it would be `unsafe-first-match,first-index`.

I am not sure how `unsafe-best-match` would be used in this way though, possibly just forbid this, although it seems like in some cases you might want to be able to use `unsafe-best-match` for a sequence of consecutive indexes and then the best match would be selected from that the indexes in that subsequence.

Another (related) complication with this approach is that it associates each strategy uniquely with a _pair_ of indexes adjacent in priority order. In general, strategies might be abstract enough such that this is unnecessarily restrictive. In the most abstract case, the user might want to specify a sequence of strategies, each of which is associated with an ordered sequence of indexes, and strategies may re-use the same index multiple times.

---

_Label `wish` added by @charliermarsh on 2024-07-28 00:40_

---
