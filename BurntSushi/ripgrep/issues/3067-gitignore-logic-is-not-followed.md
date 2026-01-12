```yaml
number: 3067
title: .gitignore logic is not followed
type: issue
state: closed
author: savchenko
labels:
  - rollup
assignees: []
created_at: 2025-06-13T00:21:24Z
updated_at: 2025-10-11T02:07:01Z
url: https://github.com/BurntSushi/ripgrep/issues/3067
synced_at: 2026-01-12T16:13:25Z
```

# .gitignore logic is not followed

---

_@savchenko_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```

## Setup

- Git repository at `$HOME/mygitrepo`
- `.gitignore` file at `$HOME/mygitrepo/.gitignore`, contains `foobar/debug`
- `$HOME/mygitrepo/foobar/some/debug/flag` file, contains `baz`
- `$HOME/mygitrepo/foobar/debug/flag2` file, also contains `baz`

Repository filetree:
```
.
└── foobar
    ├── debug
    │   └── flag2
    └── some
        └── debug
            └── flag
```


## Walkthrough

Now, `git add foobar/ && git status --branch --short` shows:
```
## master
A  foobar/some/debug/flag
```

Which is correct, as we want to ignore folder `debug` only if it is `$HOME/mygitrepo/foobar/debug` (child node of the `foobar` folder located in the `$GITROOT`), not if it is `$HOME/mygitrepo/foobar/some/debug` (folder named `debug` elsewhere in the repository).

Now let's try to use `rg` here: `cd "$HOME/mygitrepo/foobar && rg baz"`

```
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

OK, sure: `rg --debug "baz" 2>&1 | grep debug`

```
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.23/src/walk.rs:1799: ignoring ./debu: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/user/mygitrepo/.gitignore"), original: "foobar/debug", actual: "foobar/debug", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.23/src/walk.rs:1799: ignoring ./somedebug: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/user/mygitrepo/.gitignore"), original: "foobar/debug", actual: "foobar/debug", is_whitelist: false, is_only_dir: false })))
Running with --debug will show why files are being skipped.
```

What's going on? Let's try to remove the `.gitignore` temporarily: `rm ../.gitignore && rg baz`:

```
some/debug/flag
baz

debug/flag2
baz
```

Which leads us to unclear logic where `foobar/debug` in the `.gitignore` makes `rg` to ignore `foobar/some/debug/flag`.

### How did you install ripgrep?

apt

### What operating system are you using ripgrep on?

Debian 13

### Describe your bug.

ripgrep ignores files that should not be ignored

### What are the steps to reproduce the behavior?

ripgrep treats `.gitignore` differently compared to `git`

### What is the actual behavior?

```
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.23/src/walk.rs:1799: ignoring ./debu: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/user/mygitrepo/.gitignore"), original: "foobar/debug", actual: "foobar/debug", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|/usr/share/cargo/registry/ignore-0.4.23/src/walk.rs:1799: ignoring ./somedebug: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/user/mygitrepo/.gitignore"), original: "foobar/debug", actual: "foobar/debug", is_whitelist: false, is_only_dir: false })))
```

### What is the expected behavior?

ripgrep should have returned 2 matches

---

_Comment by @savchenko on 2025-06-13 01:21_

The bug appears to be fixed by [#2933](https://patch-diff.githubusercontent.com/raw/BurntSushi/ripgrep/pull/2933.patch) which applies cleanly on top of [14.1.1](https://github.com/BurntSushi/ripgrep/releases/tag/14.1.1)

---

_Comment by @MrGreenTea on 2025-10-09 08:50_

I have a similar issue where ripgrep does NOT ignore files it should ignore depending on from which directory the search is started:

Correct when run from project root (inside a monorepo, so one level deep, i.e. git root is `/root` and we're in `/root/directus`
```
> rg schedule -l
pnpm-lock.yaml
docs/how_to_generate_activities.md
schema-sync/data/seed__multiple_choice_activity_definition.json
schema-sync/data/directus_operations.json
schema-sync/data/directus_permissions.json
schema-sync/data/directus_flows.json
schema-sync/data/schema/multiple_choice_activity_definition.json
schema-sync/data/schema/weekly_schedules_question_pools.json
schema-sync/data/schema/courses.json
schema-sync/data/schema/weekly_schedules.json
extensions/news-notification/src/index.ts
extensions/activity-notifications/src/index.ts
```

But i'm only interested in the code of extensions, so I would expect this to work:

```
> rg schedule extensions/ -l
extensions/news-notification/src/index.ts
extensions/activity-notifications/src/index.ts
extensions/push-notifications/dist/index.js
extensions/news-notification/dist/index.js
extensions/activity-notifications/dist/index.js
```

But suddenly it searches ignored build artifacts (`dist/index.js`) which messes up the results.

Would this be caused by the same issue or should I create a separate one?

The relevant `.gitignore` from `/root/directus`:

```gitignore
extensions/*/node_modules
extensions/*/dist
```

---

_Label `rollup` added by @BurntSushi on 2025-10-11 01:00_

---

_Closed by @BurntSushi on 2025-10-11 02:07_

---
