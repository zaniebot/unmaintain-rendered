```yaml
number: 1705
title: Calling dataclasses.dataclass imperatively (as opposed to as a decorator) produces errors
type: issue
state: closed
author: alex
labels:
  - dataclasses
assignees: []
created_at: 2025-12-01T13:32:56Z
updated_at: 2025-12-03T08:05:27Z
url: https://github.com/astral-sh/ty/issues/1705
synced_at: 2026-01-10T01:56:40Z
```

# Calling dataclasses.dataclass imperatively (as opposed to as a decorator) produces errors

---

_Issue opened by @alex on 2025-12-01 13:32_

### Summary

In pyca/cryptography we have the following code: https://github.com/pyca/cryptography/blob/main/src/cryptography/hazmat/asn1/asn1.py#L191-L212

Running ruff over this produces:

```
error[call-non-callable]: Object of type `<decorator produced by dataclass-like function>` is not callable
   --> src/cryptography/hazmat/asn1/asn1.py:207:29
    |
205 |               )(cls)
206 |           else:
207 |               dataclass_cls = dataclasses.dataclass(
    |  _____________________________^
208 | |                 repr=False,
209 | |                 eq=False,
210 | |             )(cls)
    | |__________________^
211 |           _register_asn1_sequence(dataclass_cls)
212 |           return dataclass_cls
    |
info: rule `call-non-callable` is enabled by default

error[call-non-callable]: Object of type `<decorator produced by dataclass-like function>` is not callable
   --> src/cryptography/hazmat/asn1/asn1.py:220:25
    |
218 |           # Only add an __init__ method, with keyword-only
219 |           # parameters.
220 |           dataclass_cls = dataclasses.dataclass(
    |  _________________________^
221 | |             repr=False,
222 | |             eq=False,
223 | |             match_args=False,
224 | |             kw_only=True,
225 | |         )(cls)
    | |______________^
226 |           _register_asn1_sequence(dataclass_cls)
227 |           return dataclass_cls
    |
info: rule `call-non-callable` is enabled by default
```

Minimal reproducer on the ty playground: https://play.ty.dev/06b57da0-ed6c-4cf8-8c93-642925026161

### Version

ty 0.0.1-alpha.29 (0c3cae494 2025-11-28)

---

_Label `dataclasses` added by @AlexWaygood on 2025-12-01 13:34_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-01 13:41_

---

_Comment by @AlexWaygood on 2025-12-01 13:42_

looks like we were just very lazy here 🙃

https://github.com/astral-sh/ruff/blob/0e651b50b7d0c2614c5958e78525213e759e5b41/crates/ty_python_semantic/src/types.rs#L6201-L6202

---

_Comment by @alex on 2025-12-01 13:45_

Well yes, I suppose that would do it 😂 I've heard that good programmers are lazy though!

Thanks for taking a look!

---

_Added to milestone `Beta` by @carljm on 2025-12-02 01:37_

---

_Closed by @AlexWaygood on 2025-12-03 08:05_

---
