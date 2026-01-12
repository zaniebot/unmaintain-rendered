```yaml
number: 13686
title: Ruff server pegs CPU and blocks VSCode when changing many files
type: issue
state: closed
author: adamh-oai
labels:
  - bug
  - server
assignees: []
created_at: 2024-10-09T00:19:25Z
updated_at: 2025-03-05T03:08:49Z
url: https://github.com/astral-sh/ruff/issues/13686
synced_at: 2026-01-12T15:54:53Z
```

# Ruff server pegs CPU and blocks VSCode when changing many files

---

_@adamh-oai_

In our large mostly-python monorepo, when using the VSCode ruff extension + ruff server, if I update to a commit has many changed files, `ruff server` will consume lots of CPU (up to 1000% in some cases), and I cannot save in VSCode because it is blocked on "Getting code actions (..., "Ruff")".

<img width="443" alt="image" src="https://github.com/user-attachments/assets/2deb6677-f0bb-40d8-a438-b8c1a4bc973c">

Sample of debug logs:

```
2024-10-08 14:39:50.935 [info] Falling back to bundled executable: /Users/adamh/.vscode/extensions/charliermarsh.ruff-2024.50.0-darwin-arm64/bundled/libs/bin/ruff
2024-10-08 14:39:50.955 [info] Resolved 'ruff.nativeServer: auto' to use the native server
2024-10-08 14:39:50.955 [info] Found Ruff 0.6.6 at /Users/adamh/.vscode/extensions/charliermarsh.ruff-2024.50.0-darwin-arm64/bundled/libs/bin/ruff
...
2024-10-08 17:00:20.165 [info] [Trace - 5:00:20 PM]    0.427054875s DEBUG ThreadId(06) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: ...
2024-10-08 17:00:20.241 [info] [Trace - 5:00:20 PM] Sending notification '$/cancelRequest'.
2024-10-08 17:00:20.241 [info] [Trace - 5:00:20 PM] Sending request 'textDocument/codeAction - (6)'.
...

024-10-08 17:10:03.895 [info] [Trace - 5:10:03 PM]  584.158088958s DEBUG ThreadId(57) ruff_server::session::index::ruff_settings: Ignored path via `exclude`:
...
```

There are lots and lots of the "ignored", which isn't surprising since I run with a config that excludes most paths in the repo.  


---

_Comment by @adamh-oai on 2024-10-09 00:21_

@hauntsaninja said I should tag @dhruvmanila since you've been talking about this.

---

_Label `server` added by @dhruvmanila on 2024-10-09 03:58_

---

_Comment by @dhruvmanila on 2024-10-09 03:59_

Thanks! I'll need to try to reproduce this somehow using a large repository, maybe I'll pickup home-assistant/core for this.

---

_Comment by @adamh-oai on 2024-10-09 05:45_

Looks like ours is about 5x larger than home-assistant/core.  I can reproduce this pretty easily, if you want to give me something to test.  A few other details:

* It actually seems to crawl at low CPU usage for a while, possibly because of a different VSCode issue on large updates that spawns 50 (!) ripgrep processes for find, and so it's mostly waiting on disk
* Once those rg finish it starts using a ton of CPU.  It's been chugging at 700% CPU for ~20 minutes now.
* The "ignored path" log entries are repeated many times, I count 25x for each directory, and this isn't the full log file.  This isn't 25x in a row, they just happen over and over again.

Both this and the 50-copies-of-rg issue sort of feel like VSCode is triggering a whole bunch of update notifications and they queue up somewhere and then are processed even though they all represent the same change.

---

_Comment by @MichaReiser on 2024-10-09 06:25_

@adamh-oai is there a high likelihood that each pull changes some pyproject.toml? 

> Both this and the 50-copies-of-rg issue sort of feel like VSCode is triggering a whole bunch of update notifications and they queue up somewhere and then are processed even though they all represent the same change.

That's what I suspect as well

* If there are many changes, file watchers might give up on tracking every file and instead assume everything has changed until the changes settle down. 
* As a result, Ruff is notified that a settings file may have changed, possibly several times, making Ruff reindex your project by walking the entire tree. 
* Ruff's VS code extension hasn't implemented cancellation messages yet. That means that Ruff might finish requests to completion that are now stale. 


---

_Comment by @dhruvmanila on 2024-10-09 06:44_

If they're representing the same changes, then there's a chance that VS Code must be sending the cancel requests as well (as also noted in your logs). It might become important to implement that in the server although I'd need to verify.

---

_Comment by @adamh-oai on 2024-10-21 23:08_

@dhruvmanila this hits us pretty regularly, is there anything I can test to narrow it down?

---

_Comment by @MichaReiser on 2024-10-22 06:33_

@dhruvmanila I wonder if it's time to change our settings discovery to be lazy instead of doing it eagerly. It would not only help with the single file case but also reduce the indexing cost here because the LSP would only ever discover the settings for the files currently open in the editor instead of all settings (and only traverse upwards, but never downwards)

---

_Comment by @dhruvmanila on 2024-10-22 06:57_

Yeah, I think we might want to prioritize that. And, even if we add support for cancellation, lazy settings discovery would still be more beneficial comparatively.

---

_Comment by @dhruvmanila on 2024-10-22 14:20_

I experimented a bit using `home-assistant/core` and I think we might be re-indexing the entire project multiple times.

---

_Comment by @dhruvmanila on 2024-10-25 05:39_

@adamh-oai Can I know how many Ruff configuration files exists in your project? It could be either `pyproject.toml` / `ruff.toml` / `.ruff.toml`.

---

_Comment by @hauntsaninja on 2024-10-25 06:59_

We have well over a thousand pyproject.toml in the repo, only about 150 of which contain `tool.ruff`. It's common to have a per-project ruff setup that `extend`-s from the root ruff config. Looks like there are two ruff.toml

---

_Label `bug` added by @dhruvmanila on 2024-10-25 09:02_

---

_Comment by @dhruvmanila on 2024-10-25 09:03_

Thanks, that's very useful. I think we've narrowed down the problem and will be working towards a fix for it.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-10-25 09:03_

---

_Comment by @dhruvmanila on 2024-11-12 09:24_

This is still on our mind. I'm planning to pick it up either next or the following week.

---

_Comment by @dhruvmanila on 2025-01-15 13:25_

Hi @adamh-oai / @hauntsaninja, I've started tackling the issue this week and one of the things that I want to check is the fix in #15495. It would be very helpful to know if that fix helps in your scenario as lazy indexing is somewhat complex to implement. I'm planning to get that PR released as soon as possible but I wanted to know if it would be possible from your end to get the version tested this or the coming week.

For context, the server indexes the entire project eagerly when the server starts and similarly whenever any of the configuration file changes. This is an expensive process for very large projects for which lazy indexing is one of the solution which would only resolve the configuration for all the open files in an editor and update it's state whenever there are any changes related to the configuration files.

---

_Comment by @adamh-oai on 2025-01-15 18:24_

Awesome.  I'll give it a try, but do you know when the next release of ruff with this change will happen?

---

_Comment by @MichaReiser on 2025-01-15 19:03_

We plan on releasing a new patch version tomorrow

---

_Comment by @dhruvmanila on 2025-01-17 09:08_

> Awesome. I'll give it a try, but do you know when the next release of ruff with this change will happen?

Ruff 0.9.2 has been released with the patch to avoid indexing the workspace multiple times.

---

_Comment by @dhruvmanila on 2025-02-27 08:34_

Hi @adamh-oai / @hauntsaninja, just want to follow-up on this issue. Has the experience improved in terms of performance or does the server still consume lots of CPU?

---

_Comment by @adamh-oai on 2025-02-27 23:34_

@dhruvmanila I've tried to repro and haven't seen it with the new version.  We're re-enabling the native server and I'll ping this thread if we get any reports.  Thanks!

---

_Comment by @hauntsaninja on 2025-03-03 20:45_

I think it's been better! I would tentatively close this issue. Thanks for fixing!

---

_Comment by @dhruvmanila on 2025-03-05 03:08_

Thank you both. I'll close this now but feel free to comment or open a new issue if the problem arises again.

---

_Closed by @dhruvmanila on 2025-03-05 03:08_

---
