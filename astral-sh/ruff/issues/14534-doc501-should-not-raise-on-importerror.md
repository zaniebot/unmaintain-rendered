```yaml
number: 14534
title: DOC501 should not raise on ImportError
type: issue
state: closed
author: Matt-Ord
labels:
  - question
  - docstring
assignees: []
created_at: 2024-11-22T16:46:49Z
updated_at: 2024-11-25T09:23:05Z
url: https://github.com/astral-sh/ruff/issues/14534
synced_at: 2026-01-10T11:09:56Z
```

# DOC501 should not raise on ImportError

---

_Issue opened by @Matt-Ord on 2024-11-22 16:46_

I'm getting a 'false positive' on DOC501 when my package has an optional dependency on matplotlib
```
def get_axis_colorbar(axis: Axes) -> Colorbar | None:
    """Get a colorbar attached to the axis."""
    if plt is None:
        msg = "Matplotlib is not installed. Please install it with the 'plot' extra."
        raise ImportError(msg) # Ruff warns here
    for artist in axis.get_children():
        if isinstance(artist, plt.cm.ScalarMappable) and artist.colorbar is not None:
            return artist.colorbar
    return None
```

Using ruff 0.8. It might be nice to be able to exclude ImportError from this rule, as it is not an error which a user would be 'expected' to see during runtime.



---

_Comment by @tjkuson on 2024-11-22 21:58_

Hi, not a maintainer, just someone interested in the `DOC` rules! Would you mind explaining why `ImportError` is special? My intuition (perhaps misguided) is that I would quite like a function that specifically raises `ImportError` to document that behaviour. To me, the fact that it's an exception type that's raised explicitly rarely seems to make it all the more worth documenting.

---

_Comment by @Matt-Ord on 2024-11-22 22:07_

The thought was that it was not really a 'runtime error' as I would expect to document to a user but an install time error. If it is just the result of a user not properly installing downstream dependencies, I would say it would probably be better just to document the required dependency but not the error in this case.

---

_Label `docstring` added by @AlexWaygood on 2024-11-22 22:14_

---

_Label `question` added by @MichaReiser on 2024-11-25 09:00_

---

_Comment by @MichaReiser on 2024-11-25 09:16_

> The thought was that it was not really a 'runtime error' as I would expect to document to a user but an install time error. 

It's a nice touch that you hint users towards installing Matplotlib with the `plot` extra. But I do agree with @tjkuson that I would expect the exception to be documented if I opted into `DOC501` because a missing extra otherwise manifests as an `AttributeError` and not necessarily as an `ImportError`. The difference might surprise someone calling `get_axis_colorbar` as a library-function. 

In your situation, I would suppress the violation with `# noqa: DOC501` or consider doing the check earlier, maybe in a main function or when initializing the library to avoid repeating it over multiple functions. Unless this function belongs to a subset of functions that are only sometimes supported (e.g. when installed with an extra), then raising inside the specific library function does make sense, but I would then also expect this to be documented. 




---

_Comment by @Matt-Ord on 2024-11-25 09:21_

Thanks for taking the time to respond - this is indeed part of a subset of functions, rather than a core feature which is the reason behind the 'plot' extra. I'll close the issue in this case.

---

_Closed by @Matt-Ord on 2024-11-25 09:21_

---
