---
number: 5709
title: Rustdoc is not scraping examples
type: issue
state: closed
author: willcrichton
labels: []
assignees: []
created_at: 2024-08-30T17:22:46Z
updated_at: 2024-08-30T18:38:02Z
url: https://github.com/clap-rs/clap/issues/5709
synced_at: 2026-01-10T01:28:15Z
---

# Rustdoc is not scraping examples

---

_Issue opened by @willcrichton on 2024-08-30 17:22_

The `-Zrustdoc-scrape-examples` flag is being [ignored on docs.rs](https://docs.rs/crate/clap/4.5.16/builds/1325757) due to rust-lang/cargo#11430, which requires users to explicitly indicate that examples which rely on dev-dependencies should be scraped. (This avoids breakage because `cargo doc` does not normally require dev-dependencies.)

If the maintainers still want to use this feature, you can add `doc-scrape-examples = true` to one `[[example]]` configuration, for instance:

```toml
[[example]]
name = "demo"
required-features = ["derive"]
doc-scrape-examples = true
```

---

_Referenced in [clap-rs/clap#5710](../../clap-rs/clap/pulls/5710.md) on 2024-08-30 18:10_

---

_Comment by @epage on 2024-08-30 18:12_

While I'm fixing that, I hope its realized that opt-in for this is unsustainable.  clap is a bit unique with its examples and manually specifies most.  Having to manually specify examples in all of my projects won't work.  https://github.com/rust-lang/cargo/issues/6945 would help but then I still have to make sure I set this in every `Cargo.toml` in every repo I maintain.  Thats with knowing about this and I overlooked this even though I saw the PRs going through the Cargo repo.

---

_Comment by @willcrichton on 2024-08-30 18:36_

Yes I totally agree with the maintenance burden. My goal is to get `-Z rustdoc-scrape-examples` (or whatever it turns into once stabilized) to implicitly mean "I accept that `cargo doc` will include dev-dependencies". Your comment helps me advance the case.

---

_Closed by @epage on 2024-08-30 18:38_

---

_Closed by @epage on 2024-08-30 18:38_

---
