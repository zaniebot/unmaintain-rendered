```yaml
number: 8632
title: "Add formatting option to control spacing between consecutive `if` statements"
type: issue
state: closed
author: ofek
labels:
  - formatter
  - style
assignees: []
created_at: 2023-11-12T18:51:42Z
updated_at: 2025-01-06T08:43:21Z
url: https://github.com/astral-sh/ruff/issues/8632
synced_at: 2026-01-10T11:09:50Z
```

# Add formatting option to control spacing between consecutive `if` statements

---

_Issue opened by @ofek on 2023-11-12 18:51_

I'm upgrading formatting across codebases and am often fixing https://docs.astral.sh/ruff/rules/#flake8-return-ret

When I fix those, I am facing an internal quandary regarding spacing between statements and I noticed myself converging on a pattern that seems correct.

Basically, I think there should be 0 or 1 new lines between `if` and `elif` statements as usual. Either one looks normal to me based on all of the code I have read over the years. However, zero new lines between consecutive `if` statements looks very odd and I would appreciate an option to enforce a new line between them, which is what I am now doing everywhere.

Perhaps this option could serve the dual purpose of enforcing zero new lines between `if` and `elif` statements.

---

_Label `style` added by @charliermarsh on 2023-11-12 20:33_

---

_Label `formatter` added by @charliermarsh on 2023-11-12 20:33_

---

_Comment by @warsaw on 2023-11-15 00:35_

I generally prefer zero newlines between `if`, `elif`, and `else` clauses in the same conditional, and usually a single newline between consecutive `if` conditionals, but it's highly dependent on context.  If I can, I will put a meaningful comment between these clauses and that serves as enough of a visual separation for me.

---

_Comment by @MichaReiser on 2023-11-27 06:45_

Do I understand you correctly that this is about the number of lines between these two if statements:

```python

if True:
	...

if False:
	...
```

I do share your perspective that using a single line between if statements (in fact all control flow statements) helps improve readability, I don't think that enforcing it is something that reaches consensus in the community and may be even too opinionated for ruff. Maybe it would be better to support this as a lint rule? 



---

_Comment by @ofek on 2023-11-27 14:26_

Yes but this is very specific unlike what you said about all control flow statements. I and apparently many others would like a formatting option to make it so that there is a new line between consecutive `if` statements but zero new lines between `if`-`elif`-`else` statements.

This would be purely formatting and not a lint rule from my point of view but if that is what you want and it is fixable from the command line that I'm fine with whatever as long as it happens!

---

_Comment by @ofek on 2023-12-13 16:41_

I understand this effort has not yet begun, but am I understanding correctly that this would be an acceptable feature?

---

_Comment by @MichaReiser on 2023-12-14 03:19_

> I understand this effort has not yet begun, but am I understanding correctly that this would be an acceptable feature?

No, we haven't started with this yet. I'm currently implementing Black's 2024 preview style and try to get concensus around them. I also find improving the numpy array formatting (and other multi-dimensional arrays) more urgent because it hinders an entire sub-ecosystem from adopting ruff. 

We haven't really figured out our process for adpoting style changes but I think style changes require that the vast majority of the Python ecosystem sees the change as desired and an overall improvement because it otherwise requires a formatter option, something that we tried to avoid so far. 

I haven't seen a lot of feedback around this specific styling issue and expect that this change could be very controversial because many are opinionated (myself included) in how to space if statements (when and when not to add an empty line). So I don't think we see enough support to prioritize implementing this change ourself. 

One possibility to drive this further is to implement the change yourself and show that the generated output by the ecosystem check is an overall improvement and then seek feedback from the wider community.



---

_Closed by @MichaReiser on 2025-01-04 10:58_

---

_Comment by @ofek on 2025-01-04 14:44_

@MichaReiser Can you please link the option for posterity?

---

_Closed by @MichaReiser on 2025-01-05 08:56_

---

_Comment by @MichaReiser on 2025-01-05 08:58_

Sorry, I should have closed this as not planned. 

There has only been little support for this issue, and I'm leaning more towards relaxing the formatter's blank line rules rather than making them more strict to make the formatter appealing to more users. 

---

_Comment by @ofek on 2025-01-05 19:27_

Okay, although what do you mean by little support? Also we're not asking for default strictness but rather an option.

---

_Comment by @MichaReiser on 2025-01-06 08:43_

Ruff follows Black's philosophy of only supporting minimal configuration to avoid formatting discussions becoming formatter-option discussions. We also default to being black-compatible rather than diverting from Black's style guide. 

We've made a few exceptions where we diverge from Black's style guide or added options where Black has none but we only did so for highly requested features and they remain the exception. I don't see this feature request to meet that bar. 

---
