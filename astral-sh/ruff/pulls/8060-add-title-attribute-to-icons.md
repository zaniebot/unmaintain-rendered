```yaml
number: 8060
title: Add title attribute to icons
type: pull_request
state: merged
author: jaap3
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-10-19T08:41:29Z
updated_at: 2023-10-26T19:24:46Z
url: https://github.com/astral-sh/ruff/pull/8060
synced_at: 2026-01-12T15:55:25Z
```

# Add title attribute to icons

---

_@jaap3_

## Summary

Explain the meaning of the icon for screen readers (and mouse over). Hide "inactive" (low opacity) icons from screen readers.

Remove opacity: 1 styling, it's the default opacity.

Without this change a screen reader will just read "Hammer and spanner test tube" for the last column in each row.

---

_Comment by @jaap3 on 2023-10-19 09:20_

Sorry for the multiple force pushes / CI runs. Tried to get away with just using the GitHub interface to make a PR, but I just couldn't get the formatting right 😅

---

_@zanieb reviewed on 2023-10-19 13:44_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:25 on 2023-10-19 13:44_

I'd prefer to use "Fix available" to match our other language
```suggestion
                format!("<span title='Fix available'>{FIX_SYMBOL}</span>")
```

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:32 on 2023-10-19 13:46_

I'd prefer to use "In preview" to match our other language. The preview docs explain _when_ a rule might be in preview i.e. unstable
```suggestion
            format!("<span title='Rule is in preview'>{PREVIEW_SYMBOL}</span>")
```

Maybe this and the fix token should link to the relevant documentation (as a follow-up)

---

_@zanieb reviewed on 2023-10-19 13:46_

---

_@zanieb reviewed on 2023-10-19 13:47_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:25 on 2023-10-19 13:47_

Should we do "Rule has fix available" to be even clearer?

---

_@zanieb reviewed on 2023-10-19 13:47_

Thank you! This is a great idea.

---

_@zanieb reviewed on 2023-10-19 13:48_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:28 on 2023-10-19 13:48_

Instead of hiding should be say that a fix is _not_ available?

---

_@zanieb reviewed on 2023-10-19 13:49_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-19 13:49_

Similar to above, if instead of hiding we state the negative case then we could do "Rule is stable" and for positive case (preview) we could say "Rule is unstable" for symmetry 

---

_Label `documentation` added by @charliermarsh on 2023-10-19 14:29_

---

_Comment by @jaap3 on 2023-10-20 10:50_

Thanks for the review @zanieb, will probably find some time to update the PR on monday.

---

_Comment by @jaap3 on 2023-10-23 08:32_

Changing the wording to "Fix available" / "Fix not available" might be confusing. The meaning of the latter can be ambiguous (as in, can be interpreted as "unfixable" instead of manually fixable). I do think announcing/clarifying that a rule can be fixed automatically is warranted as it's an exception to the default status quo of a linter.

Similarly with the stable/unstable wording. I believe stating that a rule is stable is excessive information, only the special state of "preview" needs to be announced/clarified.

I experimented with fully hiding (both visually and audibly) the "inactive" icons (while still maintaining the layout) and feel that makes the rules table appear less cluttered:

![Screenshot 2023-10-23 at 10 32 16](https://github.com/astral-sh/ruff/assets/48517/83d00f2b-6694-4977-8253-fb7b692a0fdf)

So that's what I'm now proposing with the update I just pushed.

Note: Using `visibility: hidden` also removes the node from the accessibility tree, so it's not announced https://developer.mozilla.org/en-US/docs/Web/CSS/visibility#accessibility_concerns

I also tried adding a heading of "Notes" to the icon column, but felt like it wasn't adding that much, so I ended up removing it again.

---

_Review requested from @zanieb by @jaap3 on 2023-10-23 08:33_

---

_Comment by @codspeed-hq[bot] on 2023-10-23 08:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jaap3:patch-1)

### Merging #8060 will **improve performances by 4.98%**

<sub>Comparing <code>jaap3:patch-1</code> (5b98249) with <code>main</code> (67b0434)</sub>



### Summary

`⚡ 4` improvements
`✅ 21` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jaap3:patch-1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[large/dataset.py]` | 172.5 ms | 164.4 ms | +4.9% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 583.1 µs | 559.1 µs | +4.29% |
| ⚡ | `lexer[pydantic/types.py]` | 4 ms | 3.8 ms | +4.62% |
| ⚡ | `lexer[large/dataset.py]` | 8.9 ms | 8.5 ms | +4.98% |


---

_@zanieb reviewed on 2023-10-23 14:37_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-23 14:37_

Why include the symbol span at all if it's hidden? While I agree that hiding them is a cleaner experience, I don't think it's very clear what's going on when looking at a section of rules with no fixes / preview.

---

_@jaap3 reviewed on 2023-10-23 14:49_

---

_Review comment by @jaap3 on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-23 14:49_

It makes sure the icons always appear in the same spot (visibility hidden makes the element still take up space). I based this on the reasoning from @charliermarsh for the commit that added the faint opacity (c9d7c0d7d5d371c8658b65916ca99d704afefc4e)

---

_@charliermarsh reviewed on 2023-10-23 18:52_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-23 18:52_

Interesting, I actually _like_ the variant in which we show the opaque icons -- I find it clearer, because it gives framing to the column themselves. It was inspired by ESLint's documentation: https://eslint.org/docs/latest/rules/.

---

_@zanieb reviewed on 2023-10-23 19:04_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-23 19:04_

I agree with Charlie, it'd be preferable to keep the dimmed icons for now. I'd rather not bundle removing them with this accessibility related change.

---

_Review comment by @jaap3 on `crates/ruff_dev/src/generate_rules_table.rs`:34 on 2023-10-24 07:03_

Thanks for the link to the eslint docs. I see they also use aria-hidden for the dimmed icons. I restored the opacity, set aria-hidden. Hope this is acceptable now :)

---

_@jaap3 reviewed on 2023-10-24 07:03_

---

_@zanieb approved on 2023-10-25 19:09_

Thank you!

---

_Merged by @charliermarsh on 2023-10-26 17:23_

---

_Closed by @charliermarsh on 2023-10-26 17:23_

---

_Branch deleted on 2023-10-26 19:24_

---
