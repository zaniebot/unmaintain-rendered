```yaml
number: 12901
title: "(🎁) preserve node line of `noqa` comments when formatting"
type: issue
state: open
author: KotlinIsland
labels:
  - suppression
  - wish
  - incompatibility
assignees: []
created_at: 2024-08-15T05:29:54Z
updated_at: 2025-04-28T06:26:06Z
url: https://github.com/astral-sh/ruff/issues/12901
synced_at: 2026-01-12T15:54:52Z
```

# (🎁) preserve node line of `noqa` comments when formatting

---

_@KotlinIsland_

```py
dict(aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa=1)  # noqa: C408
```

# after format (fail)
```py
dict(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa=1
)  # noqa: C408
```

ruff is doing the `check`ing of this `noqa`, ruff is also the one doing the `format`ing, so it would seem logical that ruff use the `check` information to keep the `noqa` on the right line
```py
dict(  # noqa: C408
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa=1
)
```

# Additionally

this isn't exclusive to `format` as changes from `check` can also conflict, see #12179

related:
- #8232
- #12179

---

_Comment by @MichaReiser on 2024-08-15 06:05_

I understand why this is desired, but it has quite far-reaching implications, and it would significantly slow down formatting because formatting would require running the linter, a trade-off that I think isn't worth the occasional move of a noqa comment. 

Long-term, this does not just apply to running the linter but also type-checking and there are suppression comments that ruff doesn't even understand. So the problem doesn't go away, it might just happen less often (which would already be nice).

To me the most likely solution is redesigning suppression comments so that it is known what target they suppress by being more explicit about where they can be placed. E.g. only allow own-line suppression comments, maybe even on a statement level. 

---

_Label `suppression` added by @MichaReiser on 2024-08-15 06:06_

---

_Label `incompatibility` added by @MichaReiser on 2024-08-15 06:06_

---

_Label `wish` added by @MichaReiser on 2024-08-15 06:06_

---

_Comment by @KotlinIsland on 2024-08-15 07:48_

99% of the time I want to both format and check, currently it's two separate steps, and I would presume that would make it take longer

Personally I would love to trade speed for more correctness, but know not everyone thinks the same

---

_Comment by @filips123 on 2024-08-28 10:51_

I have the same problem with with mypy's `type: ignore` comments.

For example, the following line:

```py
    def parse_document(self, document: DocumentInfo, stream: BytesIO, effective: datetime.date, span: Span) -> None:  # type: ignore[override]
        ...
```

Gets reformatted as:

```py
    def parse_document(
        self, document: DocumentInfo, stream: BytesIO, effective: datetime.date, span: Span
    ) -> None:  # type: ignore[override]
        ...
```

Which makes mypy unable to detect the ignore comment, as it has been moved to the incorrect place.

Black prevents splitting lines that contain `type: ignore` comments. Maybe this could be added as an option to Ruff formatter?

---

_Comment by @PamelaM on 2024-09-12 15:04_

Another example I've run into:
Here's my one_line.py file before ruff:
```
from implementations.endpoints.name_of_some_module import very_very_long_name_of_some_function # noqa: F401
```

```
$ ruff check one_line.py
All checks passed!
$ ruff format one_line.py
1 file reformatted
$ ruff check one_line.py
... cut details ...
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Reformatted file:
```
from implementations.endpoints.name_of_some_module import (
    very_very_long_name_of_some_function,
) # noqa: F401
```

Workaround:
```
# fmt: off
from implementations.endpoints.name_of_some_module import very_very_long_name_of_some_function # noqa: F401
# fmt: on
```


---

_Comment by @KotlinIsland on 2024-09-12 23:43_

@PamelaM that looks very similar to #12179

---

_Comment by @MichaReiser on 2025-04-28 06:25_

> ### Summary
> 
> The following code
> 
> import numpy as np
> from jaxtyping import Float
> from typing import Annotated
> 
> 
> def main():
>     A: Float[np.ndarray, "n m"] = np.zeros((10, 10))  # noqa: F722
>     x: Float[np.ndarray, "m"] = np.zeros(10)  # noqa: F821
>     print(mul(A, x))
> 
>     # no issue with annotated however
>     _: Annotated[int, "n"] = 12
>     _: Annotated[int, "n m"] = 12
> 
> Raises lint rules F722 and F821. However, afaik, `jaxtyping` uses `Annotated` under the hood and doing the same type of annotation using `Annotated` instead does not raise the same error.
> 
> The other issue I have is that the ruff formatter does not prevent noqa comments from staying on the same line pre and post formatting the code. Hence a noqa comment won't act on the same part of the code after formatting with ruff. So for example, the following function raises F722 and F821
> 
> def mul(A: Float[np.ndarray, "n m"], x:Float[np.ndarray,"m"])->Float[np.ndarray,"m"]:
>     return A @ x
> 
> Then, suppressing the first F722 warning gives
> 
> ```
> def mul(A: Float[np.ndarray, "n m"], x:Float[np.ndarray,"m"])->Float[np.ndarray,"m"]:  # noqa: F722
>     return A @ x
> ```
> 
> Where the noqa acts on A,x and the return value and then formatting gives
> 
> def mul(
>     A: Float[np.ndarray, "n m"], x: Float[np.ndarray, "m"]
> ) -> Float[np.ndarray, "m"]:  # noqa: F722
>     return A @ x
> 
> raising F722 again because the noqa is on the wrong line (acting on the return value but no more on A nor x). Putting the noqa back to the correct line gives
> 
> def mul(
>     A: Float[np.ndarray, "n m"], x: Float[np.ndarray, "m"] # noqa: F722
> ) -> Float[np.ndarray, "m"]:  
>     return A @ x
> 
> which will format to
> 
> def mul(
>     A: Float[np.ndarray, "n m"],
>     x: Float[np.ndarray, "m"],  # noqa: F722
> ) -> Float[np.ndarray, "m"]:
>     return A @ x
> 
> raising the F722 error again. The end formatted result with warning suppressed (and not changing with a `Format document`) is
> 
> def mul(
>     A: Float[np.ndarray, "n m"],  # noqa: F722
>     x: Float[np.ndarray, "m"],  # noqa: F821
> ) -> Float[np.ndarray, "m"]:  # noqa: F821
>     return A @ x
> 
> Using ruff 0.11.7, confirmed from vscode's terminal
> 
> ```
> 2025-04-27 13:22:33.185 [info] Found Ruff 0.11.7 at ...
> ```
> 
> ### Version
> 
> ruff 0.11.7

*Originally posted by @Doublonmousse in https://github.com/astral-sh/ruff/issues/17654#issue-3023024254*

---
