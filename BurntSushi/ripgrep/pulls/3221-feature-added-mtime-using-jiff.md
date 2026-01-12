```yaml
number: 3221
title: "[Feature] Added mtime using jiff"
type: pull_request
state: closed
author: gege42o
labels: []
assignees: []
base: master
head: feature/mtime_and_ctime
created_at: 2025-11-16T12:59:17Z
updated_at: 2025-11-16T13:48:25Z
url: https://github.com/BurntSushi/ripgrep/pull/3221
synced_at: 2026-01-12T18:23:15Z
```

# [Feature] Added mtime using jiff

---

_@gege42o_

#### Timezone Handling
- Uses **UTC** for absolute dates (unambiguous, always available)
- Users can specify timezone explicitly in ISO format
- Relative times use system time automatically


### What Now Works

####  Relative Time
```bash
rg --mtime 7d          # Last 7 days
rg --mtime 2w          # Last 2 weeks
rg --mtime 3h          # Last 3 hours
rg --mtime "7 days"    # Long form
```

#### Find-Style Syntax (Backward Compatible)
```bash
rg --mtime +30d        # Older than 30 days
rg --mtime -7d         # Newer than 7 days
```

#### Absolute Dates (NEW!)
```bash
rg --mtime 2024-11-16                # ISO date
rg --mtime 2024/11/16                # Slash separator
rg --mtime 2024.11.16                # Dot separator
rg --mtime "2024-11-16 10:30:00"     # Date + time
rg --mtime 2024-11-16T10:30:00Z      # ISO 8601 with timezone
```

####  Natural Language
```bash
rg --mtime yesterday   # Last 24 hours
```

---

### Changes to be made

Work out a name for this command

---

_Converted to draft by @gege42o on 2025-11-16 12:59_

---

_Marked ready for review by @gege42o on 2025-11-16 13:09_

---

_Comment by @gege42o on 2025-11-16 13:09_

This is basically mtime using jiff, let me know of name changes that should be made.

---

_Comment by @BurntSushi on 2025-11-16 13:21_

There are still a lot of problems here. As I [said before](https://github.com/BurntSushi/ripgrep/pull/3220#issuecomment-3538685289), please come up with a _design_ first before submitting a PR. You can iterate on the design in #3214.

Please also disclose the extent to which you are using AI to craft your responses.

---

_Closed by @BurntSushi on 2025-11-16 13:21_

---

_Comment by @gege42o on 2025-11-16 13:27_

If you are talking about the responses on Github I do not use AI at all, it's just simple markdown. 

As for the code I mostly write it myself and if there are any errors for building and such that's when I use it, last resort. 

I am new to contributing and don't know all of the stages that I should go through.  What are you referring to when you talk about a design? Like should I  go in the issue and talk about what I **want** to do there? And after that maybe you approve the idea and I get to developing it?

---

_Comment by @BurntSushi on 2025-11-16 13:48_

There is no defined process. The design should cover user visible behavior.

Various notes:

* Please use the latest version of Jiff.
* Datetimes without time zones or offsets should attempt to use the system time zone.
* The input supported should take inspiration from [biff](https://github.com/BurntSushi/biff).

---
