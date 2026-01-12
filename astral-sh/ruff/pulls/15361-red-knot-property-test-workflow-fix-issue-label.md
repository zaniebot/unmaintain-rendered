```yaml
number: 15361
title: "[red-knot] Property test workflow: Fix issue label, link to CI run"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/property-test-updates-fix
created_at: 2025-01-08T21:34:35Z
updated_at: 2025-01-08T21:48:21Z
url: https://github.com/astral-sh/ruff/pull/15361
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Property test workflow: Fix issue label, link to CI run

---

_@sharkdp_

## Summary

See title. Had to make a minor change, because it failed the zizmor pre-commit check otherwise (FYI @AlexWaygood)

```
error[template-injection]: code injection via template expansion
  --> /home/shark/ruff/.github/workflows/daily_fuzz.yaml:68:9
   |
68 |          - uses: actions/github-script@v7
   |  __________^
69 | |          with:
70 | |            github-token: ${{ secrets.GITHUB_TOKEN }}
71 | |            script: |
   | | ___________^
72 | ||             await github.rest.issues.create({
...  ||
77 | ||               labels: ["bug", "parser", "fuzzer"],
78 | ||             })
   | ||               ^
   | ||_______________|
   |  |_______________this step
   |                  github.server_url may expand into attacker-controllable code
   |
   = note: audit confidence → High
```



---

_Label `testing` added by @sharkdp on 2025-01-08 21:34_

---

_Label `red-knot` added by @sharkdp on 2025-01-08 21:34_

---

_Comment by @AlexWaygood on 2025-01-08 21:41_

> See title. Had to make a minor change, because it failed the zizmor pre-commit check otherwise (FYI @AlexWaygood)

Ooh interesting. @woodruffw could that context field really be controlled by an attacker...? That feels like it might be a false positive to me? :-)

([Docs here](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#github-context))

---

_@AlexWaygood approved on 2025-01-08 21:45_

Thanks!! Hardcoding `github.com` seems very unlikely to break, whether or not the zizmor diagnostic is a false positive 😄

---

_Merged by @sharkdp on 2025-01-08 21:47_

---

_Closed by @sharkdp on 2025-01-08 21:47_

---

_Branch deleted on 2025-01-08 21:47_

---

_Comment by @woodruffw on 2025-01-08 21:48_

> Ooh interesting. @woodruffw could that context field really be controlled by an attacker...? That feels like it might be a false positive to me? :-)

Yep, that looks like a false positive! Thanks for the ping!

---
