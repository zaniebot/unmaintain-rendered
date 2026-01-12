```yaml
number: 764
title: Avoid allocations for binding values
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/bind-ref
created_at: 2022-11-16T05:17:49Z
updated_at: 2022-11-16T13:55:36Z
url: https://github.com/astral-sh/ruff/pull/764
synced_at: 2026-01-12T15:55:05Z
```

# Avoid allocations for binding values

---

_@charliermarsh_

This achieves a 1-1.5% performance improvement on the benchmark (shrug).


---

_Label `performance` added by @charliermarsh on 2022-11-16 05:17_

---

_@Stranger6667 reviewed on 2022-11-16 10:28_

---

_Review comment by @Stranger6667 on `src/ast/helpers.rs`:233 on 2022-11-16 10:28_

I think that this clone could be avoided as well. E.g. if the string is constructed manually with a series of “push_str” and “push” calls

---

_@messense reviewed on 2022-11-16 11:02_

---

_Review comment by @messense on `src/ast/helpers.rs`:229 on 2022-11-16 11:02_

From Rust API point of view, I think it's kinda odd to pass around `&Option<T>`, `Option<&T>` is much more natural. You can get a `Option<&T>` via [`Option::as_ref()`](https://doc.rust-lang.org/std/option/enum.Option.html#method.as_ref).

And to get a `Option<&str>` from a `&Option<String>` you can use [Option::as_deref()](https://doc.rust-lang.org/std/option/enum.Option.html#method.as_deref).

---

_@Stranger6667 reviewed on 2022-11-16 12:00_

---

_Review comment by @Stranger6667 on `src/ast/helpers.rs`:233 on 2022-11-16 12:00_

To compare these approaches I benched the following:

```rust
pub fn original(level: &Option<usize>, module: &Option<String>) -> String {
    format!(
        "{}{}",
        ".".repeat(level.unwrap_or_default()),
        module.clone().unwrap_or_default()
    )
}

pub fn second(level: &Option<usize>, module: &Option<String>) -> String {
    let mut out = String::new();
    for _ in 0..level.unwrap_or_default() {
        out.push('.');
    }
    if let Some(m) = module {
        out.push_str(m.as_str());
    }
    out
}

pub fn third(level: Option<usize>, module: Option<&str>) -> String {
    let mut out = String::with_capacity(16);
    for _ in 0..level.unwrap_or_default() {
        out.push('.');
    }
    if let Some(m) = module {
        out.push_str(m);
    }
    out
}

fn simple(c: &mut Criterion) {
    let level = black_box(3);
    let module = black_box("foobarbaz".to_string());
    let module_str = black_box("foobarbaz");
    c.bench_function("original", |b| b.iter(|| original(&Some(level), &Some(module.clone()))));
    c.bench_function("second", |b| b.iter(|| second(&Some(level), &Some(module.clone()))));
    c.bench_function("third", |b| b.iter(|| {
        // All benches make a String clone
        let _ = module.clone();
        third(Some(level), Some(module_str))
    }));
}
```


And here are the results:

```
original                time:   [97.946 ns 98.351 ns 98.802 ns]                     
                        change: [-0.6825% -0.2181% +0.3228%] (p = 0.45 > 0.05)
                        No change in performance detected.
Found 13 outliers among 100 measurements (13.00%)
  1 (1.00%) low mild
  8 (8.00%) high mild
  4 (4.00%) high severe

second                  time:   [50.236 ns 50.383 ns 50.532 ns]                    
                        change: [+1.7089% +2.0963% +2.5148%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) low mild
  2 (2.00%) high mild

third                   time:   [29.787 ns 29.853 ns 29.922 ns]                   
                        change: [-0.7170% -0.2109% +0.3007%] (p = 0.42 > 0.05)
                        No change in performance detected.
Found 6 outliers among 100 measurements (6.00%)
  2 (2.00%) high mild
  4 (4.00%) high severe
```

It is absolutely not exhaustive, but I think it gives some intuition on what takes how much. The string capacity is somewhat arbitrary.

---

_Review comment by @charliermarsh on `src/ast/helpers.rs`:229 on 2022-11-16 13:53_

Oh, thank you! That's really helpful feedback. I'm gonna fix this everywhere in a follow-up PR.

---

_@charliermarsh reviewed on 2022-11-16 13:53_

---

_@charliermarsh reviewed on 2022-11-16 13:53_

---

_Review comment by @charliermarsh on `src/ast/helpers.rs`:233 on 2022-11-16 13:53_

This is great, thanks! Always appreciate a benchmark :)

---

_Merged by @charliermarsh on 2022-11-16 13:55_

---

_Closed by @charliermarsh on 2022-11-16 13:55_

---

_Branch deleted on 2022-11-16 13:55_

---
