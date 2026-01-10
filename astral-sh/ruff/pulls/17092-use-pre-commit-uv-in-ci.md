```yaml
number: 17092
title: Use pre-commit-uv in CI
type: pull_request
state: closed
author: akx
labels:
  - ci
assignees: []
base: main
head: pre-commit-uv
created_at: 2025-03-31T12:30:02Z
updated_at: 2025-04-01T12:16:41Z
url: https://github.com/astral-sh/ruff/pull/17092
synced_at: 2026-01-10T19:40:36Z
```

# Use pre-commit-uv in CI

---

_Pull request opened by @akx on 2025-03-31 12:30_

## Summary

Follows up on #17052 to add in [pre-commit-uv](https://pypi.org/project/pre-commit-uv/), which patches `pre-commit` to use `uv` for venv creation as well.

More dogfooding! More speed! 😄 

> [!NOTE]
> I suspect caching could be removed from the `pre-commit` CI step with this... 

## Test Plan

> How was it tested? 

We're about to find out...

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-31 12:30_

---

_Marked ready for review by @akx on 2025-03-31 12:36_

---

_Comment by @github-actions[bot] on 2025-03-31 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ci` added by @AlexWaygood on 2025-03-31 13:56_

---

_@AlexWaygood reviewed on 2025-03-31 17:55_

How much do you think this is likely to buy us? Testing locally, it still seems to take pre-commit a while to setup the many virtual environments it needs if the pre-commit cache is cold, even with `pre-commit-uv`. But if the pre-commit cache is hot (which it usually is in our CI), that doesn't cost us anything at all.

I'm also not sure I _love_ the monkeypatching approach `pre-commit-uv` takes. I appreciate that there's not really any other way to get pre-commit to use uv internally, but it seems kinda fragile. If it bought us a lot, that would be one thing... but it seems like it might not?

---

_Comment by @akx on 2025-04-01 05:29_

> Testing locally, it still seems to take pre-commit a while to setup the many virtual environments it needs if the pre-commit cache is cold, even with `pre-commit-uv`.

I've locally installed my global `pre-commit` with `uv tool install pre-commit --with pre-commit-uv==4.1.4`, which has made venv creation appreciably faster, and me happier.

> But if the pre-commit cache is hot (which it usually is in our CI), that doesn't cost us anything at all.

Looks like it, yeah. The pre-commit lint in this branch is approximately the same speed as in `main` right now.

> I'm also not sure I _love_ the monkeypatching approach `pre-commit-uv` takes. I appreciate that there's not really any other way to get pre-commit to use uv internally,

I did try for more first-class support once upon a time... https://github.com/pre-commit/pre-commit/pull/3131

> but it seems kinda fragile. If it bought us a lot, that would be one thing... but it seems like it might not?

Yeah, I guess it's not worth it here – feel free to close if you like.

FWIW, I wrote a simple action (https://github.com/akx/pre-commit-uv-action/) to do this that I use for some of my projects :)

---

Now that I _am_ investigating pre-commit, though, running the same incantation as in the CI step locally (MacBook Pro, no slouch), with `--verbose`, shows it's _absolutely dominated_ by `actionlint`:

```
$ time pre-commit run --all-files --show-diff-on-failure --color=always --hook-stage=manual --verbose
Validate pyproject.toml..................................................Passed
- duration: 0.14s
mdformat.................................................................Passed
- duration: 1.32s
markdownlint-fix.........................................................Passed
- duration: 1.05s
blacken-docs.............................................................Passed
- duration: 0.31s
typos....................................................................Passed
- duration: 1.24s
cargo fmt................................................................Passed
- duration: 2.01s
ruff-format..............................................................Passed
- duration: 0.05s
ruff.....................................................................Passed
- duration: 0.02s
prettier.................................................................Passed
- duration: 0.18s
zizmor...................................................................Passed
- duration: 0.05s
Validate GitHub Workflows................................................Passed
- duration: 0.32s
Lint GitHub Actions workflow files.......................................Passed
- duration: 26.87s
________________________________________________________
Executed in   34.97 secs    fish           external
   usr time  306.37 secs    0.06 millis  306.37 secs
   sys time   14.57 secs    2.69 millis   14.57 secs
```

Looking at `htop` during that workflow linting, it seems the action spawns an unlimited number of [`shellcheck`s](https://www.shellcheck.net/) that compete for a finite number of CPUs' attention... 😩 

It could be worth it to limit `actionlint` to only run in CI if actions are actually changed?

**EDIT:** Oh... I guess it's the WASI-compiled shellcheck that's slow and heavy, but even more so on a GitHub Actions machine that's starved for cores.

**EDIT 2:** See #17108 🔧 

---

_Comment by @AlexWaygood on 2025-04-01 12:16_

> I did try for more first-class support once upon a time... [pre-commit/pre-commit#3131](https://github.com/pre-commit/pre-commit/pull/3131)

yup, I'm aware of the history 😄 thanks for your efforts there!

> I've locally installed my global `pre-commit` with `uv tool install pre-commit --with pre-commit-uv==4.1.4`, which has made venv creation appreciably faster, and me happier.

Nice. ISTM that it probably has much more of an impact locally than in CI, though. So I'm inclined not to go with this. But thanks anyway!! 

---

_Closed by @AlexWaygood on 2025-04-01 12:16_

---
