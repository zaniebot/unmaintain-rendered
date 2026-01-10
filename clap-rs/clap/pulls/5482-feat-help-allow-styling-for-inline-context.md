---
number: 5482
title: "feat(help): Allow styling for inline context"
type: pull_request
state: closed
author: GilShoshan94
labels: []
assignees: []
base: master
head: master
created_at: 2024-05-02T22:51:55Z
updated_at: 2025-07-01T20:33:01Z
url: https://github.com/clap-rs/clap/pull/5482
synced_at: 2026-01-10T01:28:24Z
---

# feat(help): Allow styling for inline context

---

_Pull request opened by @GilShoshan94 on 2024-05-02 22:51_

Allow customizing styling for inline context [default], [possible values], [env], [aliases] and [short aliases]

fix #5093 


Hi @epage,

Added the fields `inline_context` and `inline_context_value` to `Styled`.

`inline_context_value` is an `Option<anstyle::Style>`, and if not explicitly set, will fallback to `inline_context`.
The default is simple unstyled as you requested.

I just have one problem with the styling of "Possible Values:" (for enumerated values / multiple choices):
For regular help (-h): It's ok, I styled like the others inline contexts case.
But for the long_help (--help), currently the enumerated values are already styled with the literal style ([see here in code](https://github.com/clap-rs/clap/blob/d681a81dd7f4d7ff71f2e65be26d8f90783f7b40/clap_builder/src/output/help_template.rs#L714))

For now I left it as it is probabling at least a minor release.
It feels a bit incosistent now, and I think I should change the style to inline_context_value.

Let me know if I can add this extra modification.

---

_Review comment by @pksunkara on `clap_builder/src/output/help_template.rs`:836 on 2025-05-30 13:44_

Minor tangent fix: Let's rename all the mentions of `pvs` in this block of code to be `dvs`.

---

_@pksunkara reviewed on 2025-05-30 13:49_

I think changing the spec values in long help from `literal` to `inline_context_value` could be considered a breaking change and therefore, let's leave it for 5.0

Everything else looks good. I don't see any other issues or blocks risen by @epage, therefore I am going to approve and merge this.

@GilShoshan94 Can you please rebase? And can you please create an issue about `literal` possible values in long help? We will fix that when 5.0 is rolling out.


---
