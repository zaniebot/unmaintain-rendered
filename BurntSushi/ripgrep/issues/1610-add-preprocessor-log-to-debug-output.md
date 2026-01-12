```yaml
number: 1610
title: Add preprocessor log to debug output
type: issue
state: open
author: phiresky
labels:
  - enhancement
assignees: []
created_at: 2020-06-09T09:43:52Z
updated_at: 2020-06-09T12:03:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1610
synced_at: 2026-01-12T16:13:23Z
```

# Add preprocessor log to debug output

---

_@phiresky_

When `rg --debug --pre ...` is used, it would be useful if the following information would be output:

1. Which preprocessor command is run
2. What the preprocessor command stderr output was (not just if it fails).

These would be very helpful to debug preprocessor issues, for example the a preprocessor that chooses between different other preprocessors can output which one it chose to stderr.

Here is a patch that implements this:

```diff
diff --git a/crates/cli/src/process.rs b/crates/cli/src/process.rs
index 4ec5af7..3011abe 100644
--- a/crates/cli/src/process.rs
+++ b/crates/cli/src/process.rs
@@ -216,6 +216,7 @@ impl io::Read for CommandReader {
             if !self.child.wait()?.success() {
                 return Err(io::Error::from(self.stderr.read_to_end()));
             }
+            log::debug!("preprocesser command stderr: {}", self.stderr.read_to_end());
         }
         Ok(nread)
     }
diff --git a/crates/core/search.rs b/crates/core/search.rs
index 4da4057..8b1db00 100644
--- a/crates/core/search.rs
+++ b/crates/core/search.rs
@@ -392,6 +392,8 @@ impl<W: WriteColor> SearchWorker<W> {
         let bin = self.config.preprocessor.as_ref().unwrap();
         let mut cmd = Command::new(bin);
         cmd.arg(path).stdin(Stdio::from(File::open(path)?));
+        // log run command including file name
+        log::debug!("running preprocessor: {:?}", cmd);
 
         let rdr = self.command_builder.build(&mut cmd).map_err(|err| {
             io::Error::new(

```

I can send a PR if needed, I'm not sure if this should be done in a different way.


---

_Comment by @BurntSushi on 2020-06-09 11:42_

This seems OK to me, and a PR would be good, but I think this probably falls under a trace log level (enabled via `--trace`) rather than `--debug`. `--debug` mostly tries to avoid output for each individual file (other than making noise about which files are being ignored), where as `--trace` can be a bit more noisy on a per-file basis.

---

_Label `enhancement` added by @BurntSushi on 2020-06-09 11:42_

---

_Comment by @phiresky on 2020-06-09 12:03_

> think this probably falls under a trace log level

Hm.. for me, it's actually the other way around: the preprocessor debug information is more relevant to me than most of the other debug info, and not being able to see it's output without failing it completely makes debugging pretty hard. I think this is kinda true in general: If you use the `--pre` argument, then your preprocessor is probably the most fragile part of your pipeline, and thus a main reason to enable debug logging. Since preprocessors are also more expensive and probably slower than all else of ripgrep, they probably won't be run for tons of files so they should litter the logs less than other logs that happen per file - and if the preprocessor doesn't output anything to stderr, it will only add a single line per file.

So in general, it would be good if it would be possible to expose an interface similar to [env_logger](https://docs.rs/env_logger/0.7.1/env_logger/#enabling-logging) to enable / disable logging for specific crate prefixes. For example `--log-config=trace,grep_searcher=debug,globset=info`. Let me know if I should open a different issue for that.

Testing it, it seems like --trace isn't used *that* much in rg as opposed to other programs though, so I guess it would still be like 4/5 of the convenience to add this there instead of --debug as opposed to not having it at all.

---
