```yaml
number: 22759
title: numpy 2.4 function removals
type: issue
state: open
author: JohannesBuchner
labels: []
assignees: []
created_at: 2026-01-20T09:28:18Z
updated_at: 2026-01-20T09:28:18Z
url: https://github.com/astral-sh/ruff/issues/22759
synced_at: 2026-01-20T09:41:31Z
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
