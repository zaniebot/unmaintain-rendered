```yaml
number: 2747
title: Add rendering of rule markdown for terminal output
type: pull_request
state: merged
author: ngnpope
labels: []
assignees: []
merged: true
base: main
head: rule-command-improvements
created_at: 2023-02-10T22:13:52Z
updated_at: 2023-02-12T10:10:48Z
url: https://github.com/astral-sh/ruff/pull/2747
synced_at: 2026-01-12T04:52:01Z
```

# Add rendering of rule markdown for terminal output

---

_Pull request opened by @ngnpope on 2023-02-10 22:13_

Add rendering of rule markdown for terminal output
    
This is achieved by making use of the `mdcat` crate.
    
See the following links for details:
    
- https://crates.io/crates/mdcat
- https://github.com/swsnr/mdcat
- https://docs.rs/mdcat/latest/mdcat/

---

I'm a rust newbie, so this was interesting...

I thought to look to implement support for piping to a pager, which is implemented in `mdcat` - the source could be used for guidance on how to achieve that. But I wasn't sure whether that would be better implemented as a global option? This is enough for now and a vast improvement on the current output.

I also implemented support for `NO_COLOR` which will also give better output.

---

![Screenshot from 2023-02-10 22-12-12](https://user-images.githubusercontent.com/2855582/218208634-9a3e4ca0-01ce-4892-994e-304a7965bd5f.png)
![Screenshot from 2023-02-10 22-11-48](https://user-images.githubusercontent.com/2855582/218208638-d50f58d8-3770-40bc-bc48-aaeabe88c360.png)
![Screenshot from 2023-02-10 22-06-15](https://user-images.githubusercontent.com/2855582/218208641-daee0493-f10a-47f6-9c83-eba3e5b76c3e.png)
![Screenshot from 2023-02-10 22-05-29](https://user-images.githubusercontent.com/2855582/218208642-4ae58a10-e240-42f3-8bab-2c2c8e2c2e72.png)


---

_@charliermarsh reviewed on 2023-02-10 22:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules.rs`:29 on 2023-02-10 22:17_

Do you mind separating this change into a separate PR, to make this one more focused and easier to review?

---

_Comment by @charliermarsh on 2023-02-10 22:21_

Thank you for putting this together! The images look really nice. Excited to review.

Is there any way to force it to output raw markdown? We may want to preserve that option somehow.


---

_Review comment by @ngnpope on `crates/ruff/src/rules/flake8_pyi/rules.rs`:29 on 2023-02-10 22:21_

See #2749.

---

_@ngnpope reviewed on 2023-02-10 22:21_

---

_@ngnpope reviewed on 2023-02-10 22:23_

---

_Review comment by @ngnpope on `crates/ruff/src/rules/flake8_pyi/rules.rs`:29 on 2023-02-10 22:23_

Will also take [Fix some code block markers](https://github.com/charliermarsh/ruff/pull/2747/commits/aecbc08ddb9da34188fe0f8d67b24a47018ac217) over there too to avoid conflicts here...

---

_Comment by @ngnpope on 2023-02-10 22:50_

> Is there any way to force it to output raw markdown? We may want to preserve that option somehow.

Yup. Just coming up with something now...

---

_Review requested from @charliermarsh by @ngnpope on 2023-02-10 23:34_

---

_Review request for @charliermarsh removed by @ngnpope on 2023-02-10 23:34_

---

_Review requested from @charliermarsh by @ngnpope on 2023-02-10 23:35_

---

_Comment by @ngnpope on 2023-02-10 23:38_

Bedtime for me 😴 Enjoy!

---

_@charliermarsh reviewed on 2023-02-11 17:48_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands.rs`:335 on 2023-02-11 17:48_

Ironically I expected `HelpFormat::Text` to be the "raw text" and `HelpFormat::Markdown` to be the "rendered Markdown".

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands.rs`:335 on 2023-02-11 21:13_

Thinking about it more... I do think it makes sense for `--format text` to render as plain-text, and `--format markdown` to render with `mdcat`. We could then make `--format markdown` the default. What do you think?

---

_@charliermarsh reviewed on 2023-02-11 21:13_

---

_@ngnpope reviewed on 2023-02-11 22:09_

---

_Review comment by @ngnpope on `crates/ruff_cli/src/commands.rs`:335 on 2023-02-11 22:09_

Sure. I won't be able to get around to it for a few days, so feel free to rejig - should be straightforward🙂

---

_@charliermarsh reviewed on 2023-02-11 22:09_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands.rs`:335 on 2023-02-11 22:09_

Okay cool, will do :)

---

_Merged by @charliermarsh on 2023-02-12 02:32_

---

_Closed by @charliermarsh on 2023-02-12 02:32_

---

_@not-my-profile reviewed on 2023-02-12 03:22_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/args.rs`:287 on 2023-02-12 03:22_

I think we should definitely choose a different name for this new format.

Because it's actually the `text` format that prints Markdown. The new format doesn't print Markdown but some rich text with escape sequences and special formatting. That it uses "mdcat" under the hood is only an implementation detail.

---

_@not-my-profile reviewed on 2023-02-12 03:24_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/args.rs`:287 on 2023-02-12 03:24_

Maybe `pretty`? (inspired by the git pretty formats)

---

_@not-my-profile reviewed on 2023-02-12 04:09_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/args.rs`:287 on 2023-02-12 04:09_

Opened #2797 to address this.

---

_Branch deleted on 2023-02-12 10:10_

---

_Review comment by @ngnpope on `crates/ruff_cli/src/args.rs`:287 on 2023-02-12 10:10_

👍🏻

---

_@ngnpope reviewed on 2023-02-12 10:10_

---
