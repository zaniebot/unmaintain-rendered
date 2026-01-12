```yaml
number: 1835
title: Consolidate handling of dataclass-params and dataclass-transformer-params
type: issue
state: open
author: sharkdp
labels:
  - dataclasses
assignees: []
created_at: 2025-12-10T09:10:07Z
updated_at: 2025-12-23T00:42:17Z
url: https://github.com/astral-sh/ty/issues/1835
synced_at: 2026-01-12T15:54:25Z
```

# Consolidate handling of dataclass-params and dataclass-transformer-params

---

_@sharkdp_

The goal of this ticket is to consolidate and clean up our handling of dataclass-params and dataclass-transformer-params for all different kinds of dataclasses: "standard" `dataclasses.dataclass` dataclasses, decorator-based dataclass transformers, base-class-based dataclass transformers, and metaclass-based dataclass transformers.

For more context, see [this comment](https://github.com/astral-sh/ruff/pull/21820#issuecomment-3629221765). In particular, this part:

> I think we never properly added support for default_* params on base-class and metaclass-based transformers, but some contributors added partial support when synthesizing __init__. Instead of that partial support, we should either have generalized methods for querying dataclass params (from anywhere) which always fall back to the dataclass-transformer-params, or we should ensure that dataclass_params on the class itself are always already initialized with values from the transformer param defaults. (It looks like this currently happens for function dataclass transforms but not for metaclass or base-class ones.) It also looks like we store dataclass_transformer_params on the class, but this also isn't currently set for metaclass or base-class transformers. So this special-cased support for base-class and metaclass transformers that was added to __init__ synthesis ought to be generalized, and if that happened this PR wouldn't need to explicitly check the transformer params.

---

_Label `dataclasses` added by @sharkdp on 2025-12-10 09:10_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-10 09:10_

---

_Comment by @sharkdp on 2025-12-18 11:26_

For more context, see also this discussion: https://github.com/astral-sh/ruff/pull/22018/files#r2630663471

---

_Comment by @carljm on 2025-12-23 00:42_

Yeah, sounds like part of this ticket should be some clearer docstrings around `dataclass_params` and `dataclass_transformer_params` fields, that explain when we should/shouldn't expect them to be set, and the right way to access those params (and detect dataclass-ness) reliably.

---
