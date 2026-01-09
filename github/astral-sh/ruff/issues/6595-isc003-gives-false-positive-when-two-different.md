---
number: 6595
title: ISC003 gives false positive when two different string types in one line are mandatory
type: issue
state: closed
author: mmarras
labels: []
assignees: []
created_at: 2023-08-15T12:36:53Z
updated_at: 2023-08-15T17:50:30Z
url: https://github.com/astral-sh/ruff/issues/6595
synced_at: 2026-01-07T13:12:15-06:00
---

# ISC003 gives false positive when two different string types in one line are mandatory

---

_Issue opened by @mmarras on 2023-08-15 12:36_

When defining `hovertemplates` they use a magic string (e.g. `%{x}` to denote the x-data)  that cannot be inside an fstring. If one wants to combine the former with an fstring on the same line it flags the thing and it is impossible to comply to ruff ISC without breaking the code. So even if one decides to keep the rule for same line, it should make exception for mixed string types.

Correct : 
```python
import plotly.graph_objs as go
string = "Data1"
go.Figure(data=go.Scatter(
    x=[1.123123,2.12321,3.12321],
    y=[1.12321,2.12321,3.12321],
    hovertemplate="%{x:.2f}<br>%{y:.3f}" + f"<extra>{string}</extra>")
          )
```
The above codeblock but with ` hovertemplate=f"%{x:.2f}<br>%{y:.3f}<extra>{string}</extra>")` fails.
> NameError: name 'x' is not defined

Of course the correct version (see codeblock) triggers 
> Explicitly concatenated string should be implicitly concatenatedRuff[ISC003](https://beta.ruff.rs/docs/rules/explicit-string-concatenation)

or when implicitly concatenated:
> Implicitly concatenated string literals on one lineRuff[ISC001](https://beta.ruff.rs/docs/rules/single-line-implicit-string-concatenation)

_Originally posted by @mmarras in https://github.com/astral-sh/ruff/issues/5332#issuecomment-1657277651_
            

---

_Comment by @charliermarsh on 2023-08-15 13:08_

Having trouble reproducing.

Given:
```python
import plotly.graph_objs as go
string = "Data1"
go.Figure(data=go.Scatter(
    x=[1.123123,2.12321,3.12321],
    y=[1.12321,2.12321,3.12321],
    hovertemplate="%{x:.2f}<br>%{y:.3f}" + f"<extra>{string}</extra>")
          )
```

Running `ruff check foo.py --select ISC --isolated -n` doesn't yield any violations.

Was it broken over multiple lines? Like:

```python
import plotly.graph_objs as go
string = "Data1"
go.Figure(data=go.Scatter(
    x=[1.123123,2.12321,3.12321],
    y=[1.12321,2.12321,3.12321],
    hovertemplate=(
        "%{x:.2f}<br>%{y:.3f}" +
        f"<extra>{string}</extra>")
    )
)
```

---

_Comment by @mmarras on 2023-08-15 14:45_

Indeed, this only shows in my VScode (1.81.0) extension (v2023.32.0, ruff 0.0.277, I assume), not if I run ruff cli with ruff (0.0.284). My bad.

 
![image](https://github.com/astral-sh/ruff/assets/53915566/59cc6cf5-1334-48e1-8d50-8843c491945a)


---

_Comment by @zanieb on 2023-08-15 16:22_

Hm I think the VSCode extension should use your externally installed Ruff if available (so an update is not needed to get the latest version). Anyway, it sounds like we've resolved this on the latest version. Let us know if there's more we can do!

---

_Closed by @zanieb on 2023-08-15 16:22_

---

_Comment by @charliermarsh on 2023-08-15 16:23_

I think it's not quite resolved because the request is that we _don't_ flag explicit concatenations with mixed-kind strings, like:

```python
import plotly.graph_objs as go
string = "Data1"
go.Figure(data=go.Scatter(
    x=[1.123123,2.12321,3.12321],
    y=[1.12321,2.12321,3.12321],
    hovertemplate=(
        "%{x:.2f}<br>%{y:.3f}" +
        f"<extra>{string}</extra>")
    )
)
```

---

_Comment by @charliermarsh on 2023-08-15 16:24_

(The confusion above is about whether it flags for multiline strings or single-line strings, related to the repro.)

---

_Reopened by @charliermarsh on 2023-08-15 16:28_

---

_Comment by @mmarras on 2023-08-15 17:41_

> Hm I think the VSCode extension should use your externally installed Ruff if available (so an update is not needed to get the latest version). Anyway, it sounds like we've resolved this on the latest version. Let us know if there's more we can do!

So, VScode was in a different env than my terminal conda env, therefore the missmatch in ruff results editor vs cli. 

As far as I am concerned **the issue is solved** (I checked, as of 0.0.281). Conclusion stands: my bad. 

For me it was always about the single line example. I was happy as soon as an explicit concat of a mixed-kind one-line string wasn't flagged anymore. But I'm using plotly quite a bit so, if it comes up in another constellation, I can for sure report back.

---

_Comment by @charliermarsh on 2023-08-15 17:50_

Okay, sounds good! I'm glad it's resolved for this use-case, thank you for reporting back :)

---

_Closed by @charliermarsh on 2023-08-15 17:50_

---
