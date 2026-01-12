```yaml
number: 3199
title: riggrep only prints output at close
type: issue
state: closed
author: shadowpsi
labels: []
assignees: []
created_at: 2025-10-22T02:04:41Z
updated_at: 2025-10-22T12:16:07Z
url: https://github.com/BurntSushi/ripgrep/issues/3199
synced_at: 2026-01-12T16:13:25Z
```

# riggrep only prints output at close

---

_@shadowpsi_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.0.0

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)

### How did you install ripgrep?

nixpkg

### What operating system are you using ripgrep on?

NixOS unstable

### Describe your bug.

rg version 15 only appears to output once the pipe closes. Note that 1 and 2 are printed after done rather than before.  I initially noticed this when trying to filter an applications stdout. "Exiting" is printed to stderr when the application closes.

```
❯ Composer | rg Style ; echo versus ; Composer | grep Style
Exiting:  0
14:59:42.776	DEBUG	[Style.qml L: 48]	"Font details Noto Sans 11 14"
14:59:42.776	DEBUG	[Style.qml L: 49]	"Screen density: 106.19dpi"
versus
14:59:46.925	DEBUG	[Style.qml L: 48]	"Font details Noto Sans 11 14"
14:59:46.925	DEBUG	[Style.qml L: 49]	"Screen density: 106.19dpi"
Exiting:  0
```

For reference version 14 matches what I would expect using grep.
```
❯ bash -c '( echo start >&2 ; echo 1; sleep 2 ; echo 2 ; sleep 2 ; echo done >&2 ) | grep \'[0-9]\''
start
1
2
done
```

### What are the steps to reproduce the behavior?

```
❯ rg --version ; bash -c '( echo start >&2 ; echo 1; sleep 2 ; echo 2 ; sleep 2 ; echo done >&2 ) | rg \'[0-9]\''
ripgrep 15.0.0

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
start
done
1
2
```

versus 14:
```
❯ rg --version ; bash -c '( echo start >&2 ; echo 1; sleep 2 ; echo 2 ; sleep 2 ; echo done >&2 ) | rg \'[0-9]\''
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
start
1
2
done
```

### What is the actual behavior?

```
❯ rg --version ; bash -c '( echo start >&2 ; echo 1; sleep 2 ; echo 2 ; sleep 2 ; echo done >&2 ) | rg --debug \'[0-9]\''
ripgrep 15.0.0

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
start
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:954: read CWD from environment: /tmp
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1092: number of paths given to search: 0
rg: DEBUG|grep_cli|crates/cli/src/lib.rs:209: for heuristic stdin detection on Unix, found that is_file=false, is_fifo=true and is_socket=false, and thus concluded that is_stdin_readable=true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1117: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1130: heuristic chose to search stdin
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1278: found hostname for hyperlink configuration: nyx
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1288: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:175: using 1 thread(s)
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:398: opened gitignore file: *******
rg: DEBUG|globset|crates/globset/src/lib.rs:511: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 2 suffixes, 0 required extensions, 0 regexes
done
1
2
```

### What is the expected behavior?

ripgrep should output as matches are found. Tried with --line-buffered and that made no difference

---

_Comment by @BurntSushi on 2025-10-22 12:16_

This is a subtle duplicate of #3194. I'll get a release out soon.

---

_Closed by @BurntSushi on 2025-10-22 12:16_

---
