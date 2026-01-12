```yaml
number: 7404
title: Add --script option to uv init
type: pull_request
state: closed
author: jbvsmo
labels:
  - cli
assignees: []
base: main
head: feat/uv-init-script
created_at: 2024-09-15T02:56:23Z
updated_at: 2024-09-25T22:51:31Z
url: https://github.com/astral-sh/uv/pull/7404
synced_at: 2026-01-12T16:07:48Z
```

# Add --script option to uv init

---

_@jbvsmo_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This is a partial implementation of #7402 
Since there was no discussion yet, I didn't get much further than adding the command line option and writing the .py file

Feel free to use this PR (or not). I am unsure if I will have time to finish it.

There is one opinionated bit which is the readme "type" which is [part of the pep 723 spec](https://packaging.python.org/en/latest/specifications/inline-script-metadata/#specification) but you may want to think about it before introducing any such thing

Missing: 
 - The PackageName converts dots into dashes. I did not check how to fix it. You will see it when you run the command as the hello will show foo-py instead of foo.py
 - Add to the docs to teach people how to create scripts [here](https://docs.astral.sh/uv/guides/scripts/)
 - Combine python_request and requires_python as there is no standalone .python-version file
 - possibly add a shebang and make the file executable 
 - Test cases

## Test Plan

<!-- How was it tested? -->
uv init --script foo.py


---

_Converted to draft by @jbvsmo on 2024-09-15 02:58_

---

_Comment by @zanieb on 2024-09-15 14:47_

Sweet! Can you add a test case to `crates/uv/tests/init.rs` so we can see what the output looks like and iterate from there?

---

_Comment by @jbvsmo on 2024-09-15 17:44_

> Sweet! Can you add a test case to `crates/uv/tests/init.rs` so we can see what the output looks like and iterate from there?

Sure, I did that. Observe how the cli parses the file name into the package converting something.py into something-py instead of just "something". But that is also something you guys might want to decide.

A file without extension is also supported, which is common for executable scripts with a shebang. 

---

_Review comment by @T-256 on `crates/uv/tests/init.rs`:495 on 2024-09-15 18:51_

could avoid these lines?
I think most script authors won't go versioning the scripts, and they prefer to use module-docstring instead of description. the name field also inferred from filename usually.

---

_Review comment by @T-256 on `crates/uv/tests/init.rs`:478 on 2024-09-15 18:52_

could strip file extension when set the project name?

---

_@T-256 reviewed on 2024-09-15 18:52_

---

_@jbvsmo reviewed on 2024-09-15 19:29_

---

_Review comment by @jbvsmo on `crates/uv/tests/init.rs`:495 on 2024-09-15 19:29_

These are the kinds of opinion based decisions that are better chosen by the uv team, so feel free to perform them. I just wanted to kickstart the feature. I copied the same defaults from the --app solution. 

IMO though, versioning scripts is quite common and encouraged in the literature.

 

---

_@jbvsmo reviewed on 2024-09-15 19:30_

---

_Review comment by @jbvsmo on `crates/uv/tests/init.rs`:478 on 2024-09-15 19:30_

Yes, I would like so. I was afraid to break something regarding parsing package names since it is done automatically at the cli step

---

_@T-256 reviewed on 2024-09-15 19:45_

---

_Review comment by @T-256 on `crates/uv/tests/init.rs`:490 on 2024-09-15 19:45_

Can you also add no-readme tests to prevent extra newlines?

---

_@T-256 reviewed on 2024-09-15 19:45_

---

_Review comment by @T-256 on `crates/uv/tests/init.rs`:495 on 2024-09-15 19:45_

IMO, it'd be good to have minimal header metadata (just like PEP 723's [mentioned example](https://peps.python.org/pep-0723/#example)).

---

_@zanieb reviewed on 2024-09-15 23:18_

---

_Review comment by @zanieb on `crates/uv/tests/init.rs`:495 on 2024-09-15 23:18_

I would definitely omit the name here, I think.

---

_@zanieb reviewed on 2024-09-15 23:18_

---

_Review comment by @zanieb on `crates/uv/tests/init.rs`:478 on 2024-09-15 23:18_

I'd special case this entirely to say something like "Initialized script at `{path}`"

---

_Review comment by @zanieb on `crates/uv/tests/init.rs`:495 on 2024-09-15 23:19_

(Seems like the easiest way to deal with your package name problems too)

---

_@zanieb reviewed on 2024-09-15 23:19_

---

_@jbvsmo reviewed on 2024-09-16 00:23_

---

_Review comment by @jbvsmo on `crates/uv/tests/init.rs`:495 on 2024-09-16 00:23_

Ok, that is really good advice @zanieb . I believe I was complicating things trying to change how package names are parsed when in this case there is really no package
I pushed a commit with these changes and also @T-256 requests

---

_@jbvsmo reviewed on 2024-09-16 00:24_

---

_Review comment by @jbvsmo on `crates/uv/tests/init.rs`:478 on 2024-09-16 00:24_

Done too

---

_@jbvsmo reviewed on 2024-09-16 00:24_

---

_Review comment by @jbvsmo on `crates/uv/tests/init.rs`:490 on 2024-09-16 00:24_

Done

---

_Comment by @jbvsmo on 2024-09-16 00:28_

I think the only thing really missing now after the changes above are the docs update to tell people how to create scripts.

I decided against a shebang because posix does not really support multiple arguments and "#!/usr/bin/env uv run" would fail in some systems. 

---

_Marked ready for review by @jbvsmo on 2024-09-16 00:53_

---

_@zanieb reviewed on 2024-09-16 12:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:52 on 2024-09-16 12:36_

```suggestion
            anyhow::bail!("Missing script name to initialize");
```

---

_@zanieb reviewed on 2024-09-16 12:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:54 on 2024-09-16 12:36_

Should we use `try_exists` here?

Should we add metadata to the front of the script if it doesn't exist? Or should we tackle that in a separate pull request? I think we already have machinery for this for `uv add`

---

_@zanieb reviewed on 2024-09-16 12:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:56 on 2024-09-16 12:37_

```suggestion
                "Script already exists at `{}`",
```

---

_@zanieb reviewed on 2024-09-16 12:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:100 on 2024-09-16 12:37_

Generally, I'd use `matches!` instead of `!=` here. I'm not sure why. cc @BurntSushi 

---

_@zanieb reviewed on 2024-09-16 12:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:113 on 2024-09-16 12:39_

Perhaps we should have a match on `project_kind` above this? If we change something around path handling in the future, we could easily miss the `Script` case with this structure.

---

_@zanieb reviewed on 2024-09-16 12:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:51 on 2024-09-16 12:42_

Best practice is to do something like `let Some(path) = explicit_path else { bail }`.  However, similar to https://github.com/astral-sh/uv/pull/7404/files#r1761072383, you may want to just match on `project_kind` above on L44 when determining the `path` variable.

---

_Comment by @zanieb on 2024-09-16 12:43_

Thank you!

Lots of small comments on the implementation. If you would rather we just finished, let us know.

---

_Assigned to @zanieb by @zanieb on 2024-09-16 12:43_

---

_Label `cli` added by @zanieb on 2024-09-16 12:43_

---

_Comment by @jbvsmo on 2024-09-16 13:43_

@zanieb sure, go ahead thank you!

---

_@BurntSushi reviewed on 2024-09-16 13:53_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/init.rs`:100 on 2024-09-16 13:53_

`matches!` is strictly more flexible since you don't need a `PartialEq` impl to have it work. But for this specific instance, I don't think there's any meaningful advantage to one over the other. I don't mind the `!=` here personally, especially if you aren't adding `PartialEq` impls just to make it work.

---

_Comment by @charliermarsh on 2024-09-25 22:49_

Merged as https://github.com/astral-sh/uv/pull/7565. I added @jbvsmo as a co-author on that change, hope that's ok.

---

_Closed by @charliermarsh on 2024-09-25 22:49_

---

_Branch deleted on 2024-09-25 22:51_

---
