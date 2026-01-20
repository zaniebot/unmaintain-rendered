```yaml
number: 22759
title: numpy 2.4 function removals
type: issue
state: open
author: JohannesBuchner
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-20T09:28:18Z
updated_at: 2026-01-20T17:16:00Z
url: https://github.com/astral-sh/ruff/issues/22759
synced_at: 2026-01-20T17:37:00Z
```

# numpy 2.4 function removals

---

_@JohannesBuchner_

### Summary

Dear all,

ruff check supports testing for numpy 2.0 deprecations.

I was wondering if there are any plans to also check for [numpy 2.4 deprecations](https://numpy.org/doc/2.4/release/2.4.0-notes.html#expired-deprecations):

numpy.linalg.linalg -> Use numpy.linalg instead.
numpy.fft.helper -> Use numpy.fft instead.
numpy.in1d -> Use numpy.isin instead
numpy.trapz -> Use numpy.trapezoid instead (or scipy.integrate)
numpy.disp
numpy.ndindex.ndincr

Also, it may be worth supporting some [scipy function/module removals](https://docs.scipy.org/doc/scipy/release.html):

scipy.odr 
scipy.stats.find_repeats
scipy.linalg.kron
scipy.special.sph_harm
scipy.special.clpmn
scipy.special.lpn
scipy.special.lpmn
scipy.sparse.conjtransp
scipy.signal.daub
scipy.signal.qmf
scipy.signal.cascade
scipy.signal.morlet
scipy.signal.morlet2
scipy.signal.ricker
scipy.signal.cwt
scipy.signal.cmplx_sort
scipy.integrate.quadrature
scipy.integrate.romberg
scipy.stats.rvs_ratio_uniforms
scipy.special.btdtr
scipy.special.btdtri
scipy.misc.*
scipy.sparse.*.{asfptype,getrow,getcol,get_shape,getmaxprint,set_shape,getnnz,getformat}
scipy.integrate.simps -> simpson
scipy.integrate.trapz -> trapezoid
scipy.integrate.cumtrapz -> cumulative_trapezoid
scipy.interpolate.interp2d






---

_Comment by @ntBre on 2026-01-20 17:15_

Thanks for the suggestion! I think some of these 2.4 deprecations are already included in [numpy2-deprecation (NPY201)](https://docs.astral.sh/ruff/rules/numpy2-deprecation/#numpy2-deprecation-npy201). I see [`numpy.trapz`](https://github.com/astral-sh/ruff/blob/a6752942ad0d083bf2df0c2992499090257cfe7f/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L554), [`numpy.n1d`](https://github.com/astral-sh/ruff/blob/a6752942ad0d083bf2df0c2992499090257cfe7f/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L324), and [`numpy.disp`](https://github.com/astral-sh/ruff/blob/a6752942ad0d083bf2df0c2992499090257cfe7f/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L286) at least. We could probably just expand that rule with the few missing ones for 2.4.

scipy would probably require a new rule, which we might need a bit more consensus on, but I'd be open to the idea.

---

_Label `rule` added by @ntBre on 2026-01-20 17:16_

---

_Label `needs-decision` added by @ntBre on 2026-01-20 17:16_

---
