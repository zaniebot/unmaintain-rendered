```yaml
number: 2155
title: Expose extra-paths as editor setting?
type: issue
state: closed
author: thangckt
labels:
  - question
  - configuration
assignees: []
created_at: 2025-12-22T12:45:54Z
updated_at: 2025-12-24T05:11:03Z
url: https://github.com/astral-sh/ty/issues/2155
synced_at: 2026-01-12T15:54:26Z
```

# Expose extra-paths as editor setting?

---

_@thangckt_

Dear Developers,

Thank you so much for useful `ty`.

I have the resolve import problem when install local package using 
```
pip install -e .
```
When using `pylance` in `vscode`, this issue can be solved by adding `"python.analysis.extraPaths"`

Is there any in `ty` to overcome this issue?

Thanks.


---

_Comment by @sinon on 2025-12-22 12:59_

I believe `root` is the config item that maps to `extraPaths` from pylance.
Config docs: https://docs.astral.sh/ty/reference/configuration/#root
Docs explaining module discovery: https://docs.astral.sh/ty/modules/


---

_Comment by @carljm on 2025-12-22 16:56_

[`environment.extra-paths`](https://docs.astral.sh/ty/reference/configuration/#extra-paths) is our equivalent of Pylance `extraPaths`, I think, though [`environment.root`](https://docs.astral.sh/ty/reference/configuration/#root) could probably work here too. (The difference is whether we will consider those paths "first-party code" and check them, or just use them as a source of importable modules.) Neither of these options is exposed as an editor setting currently, so you'd need to put them in a `ty.toml` or `pyproject.toml` config file.

If you've configured https://docs.astral.sh/ty/reference/editor-settings/#interpreter correctly, then something which you `pip install -e` should be importable via that Python environment, but if it uses the setuptools build backend it may not be due to #475 / #585 

cc @MichaReiser, not sure which settings we want to expose as editor settings, or if this issue is useful to track possibly wanting to expose `extra-paths`?

---

_Label `configuration` added by @carljm on 2025-12-22 16:56_

---

_Renamed from "Cannot resolve imported module issue" to "Expose extra-paths as editor setting?" by @carljm on 2025-12-22 16:56_

---

_Comment by @MichaReiser on 2025-12-22 17:00_

https://github.com/astral-sh/ruff/pull/22053 added support for configuring settings inside the editor. It's not the most elegant solution but should unblock use cases like this. 

https://github.com/astral-sh/ty/issues/1084 tracks adding specific editor settings to cover the most common use cases (in a more convenient way than editing JSON).

I'll keep this open to track the question of the issue author but we can close it once their setup works as expected.

---

_Label `question` added by @MichaReiser on 2025-12-22 17:00_

---

_Comment by @thangckt on 2025-12-23 01:15_

hi @MichaReiser, 

exposing in editor settings will extremely helpful when you work with *.ipynb files. In such case, there is only workspace-based settings possible, no `pyproject.toml`.

---

_Closed by @MichaReiser on 2025-12-23 06:34_

---

_Comment by @dhruvmanila on 2025-12-24 05:10_

This should now be possible with the [`configuration`](https://docs.astral.sh/ty/reference/editor-settings/#configuration) setting in the latest release (ty `0.0.6` and `2025.76.0` VS Code extension). For example in VS Code:

```json
{
  "ty.configuration": {
    "environment": {
      "extra-paths": ["./shared/my-search-path"]
    }
  }
}
```

---
