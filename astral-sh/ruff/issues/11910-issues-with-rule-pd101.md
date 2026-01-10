```yaml
number: 11910
title: Issues with rule PD101.
type: issue
state: open
author: randolf-scholz
labels:
  - rule
  - great writeup
assignees: []
created_at: 2024-06-17T19:17:21Z
updated_at: 2024-06-18T14:12:32Z
url: https://github.com/astral-sh/ruff/issues/11910
synced_at: 2026-01-10T11:09:53Z
```

# Issues with rule PD101.

---

_Issue opened by @randolf-scholz on 2024-06-17 19:17_

There are multiple issues with [Rule PD101](https://docs.astral.sh/ruff/rules/pandas-nunique-constant-series-check/):

1. `(s[0] == s).all()` can actually be slower than `s.nunique() <=1`, for example when testing with time series data, as in pandas `s[0]` converts to `pd.Timedelta` type, which leads to overhead for shorter series:

    ```python
    import pandas as pd
    import numpy as np
    t = np.random.rand(100_000) * np.timedelta64(1, "s")
    s3 = pd.Series(t[:10**3])
    s4 = pd.Series(t[:10**4])
    s5 = pd.Series(t[:10**5])
    %timeit s3.nunique() <=1    # 51.4 µs ± 768 ns
    %timeit (s3[0] == s).all()  # 69.1 µs ± 123 ns
    %timeit s4.nunique() <=1    # 78.9 µs ± 197 ns
    %timeit (s4[0] == s).all()  # 69.2 µs ± 62.6 ns
    %timeit s5.nunique() <=1    # 375 µs ± 1.14 µs
    %timeit (s5[0] == s).all()  # 70.5 µs ± 1.5 µs
    ```
    
    This can be circumvented by testing `(s.values[0] == s.values).all()` instead. (Note: we actually should be using `.array` instead of `.values`!)
    See this graphic: https://github.com/pandas-dev/pandas/issues/54033.
    ![image](https://github.com/astral-sh/ruff/assets/39696536/af58e415-b620-4e56-a9f8-feefb492b931)

2. `(s[0] == s).all()` can yield different results than `s.nunique() <= 1`, for instance if the first entry in the series happens to be NaN / missing.

    ```python
    import pandas as pd
    s = pd.Series([None, 1.0, 1.0, 1.0])
    assert s.nunique() <= 1   # checks, since dropna=True by default
    assert (s[0] == s).all()  # fails, since comparison with NaN is falsy
    ```

   Note that it can also yield wrong answers in the other direction:

    ```python
    import pandas as pd
    s = pd.Series([None, 1, 1, 3], dtype="Int64")
    assert (s[0] == s).all()  # incorrectly passes
    assert s.nunique() <= 1   # correctly fails
    ```

3. `(s[0] == s).all()` will fail if the Series happens to be empty. Empty series can happen naturally, for example, consider slicing a 1h-interval of an irregularly sampled time series.

    ```python
    import pandas as pd
    s = pd.Series([], dtype=float)
    assert s.nunique() <= 1   # checks
    assert (s[0] == s).all()  # KeyError
    ```

4. The stated rationale is technically incorrect:
  
    > In general, .nunique() requires iterating over the entire Series, while a more efficient approach allows short-circuiting the operation as soon as a non-equal value is found.
  
    But `s[0] == s` performs the comparison for all elements, hence the runtime is O(N) regardless. An actual short-circuiting would look something like this:

    ```python
    import pandas as pd
    import numpy as np
    
    def is_constant(array):
        if len(s) <= 1:
            return True
        first = array[0]
        return all(item == first for item in array)
    
    s = pd.Series(np.random.rand(100_000))
    %timeit (s[0] == s).all()  # 75.5 µs ± 906 ns
    %timeit is_constant(s.values)     # 3.12 µs ± 49.9 ns
    ```

-----

Two things should be changed:

1. The rationale should be rewritten, removing the "short-circuiting" part.
2. The suggested replacement code should be updated to better deal with missing value and extension arrays. In particular, instead of `array = data.to_numpy()` we should consider `array = data.dropna().array`.

```python
import pandas as pd

data = pd.Series([None, 1.0, 1.0, 3.4])

# replace data.nunique() <= 1 with
array = data.dropna().array
if array.shape[0] == 0 or (array[0] == array).all():
    print("Series is constant")

# replace data.nunique(dropna=False) with
array = data.dropna().array
if array.shape[0] == 0 or (data.notna().all() and (array[0] == array).all()):
    print("Series is constant")

# replace data.nunique(dropna=dropna) withdropna=True
array = data.dropna().array
if array.shape[0] == 0 or ((dropna or data.notna().all()) and (array[0] == array).all()):
    print("Series is constant")
```

-----

## EDITS

- **2024-06-18:** For Series we should use `.array` instead of `.values` or `.to_numpy()`, as these construct `numpy` arrays, even if the series is based on a pyarrow-encoded array.
  (see: <https://pandas.pydata.org/docs/reference/api/pandas.Series.values.html>)





---

_Label `rule` added by @MichaReiser on 2024-06-18 05:49_

---

_Label `great writeup` added by @MichaReiser on 2024-06-18 05:49_

---

_Comment by @MichaReiser on 2024-06-18 05:51_

Thanks for the great write up! This is  excellent

---
