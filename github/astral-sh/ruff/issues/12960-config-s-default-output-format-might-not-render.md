---
number: 12960
title: "`config`'s default output format might not render as expected"
type: issue
state: open
author: InSyncWithFoo
labels:
  - wish
assignees: []
created_at: 2024-08-18T02:23:56Z
updated_at: 2024-08-22T07:43:41Z
url: https://github.com/astral-sh/ruff/issues/12960
synced_at: 2026-01-07T13:12:15-06:00
---

# `config`'s default output format might not render as expected

---

_Issue opened by @InSyncWithFoo on 2024-08-18 02:23_

Currently, the output of a `config` command is something like this:

```pycon
>>> subprocess.getoutput(['ruff', 'config', 'lint.preview'])
'Whether to enable preview mode. When preview mode is enabled, Ruff will\nuse unstable rules and fixes.\n\nDefault value: false\nType: bool\nExample usage:\n```toml\n# Enable preview features.\npreview = true\n```'
```

In the terminal, this looks nice:

`````markdown
Whether to enable preview mode. When preview mode is enabled, Ruff will
use unstable rules and fixes.

Default value: false
Type: bool
Example usage:
```toml
# Enable preview features.
preview = true
```
`````

However, when rendered, the "Default value", "Type" and "Example usage" lines are collapsed into a single `<p>` ([Commonmark dingus](https://spec.commonmark.org/dingus/?text=Whether%20to%20enable%20preview%20mode.%20When%20preview%20mode%20is%20enabled%2C%20Ruff%20will%0Ause%20unstable%20rules%20and%20fixes.%0A%0ADefault%20value%3A%20false%0AType%3A%20bool%0AExample%20usage%3A%0A%60%60%60toml%0A%23%20Enable%20preview%20features.%0Apreview%20%3D%20true%0A%60%60%60)):

```html
<p>Whether to enable preview mode. When preview mode is enabled, Ruff will
use unstable rules and fixes.</p>
<p>Default value: false
Type: bool
Example usage:</p>
<pre><code class="language-toml"># Enable preview features.
preview = true
</code></pre>
```

For example, here's how PyCharm renders it:

![](https://github.com/user-attachments/assets/cd768c9e-a0c1-4302-ab8a-868b3206c238)

A possible fix is to append two spaces to the end of the first two lines in question ([Commonmark dingus](https://spec.commonmark.org/dingus/?text=Whether%20to%20enable%20preview%20mode.%20When%20preview%20mode%20is%20enabled%2C%20Ruff%20will%0Ause%20unstable%20rules%20and%20fixes.%0A%0ADefault%20value%3A%20false%20%20%0AType%3A%20bool%20%20%0AExample%20usage%3A%0A%60%60%60toml%0A%23%20Enable%20preview%20features.%0Apreview%20%3D%20true%0A%60%60%60)):

```python
'...Default value: false  \nType: bool  \nExample usage:...'
```

Also, default values and types should be surrounded in backticks.

---

_Comment by @Avasam on 2024-08-18 05:07_

Note: Backslashes also work as markdown newline indicators (and are more obvious than double spaces) [demo](https://spec.commonmark.org/dingus/?text=Whether%20to%20enable%20preview%20mode.%20When%20preview%20mode%20is%20enabled%2C%20Ruff%20will%0Ause%20unstable%20rules%20and%20fixes.%0A%0ADefault%20value%3A%20false%5C%0AType%3A%20bool%5C%0AExample%20usage%3A%0A%60%60%60toml%0A%23%20Enable%20preview%20features.%0Apreview%20%3D%20true%0A%60%60%60). But if the goal is to also display the same string in a terminal, then yeah two spaces is probably the best.

---

_Renamed from "`config`'s default output format doesn't render very nicely" to "`config`'s default output format might not render as expected" by @InSyncWithFoo on 2024-08-18 05:53_

---

_Comment by @MichaReiser on 2024-08-18 07:42_

Thanks for opening this issue. I agree, the output in PyCharm isn't ideal.

I assume that the hover text in PyCharm comes from the JSON schema. If that's the case, I would prefer a local change in the schema generation to use `\` over the somewhat obscure use of two spaces at the end of the line. 

---

_Label `help wanted` added by @MichaReiser on 2024-08-18 07:42_

---

_Comment by @InSyncWithFoo on 2024-08-18 17:19_

It wasn't the schema (it would have been great [if that was the case](https://youtrack.jetbrains.com/issue/PY-66325), though); I triggered the renderer semi-manually. (Yes, I could have asked for the JSON and put it together myself, too, but that's beside the point.)

Submitting a PR.

---

_Referenced in [astral-sh/ruff#12965](../../astral-sh/ruff/pulls/12965.md) on 2024-08-18 17:19_

---

_Label `help wanted` removed by @MichaReiser on 2024-08-18 17:46_

---

_Comment by @charliermarsh on 2024-08-18 22:33_

@MichaReiser -- Is there still something to do here given the PR was closed?

---

_Comment by @MichaReiser on 2024-08-19 07:47_

`ruff config` only supports the output format `text` and `json`. The ask in this issue is to add a `markdown` output format.

---

_Label `wish` added by @MichaReiser on 2024-08-19 07:48_

---

_Comment by @InSyncWithFoo on 2024-08-21 19:30_

It turned out that the format wasn't as nice compared to other popups of the same kind, so I rendered it manually anyway. It took more efforts than I expected, but behold the result:

![](https://github.com/user-attachments/assets/5a5ac71f-502f-42d7-a24c-aabec5e19a5a)

I'll keep this issue open, though, since someone else might need it.

---

_Comment by @MichaReiser on 2024-08-22 07:43_

This looks great! 

---

_Referenced in [redhat-developer/lsp4ij#543](../../redhat-developer/lsp4ij/pulls/543.md) on 2024-09-27 14:34_

---
