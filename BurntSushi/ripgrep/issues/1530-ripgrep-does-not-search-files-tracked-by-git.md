```yaml
number: 1530
title: ripgrep does not search files tracked by git because they are ignored by gitignore
type: issue
state: closed
author: Dreomite
labels:
  - wontfix
assignees: []
created_at: 2020-03-26T11:02:23Z
updated_at: 2020-03-26T14:22:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1530
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep does not search files tracked by git because they are ignored by gitignore

---

_@Dreomite_

Ver: `12.0.0`
OS: `Arch Linux (x86_64, linux 5.5.9.arch1-2)`
Installed via: `pacman`

#### Description

If `!/some_folder` is used alongside `/**`, the folder itself will be visible for ripgrep, but its contents will be not.

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./some_folder: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/some_folder", actual: "some_folder", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./some_folder/some_other_folder_or_file: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
```


`git` parses everything correctly.


#### Steps to reproduce

1. `git clone https://github.com/Vermintide-Mod-Framework/Vermintide-Mod-Builder.git .`
2. `git checkout 87638bf` (Current HEAD commit, just in case something will be changed in the future)
3. `rg rmn`

**Expected:**

- ripgrep should find a string inside `./scripts/lib/str.js`

**Actual:**

```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(rmn)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**", re: "(?-u)^.*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "mods/*", re: "(?-u)^mods/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([Literal('m'), Literal('o'), Literal('d'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 7 literals, 9 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.git: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./.eslintrc.json: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!.eslintrc.json", actual: "**/.eslintrc.json", is_whitelist: true, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./.template-vmf: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/.template-vmf", actual: ".template-vmf", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./.template: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/.template", actual: ".template", is_whitelist: true, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template-vmf/%%name.mod: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template-vmf/core: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template-vmf/item_preview.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template-vmf/resource_packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template-vmf/scripts: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template/%%name.mod: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./LICENSE: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!LICENSE", actual: "**/LICENSE", is_whitelist: true, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template/core: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template/item_preview.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template/resource_packages: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./.template/scripts: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./README.md: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!README.md", actual: "**/README.md", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./embedded: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/embedded", actual: "embedded", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./gulpfile.js: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!gulpfile.js", actual: "**/gulpfile.js", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./logo: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/logo", actual: "logo", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./mods: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/mods", actual: "mods", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./package-lock.json: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!package-lock.json", actual: "**/package-lock.json", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./embedded/placeholder.mod: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/128.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/16.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./mods/.gitkeep: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/mods/.gitkeep", actual: "mods/.gitkeep", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./package.json: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!package.json", actual: "**/package.json", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./embedded/placeholderV1: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./embedded/placeholderV2: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/256.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/32.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/48.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/64.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/96.png: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/src.pdn: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./logo/vmb.ico: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./mods/what: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/mods/*", actual: "mods/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./scripts: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/scripts", actual: "scripts", is_whitelist: true, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./vmb-test.js: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!vmb-test.js", actual: "**/vmb-test.js", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/lib: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./vmb.js: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!vmb.js", actual: "**/vmb.js", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./.gitignore: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!.gitignore", actual: "**/.gitignore", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/print.js: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/tasks.js: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/tasks: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/tools: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/version.js: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1679: whitelisting ./scripts/vmb.js: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!vmb.js", actual: "**/vmb.js", is_whitelist: true, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1676: ignoring ./scripts/yay.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/**", actual: "**", is_whitelist: false, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```


---

_Renamed from "Incorect .gitignore parsing" to "Incorrect .gitignore parsing" by @Dreomite on 2020-03-26 11:04_

---

_Comment by @BurntSushi on 2020-03-26 13:03_

This is expected but inconsistent behavior with `git`. `git` is not "parsing" your gitignore correctly here. The issue is that `git` knows which files are being tracked in your index. But according to gitignore, those files would otherwise be ignored. It is very easy to confirm this:

```
$ touch scripts/lib/wat
$ git status
HEAD detached at 87638bf
nothing to commit, working tree clean
```

If your `.gitignore` file worked how you claimed it should, then `git` should report `wat` as an untracked file. But it doesn't, because it's ignored. So ripgrep's behavior here is correct, strictly speaking, from the perspective of your `.gitignore` rules. The inconsistency arises from the fact that ripgrep does not search files based on what is actually tracked in your git repository. The only way to fix this is to make your `.gitignore` file match the actual files being tracked or otherwise use a `.ignore` file somehow to override your `.gitignore`.

There are several other tickets of this nature.

---

_Closed by @BurntSushi on 2020-03-26 13:03_

---

_Label `wontfix` added by @BurntSushi on 2020-03-26 13:03_

---

_Renamed from "Incorrect .gitignore parsing" to "ripgrep does not search files tracked by git because they are ignored by gitignore" by @BurntSushi on 2020-03-26 13:03_

---

_Comment by @Dreomite on 2020-03-26 14:22_

Wow, it's so embarrassing I haven't recognized git file-tracking behavior, even though I'm well-aware of it. Sorry for making you spend your time on this pointless issue and thank you very much for thoroughly explaining everything.

---
