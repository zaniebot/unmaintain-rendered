```yaml
number: 8197
title: "Proposal: Formatting options to enable \"vertical formatting\" and \"grid formatting\""
type: issue
state: closed
author: NeilGirdhar
labels:
  - formatter
  - wish
assignees: []
created_at: 2023-10-25T04:36:04Z
updated_at: 2023-11-13T23:03:42Z
url: https://github.com/astral-sh/ruff/issues/8197
synced_at: 2026-01-12T15:54:47Z
```

# Proposal: Formatting options to enable "vertical formatting" and "grid formatting"

---

_@NeilGirdhar_

I realize that this may not be a popular feature request, but it's the only thing stopping me from using Black, and I love the idea of Black, so I feel compelled to propose it.  Feel free to close this as "will not implement".

Fundamentally, Black has [chosen](https://black.readthedocs.io/en/stable/the_black_code_style.html) to wrap lines in one of three ways:
* single line:
  ```python
  def super_softplus(x: JaxRealArray) -> JaxRealArray:
  ```
* hanging single line:
  ```python
    def geometry_energy(
        self, intermediate_explanation: FisherMessage, observation: FisherMessage, case: int
    ) -> RivalMessageEnergy:
  ```
* and hanging multi-line:
  ```python
    def infer_explanation_variationally(
        self,
        observation: FisherMessage,
        weights: Weights,
        *,
        key: None | KeyArray = None,
        use_variational_signal_noise: bool,
    ) -> RivalMessagePrediction:
  ```

This hanging formatting like mode 3 of isort.   While mode 3 is very versatile, some people consider it harder to read than isort's mode 1: "vertical" (see e.g., [numpy](https://github.com/numpy/numpy/blob/382eedf3891d474196677e65f3aae066afb32c5f/numpy/_core/_internal.py#L432)).  My proposal is to try vertical formatting to see if all lines fit without wrapping, and if so prefer that to either of the hanging outputs.  Thus, for the above examples, we have:
```python
    def geometry_energy(self,
                        intermediate_explanation: FisherMessage,
                        observation: FisherMessage,
                        case: int
                        ) -> RivalMessageEnergy:
```
and
```python
    def infer_explanation_variationally(self,
                                        observation: FisherMessage,
                                        weights: Weights,
                                        *,
                                        key: None | KeyArray = None,
                                        use_variational_signal_noise: bool,
                                        ) -> RivalMessagePrediction:

```

Of course legibility is in the eye of the beholder, but the two points in favour of vertical layout are that:

-  The arguments sit to the right of the function call, which makes the function call more visually obvious.
-  In the case of hanging multi-line, the vertical version uses fewer lines of vertical space.  Minimizing vertical space means that more code is visible on the screen, which makes reasoning about large functions a lot easier since you don't need to scroll and memorize code.
- In the case of hanging single line, the vertical layout puts each argument on its own line, which makes them easier to distinguish without looking for commas.

I concede that when there is a line that doesn't fit within the line length, hanging is definitely superior.  I'm not proposing eliminating that.  I'm proposing using vertical in particular cases.  In short, my proposal is to format as follows:

- Prefer single line if possible (as before)
- **Try vertical to see if all lines can be rendered without wrapping** (proposed)
- Try hanging multi-line otherwise (as before)

This algorithm would need to applied recursively for nested structures.

I realize that Black's rules are simple, but if the goal is to write code the way humans read and write code, I think a little bit of extra complexity in the rules would go a long way to making code easier for people to read.  The downside of this proposal is longer diffs, but diffs are read a lot less often than code is read as it's being worked on.

--

Edit: Similarly, it would be nice to support _grid formatting_ as another option.  This comes in handy for NumPy arrays and imports:

```python
from ...structure import (InferenceResult, InferenceResults, ListLabeler, LoggingManager, Plot,
                          Plotter, RLInference, Solver, TrainingResult, TrainingResults,
                          TrainingSolution, chooser_field, smooth_data, ui_dataclass)

np.array([[1, ...
           2, 1],
          [1, 1, 0],
          [0, 1, 1]])
```

---

_Renamed from "Proposal: Formatting option for compact output" to "Proposal: Formatting option to enable "vertical formatting"" by @NeilGirdhar on 2023-10-25 04:37_

---

_Comment by @MichaReiser on 2023-10-25 06:30_

Thanks for this detailed issue. Supporting different "styles" (similar to docstring conventions) might be something that we want to explore in the future but is currently out of scope. There's still a lot to do on the current style: from preview style, fixing deviations, `check` integration, etc... and building this style took us 6 months. 

But we do understand that we have different preferences and a single style doesn't satisfy the needs of everyone. 

---

_Label `formatter` added by @MichaReiser on 2023-10-25 06:30_

---

_Label `wish` added by @MichaReiser on 2023-10-25 06:30_

---

_Closed by @MichaReiser on 2023-10-26 23:55_

---

_Renamed from "Proposal: Formatting option to enable "vertical formatting"" to "Proposal: Formatting options to enable "vertical formatting" and "grid formatting"" by @NeilGirdhar on 2023-11-13 23:03_

---
