```yaml
number: 18297
title: "Docs a11y: improve and simplify rules table to improve readability"
type: pull_request
state: merged
author: MaddyGuthridge
labels:
  - documentation
assignees: []
merged: true
base: main
head: maddy-docs-a11y-iconography
created_at: 2025-05-25T13:14:34Z
updated_at: 2025-05-26T14:36:49Z
url: https://github.com/astral-sh/ruff/pull/18297
synced_at: 2026-01-12T15:56:16Z
```

# Docs a11y: improve and simplify rules table to improve readability

---

_@MaddyGuthridge_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Improves rules table to make information more readable. Resolves #18296

* Instead of showing a "✔️" to indicate a rule is stable, it shows nothing.
* Adds a line to the docs stating that rules are stable unless indicated otherwise
* Instead of showing a translucent "🛠️" to indicate that no auto-fix is available, it shows nothing.

In my opinion, this makes it much easier to understand the list of rules. It reduces the amount of information overload, as well as resolving issues including:

* The "✔️" is difficult to read when dark mode is activated
* The use of translucency on the "🛠️" is a little unclear

## Test Plan

<!-- How was it tested? -->

No tests, it's just documentation. I did run them to triple-check.


---

_Comment by @MichaReiser on 2025-05-26 07:25_

Thank you for working on an improvement. 

I sort of like removing the stable checkmark but it has the disadvantage that the experimental and "fix available" icons now are directly beneath each other. It took even me a moment to realise that the wrench icon doesn't indicate stability, but that a fix is available. 

<img width="1285" alt="Screenshot 2025-05-26 at 09 23 23" src="https://github.com/user-attachments/assets/9052452a-6892-4f3d-a0eb-9d4743115f16" />


One option is to use different columns for each icon. I'm not sure if this will look odd for other reasons :) Another option is to change the checkmark icon. What do you think?

---

_Label `documentation` added by @MichaReiser on 2025-05-26 07:25_

---

_Comment by @MaddyGuthridge on 2025-05-26 09:03_

Personally, I'm ok with the "experimental" and "fix available" icons being in the same column, since I believe the icons are distinct enough for it to be clear. 

I suppose the difference in interpretation is because I see them as "indicators" (car dashboard style), and so don't care about their placement; whereas you see them as checkmarks, meaning the placement is far more important. With that in mind, I think that having different columns for each icon could work. At the very least, I think that the "fix available" icon should go in a separate column. The lopsidedness may look nicer if there are little dividing  lines to make the column-ness of it more clear.

I did a little CSS fiddling, and I think that it would work nicely if there's a bit more of a gap between them. In this case, I did it by editing the style table row on the deployed docs site to `display: flex; gap: 15px; flex-direction: row-reverse;` (primarily to avoid spinning up the docs server on my poor laptop). 

Basically I'm thinking something like this, in terms of the columns being consistent, but instead using empty space in cases where the rule is stable or there's no fix available.

![image](https://github.com/user-attachments/assets/5d268681-d2fa-453f-af95-36a3eff160a7)

I think a better approach thank my hack would probably be to put them in two separate `<td>` elements, and then add something like `width: 100%` to the main text column.

Do you think this approach is worth investigating further?

---

_Comment by @MichaReiser on 2025-05-26 09:06_

> I think a better approach thank my hack would probably be to put them in two separate <td> elements, and then add something like width: 100% to the main text column.

I'm leaning towards separate columns. It has the added benefit that we can set an aria label on the column header

---

_Comment by @MaddyGuthridge on 2025-05-26 09:45_

I just tried out separate columns, and mkdocs seems to have given far too much spacing to the "status" columns, which looks pretty gross. There's no good way to fix this in Markdown as far as I know.

![image](https://github.com/user-attachments/assets/e6026ccf-148e-4f60-8d47-9a032c2d21ef)

That leaves two options imo:

* Change `fn generate_table` to produce HTML so we can style it properly. I'm pretty sure `mkdocs` will still work fine for this.
* Use my hacky approach from before to get a bit less of a gap, meaning we wouldn't need to rewrite half the function.

---

_Comment by @MaddyGuthridge on 2025-05-26 10:16_

Tried the hacky approach, and I think it works pretty nicely.

One other change on the same thread: I don't think the opacity changes to indicate deprecation should apply to the status tokens, since it makes them much harder to read. I think it'd also be nice to have a strikethrough for the rule ID, since imo that's more readable than an opacity difference.

![image](https://github.com/user-attachments/assets/d36cb332-7079-4966-a0cc-8200286fc9ea)

What do you think?

---

_Comment by @MichaReiser on 2025-05-26 12:17_

I'd prefer if we discuss striking out rules as a separate PR. I remember that we explored this in an initial design and deliberately decided against it. 

---

_Comment by @MaddyGuthridge on 2025-05-26 13:20_

Fixed it, and opened #18318 to discuss it. Is there anything else I can improve for this PR?

---

_Merged by @MichaReiser on 2025-05-26 14:35_

---

_Closed by @MichaReiser on 2025-05-26 14:35_

---

_Comment by @MaddyGuthridge on 2025-05-26 14:36_

Yippee! Thanks!

---

_Branch deleted on 2025-05-26 14:36_

---
