```yaml
number: 20641
title: "Formatter breaks Google Colab `@param` forms in ipynb"
type: issue
state: open
author: holtskinner
labels:
  - configuration
  - formatter
assignees: []
created_at: 2025-09-29T23:50:36Z
updated_at: 2025-09-30T06:04:11Z
url: https://github.com/astral-sh/ruff/issues/20641
synced_at: 2026-01-12T15:54:57Z
```

# Formatter breaks Google Colab `@param` forms in ipynb

---

_@holtskinner_

### Summary

**Issue Description:**

The Ruff formatter incorrectly wraps lines containing Google Colab's `@param` form comments, which breaks their rendering and functionality in Jupyter Notebooks.

**The problem**

In Google Colab, you can create interactive forms with [`@param`](https://colab.research.google.com/notebooks/forms.ipynb) to parameterize your code using a special comment syntax, like this:

```py
PROJECT_ID = "[your-project-id]"  # @param {type: "string", placeholder: "[your-project-id]", isTemplate: true}
```

When a line like this exceeds the configured `line-length`, `ruff format` will wrap it, like this:

```py
PROJECT_ID = (
    "[your-project-id]"  # @param {type: "string", placeholder: "[your-project-id]", isTemplate: true}
)
```

This wrapped code is valid Python, but it is no longer correctly interpreted by Colab, and the interactive form element will not render.

**Proposed Solution**

It would be beneficial to have an option to prevent the formatter from breaking these specific lines. A possible solution could be a new configuration option that would allow users to specify patterns for comments that should disable line-length formatting for that line. This would allow `ruff` to automatically ignore lines with `@param` comments without manual intervention.

**Workaround**

For users experiencing this issue, a temporary workaround is to use `# fmt: off` and `# fmt: on` to disable formatting for these specific lines:

```py
# fmt: off
PROJECT_ID = "[your-project-id]"  # @param {type: "string", placeholder: "[your-project-id]", isTemplate: true}
# fmt: on
```

Note: Adding `# fmt: skip` to the individual lines also causes the formatting to be incorrect in Colab.

While this works, it requires manually adding these comments around every `@param` line that is too long, which can be cumbersome and adds visual noise.

---

_Renamed from "### Formatter breaks Google Colab `@param` forms in ipynb" to "Formatter breaks Google Colab `@param` forms in ipynb" by @holtskinner on 2025-09-29 23:50_

---

_Label `configuration` added by @MichaReiser on 2025-09-30 06:04_

---

_Label `formatter` added by @MichaReiser on 2025-09-30 06:04_

---
