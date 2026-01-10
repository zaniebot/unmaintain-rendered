---
number: 4550
title: "Help switches from next-line back to same-line format when name/placeholder gets *wider*"
type: issue
state: closed
author: gnprice
labels:
  - C-bug
assignees: []
created_at: 2022-12-13T06:33:51Z
updated_at: 2023-01-31T22:12:33Z
url: https://github.com/clap-rs/clap/issues/4550
synced_at: 2026-01-10T01:27:57Z
---

# Help switches from next-line back to same-line format when name/placeholder gets *wider*

---

_Issue opened by @gnprice on 2022-12-13 06:33_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

master

### Minimal reproducible code

Same as #3300.

(That report is on version 3.0.5, but the same behavior is also present at master. I initially ran into it on master, but then found #3300, which provides a nice handy repro recipe.)


### Steps to reproduce the bug with the above code

Same as #3300.

### Actual Behaviour

The two versions of the application code in the #3300 repro differ by the presence of a `value_name` field on an argument. One version sets a concise `value_name`, so that the argument's synopsis is:
`--merge-conflict-theirs-diff-header-decoration-style <STYLE_STRING>`
The other version leaves out `value_name` on that argument, so that its placeholder is derived from the name and the synopsis is:
`--merge-conflict-theirs-diff-header-decoration-style <MERGE_CONFLICT_THEIRS_DIFF_HEADER_DECORATION_STYLE>`

In the version with `STYLE_STRING`, the `-h` output is shown in next-line format. By default `-h` is in same-line format, but the auto-next-line-help logic kicks in: we see that the same-line format would be very wide (around 200 columns), so we switch to next-line in order to better stay within a readable width. Excerpt of the result:
```
        --merge-conflict-ours-diff-header-decoration-style <STYLE_STRING>
            Style string for the decoration of the header above the 'ours' merge conflict diff
            [default: box]

        --merge-conflict-ours-diff-header-style <STYLE_STRING>
            Style string for the header above the 'ours' branch merge conflict diff [default:
            normal]

        --merge-conflict-theirs-diff-header-decoration-style <STYLE_STRING>
            Style string for the decoration of the header above the 'theirs' merge conflict diff
            [default: box]

        --merge-conflict-theirs-diff-header-style <STYLE_STRING>
            Style string for the header above the 'theirs' branch merge conflict diff [default:
            normal]
```

In the version with `MERGE_CONFLICT_THEIRS_DIFF_HEADER_DECORATION_STYLE`, the same-line format is even wider than that. But this time that even-wider output is actually the one we choose to show — we revert to same-line instead of next-line format. Excerpt:
```
        --merge-conflict-ours-diff-header-decoration-style <STYLE_STRING>                                            Style string for the decoration of the header above the 'ours' merge conflict diff [default: box]
        --merge-conflict-ours-diff-header-style <STYLE_STRING>                                                       Style string for the header above the 'ours' branch merge conflict diff [default: normal]
        --merge-conflict-theirs-diff-header-decoration-style <MERGE_CONFLICT_THEIRS_DIFF_HEADER_DECORATION_STYLE>    Style string for the decoration of the header above the 'theirs' merge conflict diff [default: box]
        --merge-conflict-theirs-diff-header-style <STYLE_STRING>                                                     Style string for the header above the 'theirs' branch merge conflict diff [default: normal]
```


### Expected Behaviour

When the help output in same-line format would already be so wide that we decide to switch to next-line format, and then it gets even wider still, that should cause us to stay in next-line format — not to revert back to same-line format.


### Additional Context

The key logic is this, in `src/output/help_template.rs`, as the `bool` expression determining whether we switch automatically to next-line help format:
```rs
            self.term_w >= taken
                && (taken as f32 / self.term_w as f32) > 0.40
                && h_w > (self.term_w - taken)
```
Specifically, the first line says that if the width `taken` already occupied by the longest argument synopsis is itself longer than the terminal width `self.term_w`, we stay in same-line format regardless of the other conditions. Deleting that first comparison fixes this bug.

In #3300, the reporter @dandavison wanted same-line format always, even when very wide; so this behavior, when it applied, acted as an accidental way of getting the behavior he wanted. But, as evidenced by his filing that bug, it's a fragile and counterintuitive way of doing so. There's discussion in that thread (https://github.com/clap-rs/clap/issues/3300#issuecomment-1015578541) of a more robust way one can get that behavior today, and of possible other ways in the future.

This behavior has been present since the auto-next-line-format logic was first added, in b7793a2f4d back in 2016-08. (More history described by @epage at https://github.com/clap-rs/clap/issues/3300#issuecomment-1312636492.) I don't see any discussion of this quirk in the original issue thread #597, nor in the related #587.

It's not in the changelog entries added by that commit b7793a2f4d, either. And I don't see any documentation of the auto-next-line feature outside the changelog (e.g. at https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#v2110-2016-08-28 ), so this "give up when even wider" behavior seems not to be documented at all.

As discussed at https://github.com/clap-rs/clap/issues/3300#issuecomment-1312636492 , it's not entirely clear we want this overall auto-next-line-help feature at all. But I think removing this quirk is both an improvement as long as we do keep the overall feature, and a simplifying step that makes it a bit easier to work through removing it if the decision goes in that direction.


### Debug Output

_No response_

---

_Label `C-bug` added by @gnprice on 2022-12-13 06:33_

---

_Comment by @epage on 2022-12-13 15:26_

Could you clarify what about this needs to be a separate conversation than #3300?  I do not want to split the conversation  on the next-line-help auto-detection between two Issues.  Almost all of this issue is spent re-hashing #3300 and if there is something that is unique or different about this that needs to be a separate issue, it was buried in the details. 

---

_Referenced in [clap-rs/clap#4631](../../clap-rs/clap/issues/4631.md) on 2023-01-12 14:50_

---

_Comment by @epage on 2023-01-31 22:12_

Without further details, I'm closing this out in favor of the other discussions.

---

_Closed by @epage on 2023-01-31 22:12_

---
