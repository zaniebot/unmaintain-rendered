```yaml
number: 18247
title: "Add `torch.Tensor.requires_grad_` to `flake8-boolean-trap` allowlist"
type: issue
state: closed
author: danra
labels:
  - question
  - needs-info
assignees: []
created_at: 2025-05-21T21:18:41Z
updated_at: 2025-10-16T15:01:47Z
url: https://github.com/astral-sh/ruff/issues/18247
synced_at: 2026-01-10T11:09:58Z
```

# Add `torch.Tensor.requires_grad_` to `flake8-boolean-trap` allowlist

---

_Issue opened by @danra on 2025-05-21 21:18_

### Summary

I've [tried](https://github.com/astral-sh/ruff/issues/11264#issuecomment-2899262171) extending the whitelist myself, but wasn't able to. Perhaps this possible with the internal whitelist? Or if not, it would still be good to add this to the whitelist whenever it does become possible.

---

_Comment by @MichaReiser on 2025-05-22 06:08_

Can you share a more complete example. How are you using `torch.Tensor.requires_grad_`? How is it different from https://github.com/astral-sh/ruff/issues/11264?

---

_Label `question` added by @MichaReiser on 2025-05-22 06:08_

---

_Comment by @danra on 2025-05-22 18:19_

From https://docs.pytorch.org/docs/stable/notes/autograd.html#autograd-for-complex-numbers :

> To freeze parts of your model, simply apply `.requires_grad_(False)` to the parameters that you don’t want updated. And as described above, since computations that use these parameters as inputs would not be recorded in the forward pass, they won’t have their `.grad` fields updated in the backward pass because they won’t be part of the backward graph in the first place, as desired.

One more use case is to just completely stop tracking gradients once you're done with the backprop but are still transforming some values that participated in it.

> How is it different from https://github.com/astral-sh/ruff/issues/11264?

This issue is for adding this specific function to the built-in whitelist. The other issue is about whitelist extension capabilities in general, and about how currently the function can't be whitelisted by the user.

---

_Renamed from "Add torch.Tensor.requires_grad_() to flake8-boolean-trap whitelist" to "Add torch.Tensor.requires_grad_() to flake8-boolean-trap allowlist" by @MichaReiser on 2025-05-25 11:00_

---

_Renamed from "Add torch.Tensor.requires_grad_() to flake8-boolean-trap allowlist" to "Add `torch.Tensor.requires_grad_` to `flake8-boolean-trap` allowlist" by @MichaReiser on 2025-05-25 11:01_

---

_Comment by @MichaReiser on 2025-05-25 11:03_

Could you share an example of how `requires_grad_` is used. I'm not familiar with tensor and the reference link has way too much content that doesn't seem relevant.



---

_Closed by @MichaReiser on 2025-07-24 12:03_

---

_Label `needs-info` added by @MichaReiser on 2025-07-24 12:04_

---
