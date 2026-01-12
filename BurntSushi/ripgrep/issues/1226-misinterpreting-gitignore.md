```yaml
number: 1226
title: "Misinterpreting '.gitignore'"
type: issue
state: closed
author: Alexander-Shukaev
labels:
  - question
assignees: []
created_at: 2019-03-25T22:43:33Z
updated_at: 2019-03-26T00:16:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1226
synced_at: 2026-01-12T16:13:23Z
```

# Misinterpreting '.gitignore'

---

_@Alexander-Shukaev_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

#### How did you install ripgrep?

Package manager.

#### What operating system are you using ripgrep on?

Arch Linux.

#### Describe your question, feature request, or bug.

I have `.git` repository at the root (`/`) of the OS installation and `.gitignore`.  When I run `rg -S --no-heading --line-number --color never --follow --hidden --smart-case gitignore .`, I see `rg` trying to match too many files which should have been excluded by `.gitignore`.

#### If this is a bug, what are the steps to reproduce the behavior?

```
/.gitignore
```
```
*

!*/

!/.gitignore
!/README.md
!*.pacnew
!*.pacsave
!*/tmp??????

/home/
/tmp/
/usr/share/vim/
```

```
cd / && rg -S --no-heading --line-number --color never --follow --hidden --smart-case gitignore .
```

One should only be able to search through the files which are unignored by exclamation `!`.

#### If this is a bug, what is the actual behavior?

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(XXX), Complete(xXX), Complete(XxX), Complete(xxX), Complete(XXx), Complete(xXx), Complete(Xxx), Complete(xxx)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "*/tmp??????", re: "(?-u)^[^/]*/tmp[^/][^/][^/][^/][^/][^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([ZeroOrMore, Literal('/'), Literal('t'), Literal('m'), Literal('p'), Any, Any, Any, Any, Any, Any]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 5 literals, 0 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./.gitignore: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!/.gitignore", actual: ".gitignore", is_whitelist: true, is_only_dir: false })))
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./lost+found: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./home: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/home/", actual: "home", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/media: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Music: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./TODO: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./proc/fb: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/udisks2: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./root/.cache/qt_compose_cache_little_endian_a3f81c1986b340b2a51b29c86e1d8456: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./root/.mysql_history: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/fs: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/media/aks: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./run/lightdm.pid: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/ccache: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/power: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.emacs.d: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./dev: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./run/udisks2/mounted-fs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/bus: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/notes: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/postgresql: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/dconf: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/class: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./root/.gitconfig: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./lib64: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt/smartgit: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./proc/dma: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/mm: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/NetworkManager: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/sessions: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/devices: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Pictures: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./etc: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt/exodus: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/irq: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/uevent_seqnum: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./run/atd.pid: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/fs/ext4: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/dev: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.ssh: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./.git: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt/dropbox: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/net: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/profiling: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/dbus: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/fs/jbd2: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/hypervisor: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Videos: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./boot: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt/tor-browser: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/sys: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/vmcoreinfo: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/cups: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/fs/nfsd: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/fs: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.local: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./opt/pgsql-9.6: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./bin: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/tty: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/kexec_crash_size: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/tracing: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/slab: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/ccache/7: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Public: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/wakeup_count: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/pm_test: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/resume: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/pm_print_times: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/wake_unlock: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/iommu_groups: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/ccache/4: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Desktop: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Templates: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./proc/acpi: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./run/utmp: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/bus: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/pm_wakeup_irq: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/security: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/ccache/1: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./lib: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.hplip: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./proc/keys: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/user: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/firmware: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/pm_trace_dev_match: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/kernel/irq: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./root/.cache/ccache/ccache.conf: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./tmp: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/tmp/", actual: "tmp", is_whitelist: false, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.config: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./proc/kmsg: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/svnserve: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/block: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/wake_lock: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/state: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/resume_offset: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/mem_sleep: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.android: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/Documents: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./run/sudo: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sys/module: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/kernel/fscaps: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./root/.cache/ccache/b: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1614: whitelisting ./sbin: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./sys/power/image_size: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./proc/misc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
...
```

**NOTE:** There is obviously a lot more output following.

#### If this is a bug, what is the expected behavior?

It should have only found matches in e.g. `/.gitignore` or any other unignored file which has `.gitignore` string but there should be either none or very little.

---

_Comment by @Alexander-Shukaev on 2019-03-25 22:45_

On a side note, in the similar case, `ag` (@ggreer) finds nothing (which is also wrong).

---

_Comment by @BurntSushi on 2019-03-25 22:53_

The debug logs are telling you what the problem is. `!*/` is whitelisting every `/foo` where `foo` is a directory. But those are directories. The debug logs are also telling you that many files are being ignored, because of your initial `*` rule, which seems correct.

The issue template asks for details about *behavior*. The debug logs are useful for explaining behavior, but they are not themselves a behavior.

Could you please provide a simple example that someone else can reproduce that indicates the bug? Include not only the debug output, but the actual output itself. Please do your best to minimize the problem to only a few small files.

---

_Comment by @Alexander-Shukaev on 2019-03-25 23:13_

I will try to create a small scenario.  Meanwhile, please have a look at this excerpt of non-debug output:

```
./.gitignore:15:!/.gitignore
./root/.opensipsctlrc: No such file or directory (os error 2)
File system loop found: ./proc/thread-self/cwd points to an ancestor .
File system loop found: ./proc/thread-self/root points to an ancestor .
...
File system loop found: ./proc/29798/task/29828/root points to an ancestor .
File system loop found: ./proc/29798/task/29829/root points to an ancestor .
./proc/31087/task/24861: No such file or directory (os error 2)
./root/.emacs.d/elpa/magit-20190305.1106/magit.el:579:    (require 'magit-gitignore)
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1131:;;;### (autoloads nil "magit-gitignore" "magit-gitignore.el" (0 0
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1133:;;; Generated autoloads from magit-gitignore.el
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1134: (autoload 'magit-gitignore "magit-gitignore" nil t)
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1136:(autoload 'magit-gitignore-in-topdir "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1137:Add the Git ignore RULE to the top-level \".gitignore\" file.
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1143:(autoload 'magit-gitignore-in-subdir "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1144:Add the Git ignore RULE to a \".gitignore\" file.
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1146:\".gitignore\" file in that directory.  Since such files are
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1152:(autoload 'magit-gitignore-in-gitdir "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1158:(autoload 'magit-gitignore-on-system "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1164:(autoload 'magit-skip-worktree "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1169:(autoload 'magit-no-skip-worktree "magit-gitignore" "\
./root/.emacs.d/elpa/magit-20190305.1106/magit-autoloads.el:1174:(if (fboundp 'register-definition-prefixes) (register-definition-prefixes "magit-gitignore" '("magit-")))
./root/.emacs.d/elpa/magit-20190305.1106/magit-mode.el:379:           (define-key map (kbd "C-c C-i") 'magit-gitignore))
./root/.emacs.d/elpa/magit-20190305.1106/magit-mode.el:397:           (define-key map (kbd   "i") 'magit-gitignore)
./root/.emacs.d/elpa/magit-20190305.1106/magit-mode.el:398:           (define-key map (kbd   "I") 'magit-gitignore)))
./root/.emacs.d/elpa/magit-20190305.1106/magit-mode.el:527:    ["Ignore globally" magit-gitignore-globally t]
./root/.emacs.d/elpa/magit-20190305.1106/magit-mode.el:528:    ["Ignore locally" magit-gitignore-locally t]
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:1:;;; magit-gitignore.el --- intentionally untracked files  -*- lexical-binding: t -*-
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:26:;; This library implements gitignore commands.
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:37:;;;###autoload (autoload 'magit-gitignore "magit-gitignore" nil t)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:38:(define-transient-command magit-gitignore ()
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:40:  :man-page "gitignore"
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:41:  ["Gitignore"
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:42:   ("t" "shared at toplevel (.gitignore)"
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:43:    magit-gitignore-in-topdir)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:44:   ("s" "shared in subdirectory (path/to/.gitignore)"
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:45:    magit-gitignore-in-subdir)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:47:    magit-gitignore-in-gitdir)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:48:   ("g" magit-gitignore-on-system
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:57:;;; Gitignore Commands
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:60:(defun magit-gitignore-in-topdir (rule)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:61:  "Add the Git ignore RULE to the top-level \".gitignore\" file.
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:64:  (interactive (list (magit-gitignore-read-pattern)))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:66:    (magit--gitignore rule ".gitignore")
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:67:    (magit-run-git "add" ".gitignore")))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:70:(defun magit-gitignore-in-subdir (rule directory)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:71:  "Add the Git ignore RULE to a \".gitignore\" file.
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:73:\".gitignore\" file in that directory.  Since such files are
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:76:  (interactive (list (magit-gitignore-read-pattern)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:79:    (let ((file (expand-file-name ".gitignore" directory)))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:80:      (magit--gitignore rule file)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:81:      (magit-run-git "add" ".gitignore"))))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:84:(defun magit-gitignore-in-gitdir (rule)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:87:  (interactive (list (magit-gitignore-read-pattern)))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:88:  (magit--gitignore rule (magit-git-dir "info/exclude"))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:92:(defun magit-gitignore-on-system (rule)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:95:  (interactive (list (magit-gitignore-read-pattern)))
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:96:  (magit--gitignore rule
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:101:(defun magit--gitignore (rule file)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:114:(defun magit-gitignore-read-pattern ()
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:154:(provide 'magit-gitignore)
./root/.emacs.d/elpa/magit-20190305.1106/magit-gitignore.el:155:;;; magit-gitignore.el ends here
./root/.emacs.d/elpa/gitattributes-mode-20180318.1956/gitattributes-mode.el:163:    ;; Pattern highlight similar to `gitignore-mode-font-lock-keywords'
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:1:;;; gitignore-mode-autoloads.el --- automatically extracted autoloads
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:9:;;;### (autoloads nil "gitignore-mode" "gitignore-mode.el" (0 0 0
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:11:;;; Generated autoloads from gitignore-mode.el
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:13:(autoload 'gitignore-mode "gitignore-mode" "\
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:14:A major mode for editing .gitignore files.
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:18:(dolist (pattern (list "/\\.gitignore\\'" "/info/exclude\\'" "/git/ignore\\'")) (add-to-list 'auto-mode-alist (cons pattern 'gitignore-mode)))
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:20:(if (fboundp 'register-definition-prefixes) (register-definition-prefixes "gitignore-mode" '("gitignore-mode-font-lock-keywords")))
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-autoloads.el:30:;;; gitignore-mode-autoloads.el ends here
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:1:;;; gitignore-mode.el --- Major mode for editing .gitignore files -*- lexical-binding: t; -*-
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:29:;; A major mode for editing .gitignore files.
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:35:(defvar gitignore-mode-font-lock-keywords
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:43:(define-derived-mode gitignore-mode conf-unix-mode "Gitignore"
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:44:  "A major mode for editing .gitignore files."
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:48:  (setq font-lock-defaults '(gitignore-mode-font-lock-keywords t t))
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:52:(dolist (pattern (list "/\\.gitignore\\'"
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:55:  (add-to-list 'auto-mode-alist (cons pattern 'gitignore-mode)))
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:58:(provide 'gitignore-mode)
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode.el:62:;;; gitignore-mode.el ends here
File system loop found: ./sys/class/thermal/cooling_device1/device/thermal_cooling points to an ancestor ./sys/class/thermal/cooling_device1
./root/.emacs.d/elpa/gitignore-mode-20180318.1956/gitignore-mode-pkg.el:2:(define-package "gitignore-mode" "20180318.1956" "Major mode for editing .gitignore files" 'nil :commit "55468314a5f6b77d2c96be62c7005ac94545e217" :keywords '("convenience" "vc" "git") :authors '(("Sebastian Wiesner" . "lunaryorn@gmail.com")) :maintainer '("Jonas Bernoulli" . "jonas@bernoul.li") :url "https://github.com/magit/git-modes")
File system loop found: ./sys/class/powercap/intel-rapl:0:0/device/subsystem points to an ancestor ./sys/class/powercap
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:1441:	* .gitignore: Ignore only preview.el in top directory.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:1632:	Remove also a gitignore
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:1634:	* Makefile.in (EXCLUDEDFILES): Add latex/.gitignore to excluded files.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:1649:	* .gitignore: Ignore some files automatically created during compilation.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:4141:	* .gitignore: Add ChangeLog and auto dirs in tests.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:4531:	* .gitignore: Do not ignore auto.el.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:5639:	* .gitignore: Merge from preview/.gitignore.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:5688:	* preview/.gitignore: Remove.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:6001:	* .gitignore: Ignore preview/preview.el.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:7646:	* doc/.gitignore: Rename from .cvsignore.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:7648:	* preview/.gitignore: Ditto.
./root/.emacs.d/elpa/auctex-12.1.2/ChangeLog.1:7650:	* preview/latex/.gitignore: Ditto.
File system loop found: ./sys/class/thermal/thermal_zone0/device/thermal_zone points to an ancestor ./sys/class/thermal/thermal_zone0
./root/.emacs.d/elpa/auctex-12.1.2/Makefile.in:63:EXCLUDEDFILES=autogen.sh .gitignore doc/.gitignore doc/tex-ref.log \
./root/.emacs.d/elpa/auctex-12.1.2/Makefile.in:64:	latex/.gitignore README.GIT tests build-aux
File system loop found: ./sys/class/thermal/thermal_zone0/cdev3/subsystem points to an ancestor ./sys/class/thermal
File system loop found: ./sys/class/thermal/thermal_zone0/cdev1/subsystem points to an ancestor ./sys/class/thermal
...
./proc/5004/cwd/.config/chromium/SingletonCookie: No such file or directory (os error 2)
./proc/6220/task/6220/cwd/hdcommon.py: Permission denied (os error 13)
./proc/6220/cwd/test/.gitignore:5:!/.gitignore
./proc/6259/task/6259/cwd/hdcommon.py: Permission denied (os error 13)
./proc/6259/cwd/test/.gitignore:5:!/.gitignore
./proc/10697/cwd/.thunderbird/1202x2s4.default/lock: No such file or directory (os error 2)
./proc/10697/cwd/.config/chromium-backup-crashrecovery-20190104_233215/SingletonSocket: No such file or directory (os error 2)
...
./proc/10697/cwd/.config/chromium-backup-crashrecovery-20190104_233215/SingletonCookie: No such file or directory (os error 2)
./proc/20596/task/20596/cwd/hdcommon.py: Permission denied (os error 13)
./proc/20597/task/20597/cwd/hdcommon.py./proc/20596/cwd/test/.gitignore:5:!/.gitignore
: Permission denied (os error 13)
./proc/20597/cwd/test/.gitignore:5:!/.gitignore
./proc/27309/task/27309/fd: No such file or directory (os error 2)
./proc/27309/task/27309/ns: No such file or directory (os error 2)
```

My observations:

1.  The first match is correct.
2.  All the errors are quite noisy, I understand that since directories were whitelisted it needs to recurse in each of them and check each filename against whitelisted patterns.  However, I'm not sure whether it should really attempt to access all these files if they don't match whitelisted patterns but I might be wrong here.  Anyway, is there a way to reduce that noise?
3.  There are more matches with `gitignore`, which should have definitely been ignored because the files they were found in do not match whitelisted patterns.

---

_Comment by @BurntSushi on 2019-03-25 23:25_

> All the errors are quite noisy, I understand that since directories were whitelisted it needs to recurse in each of them and check each filename against whitelisted patterns. However, I'm not sure whether it should really attempt to access all these files if they don't match whitelisted patterns but I might be wrong here. Anyway, is there a way to reduce that noise?

It definitely has to access all of the files, not just for searching, but just for walking the directory hierarchy, since you've whitelisted all directories. The "detected a loop" messages are because you asked ripgrep to follow symbolic links.

Use `--no-messages` or redirect stderr to squash error messages. My guess is that a similar invocation of `find` (with `-L`) will display similar error messages.

> There are more matches with gitignore, which should have definitely been ignored because the files they were found in do not match whitelisted patterns.

Based on the limited data you've given me, I agree. But I need to see the debug logs for _those_ paths. But trying to reproduce this in a more limited scenario doesn't seem to work:

```
$ tree
.
└── root

1 directory, 0 files

$ cat .ignore
*

!*/

!/.gitignore
!/README.md
!*.pacnew
!*.pacsave
!*/tmp??????

/home/
/tmp/
/usr/share/vim/

$ cat root/.emacs.d/elpa/magit-20190305.1106/magit.el
test

$ rg --follow --hidden --files .

$ rg --follow --hidden --files . --debug
DEBUG|rg::config|src/config.rs:39: /home/andrew/.ripgreprc: arguments loaded from config file: ["--max-columns=500", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go"]
DEBUG|rg::args|src/args.rs:518: final argv: ["rg", "--max-columns=500", "--colors=match:bg:0xff,0x7f,0x00", "--colors=match:fg:white", "--colors=line:none", "--colors=line:fg:magenta", "--colors=path:fg:green", "--type-add=got:*_test.go", "--follow", "--hidden", "--files", ".", "--debug"]
DEBUG|globset|globset/src/lib.rs:429: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: glob converted to regex: Glob { glob: "*/tmp??????", re: "(?-u)^[^/]*/tmp[^/][^/][^/][^/][^/][^/]$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([ZeroOrMore, Literal('/'), Literal('t'), Literal('m'), Literal('p'), Any, Any, Any, Any, Any, Any]) }
DEBUG|globset|globset/src/lib.rs:434: built glob set; 5 literals, 0 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./.ignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|ignore/src/walk.rs:1620: whitelisting ./root: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1620: whitelisting ./root/.emacs.d: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1620: whitelisting ./root/.emacs.d/elpa: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1620: whitelisting ./root/.emacs.d/elpa/magit-20190305.1106: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!*/", actual: "**/*", is_whitelist: true, is_only_dir: true })))
DEBUG|ignore::walk|ignore/src/walk.rs:1617: ignoring ./root/.emacs.d/elpa/magit-20190305.1106/magit.el: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
```

---

_Comment by @BurntSushi on 2019-03-25 23:28_

Also, when showing output of a command, please include the full command used to generate that output, just like I did in my previous comment.

Thank you for looking for a smaller example.

---

_Comment by @Alexander-Shukaev on 2019-03-25 23:53_

Right,

```
.
├── .git/
├── .gitignore
├── test/
│   ├── elpa/
│   │   ├── .git/
│   │   ├── .gitignore
│   │   └── zzz
│   └── yyy
└── xxx

19 directories, 28 files
```

```
./.gitignore
```

```
*

!*/

!/.gitignore
```

Every other file (`xxx`, `yyy`, `zzz`) contains `gitignore` pattern.

```
./test/elpa/.gitignore
```

is empty:

```
```

Now, if you simply either `mkdir ./test/elpa/.git` or `touch ./test/elpa/.git`, then

```
rg -S --no-heading --line-number --color never --follow --hidden --smart-case gitignore .
./.gitignore:5:!/.gitignore
./test/elpa/zzz:1:gitignore
```

but if you otherwise `rm -f -r ./test/elpa/.git`, then

```
rg -S --no-heading --line-number --color never --follow --hidden --smart-case gitignore .
./.gitignore:5:!/.gitignore
```

It means that those other weird matches were due to presence of more nested `.gitignore` files which were also accompanied by either `.git` files or directories (in the same directories) as it most probably nullifies the parent `.gitignore`.  Is this mimicking the real Git behavior?

---

_Comment by @BurntSushi on 2019-03-26 00:10_

>  Is this mimicking the real Git behavior?

Yes. From what I can tell, what you're seeing is expected behavior. `.gitignore` files from parent git repos don't impact sub-directories which contain git repositories. This would be disastrous if it did. Many folks, including myself, for example, have their home directory in a git repository with typically odd `.gitignore` files for dealing with junk in `$HOME` that you don't want tracked.

Consider using a `.ignore` file if you want ignore rules to descend arbitrarily into directories without regard to whether a git repository exists or not.

---

_Closed by @BurntSushi on 2019-03-26 00:10_

---

_Label `question` added by @BurntSushi on 2019-03-26 00:10_

---

_Comment by @Alexander-Shukaev on 2019-03-26 00:16_

Thank you for the explanation.

---
