```yaml
number: 954
title: Grouped format implementation
type: pull_request
state: merged
author: hay-kot
labels: []
assignees: []
merged: true
base: main
head: feat/grouped-format
created_at: 2022-11-29T02:22:33Z
updated_at: 2022-11-29T23:52:09Z
url: https://github.com/astral-sh/ruff/pull/954
synced_at: 2026-01-12T15:55:05Z
```

# Grouped format implementation

---

_@hay-kot_

This PR adds an additional option to the `--format` flag "grouped" which changes the output. Things to note

1. I extracted the pre and post printing parts into their own functions
2. This implementation relies on the diagnostic messages to be sorted by file
3. I don't  know Rust that well so please let me know if anything needs fixing! 

Closes #948 

**Output Example**

<img width="590" alt="CleanShot 2022-11-28 at 21 33 41@2x" src="https://user-images.githubusercontent.com/64056131/204456148-4a9bba08-189d-4a34-8bbb-64cccd4eb14d.png">



---

_Marked ready for review by @hay-kot on 2022-11-29 06:37_

---

_Renamed from "wip: basic grouped implementation" to "Grouped format implementation" by @hay-kot on 2022-11-29 06:39_

---

_Comment by @charliermarsh on 2022-11-29 18:01_

Awesome, thank you! Will review today.

---

_Review requested from @charliermarsh by @charliermarsh on 2022-11-29 18:01_

---

_@charliermarsh reviewed on 2022-11-29 22:27_

---

_Review comment by @charliermarsh on `src/printer.rs`:62 on 2022-11-29 22:27_

Nit: small typo (`post_text`)

---

_@charliermarsh reviewed on 2022-11-29 22:28_

---

_Review comment by @charliermarsh on `src/printer.rs`:121 on 2022-11-29 22:28_

I think this assumes that the messages are sorted by filename. Maybe slightly preferable to create a HashMap from filename to messages? That can also help us avoid the clones.


---

_@charliermarsh reviewed on 2022-11-29 22:31_

---

_Review comment by @charliermarsh on `src/printer.rs`:121 on 2022-11-29 22:31_

Ehh... maybe this is simpler, actually.

---

_Comment by @charliermarsh on 2022-11-29 22:32_

Looks good to me! I'm going to make some minor tweaks to the output formatting, then merge.

---

_@hay-kot reviewed on 2022-11-29 22:45_

---

_Review comment by @hay-kot on `src/printer.rs`:121 on 2022-11-29 22:45_

I took a stab at a hashmap for the sake of it. I can't really make an educated guess on perf, but I think either is simple enough to understand. 🤷 

```rust
let mut map = std::collections::HashMap::new();

for message in &diagnostics.messages {
    map.entry(&message.filename)
        .or_insert_with(Vec::new)
        .push(message);
}

for (filename, messages) in map {
    println!(
        "\n{}:",
        relativize_path(Path::new(&filename)).bold().underline()
    );
    for message in messages {
        println!(
            "    {}{}{} {}  {}",
            message.location.row(),
            ":".cyan(),
            message.location.column(),
            message.kind.code().as_ref().red().bold(),
            message.kind.body(),
        );
    }
}
```

---

_Comment by @charliermarsh on 2022-11-29 22:50_

(Namely, I'm going to change it to mirror ESLint's behavior, such that the `row:column` colon is always aligned, and there are always two spaces between each section.)

---

_Merged by @charliermarsh on 2022-11-29 23:45_

---

_Closed by @charliermarsh on 2022-11-29 23:45_

---

_Comment by @charliermarsh on 2022-11-29 23:51_

Thank you!

---

_Comment by @charliermarsh on 2022-11-29 23:52_

We should make `format` a supported option in `pyproject.toml` too.

---
