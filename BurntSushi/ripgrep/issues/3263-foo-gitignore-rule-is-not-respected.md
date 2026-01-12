```yaml
number: 3263
title: "`foo/*/*` gitignore rule is not respected"
type: issue
state: closed
author: injust
labels:
  - wontfix
assignees: []
created_at: 2026-01-09T19:12:35Z
updated_at: 2026-01-09T19:46:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3263
synced_at: 2026-01-12T16:13:25Z
```

# `foo/*/*` gitignore rule is not respected

---

_@injust_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.1.0

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 15.7.3

### Describe your bug.

In my dotfiles repo, I have several submodules in `catppuccin/`, e.g. `catpuccin/bat`. I gitignore `catppuccin/*/*` to ignore the _contents_ of the submodules, but not the submodules themselves.

However, ripgrep doesn't respect this gitignore rule.

### What are the steps to reproduce the behavior?

1. Check out my [dotfiles repo](https://github.com/injust/dotfiles), including submodules
2. `rg DOCTYPE`

### What is the actual behavior?

This line seems interesting:

```
rg: DEBUG|globset|crates/globset/src/lib.rs:506: glob `Glob("catppuccin/*/*")` converted to regex: `"(?-u)^catppuccin/[^/]*/[^/]*$"`
```

Full logs:

```
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /Users/jsu/.config/ripgrep/config: arguments loaded from config file: []
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:954: read CWD from environment: /Users/jsu/code/dotfiles
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1092: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1117: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1127: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1278: found hostname for hyperlink configuration: MBP-A1989
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1288: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:175: using 8 thread(s)
rg: DEBUG|grep_regex::config|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:398: opened gitignore file: /Users/jsu/.config/git/ignore
rg: DEBUG|globset|crates/globset/src/lib.rs:506: glob `Glob("**/Icon[\r]")` converted to regex: `"(?-u)^(?:/?|.*/)Icon[\r]$"`
rg: DEBUG|globset|crates/globset/src/lib.rs:506: glob `Glob("**/._*")` converted to regex: `"(?-u)^(?:/?|.*/)\\._[^/]*$"`
rg: DEBUG|globset|crates/globset/src/lib.rs:515: built glob set; 0 literals, 16 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:398: opened gitignore file: ./.gitignore
rg: DEBUG|globset|crates/globset/src/lib.rs:506: glob `Glob("catppuccin/*/*")` converted to regex: `"(?-u)^catppuccin/[^/]*/[^/]*$"`
rg: DEBUG|globset|crates/globset/src/lib.rs:515: built glob set; 7 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:398: opened gitignore file: ./.git/info/exclude
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.ruff_cache: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.gitmodules: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./raycast: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "raycast/", actual: "**/raycast", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.ssh: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.prettierrc.yaml: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.screenrc: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.hushlogin: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./git/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|rg::haystack|crates/core/haystack.rs:70: ignoring ./git/catppuccin.gitconfig: failed to pass haystack filter: file type: Some(FileType { is_file: false, is_dir: false, is_symlink: true, .. }), metadata: Ok(Metadata { file_type: FileType { is_file: false, is_dir: false, is_symlink: true, .. }, permissions: Permissions(FilePermissions { mode: 0o120755 (lrwxr-xr-x) }), len: 40, modified: SystemTime { tv_sec: 1767712730, tv_nsec: 666419329 }, accessed: SystemTime { tv_sec: 1767712730, tv_nsec: 666419329 }, created: SystemTime { tv_sec: 1767712730, tv_nsec: 666419329 }, .. })
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./micro/buffers: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "micro/buffers/", actual: "micro/buffers", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./fish/fisher: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "fish/fisher/", actual: "fish/fisher", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./micro/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./zed/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
iStat Menus Settings.ismp7
2:<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./fish/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/bat/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./micro/plug: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "micro/plug/", actual: "micro/plug", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/micro/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/delta/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./zed/prompts: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "zed/prompts/", actual: "zed/prompts", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/bat/.editorconfig: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/btop/.DS_Store: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/jsu/.config/git/ignore"), original: ".DS_Store", actual: "**/.DS_Store", is_whitelist: false, is_only_dir: false })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./micro/backups: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "micro/backups/", actual: "micro/backups", is_whitelist: false, is_only_dir: true })))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/micro/.editorconfig: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/delta/.editorconfig: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/bat/.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/btop/.editorconfig: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/micro/.github: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/delta/.github: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/delta/.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/btop/.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/micro/.git: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/bat/.vscode: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1942: ignoring ./catppuccin/delta/assets/.gitkeep: Ignore(IgnoreMatch(Hidden))
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/delta/assets/macchiato.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/btop/assets/macchiato.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/delta/assets/mocha.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/btop/assets/mocha.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/btop/assets/frappe.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/delta/assets/frappe.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/btop/assets/latte.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/delta/assets/latte.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/btop/assets/screenshot.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/micro/assets/macchiato.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/delta/assets/preview.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/micro/assets/mocha.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/bat/assets/macchiato.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/micro/assets/latte.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/micro/assets/frappe.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/bat/assets/mocha.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/micro/assets/preview.webp: found binary data at offset 6

catppuccin/bat/themes/Catppuccin Macchiato.tmTheme
2:<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
765:        <string>HTML/XML DOCTYPE as keyword</string>
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/bat/assets/latte.webp: found binary data at offset 6
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/bat/assets/preview.webp: found binary data at offset 7
rg: DEBUG|grep_printer::standard|/private/tmp/ripgrep-20251023-8578-1g1ja4/ripgrep-15.1.0/crates/printer/src/standard.rs:830: ignoring catppuccin/bat/assets/frappe.webp: found binary data at offset 6

catppuccin/bat/themes/Catppuccin Mocha.tmTheme
2:<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
765:        <string>HTML/XML DOCTYPE as keyword</string>

catppuccin/bat/themes/Catppuccin Frappe.tmTheme
2:<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
765:        <string>HTML/XML DOCTYPE as keyword</string>

catppuccin/bat/themes/Catppuccin Latte.tmTheme
2:<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
765:        <string>HTML/XML DOCTYPE as keyword</string>
```

### What is the expected behavior?

Results in `catppuccin/bat/themes/` should be ignored.

---

_Renamed from "`foo/*/*` gitignore is ignored" to "`foo/*/*` gitignore rule is not respected" by @injust on 2026-01-09 19:12_

---

_Comment by @BurntSushi on 2026-01-09 19:20_

This is expected behavior. `.gitignore` files don't apply across git repository boundaries. Since `catppuccin/bat` is actually a separate git repository, the `.gitignore` in the root of the parent repository is ignored.

You can disable this checking with `--no-require-git`. With that flag, ripgrep will disregard the existence of `.git` and always respect `.gitignore`.

---

_Closed by @BurntSushi on 2026-01-09 19:20_

---

_Label `wontfix` added by @BurntSushi on 2026-01-09 19:21_

---

_Comment by @injust on 2026-01-09 19:37_

Thanks for the explanation, `--no-require-git` solves my problem.

> `.gitignore` files don't apply across git repository boundaries

Just to confirm, this is a ripgrep implementation detail? Since Git respects the gitignore rule in the way I expected.

---
