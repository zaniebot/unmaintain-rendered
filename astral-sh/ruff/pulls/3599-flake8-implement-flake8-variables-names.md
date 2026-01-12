```yaml
number: 3599
title: "[`flake8`]: Implement `flake8-variables-names`"
type: pull_request
state: closed
author: latonis
labels: []
assignees: []
base: main
head: flake-8-variable-names
created_at: 2023-03-18T18:27:09Z
updated_at: 2023-06-14T20:23:10Z
url: https://github.com/astral-sh/ruff/pull/3599
synced_at: 2026-01-12T03:43:29Z
```

# [`flake8`]: Implement `flake8-variables-names`

---

_Pull request opened by @latonis on 2023-03-18 18:27_

Implementing [flake8-variables-names](https://github.com/best-doctor/flake8-variables-names)

Referenced and requested in #3463 

---

_Comment by @latonis on 2023-03-18 18:52_

~~Adding the `use-varnames-strict-mode` setting currently. Is there a convention for how the options should be defined or can this be ported over directly?~~

Ended up adding the config option as `use_varnames_strict_mode` and keeping it to as close as original as possible.

---

_Renamed from "[`flake8`]: flake8-variable-names core functionality implemented" to "[`flake8`]: flake8-variables-names core functionality implemented" by @latonis on 2023-03-18 21:04_

---

_Renamed from "[`flake8`]: flake8-variables-names core functionality implemented" to "[`flake8`]: Implement `flake8-variables-names`" by @latonis on 2023-03-18 21:29_

---

_@charliermarsh reviewed on 2023-03-19 03:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:761 on 2023-03-19 03:40_

Nit: I think these should be `VNE` to match the upstream plugin -- or was this an intentional deviation?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_variables_names/settings.rs`:22 on 2023-03-19 03:44_

We can probably just shorten this to `strict`, since unlike in Flake8, this is scoped under the plugin settings, which makes it clear that it applies to `flake8-variable-names`.

---

_@charliermarsh reviewed on 2023-03-19 03:44_

---

_@latonis reviewed on 2023-03-19 03:44_

---

_Review comment by @latonis on `crates/ruff/src/registry.rs`:761 on 2023-03-19 03:44_

I originally had that but changed it due to the few above not having E in their prefix, but makes sense to keep it to the original 🙂

---

_@charliermarsh reviewed on 2023-03-19 03:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_variables_names/settings.rs`:21 on 2023-03-19 03:44_

Can we add some notes on the effect that strict `True` vs. `False` has on the checks?

---

_@charliermarsh reviewed on 2023-03-19 03:51_

This is really good work. My only hesitation here is that this strikes me as a very strict set of rules (e.g., I don't think I'd enable these on my own projects). But, we're yet to define clear criteria for rule and plugin inclusion, and our current approach has generally been not to avoid injecting too much subjectivity into the plugins we choose to incorporate.

For reference, the factors we'd laid out in the past were: (1) some evidence of usefulness / popularity, (2) non-overlapping behavior with existing plugins, (3) reasonably accurate (few false positives and negatives), and (4) complexity doesn't outweigh its usefulness. Though we don't have formal criteria yet. So, I'm open to incorporating. The main "cost" is that we'll likely surface a bunch of violations to those with `--select ALL` enabled, and they'll be forced to disable or fix.

@MichaReiser is thinking about some of these questions, so \cc'ing him :)


---

_Comment by @latonis on 2023-03-19 15:03_

> This is really good work. My only hesitation here is that this strikes me as a very strict set of rules (e.g., I don't think I'd enable these on my own projects). But, we're yet to define clear criteria for rule and plugin inclusion, and our current approach has generally been not to avoid injecting too much subjectivity into the plugins we choose to incorporate.
> 
> For reference, the factors we'd laid out in the past were: (1) some evidence of usefulness / popularity, (2) non-overlapping behavior with existing plugins, (3) reasonably accurate (few false positives and negatives), and (4) complexity doesn't outweigh its usefulness. Though we don't have formal criteria yet. So, I'm open to incorporating. The main "cost" is that we'll likely surface a bunch of violations to those with `--select ALL` enabled, and they'll be forced to disable or fix.
> 
> @MichaReiser is thinking about some of these questions, so \cc'ing him :)

Hey Charlie, absolutely understand the concern for how strict it is, I had the same concern after running it and seeing the ecosystem results. No worries at all if this one is too strict and we don't want to include it at this time (or ever). I'll defer to you if you'd like to keep this open or close it, but I'll update the following requests above and implement the changes requested just in-case it is merged in at some point. :)

---

_Review comment by @latonis on `crates/ruff/src/rules/flake8_variables_names/settings.rs`:22 on 2023-03-19 15:04_

Makes sense to me!

---

_@latonis reviewed on 2023-03-19 15:04_

---

_@latonis reviewed on 2023-03-19 15:18_

---

_Review comment by @latonis on `crates/ruff/src/rules/flake8_variables_names/settings.rs`:21 on 2023-03-19 15:18_

definitely; it has been updated

---

_Comment by @github-actions[bot] on 2023-03-28 01:41_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.4±0.55ms  2041.8 KB/sec    1.01     20.5±0.80ms  2029.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.0±0.18ms     3.3 MB/sec    1.00      4.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.3±34.00µs     4.7 MB/sec    1.00   632.3±24.19µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.8±0.38ms     2.9 MB/sec    1.00      8.6±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.40ms     3.8 MB/sec    1.00     10.6±0.34ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.5 MB/sec    1.03      2.3±0.07ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   265.8±17.16µs    11.1 MB/sec    1.05   280.3±13.96µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.24ms     5.4 MB/sec    1.03      4.9±0.19ms     5.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.4±0.03ms     3.0 MB/sec    1.00     13.3±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   348.6±13.33µs     8.5 MB/sec    1.00    346.0±2.25µs     8.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.02ms     4.6 MB/sec    1.01      5.6±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.02ms     5.8 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1450.4±6.83µs    11.5 MB/sec    1.00   1451.7±4.59µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.9±1.16µs    19.4 MB/sec    1.01    153.4±3.42µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_variables_names/rules/non_descript_variable_name.rs`:49 on 2023-03-28 09:59_

Nit: use denylist instead of blocklist (more inclusive)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_variables_names/rules/single_letter_variable_name.rs`:35 on 2023-03-28 10:01_

Nit: you can write this with a match
```suggestion
        && (!matches!(name, "i", "_", "T") || (strict_mode && !matches!(name, "_", "T)))
```

---

_@MichaReiser approved on 2023-03-28 10:02_

---

_@latonis reviewed on 2023-03-30 13:06_

---

_Review comment by @latonis on `crates/ruff/src/rules/flake8_variables_names/rules/single_letter_variable_name.rs`:35 on 2023-03-30 13:06_

> Nit: you can write this with a match

```
no rules expected the token `"_"`                        
  --> crates/ruff/src/rules/flake8_variables_names/rules/single_
letter_variable_name.rs:33:34                                   
   |                                                            
33 |         && (!matches!(name, "i", "_", "T") || (strict_mode 
&& !matches!(name, "_", "T")))                                     |                                  ^^^ no rules expected this token in macro call                                               |                                                               = note: while trying to match sequence end  
```

Currently getting this when implementing as a match statement

---

_Review comment by @latonis on `crates/ruff/src/rules/flake8_variables_names/rules/non_descript_variable_name.rs`:49 on 2023-03-30 13:14_

Implemented :) 

---

_@latonis reviewed on 2023-03-30 13:14_

---

_@latonis reviewed on 2023-03-30 13:17_

---

_Review comment by @latonis on `crates/ruff/src/rules/flake8_variables_names/rules/single_letter_variable_name.rs`:35 on 2023-03-30 13:17_

Ah I see what I did incorrectly

needs to be 
```
!matches!(name, "i" | "_" | "T") 
```

not 
```
matches!(name, "i", "_", "T") 
```

---

_Review requested from @charliermarsh by @latonis on 2023-03-30 13:52_

---

_Comment by @latonis on 2023-04-01 00:53_

pre-commit failed due to my own rules, not sure if that is the intended result? @charliermarsh :smile_cat: 
![image](https://user-images.githubusercontent.com/6127011/229257580-5eea098a-af97-499e-8242-cb2b29e8d64c.png)


---

_Closed by @latonis on 2023-06-14 20:13_

---

_Branch deleted on 2023-06-14 20:14_

---

_Comment by @charliermarsh on 2023-06-14 20:15_

Sorry for letting this sit @latonis, and for not giving clearer feedback prior to working on the issue. There is actually a chance we re-open this in the future, since it is a popular plugin... But I will take ownership of rebasing etc. if that time comes.

---

_Comment by @latonis on 2023-06-14 20:22_

No worries at all @charliermarsh, I closed per our previous conversations about not being sure about too strict of rules being included when running all of them and that sort. 

Happy to re-do the work if the time comes, I just figured with the amount of improvements ruff has had internally it would probably be easier to re-implement the logic rather than try to re-base it 🙂

---

_Branch restored on 2023-06-14 20:23_

---
