---
number: 786
title: Consider adding server setting to provide configuration file
type: issue
state: closed
author: kkpattern
labels:
  - configuration
  - needs-decision
  - server
assignees: []
created_at: 2025-07-09T07:18:50Z
updated_at: 2025-12-22T15:13:21Z
url: https://github.com/astral-sh/ty/issues/786
synced_at: 2026-01-10T01:52:52Z
---

# Consider adding server setting to provide configuration file

---

_Issue opened by @kkpattern on 2025-07-09 07:18_

Since ty now supports the `--config-file` flag, we can add this flag to the `server` command. This would greatly benefit users running ty server within a monorepo.



---

_Label `configuration` added by @AlexWaygood on 2025-07-09 07:19_

---

_Label `server` added by @AlexWaygood on 2025-07-09 07:19_

---

_Comment by @dhruvmanila on 2025-07-09 08:01_

Thanks for the request!

I agree that it'll be useful to have something similar to `--config-file` for the server but it shouldn't be a command-line flag. This is because starting a language server is mostly done by the editor and is not something a user would do quite often. So, this kind of configuration should be exposed as server setting. I'll be working on that soon (#82) after which we can start including certain settings that the user can set directly in their editor.

I'm going to convert this issue in a way to capture what I've said above and make it a sub-issue for client settings.

---

_Renamed from "Add `--config-file` to `server` command" to "Consider adding server setting to provide configuration file" by @dhruvmanila on 2025-07-09 08:02_

---

_Label `needs-decision` added by @dhruvmanila on 2025-07-09 08:03_

---

_Comment by @dhruvmanila on 2025-07-09 08:03_

I've added `needs-decision` because it might be useful to first discuss whether something like this is useful to add. For context, Ruff provides this option: https://docs.astral.sh/ruff/editors/settings/#configuration.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 09:13_

---

_Comment by @ColemanDunn on 2025-12-18 17:05_

I would like to add right now it is impossible to use the ty vscode extension in a monorepo setup. More info here: https://github.com/astral-sh/ty/issues/2034#issue-3740386726

I also want to point out that the ruff extension is able to work in a monorepo (and it doesn't even have an option in the extension to specify a config file, it finds it automatically and just works)

Some feature parity between ty and ruff here would be nice 

---

_Comment by @MichaReiser on 2025-12-18 17:25_

I should have a PR for this any minute.

---

_Closed by @MichaReiser on 2025-12-22 15:13_

---
