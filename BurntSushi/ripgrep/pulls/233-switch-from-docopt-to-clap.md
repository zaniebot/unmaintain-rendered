```yaml
number: 233
title: Switch from Docopt to Clap.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: clap
created_at: 2016-11-13T02:46:43Z
updated_at: 2016-11-21T11:50:13Z
url: https://github.com/BurntSushi/ripgrep/pull/233
synced_at: 2026-01-12T18:23:12Z
```

# Switch from Docopt to Clap.

---

_@BurntSushi_

There were two important reasons for the switch:

1. Performance. Docopt does poorly when the argv becomes large, which is
   a reasonable common use case for search tools. (e.g., use with xargs)
2. Better failure modes. Clap knows a lot more about how a particular
   argv might be invalid, and can therefore provide much clearer error
   messages.

While both were important, (1) made it urgent.

Note that since Clap requires at least Rust 1.11, this will in turn
increase the minimum Rust version supported by ripgrep from Rust 1.9 to
Rust 1.11. It is therefore a breaking change, so the soonest release of
ripgrep with Clap will have to be 0.3.

There is also at least one subtle breaking change in real usage.
Previous to this commit, this used to work:

    rg -e -foo

Where this would cause ripgrep to search for the string `-foo`. Clap
currently has problems supporting this use case
(see: https://github.com/kbknapp/clap-rs/issues/742),
but it can be worked around by using this instead:

    rg -e [-]foo

or even

    rg [-]foo

and this still works:

    rg -- -foo

This commit also adds Bash, Fish and PowerShell completion files to the
release, fixes a bug that prevented ripgrep from working on file
paths containing invalid UTF-8 and shows short descriptions in the
output of `-h` but longer descriptions in the output of `--help`.

Fixes #136, #189, #210, #230

---

_Comment by @BurntSushi on 2016-11-13 02:46_

cc @kbknapp


---

_Comment by @kbknapp on 2016-11-14 23:47_

I'm on mobile right now so I still want to read through this in more detail but it looks good so far! And I really, really like some of the things you did that I haven't thought of doing!

I'll update here once kbknapp/clap-rs#742 has been implemented.


---

_Comment by @BurntSushi on 2016-11-14 23:47_

@kbknapp Thanks! I was hoping you'd take a peek. ^_^


---

_Merged by @BurntSushi on 2016-11-18 01:22_

---

_Closed by @BurntSushi on 2016-11-18 01:22_

---

_Branch deleted on 2016-11-18 01:22_

---

_Comment by @BurntSushi on 2016-11-18 01:22_

Woohoo! @kbknapp Thanks for clap! ^_^


---

_Comment by @kbknapp on 2016-11-21 04:09_

@BurntSushi I saw you already noticed kbknapp/clap-rs#755 but this should address a few of the items you mentioned. For the use of a hyphen in specific option and not command wide there are plenty of edge cases that don't have 100% solutions (i.e. many people may want many different things). So I've implemented what I think to be the 95% solution.

Namely, edge cases arise when one of two things happen, 

* The option in question accepts multiple values
* The value the user wishes to supply partially or fully matches a valid flag

For the primary edge case example, assume `-e PAT...` where `PAT` can start with a hyphen. Also assume `-v`, `-a`, and `-l` are all valid short flags. Using `-e valid_pattern -val` can cause ambiguities. This can be solved by forcing `-e` to accept only value at a time `Arg::number_of_values(1)` and `Arg::multiple(true)`. This allows `-e PAT -e PAT` but disallows `-e PAT PAT`.

So there are workarounds, I just wanted to ensure you're aware some testing may be required :wink:

Also the ZSH completions should be fixed for you.

---

_Comment by @BurntSushi on 2016-11-21 11:50_

@kbknapp Ah that sounds perfect! I think I've banned all uses of `--flag val1 val2` anyway since it feels incredibly non-standard to me. :-) Thanks so much for the quick turnaround!

---
