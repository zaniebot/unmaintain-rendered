```yaml
number: 3287
title: Action
type: pull_request
state: closed
author: brucearctor
labels: []
assignees: []
base: main
head: action
created_at: 2023-03-01T00:29:37Z
updated_at: 2023-03-17T20:57:15Z
url: https://github.com/astral-sh/ruff/pull/3287
synced_at: 2026-01-12T15:55:12Z
```

# Action

---

_@brucearctor_

_No description provided._

---

_Comment by @henryiii on 2023-03-01 17:53_

Is there a reason you can't just use `run: pipx run ruff==0.0.253`? It's a single line and far less complicated than this action. The _only_ reason to make an action IMO (which itself should be a composite action running the pipx line above) would be to allow Dependabot to update GHA pinned versions, which this implementation doesn't allow!

---

_Comment by @brucearctor on 2023-03-01 18:55_

LOL 🤦  -- I didn't mean for this to be a PR in here [ until I worked it all out ].  Had been running PRs in my own repo/branch.  

---

_Comment by @brucearctor on 2023-03-01 19:00_

Closing this PR until it actually works, or is deemed desired.  

@henryiii -- to your point, that is reasonable.  BUT, so would -->

`uses: charliemarsh/ruff@v0` And that could accept more parameters.  

This is partially a knowledge and patterns/practices thing for my teams of Data Engineers.  I am looking to arrive at appropriate actions to use, rather than them running bash/etc commands directly in actions.  

---

_Closed by @brucearctor on 2023-03-01 19:00_

---

_Comment by @henryiii on 2023-03-02 14:25_

There's nothing wrong with using `run:` actions and supported tools like `pipx`. That's what Actions was made for. You should look for an action only if it provides a benefit over using `run:`. `charliemarsh/ruff@v0` would provide no benefit over just directly using `pipx run ruff`, and it would require wrapping every option in a pass-through - not ideal at all. An action should only be added if it provides a benefit - better caching, pinning with auto-updates, simper setup, etc.

I'd say an action that uses a release and does not use the source code (which would always be true of a Ruff action, I think) should be in a separate repo anyway, like the pre-commit support already is. (FYI, you might look into pre-commit, it has an action & supports lots of things too - see https://scikit-hep.org/developer/style & https://scikit-hep.org/developer/gha_basic for some details).

---
