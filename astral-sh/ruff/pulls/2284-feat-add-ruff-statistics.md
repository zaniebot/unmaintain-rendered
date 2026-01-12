```yaml
number: 2284
title: "feat: add ruff --statistics"
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: statistics
created_at: 2023-01-28T01:08:58Z
updated_at: 2023-01-29T19:36:34Z
url: https://github.com/astral-sh/ruff/pull/2284
synced_at: 2026-01-12T04:52:00Z
```

# feat: add ruff --statistics

---

_Pull request opened by @spaceone on 2023-01-28 01:08_

Closes #1814.

---

_@spaceone reviewed on 2023-01-28 01:17_

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:363 on 2023-01-28 01:17_

can you help me here? I don't want to map every rule but only the `.first()` one but don't know how to get the first instance. `.first()` gives me only errors.

---

_Review comment by @charliermarsh on `ruff_cli/src/printer.rs`:363 on 2023-01-28 01:19_

I think you want something like:

```rust
let message: Option<String> = diagnostics.messages.iter().find(|message| message.kind.rule() == rule).map(|message| message.kind.body());
if let Some(message) = message{
  writeln!(...)
}
```

---

_@charliermarsh reviewed on 2023-01-28 01:19_

---

_@charliermarsh reviewed on 2023-01-28 01:33_

---

_Review comment by @charliermarsh on `ruff_cli/src/printer.rs`:363 on 2023-01-28 01:33_

Note that the `.map` in my version and your version are slightly different. In your version, you're `.map`-ing over an iterator, kind of like Python's `map()`. In my version, it's `map` on an `Option<String>`, which basically means: if the value is `None`, return `None`; if the value is `Some(String)`, run this function over `String`.

---

_@spaceone reviewed on 2023-01-28 01:59_

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:363 on 2023-01-28 01:59_

i try to further refactor, so that we can generate also JSON output.

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:363 on 2023-01-28 02:18_

@charliermarsh enhanced in 0d66a24bacbaec144351e5a9aba36483613cdc03 - what do you think?

The JSON output is:
```
[
  {
    "count": 3,
    "code": "E101",
    "message": "Indentation contains mixed spaces and tabs"
  },
]
```
would you prefer?:
```
{
  "E101": {                                                                                                                                                                         
      "count": 3,                                                     
      "message": "Indentation contains mixed spaces and tabs"       
   }, 
}
```

and the output is just appended to the regular output. would one want to combine it in case of the JSON formatter? i am unsure how to handle this. there would be two different items in the list then.

---

_@spaceone reviewed on 2023-01-28 02:18_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:388 on 2023-01-28 02:48_

Shouldn't these rather `anyhow::bail!()`?

---

_@not-my-profile reviewed on 2023-01-28 02:48_

---

_@not-my-profile reviewed on 2023-01-28 02:51_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:398 on 2023-01-28 02:51_

It would be nice to add a comment that this uses the same output format as `flake8 --statistics`.

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:388 on 2023-01-28 10:44_

added that. can we handle that on argparsing level somehow as well?

---

_@spaceone reviewed on 2023-01-28 10:44_

---

_@spaceone reviewed on 2023-01-28 10:45_

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:398 on 2023-01-28 10:45_

added a comment

---

_@not-my-profile reviewed on 2023-01-28 11:29_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:386 on 2023-01-28 11:29_

It probably makes sense to mention which formats are supported in the error message.

---

_@not-my-profile reviewed on 2023-01-28 11:30_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:398 on 2023-01-28 11:30_

nit: it would be nice to specifically mention `flake8 --statistics` instead of just flake8

---

_@spaceone reviewed on 2023-01-28 11:32_

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:398 on 2023-01-28 11:32_

done

---

_@spaceone reviewed on 2023-01-28 11:32_

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:386 on 2023-01-28 11:32_

done

---

_@not-my-profile reviewed on 2023-01-28 11:33_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:388 on 2023-01-28 11:33_

For a clean solution where we have a different enum we'd have to turn `statistics` into a subcommand ... I don't think we want to make it a top-level subcommand and a subsubcommand for the `checker` subcommand might be too confusing for the user.

Otherwise we can probably somehow hook into clap's conflict system but we'd still have the same enum so I don't really see the point of that ... it's better if this is defined in the printer.

---

_Comment by @spaceone on 2023-01-28 11:35_

@charliermarsh some off topic questions:
* ruff is released once a day (if there were changes)? this is done automatically?
* who are the maintainers - only you or is there a team already?

---

_Review comment by @spaceone on `ruff_cli/src/printer.rs`:388 on 2023-01-28 11:43_

the subcommand thing has been developed in the last days, right?
so the regular `ruff` command should be `ruff check` in the future?
i consider the additional output of statistics still as a option then.
`ruff statistics` or `ruff check statistics` doesn't fit for my feeling.

---

_@spaceone reviewed on 2023-01-28 11:43_

---

_Review comment by @not-my-profile on `ruff_cli/src/printer.rs`:388 on 2023-01-28 11:48_

Yes I agree since it makes sense to combine `--statistics` with most of the other options of the (implicit) `check` command it does make sense to keep it as an option :)

---

_@not-my-profile reviewed on 2023-01-28 11:48_

---

_Comment by @not-my-profile on 2023-01-28 11:54_

We don't have an auto-release setup. Ruff releases only happen when Charlie decides to make a release, see also https://github.com/charliermarsh/ruff/pull/2288#issuecomment-1407277090. Afaik Charlie is the only maintainer but he has been working on ruff full-time.

---

_Comment by @charliermarsh on 2023-01-29 01:25_

@spaceone - That's right -- I cut a release about once a day, but it's done manually by me. (Mostly just consists of bumping the version via commit, creating a release in GitHub, and ensuring that the GitHub Action does its job.)

I'm working on Ruff full-time. But we have a lot of contributors who are doing heavy lifting :)


---

_Comment by @charliermarsh on 2023-01-29 03:00_

This looks good, and I'll merge it as-is. But am I crazy to think that with `--statistics`, we should just show the statistics, and not the individual error codes? I know Flake8 shows both, but I was surprised by it, and it makes it much harder to pipe into other Unix tools (e.g., to sort the table).

---

_Comment by @charliermarsh on 2023-01-29 03:01_

(That would also resolve your question about the two JSON items.)

---

_Comment by @spaceone on 2023-01-29 08:48_

OK: I adjusted the branch to shows statistics only and not the individual errors.

---

_Merged by @charliermarsh on 2023-01-29 18:44_

---

_Closed by @charliermarsh on 2023-01-29 18:44_

---

_Comment by @JonathanPlasse on 2023-01-29 18:49_

Will you update the Readme to mention the use of this option?

---

_Comment by @charliermarsh on 2023-01-29 18:51_

@JonathanPlasse - Yeah we should probably change the `--help` in the README to use `run check --help` instead. Would that resolve it?

---

_Comment by @JonathanPlasse on 2023-01-29 19:36_

I think so yes.

---
