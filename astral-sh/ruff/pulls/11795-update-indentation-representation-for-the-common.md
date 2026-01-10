```yaml
number: 11795
title: Update indentation representation for the common case
type: pull_request
state: closed
author: dhruvmanila
labels: []
assignees: []
base: main
head: dhruv/indentations
created_at: 2024-06-07T13:26:32Z
updated_at: 2024-06-14T06:25:37Z
url: https://github.com/astral-sh/ruff/pull/11795
synced_at: 2026-01-10T21:56:00Z
```

# Update indentation representation for the common case

---

_Pull request opened by @dhruvmanila on 2024-06-07 13:26_

## Summary



## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-06-07 13:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2024-06-14 05:12_

Update: This doesn't seem to provide any performance improvements on Codspeed although locally it gives me around 5% difference in the parser benchmarks.

```
group                         indentations-parser                    main-parser
-----                         -------------------                    -----------
parser/large/dataset.py       1.00    661.6±2.31µs    61.5 MB/sec    1.03    681.6±2.58µs    59.7 MB/sec
parser/numpy/ctypeslib.py     1.00    107.1±0.35µs   155.5 MB/sec    1.05    112.0±0.40µs   148.7 MB/sec
parser/numpy/globals.py       1.00      8.2±0.03µs   359.7 MB/sec    1.05      8.6±0.03µs   343.0 MB/sec
parser/pydantic/types.py      1.00    251.8±0.88µs   101.3 MB/sec    1.04    262.1±0.98µs    97.3 MB/sec
parser/unicode/pypinyin.py    1.00     33.0±0.20µs   127.5 MB/sec    1.03     33.9±0.15µs   124.0 MB/sec
```

I've tried the following three representations:

1. Save the first indentation and keep a counter which is incremented and decremented accordingly. If the new indentation isn't in multiples of the first indentation, we convert it into a stack and continue.
```rs
struct IndentationCounter {
    /// The first indentation in the source code.
    first: Option<Indentation>,
    /// The current indentation level.
    level: u32,
}
```

2. Similar to (1), but adds a new field which reflects the current indentation. This optimizes for accessing the current indentation while in (1) the current indentation needs to be computed which adds a bit of overhead.
```rs
struct IndentationCounter {
    /// The current indentation.
    current: Indentation,
    /// The first indentation in the source code.
    first: Option<Indentation>,
    /// The current indentation level.
    level: u32,
}
```

3. Represent an indentation at a more granular level by including the indent style. This will be checked against the new indentation style and creates a short-circuiting behavior to avoid checking the size. But, this adds an overhead of creating the indent style flag in the lexer.

```rs
bitflags! {
    #[derive(Debug, Default, Copy, Clone, PartialEq, Eq)]
    pub(super) struct IndentStyle: u8 {
        const SPACE = 1 << 0;
        const TAB = 1 << 1;
    }
}

#[derive(Debug, Default, Clone)]
struct IndentationCounter {
    /// The indentation style i.e., tabs or spaces
    style: IndentStyle,
    /// The size of a single indentation in terms of the number of characters.
    size: Character,
    /// The current indentation level.
    level: u32,
}
```

I think the reason it doesn't really give performance improvement is that the overhead of checking whether the indentation changed and computing the current indentation offsets any performance gains.

Currently, the change in this PR reflects solution (2) from above. I don't think it's worth adding the complexity if there's no performance gains from it which is why I'm thinking of removing it.

---

_Marked ready for review by @dhruvmanila on 2024-06-14 05:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-14 05:13_

---

_Closed by @dhruvmanila on 2024-06-14 05:13_

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-06-14 05:13_

---

_Renamed from "WIP: Update indentation representation for the common case" to "Update indentation representation for the common case" by @dhruvmanila on 2024-06-14 05:13_

---

_Comment by @MichaReiser on 2024-06-14 06:13_

Codspeed does show a 2% lexer perf improvement and about a 1% for the parser. 

---

_Comment by @dhruvmanila on 2024-06-14 06:25_

> Codspeed does show a 2% lexer perf improvement and about a 1% for the parser.

Yeah, I'm not sure if the increase in code complexity is worth the nominal gain

---
