```yaml
number: 16533
title: uv crash with exit code 132 on older CPUs
type: issue
state: closed
author: eyalmazuz
labels:
  - bug
assignees: []
created_at: 2025-10-31T10:49:07Z
updated_at: 2025-10-31T14:53:35Z
url: https://github.com/astral-sh/uv/issues/16533
synced_at: 2026-01-12T16:02:33Z
```

# uv crash with exit code 132 on older CPUs

---

_@eyalmazuz_

### Summary

We have a slurm cluster, and some of the compute nodes have Intel Xeon E5-2640 v2 (32)  CPUs


## REPL error
when I run in my project directory the command
```bash
uv run python3
```

It opens the REPL 
but when I do something like
```py
import polars
```
It just exits without any output whatsoever

<img width="648" height="169" alt="Image" src="https://github.com/user-attachments/assets/5dbff906-0d75-498e-b49b-62898c5b8e16" />


## Sbatch error

Similarly if I submit a job via sbatch I get an exit code 132

<img width="587" height="145" alt="Image" src="https://github.com/user-attachments/assets/08e266aa-7329-4a1f-8bbc-dad77d8263c7" />

These are the contents of my sbatch file

```
#!/bin/bash

#SBATCH --partition cpu
#SBATCH --time 7-00:00:00
#SBATCH --job-name gemini-question-generation	
#SBATCH --output jobs/job-%J.out
#SBATCH --mem=4G

### Print some data to output file ###
echo `date`
echo -e "\nSLURM_JOBID:\t\t" $SLURM_JOBID
echo -e "SLURM_JOB_NODELIST:\t" $SLURM_JOB_NODELIST "\n\n"

export GEMINI_API_KEY="KEYKEYKEYKEYKEYKEYKEY"

srun --unbuffered --export=ALL uv run gemini_eval.py \
    --prompt-path ./data/prompts/NACO_prompt.txt \
    --data-path ./data/qa_test.csv \
    --model "gemini-2.5-pro"
```



But on other nodes that for example have AMD EPYC 7302 CPUs both methods works just fine
I can import polars, and I can submit a job and have it run until completion 


Here is the contents of my pyproject.toml

```toml
[project]
name = "question-generation"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "accelerate>=1.10.1",
    "bitsandbytes>=0.48.1",
    "google-genai>=1.46.0",
    "openai>=2.3.0",
    "optimum>=2.0.0",
    "pillow>=11.3.0",
    "polars>=1.34.0",
    "scikit-learn>=1.7.2",
    "torch>=2.8.0",
    "transformers>=4.57.0",
]

[tool.ruff]
# Always generate Python 3.11-compatible code.
target-version = "py311"
```

### Platform

Rocky Linux 9.5 (Blue Onyx) 
Kernel: 5.14.0-503.29.1.el9_5.x86_64 

### Version

uv 0.9.6

### Python version

Python 3.11.11

---

_Label `bug` added by @eyalmazuz on 2025-10-31 10:49_

---

_Comment by @konstin on 2025-10-31 14:31_

Does this happen with pip too, or only with uv? Most likely, polars requires newer CPU instructions than that CPU supports. Python packages don't encode this information, so it's impossible for uv to tell.

---

_Comment by @eyalmazuz on 2025-10-31 14:53_

> Does this happen with pip too, or only with uv? Most likely, polars requires newer CPU instructions than that CPU supports. Python packages don't encode this information, so it's impossible for uv to tell.

I ditched pip a long while ago, it never occurred to me to check 😅 
Yeah, apparently it also fails on pip, and somehow running python with vent instead of uv does give a detailed message 
I wonder why UV just kicks the bucket without putting anything? 🤔 

```py
>>> import polars
/home/mazuze/venv/lib64/python3.9/site-packages/polars/_cpu_check.py:250: RuntimeWarning: Missing required CPU features.

The following required CPU features were not detected:
    avx2, fma, bmi1, bmi2, lzcnt, movbe
Continuing to use this version of Polars on this processor will likely result in a crash.
Install `polars[rtcompat]` instead of `polars` to run Polars with better compatibility.

Hint: If you are on an Apple ARM machine (e.g. M1) this is likely due to running Python under Rosetta.
It is recommended to install a native version of Python that does not run under Rosetta x86-64 emulation.

If you believe this warning to be a false positive, you can set the `POLARS_SKIP_CPU_CHECK` environment variable to bypass this check.

  warnings.warn(
Illegal instruction (core dumped)
```

Since it's not a uv issue
I'll close the issue now

Thank you for your help

---

_Closed by @eyalmazuz on 2025-10-31 14:53_

---
