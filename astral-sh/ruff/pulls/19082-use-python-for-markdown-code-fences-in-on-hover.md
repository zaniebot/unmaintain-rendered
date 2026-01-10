```yaml
number: 19082
title: "Use \"python\" for markdown code fences in on-hover content"
type: pull_request
state: merged
author: zanieb
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: zb/python-hover
created_at: 2025-07-02T02:31:20Z
updated_at: 2025-07-03T05:20:36Z
url: https://github.com/astral-sh/ruff/pull/19082
synced_at: 2026-01-10T18:33:12Z
```

# Use "python" for markdown code fences in on-hover content

---

_Pull request opened by @zanieb on 2025-07-02 02:31_

Instead of "text".

Closes https://github.com/astral-sh/ty/issues/749

We may not want this because the type display implementations are not guaranteed to be valid Python, however, unless they're going to highlight invalid syntax this seems like a better interim value than "text"? I'm not the expert though. See https://github.com/astral-sh/ty/issues/749#issuecomment-3026201114 for prior commentary.

edit: Going back further to https://github.com/astral-sh/ruff/pull/17057#discussion_r2028151621 for prior context, it turns out they _do_ highlight invalid syntax in red which is quite unfortunate and probably a blocker here.


---

_Label `ty` added by @zanieb on 2025-07-02 02:31_

---

_Label `server` added by @zanieb on 2025-07-02 02:31_

---

_Comment by @zanieb on 2025-07-02 02:32_

I didn't test this in an editor, that seems harder than finding the place to change this literal :)

---

_Comment by @github-actions[bot] on 2025-07-02 02:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~97MB
+     memo fields = ~88MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~54MB
+     memo fields = ~49MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB
-     memo fields = ~106MB
+     memo fields = ~97MB

schemathesis (https://github.com/schemathesis/schemathesis)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~156MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~129MB
+     memo fields = ~142MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB

```
</details>


---

_Marked ready for review by @zanieb on 2025-07-02 03:01_

---

_Review requested from @carljm by @zanieb on 2025-07-02 03:01_

---

_Review requested from @MichaReiser by @zanieb on 2025-07-02 03:01_

---

_Review requested from @AlexWaygood by @zanieb on 2025-07-02 03:01_

---

_Review requested from @sharkdp by @zanieb on 2025-07-02 03:01_

---

_Review requested from @dcreager by @zanieb on 2025-07-02 03:01_

---

_Converted to draft by @zanieb on 2025-07-02 03:23_

---

_Comment by @sharkdp on 2025-07-02 06:37_

> edit: Going back further to [#17057 (comment)](https://github.com/astral-sh/ruff/pull/17057#discussion_r2028151621) for prior context, it turns out they _do_ highlight invalid syntax in red which is quite unfortunate and probably a blocker here.

I tried in VS Code and everything looks kind of nice, actually. I don't see any invalid syntax highlighting.

![image](https://github.com/user-attachments/assets/823a86d1-e79b-47d6-897d-128d814fee4d)

![image](https://github.com/user-attachments/assets/2a645d9b-7456-4168-924d-231ebaa73882)

![image](https://github.com/user-attachments/assets/0fc96203-bafe-4ca8-9a59-e0939f75cdcd)


---

_Comment by @dhruvmanila on 2025-07-02 10:18_

I guess that would be dependent on the color scheme.

Regardless, I think this should be fine. I'm not even sure whether the red color is coming from it being a syntax error. For the color scheme that I use (Gruvbox), the keywords are highlighted as red, so I only see the red color for keywords e.g., in `<class 'Foo'>` the "class" is red colored but it's not due to the syntax error.

---

_Comment by @zanieb on 2025-07-02 13:22_

I will defer to ya'll for sure.

---

_Marked ready for review by @zanieb on 2025-07-02 13:22_

---

_@carljm approved on 2025-07-02 17:37_

If the results here aren't obviously bad, then I think we should go ahead and do it. It's clearly what we want, and it will help us find and motivate us to fix any specific cases where our type display doesn't work well with Python syntax highlighting.

---

_@dhruvmanila approved on 2025-07-03 05:20_

---

_Merged by @dhruvmanila on 2025-07-03 05:20_

---

_Closed by @dhruvmanila on 2025-07-03 05:20_

---

_Branch deleted on 2025-07-03 05:20_

---
