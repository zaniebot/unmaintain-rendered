```yaml
number: 2325
title: "Include both `ruff help` and `ruff help check` in README"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/help
created_at: 2023-01-29T18:59:23Z
updated_at: 2023-01-30T03:56:32Z
url: https://github.com/astral-sh/ruff/pull/2325
synced_at: 2026-01-12T04:52:00Z
```

# Include both `ruff help` and `ruff help check` in README

---

_Pull request opened by @charliermarsh on 2023-01-29 18:59_

See discussion in #2284.

---

_Review comment by @JonathanPlasse on `README.md`:379 on 2023-01-29 19:47_

Would it be possible to use `ruff check` instead of `check` only?
```suggestion
Usage: ruff check [OPTIONS] [FILES]...
```

---

_@JonathanPlasse reviewed on 2023-01-29 19:47_

---

_@not-my-profile reviewed on 2023-01-29 19:50_

---

_Review comment by @not-my-profile on `README.md`:373 on 2023-01-29 19:50_

We may want to recommend `ruff help check` over `ruff check --help`.

---

_Comment by @not-my-profile on 2023-01-29 19:50_

Since the `ruff help` output isn't that long we may want to include both the output of `ruff help` and the output of `ruff help check`? Because otherwise the README will not list the other subcommands.

---

_Comment by @charliermarsh on 2023-01-29 19:52_

@not-my-profile - Yeah I considered that -- wasn't sure... I'll go ahead and make that change, since that's another vote in favor.

---

_@charliermarsh reviewed on 2023-01-29 20:49_

---

_Review comment by @charliermarsh on `README.md`:379 on 2023-01-29 20:49_

I really can't get Clap to do this through the programmatic API. I don't understand why. If I run `cargo run -- help check` or any equivalent command, it does the right thing (includes "ruff check" here).

---

_Review comment by @charliermarsh on `README.md`:379 on 2023-01-29 20:49_

I might have to call the binary directly or something.

---

_@charliermarsh reviewed on 2023-01-29 20:49_

---

_Review comment by @not-my-profile on `README.md`:379 on 2023-01-29 20:52_

Have you tried `Args::parse_from(["help", "check"])`?

---

_@not-my-profile reviewed on 2023-01-29 20:52_

---

_Review comment by @not-my-profile on `README.md`:379 on 2023-01-29 20:53_

Ah nvm ... it's about the name shown after the `Usage:` .... ok yeah I don't know.

---

_@not-my-profile reviewed on 2023-01-29 20:53_

---

_@charliermarsh reviewed on 2023-01-29 21:10_

---

_Review comment by @charliermarsh on `README.md`:379 on 2023-01-29 21:10_

I got this working by invoking the binary, same as in the integration tests (maybe there's a cleaner way to do that? Just going with what I know). It's obviously worse than using `Clap` directly, but since this is a dev-only command and will fail in CI if broken, I'm fine with it. The output is better.

---

_@charliermarsh reviewed on 2023-01-29 21:35_

---

_Review comment by @charliermarsh on `README.md`:379 on 2023-01-29 21:35_

Hmm, this actually doesn't trigger a build of the `ruff` binary unfortunately.

---

_@not-my-profile reviewed on 2023-01-29 21:44_

---

_Review comment by @not-my-profile on `README.md`:379 on 2023-01-29 21:44_

How about just generating the help via the library and doing a `.replace("Usage:", "Usage: ruff")` ... it's a hack but it works and it doesn't execute any binaries, which I think is a plus.

---

_@charliermarsh reviewed on 2023-01-29 21:46_

---

_Review comment by @charliermarsh on `README.md`:379 on 2023-01-29 21:46_

Yeah, that sounds reasonable.

---

_Renamed from "Use ruff check --help for README" to "Include both `ruff help` and `ruff help check` in README" by @charliermarsh on 2023-01-29 21:57_

---

_Merged by @charliermarsh on 2023-01-29 22:01_

---

_Closed by @charliermarsh on 2023-01-29 22:01_

---

_Branch deleted on 2023-01-29 22:01_

---

_@JonathanPlasse reviewed on 2023-01-29 22:16_

---

_Review comment by @JonathanPlasse on `README.md`:379 on 2023-01-29 22:16_

Maybe we should report it upstream to improve the API.

---

_Review comment by @not-my-profile on `README.md`:379 on 2023-01-30 03:56_

Reported to clap as https://github.com/clap-rs/clap/issues/4685 :)

---

_@not-my-profile reviewed on 2023-01-30 03:56_

---
