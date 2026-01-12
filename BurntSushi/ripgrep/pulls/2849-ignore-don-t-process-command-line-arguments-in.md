```yaml
number: 2849
title: "ignore: don't process command line arguments in reverse order"
type: pull_request
state: closed
author: th1000s
labels:
  - rollup
assignees: []
base: master
head: revrev
created_at: 2024-07-03T20:30:08Z
updated_at: 2025-09-20T01:08:27Z
url: https://github.com/BurntSushi/ripgrep/pull/2849
synced_at: 2026-01-12T18:23:14Z
```

# ignore: don't process command line arguments in reverse order

---

_@th1000s_

```
When searching in parallel with many more arguments than threads, the
first arguments are searched last -- unlike in the -j1 case.

This is unexpected for users who know about the parallel nature of rg and
think they can give the scheduler a hint by positioning larger input files
(L1, L2, ..) before smaller ones (█, ██). Instead, this can result in
sub-optimal thread usage and thus longer runtime
(simplified example with 2 threads):

 T1:  █ ██ █ █ █ █ ██ █ █ █ █ █ ██ ╠═════════════L1════════════╣
 T2:  █ █ ██ █ █ ██ █ █ █ ██ █ █ ╠═════L2════╣

                                       ┏━━━━┳━━━━┳━━━━┳━━━━┓
This is caused by assigning work to    ┃ T1 ┃ T2 ┃ T3 ┃ T4 ┃
 per-thread stacks in a round-robin    ┡━━━━╇━━━━╇━━━━╇━━━━┩
              manner, starting here  → │ L1 │ L2 │ L3 │ L4 │ ↵
                                       ├────├────┼────┼────┤
                                       │ s5 │ s6 │ s7 │ s8 │ ↵
                                       ├────┼────┼────┼────┤
                                       ╷ .. ╷ .. ╷ .. ╷ .. ╷
                                       ├────┼────┼────┼────┤
                                       │ st │ su │ sv │ sw │ ↵
                                       ├────┼────┼────┼────┘
                                       │ sx │ sy │ sz │
                                       └────┴────┴────┘
   and then processing them bottom-up:   ↥    ↥    ↥    ↥

                                       ╷ .. ╷ .. ╷ .. ╷ .. ╷
This patch reverses the input order    ├────┼────┼────┼────┤
so the two reversals cancel each other │ s7 │ s6 │ s5 │ L4 │ ↵
out. Now at least the first N          ├────┼────┼────┼────┘
arguments, N=number-of-threads, are    │ L3 │ L2 │ L1 │
processed before any others (then      └────┴────┴────┘
work-stealing may happen):

 T1:  ╠═════════════L1════════════╣ █ ██ █ █ █ █ █ █ ██
 T2:  ╠═════L2════╣ █ █ ██ █ █ ██ █ █ █ ██ █ █ ██ █ █ █

(With some more shuffling T1 could always be assigned L1 etc., but
that would mostly be for optics).
```

---

_Label `rollup` added by @BurntSushi on 2025-07-26 14:24_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
