```yaml
number: 2443
title: search strings are being missed / overlooked
type: issue
state: closed
author: tjex
labels:
  - invalid
assignees: []
created_at: 2023-03-04T12:40:53Z
updated_at: 2023-03-04T20:43:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2443
synced_at: 2026-01-12T16:13:24Z
```

# search strings are being missed / overlooked

---

_@tjex_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS 12.6.1

#### Describe your bug.

running `rg <search-term>` (with or without the `-i` flag) finds some occurrences of the string within some sub-directories, but not all. 

#### What are the steps to reproduce the behavior?

The corpus is an open source docs repo [actualbudget/docs](https://github.com/actualbudget/docs). 

1. clone the repo above
2. `cd docs`
3. `rg -i actual-config` (this finds occurrences in two files. see screenshot)
4. `cd ./docs` (ie, the 'docs' folder at the repo root)
6. `rg -i actual-config` (same result as in step 3)

command line commands
```diff
- note: I've renamed my repo root folder to `abudget-docs`
```
<img width="440" alt="Screen Shot 2023-03-04 at 13 21 32" src="https://user-images.githubusercontent.com/83029642/222900833-67493d63-9ca5-4931-8b0a-637392e8e5d9.png">

neovim showing file path and instances of string that are not found in above results.
The below image shows the `Restore.md` file at `<repo-root>/docs/Backup-Restore/`.

<img width="853" alt="Screen Shot 2023-03-04 at 13 24 43" src="https://user-images.githubusercontent.com/83029642/222900938-c330c2e7-3237-4955-98f1-4e1f1f130554.png">


#### What is the actual behavior?

ripgrep returns search results from files in other sub-folders, but not from all - as observed manually in neovim (above).
Search results are found in: `docs/Installing/fly/..`, `docs/Getting-Started/migration/...`, but there are instances of the search term in `docs/Backup-Restore/Backups.md` that are not found.

Not huge, but still: [gist](https://gist.github.com/tjex/df59cf31d5afde4a9e2b465b3472260b)

#### What is the expected behavior?

The instances of the string `actual-config` should have also been returned from `docs/Backup-Restore/Backups.md` (as well as any other files in the corpus that I don't know about)


---

_Comment by @BurntSushi on 2023-03-04 13:03_

The answer is in the `--debug` output. Did you look at it? Note this line:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./docs/Backup-Restore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "Backup*/", actual: "**/Backup*", is_whitelist: false, is_only_dir: true })))
```

Your `./docs/Backup-Restore` directory is matched by a rule in your `.gitignore`. So ripgrep skips that directory.

---

_Comment by @tjex on 2023-03-04 20:33_

oh pwoh. I did, but overlooked this!
thanks for taking the time to point it out 🙏

---

_Closed by @tjex on 2023-03-04 20:33_

---

_Label `invalid` added by @BurntSushi on 2023-03-04 20:43_

---
