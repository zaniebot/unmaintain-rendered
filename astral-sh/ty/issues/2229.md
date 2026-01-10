---
number: 2229
title: Narrowing to Any/Unknown loses type information
type: issue
state: open
author: Decodetalkers
labels:
  - needs-decision
  - type-inference
assignees: []
created_at: 2025-12-26T15:22:22Z
updated_at: 2025-12-26T17:29:24Z
url: https://github.com/astral-sh/ty/issues/2229
synced_at: 2026-01-10T01:52:52Z
---

# Narrowing to Any/Unknown loses type information

---

_Issue opened by @Decodetalkers on 2025-12-26 15:22_

### Summary

```py
import numpy as np
import matplotlib.pyplot as plt
from model import Perception

data = np.genfromtxt("perceptron_toydata.txt", delimiter="\t")
X: np.ndarray
Y: np.ndarray
X, Y = data[:, :2], data[:, 2]
Y = Y.astype(np.int64)

print("Class label counts:", np.bincount(Y))
print("X.shape:", X.shape)
print("y.shape:", Y.shape)

shuffle_idx = np.arange(Y.shape[0])
shuffle_rng = np.random.RandomState(123)
shuffle_rng.shuffle(shuffle_idx)
X = X[shuffle_idx]
Y = Y[shuffle_idx]

X_train: np.ndarray
X_test: np.ndarray
Y_train: np.ndarray
Y_test: np.ndarray

X_train, X_test = X[shuffle_idx[:70]], X[shuffle_idx[70:]]
Y_train, Y_test = Y[shuffle_idx[:70]], Y[shuffle_idx[70:]]
```

So I have already mark the X_train is np.ndarray, but the type hint did not work

<img width="1147" height="409" alt="Image" src="https://github.com/user-attachments/assets/bb36032b-25f5-48d3-8253-966957760229" />

### Version
ty 0.0.7


---

_Comment by @ntBre on 2025-12-26 16:01_

Thanks for the report! I'll transfer this to the ty repo.

I think this might be related to https://github.com/astral-sh/ty/issues/136, but someone else will probably have a better answer than I do :)

---

_Renamed from "ty: Type Hint does not work very well" to "Type Hint does not work very well" by @ntBre on 2025-12-26 16:02_

---

_Renamed from "Type Hint does not work very well" to "Narrowing to Any/Unknown loses type information" by @carljm on 2025-12-26 16:53_

---

_Comment by @carljm on 2025-12-26 16:56_

This is definitely closely related to #136, but I'll keep it open as a separate sub-issue; the plan in #136 is to prefer the annotation over the inferred type in an annotated assignment, but that actually wouldn't help in this case. In this case we either want to avoid "narrowing" to `Any` / `Unknown` at all, or we want to narrow to the intersection (`ndarray & Unknown`).

---

_Added to milestone `Stable` by @carljm on 2025-12-26 16:56_

---

_Label `needs-decision` added by @carljm on 2025-12-26 17:29_

---

_Label `type-inference` added by @carljm on 2025-12-26 17:29_

---
