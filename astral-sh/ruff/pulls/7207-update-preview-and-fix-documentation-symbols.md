```yaml
number: 7207
title: Update preview and fix documentation symbols
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zanie/preview-symbol
created_at: 2023-09-06T17:47:20Z
updated_at: 2023-09-11T23:08:01Z
url: https://github.com/astral-sh/ruff/pull/7207
synced_at: 2026-01-12T02:45:38Z
```

# Update preview and fix documentation symbols

---

_Pull request opened by @zanieb on 2023-09-06 17:47_

I don't love the sunrise emoji and 🧪 seems nice :)

Requires #7195 

---

_Comment by @codspeed-hq[bot] on 2023-09-06 17:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-symbol)

### Merging #7207 will **degrade performances by 10.49%**

<sub>Comparing <code>zanie/preview-symbol</code> (75cbc11) with <code>zanie/rule-preview</code> (6351494)</sub>



### Summary

`❌ 9` regressions
`✅ 16` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-symbol)._

### Benchmarks breakdown

|     | Benchmark | `zanie/rule-preview` | `zanie/preview-symbol` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `lexer[numpy/ctypeslib.py]` | 2.2 ms | 2.3 ms | -2.19% |
| ❌ | `lexer[pydantic/types.py]` | 4.7 ms | 4.8 ms | -2.52% |
| ❌ | `formatter[unicode/pypinyin.py]` | 3.7 ms | 3.8 ms | -4.64% |
| ❌ | `formatter[numpy/ctypeslib.py]` | 10.9 ms | 11.6 ms | -6.12% |
| ❌ | `lexer[unicode/pypinyin.py]` | 684 µs | 700.4 µs | -2.35% |
| ❌ | `lexer[large/dataset.py]` | 11.3 ms | 11.6 ms | -2.54% |
| ❌ | `formatter[pydantic/types.py]` | 20.1 ms | 21.3 ms | -5.72% |
| ❌ | `formatter[large/dataset.py]` | 58 ms | 59.8 ms | -3.01% |
| ❌ | `formatter[numpy/globals.py]` | 1.1 ms | 1.3 ms | -10.49% |


---

_Comment by @tdulcet on 2023-09-06 19:08_

While you are at it, maybe also fix the "Hammer and Wrench emoji" to use the actual emoji character. It is currently missing the required variation selector, so it renders as a black and white symbol instead of as a color emoji:
```
🛠 -> 🛠️
```

---

_@konstin reviewed on 2023-09-07 08:28_

---

_Review comment by @konstin on `crates/ruff_dev/src/generate_rules_table.rs`:13 on 2023-09-07 08:28_

this would be @tdulcet's suggestion:

```suggestion
const FIX_SYMBOL: &str = "🛠️";
```

---

_Comment by @konstin on 2023-09-07 08:35_

Context for emoji variant selectors: https://www.codejam.info/2021/11/emoji-variation-selector.html. Debugging tool: https://qaz.wtf/u/show.cgi. For me (firefox on ubuntu), it doesn't actually display black and white but just a different emoji:
![image](https://github.com/astral-sh/ruff/assets/6826232/35f042db-0232-43ac-9fc8-73d07ec2828c)

I've checked that hammer (🛠) is in https://www.unicode.org/Public/emoji/5.0/emoji-variation-sequences.txt while construction sign (🚧) isn't. 

---

_Comment by @tdulcet on 2023-09-07 08:51_

> For me (firefox on ubuntu), it doesn't actually display black and white but just a different emoji:

Thanks for providing all that information. For reference, this is what I see with Firefox on Windows 11:
![image](https://github.com/astral-sh/ruff/assets/15305150/a71d5c76-d47f-445b-8ce3-e294051d11f0)

---

_Comment by @zanieb on 2023-09-07 13:47_

They're hilariously identical on Firefox / macOS. I can update it though :)

---

_Comment by @MichaReiser on 2023-09-07 15:27_

Wdyt of a flask icon (I haven't searched emojis yet) https://fontawesome.com/search?q=experimental&o=r I think it captures the experimental nature better than under construction (because preview features are functional features that need fine tuning, not unfinished work, which is what I associate with the construction emoji)

---

_Comment by @tdulcet on 2023-09-07 15:50_

> Wdyt of a flask icon (I haven't searched emojis yet)

It looks like the closest emoji would be the Test Tube: 🧪 or Alembic: ⚗️ (the latter would also require a variation selector).

---

_Comment by @zanieb on 2023-09-07 16:16_

A flask would be nice. I don't like the test tube as much but it might be as close as we can get. I agree they seem better than the construction sign.

---

_Comment by @konstin on 2023-09-07 16:30_

it should obviously be an eppendorf tube rather than a test tube :P but i think either will do fine

---

_Renamed from "Update preview symbol to construction sign emoji" to "Update preview and fix documentation symbols" by @zanieb on 2023-09-07 17:17_

---

_Marked ready for review by @zanieb on 2023-09-11 17:33_

---

_Label `documentation` added by @zanieb on 2023-09-11 18:38_

---

_Review requested from @charliermarsh by @zanieb on 2023-09-11 18:51_

---

_@charliermarsh approved on 2023-09-11 22:01_

---

_@charliermarsh approved on 2023-09-11 22:02_

---

_Merged by @zanieb on 2023-09-11 23:08_

---

_Closed by @zanieb on 2023-09-11 23:08_

---

_Branch deleted on 2023-09-11 23:08_

---
