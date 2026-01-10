```yaml
number: 6183
title: "Display the default value of `--link-mode` in help"
type: pull_request
state: closed
author: eth3lbert
labels: []
assignees: []
base: main
head: help-link-mode
created_at: 2024-08-18T16:08:14Z
updated_at: 2024-10-09T22:35:49Z
url: https://github.com/astral-sh/uv/pull/6183
synced_at: 2026-01-10T12:53:33Z
```

# Display the default value of `--link-mode` in help

---

_Pull request opened by @eth3lbert on 2024-08-18 16:08_

## Summary

Part of #6173 .

## Test Plan
Inspect the help output for the following commands.

`cargo run pip install --help`
<img width="1512" alt="image" src="https://github.com/user-attachments/assets/3272ca28-ba07-4760-a33c-cc304b112050">

`cargo run pip sync --help`
<img width="1504" alt="image" src="https://github.com/user-attachments/assets/ababcc9e-424e-459f-8855-ae10948a1b92">

`cargo run pip venv --help`
<img width="1493" alt="image" src="https://github.com/user-attachments/assets/65042a5b-aaba-4584-9ff4-db9959fe6436">


---

_Comment by @eth3lbert on 2024-08-18 16:34_

Based on the [CI failure](https://github.com/astral-sh/uv/actions/runs/10442002933/job/28913829027#step:8:14394)
```

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: tool_install_version-3
Source: crates/uv/tests/tool_install.rs:306
────────────────────────────────────────────────────────────────────────────────
Expression: fs_err::read_to_string(tool_dir.join("black").join("uv-receipt.toml")).unwrap()
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    4     4 │     { name = "blackd", install-path = "[TEMP_DIR]/bin/blackd" },
    5     5 │ ]
    6     6 │ 
    7     7 │ [tool.options]
    8       │-exclude-newer = "2024-03-25T00:00:00Z"
          8 │+exclude-newer = "2024-03-25T00:00:00Z"
          9 │+link-mode = "hardlink"
────────────┴───────────────────────────────────────────────────────────────────
````
It seems the tool receipt will record the given options. Therefore, we should only set the value when it differs from the default.

---

_Comment by @eth3lbert on 2024-08-18 16:47_

Also note that the default value varies by platform. We might need some help text about this to prevent confusion when reading the cli reference on the site.


---

_Comment by @eth3lbert on 2024-08-18 17:05_

I'm not sure how we should handle the snapshot of cli references, as the output depends on the platform it's generated on. :sweat_smile:

---

_Comment by @zanieb on 2024-08-19 14:22_

Lots of complications to consider here! I'm glad you just started with one option. We're going to be focused on 0.3.0 for a bit here — but after that I can try to help with the CLI reference guide and the snapshots. @charliermarsh is also going to need to take a look regarding the `.then_some` pattern, I'm not sure what implications this has for our tracking of settings in general.

---

_Comment by @eth3lbert on 2024-08-19 14:57_

Or perhaps for this kind of platform-dependent default value, it would be better to display something similar to the CLI reference?  e.g.
``` shell-session
      --link-mode <LINK_MODE>                  The method to use when installing packages from the global cache. Defaults to `clone` (also known as Copy-on-Write) on macOS, and `hardlink` on Linux and
                                               Windows [env: UV_LINK_MODE=] [possible values: clone, copy, hardlink, symlink]
```

---

_Comment by @zanieb on 2024-08-19 15:48_

I think it's okay to show the default value per-platform in the CLI help menu, but yeah we'll want show both in the reference documentation — sounds complicated. Might be easier to always show them both for the sake of snapshots — but hopefully it's only a small amount of snapshots that actually include the help menu and we can just use filters...

---

_Comment by @eth3lbert on 2024-08-20 01:26_

Based on the CI feedback, resetting only for `ToolOptions` seems more correct. Now, it only fails on cli references :)

---

_Comment by @eth3lbert on 2024-08-20 09:33_

I also noticed that we could implement the `Display` for `LinkMode` to allow setting the default value via `default_value = LinkMode::default().to_string()`. This way, we wouldn't need to change the type. Both approaches would set the value to the default if it's not provided.

> Might be easier to always show them both for the sake of snapshots — but hopefully it's only a small amount of snapshots that actually include the help menu and we can just use filters...

Or we could add it (and other options whose doc already shows more descriptive information about the default value) to a group like `doc-hide-default`, which indicates that we don't want this to be displayed in the cli reference and allows us to omit this default value during generation.

---

_@charliermarsh reviewed on 2024-08-28 01:38_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2004 on 2024-08-28 01:38_

I think we can't do this because we need to know if the value is provided on the CLI _at all_.

---

_@eth3lbert reviewed on 2024-08-29 08:25_

---

_Review comment by @eth3lbert on `crates/uv-cli/src/lib.rs`:2004 on 2024-08-29 08:25_

Ok, I think the best approach is to use the `value_source` to determine where the value is being set. Then, if the source is not from the command line, we can simply reset it to `None`.

---

_Review comment by @eth3lbert on `crates/uv-cli/src/lib.rs`:2004 on 2024-08-30 09:45_

Another simpler but trickier approach would be to leverage non-printable characters.

Consider the following:
```rust
#[derive(Clone, Debug)]
pub enum MaybeGiven<T> {
    Default(T),
    Given(T),
}

impl<T: Default> Default for MaybeGiven<T> {
    fn default() -> Self {
        Self::Default(T::default())
    }
}

impl<T> MaybeGiven<T> {
    pub fn into_given(self) -> Option<T> {
        match self {
            MaybeGiven::Default(_) => None,
            MaybeGiven::Given(x) => Some(x),
        }
    }

    pub fn into_inner(self) -> T {
        match self {
            MaybeGiven::Default(x) => x,
            MaybeGiven::Given(x) => x,
        }
    }
}

impl<T> AsRef<T> for MaybeGiven<T> {
    fn as_ref(&self) -> &T {
        match self {
            MaybeGiven::Default(x) => x,
            MaybeGiven::Given(x) => x,
        }
    }
}

impl<T: Display> Display for MaybeGiven<T> {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        if matches!(self, MaybeGiven::Default(_)) {
            f.write_str("\0")?;
            self.as_ref().fmt(f)?;
            f.write_str("\0")
        } else {
            self.as_ref().fmt(f)
        }
    }
}

impl<T: FromStr> FromStr for MaybeGiven<T> {
    type Err = T::Err;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        let trimmed = s.trim_matches('\0');
        if trimmed != s {
            return Ok(Self::Default(T::from_str(trimmed)?));
        }
        Ok(Self::Given(T::from_str(s)?))
    }
}
```

It checks if the string is surrounded by '\0' to determine whether it is from the default value or given by the user. This is a more hacky but simpler implementation without modifying the existing codebase too much, IMO.

If this is considered too hacky, I could implement it by checking the `value_source`.

---

_@eth3lbert reviewed on 2024-08-30 09:45_

---

_Comment by @zanieb on 2024-08-30 12:42_

As more reference, I had to undo the default value in https://github.com/astral-sh/uv/pull/6835

We don't have access to `ArgMatches`, though maybe we could refactor things so we do.

---

_Comment by @eth3lbert on 2024-08-30 17:10_

> As more reference, I had to undo the default value in #6835
> 
> We don't have access to `ArgMatches`, though maybe we could refactor things so we do.

I guess the following should work:

```diff
diff --git a/crates/uv/src/lib.rs b/crates/uv/src/lib.rs
index 080ed988..c5426578 100644
--- a/crates/uv/src/lib.rs
+++ b/crates/uv/src/lib.rs
@@ -1265,0 +1267,15 @@ async fn run_project(
+fn get_given_args(matches: &clap::ArgMatches) -> std::collections::HashSet<&clap::Id> {
+    let mut args = matches;
+    while let Some((_sub_cmd, sub_args)) = args.subcommand() {
+        args = sub_args;
+    }
+    args.ids()
+        .filter(|id| {
+            !matches!(
+                args.value_source(id.as_str()),
+                Some(clap::parser::ValueSource::DefaultValue)
+            )
+        })
+        .collect()
+}
+
@@ -1278,3 +1294,6 @@ where
-    // `std::env::args` is not `Send` so we parse before passing to our runtime
-    // https://github.com/rust-lang/rust/pull/48005
-    let cli = match Cli::try_parse_from(args) {
+    let cmd = Cli::command();
+    let matches = cmd.get_matches_from(args);
+    let cli = match clap::FromArgMatches::from_arg_matches(&matches) {
+        // `std::env::args` is not `Send` so we parse before passing to our runtime
+        // https://github.com/rust-lang/rust/pull/48005
+        // let cli = match Cli::try_parse_from(args) {
```

This should not alter the way uv currently parses arguments but provide us with the ability to retrieve given arguments from `ArgMatches`.

---

_Comment by @charliermarsh on 2024-10-09 22:35_

I think we may just need to keep this as documentation and not as a structured Clap default. As-is (in this PR) it's incorrect in the generated reference too, since it's dynamic.

---

_Closed by @charliermarsh on 2024-10-09 22:35_

---

_Comment by @charliermarsh on 2024-10-09 22:35_

I'm going to close for now but let me know if you want to follow-up in some way. Thank you for your work here.

---
