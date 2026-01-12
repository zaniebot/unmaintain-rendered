```yaml
number: 1451
title: "-M is ignored when using --json format"
type: issue
state: closed
author: svenefftinge
labels:
  - question
assignees: []
created_at: 2019-12-28T10:41:13Z
updated_at: 2019-12-30T19:51:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1451
synced_at: 2026-01-12T16:13:23Z
```

# -M is ignored when using --json format

---

_@svenefftinge_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Through vscode-ripgrep node module.

#### What operating system are you using ripgrep on?

Linux (ubuntu)

#### Describe your question, feature request, or bug.

For Theia, I'd like to limit the line length of matches when using the json outputformat.
But it seems as the -M option doesn't have any effct when used together with --json.

#### If this is a bug, what is the expected behavior?

It would be great if ripgrep would not include the full text of the matched line, but a trimmed version when using `-M [n] --max-columns-preview`.


---

_Comment by @svenefftinge on 2019-12-28 13:06_

I noticed this seem to be documented
```
Other flags that control aspects of the standard output such as
            --only-matching, --heading, --replace, --max-columns, etc., have no effect
            when --json is set.
```
Still it would be great to have an option to skip sending megabytes of line contents when using --json.

---

_Comment by @BurntSushi on 2019-12-28 13:42_

The thinking here is that -M and other options like it are for the benefit of the human reading the output. JSON is meant to be consumed by machines, not humans, so options like this don't apply. Machines that want to reformat the display into a truncated version can do that themselves.

Could you say more about why specifically you want this other than "it would be nice"?

---

_Comment by @svenefftinge on 2019-12-28 14:55_

We use ripgrep in Theia for the full text search which works similarly to the one in VS Code. We don't need to see the full line contents, but only the offset information and a little bit of context around a match for the UI. When searching in large single-line files such as generated webpack output parsing the big lines are quite expensive. Unlike VS Code, Theia supports the web IDE use case as well, in which case overly large messages need to be avoided.

---

_Comment by @BurntSushi on 2019-12-29 12:49_

I don't see myself supporting `-M` in the JSON output, mostly due to implementation complexity. It falls outside the goals of the JSON printer, which really isn't supposed to take "display" stuff into account. I'm quite hesitant to start adding options which were _intended for better display for humans_ as a performance hack. For example, the JSON output also includes a `submatches` array, which are byte offsets into the `lines` text. If  `-M` is used to cut off part of the `lines` text, then the invariant that all byte offsets listed in `submatches` are valid offsets into `lines` is broken. Which means we either need to abide this broken invariant or remove sub-match offsets that aren't actually displayed in `lines`.

This complicates the JSON printer in a way that I'm not particularly happy with.

> When searching in large single-line files such as generated webpack output parsing the big lines are quite expensive. Unlike VS Code, Theia supports the web IDE use case as well, in which case overly large messages need to be avoided.

ripgrep has to read these lines anyway. It seems like reading them again in a JSON parser _shouldn't_ meaningfully change the performance profile.

I'd also suggest that if end users are having performance problems searching files that aren't human readable, then they might consider adding them to their `.ignore` or `.rgignore`. Then you side-step the problem entirely.

Since ripgrep emits JSON objects one per line, another approach you could take is to simply skip parsing a JSON object that is very long (and perhaps report this skipped match to the end user).

---

_Label `question` added by @BurntSushi on 2019-12-29 12:50_

---

_Comment by @svenefftinge on 2019-12-30 19:51_

Ok, I understand and agree.

---

_Closed by @svenefftinge on 2019-12-30 19:51_

---
