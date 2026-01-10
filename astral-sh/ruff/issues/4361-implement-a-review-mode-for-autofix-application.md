```yaml
number: 4361
title: "Implement a \"review mode\" for autofix application"
type: issue
state: open
author: charliermarsh
labels:
  - core
  - cli
  - help wanted
assignees: []
created_at: 2023-05-10T20:38:52Z
updated_at: 2023-12-23T23:43:37Z
url: https://github.com/astral-sh/ruff/issues/4361
synced_at: 2026-01-10T11:09:47Z
```

# Implement a "review mode" for autofix application

---

_Issue opened by @charliermarsh on 2023-05-10 20:38_

This has been discussed in a few other issues (#2395, #3642), but I want to create a parent issue to track the scope and implementation.

We'd like to implement a new CLI mode in which users are presented with autofix suggestions one-by-one and given the opportunity to accept or reject fixes iteratively.

This likely wouldn't be the default, but rather would be put behind a `--interactive` or `--fix-interactive` flag.

`elm-review` is a good example of the behavior we want to implement -- see [here](https://github.com/charliermarsh/ruff/issues/3642#issuecomment-1480115355):

![FaHfMbbX0AA_Sba](https://user-images.githubusercontent.com/3869412/227010991-a19a6003-2faf-4504-b819-6bad8c7c486e.png)


---

_Label `core` added by @charliermarsh on 2023-05-10 20:38_

---

_Label `cli` added by @charliermarsh on 2023-05-10 20:38_

---

_Label `help wanted` added by @charliermarsh on 2023-05-10 20:38_

---

_Comment by @charliermarsh on 2023-05-10 21:03_

Here's the behavior I'm imaging:

- User runs `ruff --fix-interactive src/`.
- Internally, check the files.
- If there are any fixable violations, present them iteratively, accepting-and-applying or skipping as we go.
- Once we've cycled through, if at least one fix was applied, re-check the file.
- Repeat this process, since fixes can cause new fixable violations to appear (e.g., removing a usage could then cause an import to be unused). (We might end up showing duplicates here that need to be skipped again.)
- Once the review process is a no-op (all skipped, or no fixable violations), finish the interactive session and print out the remaining violations.

If a contributor is interested in taking this on, I'm happy to help with scoping and create separate issues for the various subtasks.

A few things that come to mind:

- We likely need is a variant on `autofix::apply_fixes` that prompts the user when applying each fix and prints out the diagnostic to the CLI.
- Right now, we process each file independently in parallel, apply fixes + re-check each file iteratively on its own, and then aggregate the results (How many errors were fixed? Which errors remain?). In order to support this, we likely need to move the fix application + re-checking up a level, such that we lint all the files, collect all the diagnostics, then iterate through to apply fixes on a file-by-file basis, and re-run the linter phase if any fixes were applied.


---

_Comment by @sladyn98 on 2023-05-16 21:24_

@charliermarsh I could give this a shot !, I might just need some extra guidance as I am still getting used to the codebase 

---

_Comment by @zanieb on 2023-05-16 21:34_

@sladyn98 I've also been eyeing this one as a fun project — happy to help out if you want to collaborate.

---

_Comment by @evanrittenhouse on 2023-05-16 21:50_

Ha, so was I!  @sladyn98 I'm also happy to help out if you'd like.

---

_Comment by @sladyn98 on 2023-05-16 21:57_

Lets go  ! @madkinsz @evanrittenhouse we can all do this together. 

---

_Comment by @charliermarsh on 2023-05-16 22:46_

Since this is a pretty substantial project, I might suggest that someone start by scoping out the work involved in whatever form they see fit for comments / questions, perhaps via a GitHub Discussion? This should probably also involve a little bit of hacking on Ruff (e.g., prototyping) to see around any corners and inform decisions.


---

_Comment by @sladyn98 on 2023-05-31 08:57_

@charliermarsh Yeah I am happy to open a discussion thread, I have been going through the rust code and have some questions. 

---

_Comment by @jfmengels on 2023-05-31 08:59_

(Author of `elm-review` here) I'm happy to answer questions as well (not on the implementation though, I don't know Rust not the Ruff codebase)

---

_Comment by @sladyn98 on 2023-06-07 19:53_

https://github.com/charliermarsh/ruff/discussions/4937 I Started a discussion here. Feel free to comment. I have started hacking a bit hoping to learn and implement more as I go on : )

---

_Comment by @tdulcet on 2023-12-23 16:44_

I was recently contributing to an open source project and trying to help them get started with the Ruff linter. The maintainer of the project requested that I only fix one type of issue per commit to make it easier for them to review. It is a small-medium sized project (8,443 lines of Python across 23 files), but they had never used a Python linter or formatter before, so there were over 900 errors from the subset of rules I decided to enable.

Anyway, because Ruff does not support this natively, I decided to write a simple wrapper script to allow interactively applying the autofixes per rule. The script also optionally supports committing the changes. In case it is helpful for anyone else, the script is available on GitHub: https://gist.github.com/tdulcet/c1f6415bcae66a4b0cfb6eb96b15444b. Feedback is welcome. For reference, the resulting PR is here: https://github.com/mail-in-a-box/mailinabox/pull/2343.

The script first runs the `ruff check --output-format json` command to get the full list of errors in JSON format. It then groups those errors by rule. For each rule, it shows the output of the `ruff rule <code>` and `ruff check --select <code>` commands. Then it checks if there are any autofixes available from the JSON output. If so, it additionally shows the output from the `ruff check --diff --select <code>` command (#7352 could eliminate the need for this). It then asks the user if they want to apply the autofixes. If the user says yes, it runs the `ruff check --select <code> --fix` command. Lastly, it gives the user a chance to make any manual fixes. After the user presses Enter, it continues on to the next rule. It only checks each rule once, but users can of course run the script multiple times as needed until everything is fixed.

Any command line options provided to the script are forwarded to that first `ruff check` command. There is also a `ARGS` list near the top of the script that allow providing additional arguments to every `ruff check` command, such as `--preview` or `--unsafe-fixes`.

---

_Comment by @eli-schwartz on 2023-12-23 23:43_

That is pretty neat! I just a few days ago did the same thing slowly by hand, repeatedly running `ruff check --select ...` while running over each error code in `UP` to see if there was anything to fix. Same rationale of wanting to commit each type of fix in its own commit. That script could have saved me some time...

---
