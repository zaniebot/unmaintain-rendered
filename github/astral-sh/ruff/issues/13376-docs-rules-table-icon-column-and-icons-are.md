---
number: 13376
title: "Docs: Rules table icon column and icons are inaccessible"
type: issue
state: open
author: MHLut
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-09-17T09:17:36Z
updated_at: 2024-09-17T14:42:55Z
url: https://github.com/astral-sh/ruff/issues/13376
synced_at: 2026-01-07T13:12:15-06:00
---

# Docs: Rules table icon column and icons are inaccessible

---

_Issue opened by @MHLut on 2024-09-17 09:17_

The last column of the [rules table](https://docs.astral.sh/ruff/rules/) produced by [crates/ruff_dev/src/generate_rules_table.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_dev/src/generate_rules_table.rs) is inaccessible:

* The column has no text in its heading cell.
* Every column heading cell misses the `scope` attribute.
* There is no descriptive text for the icons.

There is a `title` attribute on a span surrounding each icon, but title attributes do nothing accessibility-wise.

#### Possible solution

A solution could be to use [visually-hidden](https://css-tricks.com/inclusively-hidden/) spans with descriptive text and hide the icons.

This code:

```html
<td style="text-align: right">
  <span title="Rule is stable" style="opacity: 0.6">✔️</span>
  <span title="Automatic fix available">🛠️</span>
</td>
```

Could become this:

```html
<td style="text-align: right">
  <span class="visually-hidden">Rule is stable</span>
  <span title="Rule is stable" style="opacity: 0.6">✔️</span>
  <span class="visually-hidden">Automatic fix available</span>
  <span title="Automatic fix available">🛠️</span>
</td>
```

This solution reuses the existing copy and makes a minor change to the table generator.

A bonus to this approach is that tools like Google Translate translate regular text on the page, whereas they ignore attribute contents (like `title` or `aria-label`).

#### Further accessibility concerns

The solution I mentioned above is only helpful for screen reader users, which leaves an accessibility gap:

The `title` attribute on the icons only works for desktop users who can hover over the icon with a mouse. This leaves out mobile users and anyone who cannot hover or use a mouse (e.g., keyboard or speech recognition software users).

If you want to use tooltips, there should be other ways to trigger them (for example, by acting on keyboard focus, using toggle tips).

---

_Comment by @MHLut on 2024-09-17 09:20_

Suggestions for the CSS portion of this fix are welcome!

---

_Label `documentation` added by @charliermarsh on 2024-09-17 14:09_

---

_Label `help wanted` added by @charliermarsh on 2024-09-17 14:09_

---
