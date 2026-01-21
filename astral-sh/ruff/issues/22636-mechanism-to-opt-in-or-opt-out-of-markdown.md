```yaml
number: 22636
title: Mechanism to opt-in or opt-out of markdown formatting
type: issue
state: open
author: amyreese
labels:
  - configuration
assignees: []
created_at: 2026-01-16T23:26:57Z
updated_at: 2026-01-21T09:57:25Z
url: https://github.com/astral-sh/ruff/issues/22636
synced_at: 2026-01-21T10:59:48Z
```

# Mechanism to opt-in or opt-out of markdown formatting

---

_@amyreese_

Once we have more of the feature set completed and want to stabilize support, there are a few paths we could take:

- Require users who want markdown formatting on-by-default to set `extend-include = ["**/*.md"]` in their project config (opt-in).
- Add a new `format.markdown-code-blocks = true` config option, similar to docstring formatting, that would automatically include `.md` files in the global default search path (opt-in). What do do if the user explicitly passes a `.md` file while the feature is disabled is tbd.
- Turn it on by default, including `.md` in the global default search path, and allow users to set `exclude = ["**/*.md"]` or similar in their project config if they don't want the feature (opt-out).


---

_Assigned to @amyreese by @amyreese on 2026-01-16 23:26_

---

_Comment by @amyreese on 2026-01-20 00:47_

Thinking about this some more (as I was adding a new option in #22750) should the "opt-in" for now simply be something like recommending `extend-include = ["**/*.md"]` and let `ruff format foo.md` just work even without needing `--preview` or a dedicated config option?

@MichaReiser @ntBre 

---

_Comment by @MichaReiser on 2026-01-20 07:12_

Ah sorry. I didn't see your comment here before posting on the PR.

I don't know. But using `include`/`exclude` is definitely the first thing that comes to mind when it's about defining on which files Ruff should run. I don't have the answer here but we should explore the different options and then decide on a design


---

_Comment by @ntBre on 2026-01-20 15:36_

I also don't know, but it makes some sense to me to reuse `include` and `exclude`. It does seem a bit annoying to have to `extend-include = ["**/*.md"]` to enable the feature though. Are we planning to make markdown formatting off by default?

---

_Comment by @amyreese on 2026-01-20 15:59_

The main question to me was whether we add `*.md` to the global/default `INCLUDE` or `INCLUDE_PREVIEW` and use a feature flag similar to `docstring-code-format` to turn on/off actually *formatting* (or linting?) those files, or if we just support the ability to format/lint markdown files with no feature flags, and just leave it to users to add `*.md` to their `include`/`extend-include` lists themselves in order to "turn it on".

---

_Comment by @amyreese on 2026-01-20 17:15_

Will ask in the main issue if users want ruff to format markdown files by default or want explicit opt-in

---

_Comment by @Avasam on 2026-01-21 00:34_

Since opinions were requested: I would enable this feature. Formatting code blocks is really neat and I believe something the `dprint` formatter Markdown plugin also does by offloading to its other plugins (similarly with the html/css/ts plugins).

Since I'm using shared configs I don't really care if it's opt-in/out by default. But I'd lean to say opt in until stable/confident enough, then switch in a breaking version.

---

_Comment by @DetachHead on 2026-01-21 03:28_

> Turn it on by default, including `.md` in the global default search path, and allow users to set `exclude = ["**/*.md"]` or similar in their project config if they don't want the feature (opt-out).

i think this is the better choice. i prefer when linters enable all checks by default because it's much better for discoverability. you can be confident that all users who disabled it have done so because they don't want it, and that all users who keep it enabled do.

if this feature was disabled by default, a vast majority of people who don't check changelogs would be missing out on this feature that they would enable if they knew it existed.

---

_Label `configuration` added by @MichaReiser on 2026-01-21 07:52_

---

_Comment by @paddyroddy on 2026-01-21 09:57_

I would definitely be using this feature when available. My feeling is it is probably better to be opt-out. I would happily just let ruff do its thing. I can see some people who have opinions to let them be won over by automatic linting. The only exception I can see is where your quite short example spans many lines in markdown due to many arguments in a function (for example). But that could be "fixed" by adjusting the characters per row.

---
