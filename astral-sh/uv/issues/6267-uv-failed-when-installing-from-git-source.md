```yaml
number: 6267
title: uv failed when installing from git source
type: issue
state: open
author: robert1003
labels:
  - uv pip
assignees: []
created_at: 2024-08-20T18:49:23Z
updated_at: 2024-08-20T18:52:26Z
url: https://github.com/astral-sh/uv/issues/6267
synced_at: 2026-01-12T15:59:02Z
```

# uv failed when installing from git source

---

_@robert1003_

As titled, uv failed the following command while pip works. Am I missing anything here? Thank you!

```
export UV_INDEX_STRATEGY="unsafe-best-match"
export UV_NO_BUILD_ISOLATION="1"

mamba create \
  --prefix $HOME/.conda/envs/test_uv2 \
  --channel conda-forge \
  --channel anaconda \
  --channel pytorch \
  --yes \
  python==3.11.6 cmake cython libopenblas libprotobuf libunwind "pip>=22.2" psycopg2 py-opencv==4.7.0 pybind11

conda activate $HOME/.conda/envs/test_uv2

TARGET_FOLDER=$(mktemp -d)
RL_GAMES_TMPDIR=$(mktemp -d)
git clone https://github.com/Denys88/rl_games.git $RL_GAMES_TMPDIR
sed -i 's/python = ">=3.7.1,<3.11"/python = ">=3.7.1,<3.12"/g' $RL_GAMES_TMPDIR/pyproject.toml
sed -i "s/version='1.6.1'/version='1.6.1.ap'/g" $RL_GAMES_TMPDIR/setup.py
uv pip install --target $TARGET_FOLDER --no-deps $RL_GAMES_TMPDIR
```
failed with
```
❯ uv pip install --target $TARGET_FOLDER --no-deps $RL_GAMES_TMPDIR
⠙ Resolving dependencies...                                                                                                                                                                                           error: Failed to build `rl-games @ file:///tmp/tmp.h3jt96KBny`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'poetry'
---
```
while pip works:
```
❯ pip install --target $TARGET_FOLDER --no-deps $RL_GAMES_TMPDIR
Processing /tmp/tmp.h3jt96KBny
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: rl_games
  Building wheel for rl_games (pyproject.toml) ... done
  Created wheel for rl_games: filename=rl_games-1.6.1-py3-none-any.whl size=235795 sha256=d39bb3c42204893dd9827e63aac04612fd583ef9bbf1a673d513ccdb115aed62
  Stored in directory: /tmp/pip-ephem-wheel-cache-4ae9vbwt/wheels/87/a4/0f/5749b5c55fc514e636709ce26ba003bb3cc8660414797961e7
Successfully built rl_games
Installing collected packages: rl_games
Successfully installed rl_games-1.6.1
```

---

_Label `uv pip` added by @zanieb on 2024-08-20 18:52_

---
