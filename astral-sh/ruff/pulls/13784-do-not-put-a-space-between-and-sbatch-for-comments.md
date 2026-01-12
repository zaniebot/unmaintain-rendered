```yaml
number: 13784
title: "Do not put a space between `#` and `SBATCH` for comments"
type: pull_request
state: closed
author: smsutherland
labels:
  - formatter
  - needs-decision
assignees: []
base: main
head: sbatch-comments
created_at: 2024-10-16T22:11:31Z
updated_at: 2024-10-19T19:57:52Z
url: https://github.com/astral-sh/ruff/pull/13784
synced_at: 2026-01-12T15:55:45Z
```

# Do not put a space between `#` and `SBATCH` for comments

---

_@smsutherland_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
The Slurm workload manager allows users to create job scripts and specify parameters to run the job with `#SBATCH [flag]`.
ruff currently reformats these to `# SBATCH [flag]`, which isn't recognized by Slurm because of the extra space.
This commit checks if comments start with `SBATCH`, and if so, does not add the space, similar to how shebangs are treated.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I didn't see any existing tests for adding that space, so I didn't add any of my own. I could go back and add tests though, if requested.
<!-- How was it tested? -->

## Additional Note
I understand if this doesn't get merged, since this change does technically constitute intentionally violating formatting guidelines.
However, this violation is in a situation where the user would very rarely want the current behavior.


---

_Review requested from @MichaReiser by @smsutherland on 2024-10-16 22:11_

---

_Comment by @zanieb on 2024-10-16 22:13_

I wonder if we need a regex configuration option or something for comment exclusions like this?

---

_Comment by @github-actions[bot] on 2024-10-16 22:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @MichaReiser on 2024-10-17 05:48_

---

_Comment by @MichaReiser on 2024-10-17 05:53_

The implementation itself looks correct to me but I don't think we want hardcoded exclusions in the formatter. A regex is an option, but it opens the door for many other regex-like settings.

There was a recent ask for skipping formatting of x number of lines at the beginning of a file and I would rather like to solve this with as few options as possible. Could you share a few examples of where these SBATCH comments are allowed? Would the Slurm manager be open to also recognize `#:SBATCH` or `# SBATCH`?

---

_Label `needs-decision` added by @MichaReiser on 2024-10-17 05:53_

---

_Comment by @smsutherland on 2024-10-17 12:41_

Skipping formatting of x lines at the start could solve this, but the number of lines needing to be skipped would vary from file to file, and would need to be changed if someone ever changes their script. Looking into this more, the desired behavior can be achieved by wrapping the relevant comments in `# fmt: off` `# fmt: on`. While a regex-based config option to, for example, disable formatting on lines matching some regex would be useful for similar cases which don't allow for the inclusion of an extra comment like `# fmt: off`, it wouldn't be in the scope of this PR. I'm happy to close this if there are no objections.

To answer your question though, `#SBATCH` comments must appear before any non-comment lines (not counting empty lines) of whichever script they appear in. The line *must* start with `#SBATCH` exactly to count (technically `#SLURM` is also recognized but this behavior is deprecated). As an example
```sh
#!/bin/bash
#SBATCH -a
# SBATCH -b
# another comment

#SBATCH --c=d
echo hello, world!
#SBATCH -e
```
would only recognize the options `-a --c=d`.
For the curious, the this behavior is encoded here: https://github.com/SchedMD/slurm/blob/master/src/sbatch/opt.c#L582-L609.

---

_Comment by @Skylion007 on 2024-10-17 13:27_

Just being able to define comment types as a "pragma" with a regex would solve this. Some comments are unfortunately magic.

---

_Comment by @zanieb on 2024-10-17 13:33_

I don't really buy the slippery-slope argument. Pragmas are a real use-case that we should clearly avoid breaking — I don't think adding support for excluding those means we'll be forced to support other configuration.

---

_Comment by @MichaReiser on 2024-10-17 14:08_

> Pragmas are a real use-case that we should clearly avoid breaking 

The challenge here is that many pragma comments are fine with whitespace insertion. So the configuration isn't a regex that specifies what pragma comments are. The formatter has other behavior around pragma comments: It excludes them from the measured line width. Would that behavior apply as well? 

---

_Comment by @zanieb on 2024-10-17 14:11_

> The formatter has other behavior around pragma comments: It excludes them from the measured line width. Would that behavior apply as well?

I don't see why not.

Perhaps someone should open an issue with SLURM asking if they're willing to support pragmas with leading whitespace as is standard in Python?

---

_Comment by @MichaReiser on 2024-10-17 14:45_

> I don't see why not.

Because that's where I think it becomes less clear what the setting configures because other pragma comments allow whitespace. The configuration would really be about a specific subset of pragma comments.  

---

_Comment by @zanieb on 2024-10-17 15:04_

> Because that's where I think it becomes less clear what the setting configures because other pragma comments allow whitespace. The configuration would really be about a specific subset of pragma comments.

I don't think it needs to change the semantics for pragma comments in general, just allow definition of _additional_ patterns to be treated pragma comments.

---

_Comment by @MichaReiser on 2024-10-17 20:28_

Agree, but we would have to distinguish between pragmas where the whitespace normalization should take place and those where it shouldn't 

---

_Comment by @MichaReiser on 2024-10-19 19:57_

I'll close this PR as is because we first need to align on a design. Let's continue the discussion in an issue if needed

---

_Closed by @MichaReiser on 2024-10-19 19:57_

---
