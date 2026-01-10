```yaml
number: 2186
title: Add support for rule aliasing
type: issue
state: open
author: charliermarsh
labels:
  - core
assignees: []
created_at: 2023-01-26T03:56:47Z
updated_at: 2024-05-07T22:05:01Z
url: https://github.com/astral-sh/ruff/issues/2186
synced_at: 2026-01-10T11:09:45Z
```

# Add support for rule aliasing

---

_Issue opened by @charliermarsh on 2023-01-26 03:56_

This is known thing we're working on and have mentioned in a bunch of other issues and PRs.

Apart from tracking the implementation, I want to use this issue to catalog any known aliases:

- `PGH002` (`pygrep-hooks`) is an alias of `G010` (`flake8-logging-format`)
- `B904` is an alias of `TRY200` (see: #2347)
- `R100` (`flake8-raise`) is an alias of `TRY200`
- `R101` (`flake8-raise`) is an alias of `TRY201`

These aren't one-to-one-aliases, but:

- `UP031` and `UP032` reimplement [`flynt`](https://github.com/ikamensh/flynt)
- `RUF100` reimplements [`yesqa`](https://github.com/asottile/yesqa)


---

_Label `core` added by @charliermarsh on 2023-01-26 03:56_

---

_Comment by @charliermarsh on 2023-01-26 04:27_

The Pylint rules are mapped to Ruff aliases in #970.

---

_Comment by @not-my-profile on 2023-01-26 06:54_

I'd call it "many-to-many mapping between codes and rules" since I think we also want to move away from rules having a primary code and rules needing a code at all (see #1773) and since we also want to support the mapping of one code to multiple rules.

Note that I have already nearly implemented this (other than being recently side-tracked by my CLI refactor #2155). I still want to perform the switch to subcommands (#2190) before tackling this issue, since introducing a new subcommand to list linters would help with tackling this issue :)

---

_Comment by @sbrugman on 2023-01-26 13:40_

`flake8-use-pathlib` (PTH) is an alias for most of [`refurb`](https://github.com/dosisod/refurb/tree/master/refurb/checks/pathlib)s pathlib violations

Just adding a thought to the discussion: users may care about `ruff`'s plugin coverage before making the switch. Developers might also wonder which are still open. If the many-to-many mapping registers aliases (and perhaps also _won't implement_) this could be computed/documented automatically.

PGH001 no-eval = PLW0123 eval-used
N804 = PLC0202

---

_Comment by @charliermarsh on 2023-01-26 15:00_

I hadn't really considered a many-to-many mapping -- I'd been assuming one-to-many. Interesting.

---

_Comment by @charliermarsh on 2023-02-13 17:39_

SIM104 is an alias for UP028.

---

_Comment by @Skylion007 on 2023-05-02 14:00_

PIE792 would be an alias of UP004 if we implemented it.

---

_Comment by @Skylion007 on 2023-09-30 16:10_

This problem is only growing as we implement more and more and rules and forces us to make decision like: https://github.com/astral-sh/ruff/pull/7506 We really should revisit this and try to make it a higher pri feature to prevent the maintainer burden / tech debt from continuing to grow.

---

_Comment by @Skylion007 on 2023-09-30 16:12_

There are a ton of aliases in pylint rules: https://github.com/astral-sh/ruff/issues/970

---

_Comment by @charliermarsh on 2023-09-30 16:56_

Yeah the rule system has been a highly important but hard-to-prioritize thing for a while now. Candidly, I don't know that rule aliasing is actually the right solution -- I feel like it addresses a symptom but creates other problems. For example, if a rule is aliased, does it show up twice if you enable it and the alias? If not, which code should it use? When you select a rule via a code that's an alias for that rule, what code shows up when we report diagnostics? Is it the code you selected, or the canonical code, which you didn't? How do `# noqa` identifiers work for aliased rules? Etc.

I want to view the problem holistically. Ideally, I'd like to recategorize all the rules into semantically meaningful categories, and add tooling to aid in migrations from Flake8 to Ruff (and from older to newer Ruff versions).


---

_Comment by @Skylion007 on 2023-09-30 17:06_

>  I feel like it addresses a symptom but creates other problems. For example, if a rule is aliased, does it show up twice if you enable it and the alias?

I think the more explicit use case here is category rules. If a rule is selected by categories, both aliases/rule codes will appear (they can both indicate the same problem). Flake8 has this problem when multiple plugins are enabled after all so we can copy their display logic. The main difference is that we actually deduplicate the rules on the backend?

` noqa: ` presumably noqa would need to block both rule codes? We could add a command that automatically "cannonicalizes" the aliases. Once again, we can look to flake8, copy what they have an migrate to our own better version gradually.

I agree all these problems are pretty good to fix in the long term, but many of have workarounds or solution courtesy flake8's admittingly flawed rule code system. While we are borrowing their rule code system, why not also borrow their way of handling duplicate/overlapping rules too?

My biggest concern currently is that having all these alias written down in the codebase somewhere instead of scattered throughout issues will make it easier to migrate to whatever final system we end up choosing.

---

_Comment by @charliermarsh on 2023-09-30 17:12_

Yeah, that's true, one model for aliases (similar to what you're describing) would be that the user-facing behavior is entirely unchanged from (e.g.) having two separate rules, and instead it's just an abstraction within the codebase to allow exposing a violation under multiple codes, which lets us categorize these internally and ensure that (e.g.) all Pylint rules are added when you enable Pylint.

This would have the nice effect that enabling Pylint gives you all the Pylint rules.

The downside is that we'd still be exposing the same rule multiple times in the docs and elsewhere (so, e.g., would https://github.com/astral-sh/ruff/pull/7506 be solved?), and if you run with `ALL` or overlapping categories, you'd see the same violation multiple times. Both of these could be sources of confusion (though I'm not commenting here on whether it's the right tradeoff or not).


---

_Comment by @Skylion007 on 2023-10-01 15:34_

True, but this tradeoff has to be balanced with the current behavior where enabling the PyLint code does not actually enable all the pylint checks we support.

---

_Comment by @zanieb on 2023-10-01 16:33_

It feels like this capability might just be a stop-gap until we recategorize the rules, but it's hard to know when we'll do that. I think I may prefer to focus our efforts on recategorization.

---

_Comment by @mikaelarguedas on 2024-01-06 22:36_

A couple other rule aliasing (not duplicated within Ruff but useful to know for displaying to people migrating or for a migration tool)

(flake8-super) SPR100 : UP008
(flake8-bugbear) B035 : RUF011
(flake8-loopy) LPY100 : FURB148
(flake8-tuple) T801 : COM818
(flake8-simplify) SIM203 : E713

---

_Comment by @mscheifer on 2024-05-07 22:05_

I think B036 is an alias of BLE001.

---
