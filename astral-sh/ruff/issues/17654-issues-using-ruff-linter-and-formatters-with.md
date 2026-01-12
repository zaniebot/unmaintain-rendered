```yaml
number: 17654
title: "Issues using ruff linter and formatters with `jaxtyping`"
type: issue
state: closed
author: Doublonmousse
labels: []
assignees: []
created_at: 2025-04-27T11:35:24Z
updated_at: 2025-04-28T06:24:45Z
url: https://github.com/astral-sh/ruff/issues/17654
synced_at: 2026-01-12T15:54:56Z
```

# Issues using ruff linter and formatters with `jaxtyping`

---

_@Doublonmousse_

### Summary

The following code 
```python
import numpy as np
from jaxtyping import Float
from typing import Annotated


def main():
    A: Float[np.ndarray, "n m"] = np.zeros((10, 10))  # noqa: F722
    x: Float[np.ndarray, "m"] = np.zeros(10)  # noqa: F821
    print(mul(A, x))

    # no issue with annotated however
    _: Annotated[int, "n"] = 12
    _: Annotated[int, "n m"] = 12
```` 
Raises lint rules F722 and F821. However, afaik, `jaxtyping` uses `Annotated` under the hood and doing the same type of annotation using `Annotated` instead does not raise the same error.


The other issue I have is that the ruff formatter does not prevent noqa comments from staying on the same line pre and post formatting the code. Hence a noqa comment won't act on the same part of the code after formatting with ruff. So for example, the following function raises F722 and F821
```py
def mul(A: Float[np.ndarray, "n m"], x:Float[np.ndarray,"m"])->Float[np.ndarray,"m"]:
    return A @ x
```
Then, suppressing the first F722 warning gives
```
def mul(A: Float[np.ndarray, "n m"], x:Float[np.ndarray,"m"])->Float[np.ndarray,"m"]:  # noqa: F722
    return A @ x
```
Where the noqa acts on A,x and the return value and then formatting gives
```py
def mul(
    A: Float[np.ndarray, "n m"], x: Float[np.ndarray, "m"]
) -> Float[np.ndarray, "m"]:  # noqa: F722
    return A @ x
````
raising F722 again because the noqa is on the wrong line (acting on the return value but no more on A nor x). Putting the noqa back to the correct line gives
```py
def mul(
    A: Float[np.ndarray, "n m"], x: Float[np.ndarray, "m"] # noqa: F722
) -> Float[np.ndarray, "m"]:  
    return A @ x
```
which will format to 
```py
def mul(
    A: Float[np.ndarray, "n m"],
    x: Float[np.ndarray, "m"],  # noqa: F722
) -> Float[np.ndarray, "m"]:
    return A @ x
```
raising the F722 error again. The end formatted result with warning suppressed (and not changing with a `Format document`) is 
```py
def mul(
    A: Float[np.ndarray, "n m"],  # noqa: F722
    x: Float[np.ndarray, "m"],  # noqa: F821
) -> Float[np.ndarray, "m"]:  # noqa: F821
    return A @ x
```


Using ruff 0.11.7, confirmed from vscode's terminal
````
2025-04-27 13:22:33.185 [info] Found Ruff 0.11.7 at ...
````

### Version

ruff 0.11.7

---

_Comment by @Daverball on 2025-04-27 14:36_

This is a duplicate of #17386 

See that issue for a possible solution and a potential workaround that could be used in the meantime.

---

_Comment by @MichaReiser on 2025-04-28 06:24_

I'll merge the formatter noqa feedback into https://github.com/astral-sh/ruff/issues/12901 and close this in favor of 

---

_Closed by @MichaReiser on 2025-04-28 06:24_

---
