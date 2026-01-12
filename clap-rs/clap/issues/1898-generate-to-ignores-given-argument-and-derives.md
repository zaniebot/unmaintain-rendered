```yaml
number: 1898
title: generate_to ignores given argument and derives its own
type: issue
state: closed
author: alerque
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2020-05-02T22:34:32Z
updated_at: 2021-04-06T08:46:51Z
url: https://github.com/clap-rs/clap/issues/1898
synced_at: 2026-01-12T16:14:11Z
```

# generate_to ignores given argument and derives its own

---

_@alerque_

Trying to get shell completion generators working on my app (porting from StructOpt) I ran into trouble using the `generate_to()` function.

https://github.com/clap-rs/clap/blob/306dc830361438fbe140e55863b75924bff1b74c/clap_generate/src/lib.rs#L124-L139

Even with code pretty much copied out of the docs (marked as ignore):

```rust
let outdir = match env::var_os("OUT_DIR") {
    None => return,
    Some(outdir) => outdir,
};

let mut app = build_cli();
generate_to::<Bash, _, _>(
    &mut app,     // We need to specify what generator to use
    "myapp",      // We need to specify the bin name manually
    outdir,       // We need to specify where to write to
);
```

This code explicitly passes a value for `bin_name`, yet it is unused. L130 of the function above is fetching the bin name out of the app definition and using that to build a file path, ignoring the value we passed.

Furthermore if the app doesn't happen to have a bin_name set already, the code will panic!

---

_Added to milestone `3.0` by @pksunkara on 2020-05-03 07:01_

---

_Label `T: bug` added by @pksunkara on 2020-05-03 07:01_

---

_Comment by @pksunkara on 2020-05-03 07:02_

Thanks for reporting. This actually ties into https://github.com/clap-rs/clap/issues/1764. We might want just one file name but different bin names.

---

_Comment by @kbknapp on 2020-05-04 16:52_

I'm trying to remember exactly, but I *think* the reason I originally had the user pass in the binary name was because the `App`'s `bin_name` doesn't get determined until runtime, however many shell completion script generations happen in a `build.rs` at compile time. So we need a way to tell the generating code what the binary would be called.

Of course this can be fixed by making `App::bin_name` or `App::set_bin_name` mandatory (side note we should fix the naming of those methods so they're less confusing. I know they one is `&self` and returns `Sefl` while other is just `&mut self`...I think the Rust idiom here is to make the `&mut  self`'s method name end in `_mut`...but I digress). But that should be well documented, or this invariant should be checked inside `genereate_*` methods via `assert!`s.

---

_Label `C: generator` added by @CreepySkeleton on 2020-06-30 09:42_

---

_Comment by @saecki on 2021-02-15 15:47_

So just a follow up as I encountered this myself. How about using the passed bin_name as a file name if the app doesn't have one. Or providing a more helpful error message with `expect` instead of `unwrap` that says "App is missing a bin_name, consider adding one by using App::bin_name" or something similar.

---

_Closed by @pksunkara on 2021-04-06 08:46_

---
