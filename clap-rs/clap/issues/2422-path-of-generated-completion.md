---
number: 2422
title: Path of generated completion
type: issue
state: closed
author: dalance
labels:
  - A-completion
assignees: []
created_at: 2021-03-26T16:29:38Z
updated_at: 2021-04-06T08:46:51Z
url: https://github.com/clap-rs/clap/issues/2422
synced_at: 2026-01-10T01:27:17Z
---

# Path of generated completion

---

_Issue opened by @dalance on 2021-03-26 16:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

Currently, `clap_generate::generate_to` ( v3 ) and `clap::gen_completions` ( v2 ) don't provide the file path of the generated file.
So user can't show the verbose message like below:

```
$ app --generate-completion bash
completion file is generated: /home/user/app.bash
```

### Describe the solution you'd like

I think `PathBuf` of the generated file can be returned like below.
`Result<PathBuf, std::io::Error>` may be better than `panic!`.

```rust
pub fn generate_to<G, S, T>(app: &mut clap::App, bin_name: S, out_dir: T) -> PathBuf
where
    G: Generator,
    S: Into<String>,
    T: Into<OsString>,
{
    let out_dir = PathBuf::from(out_dir.into());
    let file_name = G::file_name(app.get_bin_name().unwrap());

    let path = out_dir.join(file_name);
    let mut file = match File::create(&path) {
        Err(why) => panic!("couldn't create completion file: {}", why),
        Ok(file) => file,
    };

    generate::<G, S>(app, bin_name, &mut file);
    path
}
```

### Alternatives, if applicable

_No response_

### Additional Context

A related issue: https://github.com/dalance/procs/issues/130



---

_Label `T: new feature` added by @dalance on 2021-03-26 16:29_

---

_Added to milestone `3.0` by @pksunkara on 2021-04-03 21:16_

---

_Label `C: generator` added by @pksunkara on 2021-04-03 21:16_

---

_Comment by @pksunkara on 2021-04-03 21:16_

Should be an easy fix. Would welcome a PR.

---

_Referenced in [clap-rs/clap#2434](../../clap-rs/clap/pulls/2434.md) on 2021-04-05 01:30_

---

_Closed by @pksunkara on 2021-04-06 08:46_

---
