```yaml
number: 836
title: "Proof of concept: skip unused Gitignore matchers"
type: pull_request
state: closed
author: sharkdp
labels: []
assignees: []
base: master
head: skip-unused-ignore-matchers
created_at: 2018-02-25T16:42:35Z
updated_at: 2018-04-24T17:52:53Z
url: https://github.com/BurntSushi/ripgrep/pull/836
synced_at: 2026-01-12T18:23:13Z
```

# Proof of concept: skip unused Gitignore matchers

---

_@sharkdp_

This is just a proof of concept. I'm not sure if this is the best approach. Even if it is, the code should probably be clean up.

This PR addresses the findings in #835 and disables unneeded `Gitignore` matchers completely. This leads to a performance improvement for use cases where one or more of the matchers (custom ignore files, `.ignore` files, `.gitignore` files, `.git/info/exclude`) are not needed.



### Benchmark 1: `rg -uu --files`

master
```
▶ hyperfine --warmup 10 'rg -uu --files'
Benchmark #1: rg -uu --files

  Time (mean ± σ):      1.298 s ±  0.067 s    [User: 4.348 s, System: 2.667 s]
 
  Range (min … max):    1.203 s …  1.398 s
```

this branch (~ 8% faster):
```
▶ hyperfine --warmup 10 'rg -uu --files'
Benchmark #1: rg -uu --files

  Time (mean ± σ):      1.201 s ±  0.032 s    [User: 3.732 s, System: 2.532 s]
 
  Range (min … max):    1.165 s …  1.264 s
```

### Benchmark 2: `rg -uu --files --glob "Cargo.toml"`

master:
```
▶ hyperfine --warmup 10 'rg -uu --files --glob "Cargo.toml"'
Benchmark #1: rg -uu --files --glob "Cargo.toml"

  Time (mean ± σ):     662.5 ms ±   2.2 ms    [User: 2.991 s, System: 2.198 s]
 
  Range (min … max):   659.3 ms … 665.7 ms
```

this branch (~ 10% faster):
```
▶ hyperfine --warmup 10 'rg -uu --files --glob "Cargo.toml"' 
Benchmark #1: rg -uu --files --glob "Cargo.toml"

  Time (mean ± σ):     602.6 ms ±   1.7 ms    [User: 2.527 s, System: 2.183 s]
 
  Range (min … max):   599.1 ms … 604.8 ms
```

### Benchmark 3: `rg --files --glob "Cargo.toml"`

As expected, there is no significant change if `-uu` is not used:

master:
```
▶ hyperfine --warmup 10 'rg --files --glob "Cargo.toml"'    
Benchmark #1: rg --files --glob "Cargo.toml"

  Time (mean ± σ):     101.1 ms ±   1.6 ms    [User: 420.4 ms, System: 327.0 ms]
 
  Range (min … max):    99.5 ms … 107.1 ms
```

this branch:
```
▶ hyperfine --warmup 10 'rg --files --glob "Cargo.toml"'
Benchmark #1: rg --files --glob "Cargo.toml"

  Time (mean ± σ):     101.6 ms ±   0.6 ms    [User: 419.2 ms, System: 331.5 ms]
 
  Range (min … max):   100.3 ms … 103.0 ms
```

---

_Comment by @BurntSushi on 2018-03-10 13:32_

@sharkdp Thanks for this! I think I'd be OK with this PR, but just to cover our bases, did you explore whether this improvement was possible by just using `Gitignore::empty()`? That was basically my intent, where if a matcher wasn't needed, then we could create an empty one and the empty cases would just get folded into the matcher instead of needing to add a bunch of explicit case analysis everywhere.

Let's work through this. Here's the definition of `Gitignore::empty()`:

```rust
    pub fn empty() -> Gitignore {
        GitignoreBuilder::new("").build().unwrap()
    }
```

OK, so this is creating a new builder with an empty root, not adding any globs and then building it. That means running this code:

```rust
    pub fn new<P: AsRef<Path>>(root: P) -> GitignoreBuilder {
        let root = root.as_ref();
        GitignoreBuilder {
            builder: GlobSetBuilder::new(),
            root: strip_prefix("./", root).unwrap_or(root).to_path_buf(),
            globs: vec![],
            case_insensitive: false,
        }
    }
```

and this code

```rust
    pub fn build(&self) -> Result<Gitignore, Error> {
        let nignore = self.globs.iter().filter(|g| !g.is_whitelist()).count();
        let nwhite = self.globs.iter().filter(|g| g.is_whitelist()).count();
        let set =
            self.builder.build().map_err(|err| {
                Error::Glob {
                    glob: None,
                    err: err.to_string(),
                }
            })?;
        Ok(Gitignore {
            set: set,
            root: self.root.clone(),
            globs: self.globs.clone(),
            num_ignores: nignore as u64,
            num_whitelists: nwhite as u64,
            matches: Arc::new(ThreadLocal::default()),
        })
    }
```

The constructor seems pretty cheap to me. There's a call to `to_path_buf` which might invoke an allocation, although given it's an empty string, that probably gets  optimized out. (IIRC, `vec![]` doesn't actually allocate, for example, and `PathBuf` is certainly built on `Vec`.)

The `build` method is a little more interesting. The first couple lines filter over the globs, but we have none, so that should be negligible. Then we build the `GlobSet` itself. This is actually cheap since `GlobSet::new(&[])` is special cases to do no work and just return a set that never matches.

The builder otherwise does some cloning (but again, those should all be empty), and sets up a scratch buffer (which also defaults to empty).

So my question is: if we changed this

```rust
        let custom_ig_matcher =
            {
                let (m, err) =
                    create_gitignore(&dir, &self.0.custom_ignore_filenames);
                errs.maybe_push(err);
                m
            };
```

to be more in line with how the other matchers handle the empty case on master

```rust
        let custom_ig_matcher =
            if self.custom_ignore_filenames.is_empty() {
                Gitignore::empty()
            } else {
                let (m, err) =
                    create_gitignore(&dir, &self.0.custom_ignore_filenames);
                errs.maybe_push(err);
                m
            };
```

then does that solve the performance problem? Or are there costs in `Gitignore::empty()` that I am not accounting for?

The reason why I think the above might work is because the `create_gitignore` function definitely does have overhead associated with it, and always does at least a syscall to open a file.

---

_Comment by @sharkdp on 2018-03-12 22:52_

@BurntSushi Thank you for the detailed answer.

I should have explained this better:

1. I'm pretty sure that the problem is *not* with the additional `Gitignore::empty` invocation. The expensive operation is the `matched(&path, is_dir)` call on the four `Gitignore` instances in (every loop iteration inside) `Ignore::matched_ignore`.

2. This PR does not only resolve the performance regression in #835 but improves the speed beyond that (if more than one matcher is disabled).

---

_Closed by @BurntSushi on 2018-04-24 15:19_

---

_Branch deleted on 2018-04-24 17:52_

---
