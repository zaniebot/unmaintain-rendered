```yaml
number: 4059
title: "`uv` is trying to solve for dependencies that are already installed, failing if they are not available from the package repository"
type: issue
state: closed
author: hoechenberger
labels:
  - compatibility
assignees: []
created_at: 2024-06-05T18:14:43Z
updated_at: 2024-06-06T05:00:17Z
url: https://github.com/astral-sh/uv/issues/4059
synced_at: 2026-01-12T15:58:47Z
```

# `uv` is trying to solve for dependencies that are already installed, failing if they are not available from the package repository

---

_@hoechenberger_

Hello,

I was trying to switch my package installation workflow to `uv`, but I'm facing an issue because in this particular use case, `uv` behaves differently than `pip`.

I'm working on [MNE-Python](https://github.com/mne-tools/mne-python) on `aarch64` (ARM64 Linux running natively on Apple Silicon), and some of my package dependencies are not available from PyPI for this platform. Hence, I install these dependencies from `conda-forge` before installing the remaining dependencies via `pip`:

```shell
$ conda create -c conda-forge -n uv-test python h5py vtk pyside6 cftime psutil
$ conda activate uv-test
$ pip install -e ".[full-pyside6]"
...
Requirement already satisfied: vtk in /opt/conda/envs/uv-test/lib/python3.11/site-packages (from mne==1.8.0.dev106+g17bfca4e2.d20240605) (9.2.6)
...
```
`pip` figures that some dependencies (in this case, I limited the output to the one related to `vtk`) are already installed, so it skips them. The installation completes successfully, yielding an environment with packages that are not available from PyPI, but were installed from `conda-forge` instead.

Now, let's try to reproduce the same via `uv`:
```shell
$ pipx run uv pip install -e ".[full-pyside6]"
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of vtk are available:
          vtk<=9.2.6
          vtk==9.3.0
      and vtk==8.1.0 has no wheels are available with a matching Python implementation, we can conclude that any of:
          vtk<8.1.1
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==8.1.1 has no wheels are available with a matching Python implementation, we can conclude that any of:
          vtk<8.1.2
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==8.1.2 has no wheels are available with a matching Python implementation and vtk==9.0.0 has no wheels are available with a
      matching Python ABI, we can conclude that any of:
          vtk<9.0.1
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==9.0.1 has no wheels are available with a matching Python ABI and vtk==9.0.2 has no wheels are available with a matching Python
      ABI, we can conclude that any of:
          vtk<9.0.3
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==9.0.3 has no wheels are available with a matching Python ABI and vtk==9.1.0 has no wheels are available with a matching Python
      ABI, we can conclude that any of:
          vtk<9.2.2
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==9.2.2 has no wheels are available with a matching Python ABI and vtk==9.2.4 has no wheels are available with a matching Python
      ABI, we can conclude that any of:
          vtk<9.2.5
          vtk>9.2.6,<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==9.2.5 has no wheels are available with a matching Python ABI and vtk==9.2.6 has no wheels are available with a matching Python
      ABI, we can conclude that any of:
          vtk<9.3.0
          vtk>9.3.0
       cannot be used.
      And because vtk==9.3.0 has no wheels are available with a matching Python implementation and mne[full-pyside6]==1.8.0.dev106+g17bfca4e2.d20240605
      depends on vtk, we can conclude that mne[full-pyside6]==1.8.0.dev106+g17bfca4e2.d20240605 cannot be used.
      And because only mne[full-pyside6]==1.8.0.dev106+g17bfca4e2.d20240605 is available and you require mne[full-pyside6], we can conclude that the
      requirements are unsatisfiable.

      hint: mne[full-pyside6] was requested with a pre-release marker (e.g., any of:
          mne[full-pyside6]<1.8.0.dev106+g17bfca4e2.d20240605
          mne[full-pyside6]>1.8.0.dev106+g17bfca4e2.d20240605
      ), but pre-releases weren't enabled (try: `--prerelease=allow`)

      hint: Pre-releases are available for vtk in the requested range (e.g., 9.3.20230807rc0), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```
As you can see, it fails, as `uv` correctly determines that `vtk` is not available from PyPI for my platform.

Unlike `pip`, however, it wasn't happy with the fact that the package is actually already installed, hence there's no need to pull it from PyPI.

Now while I know that it's commonly not recommended to mix `pip`- and `conda`-installed packages in the same environment, this approach has worked quite well for me in the past in this specific situation. It would be great if we could find a solution to this issue with `uv`.

```shell
 $ pipx run uv --version                       
uv 0.2.6
```

---

_Comment by @charliermarsh on 2024-06-05 18:21_

Thanks. In general we do discover already-installed requirements, so I would expect what you described above to work. It's possible, though, that `vtk` is installed in some legacy format that we don't support. I'll try to reproduce.

---

_Label `compatibility` added by @charliermarsh on 2024-06-05 18:21_

---

_Comment by @hoechenberger on 2024-06-05 18:23_

Thanks, @charliermarsh! Please let me know if I should test something or if you need some "actual" MWE (I tried to produce one but failed in my first attempt, so I then ended up posting what I posted above … which is not really minimal.)

---

_Comment by @hoechenberger on 2024-06-05 18:25_

@charliermarsh Also, I can share with you my Dev Container configuration if you want. I'm working on Linux in Docker on an M2 MacBook Pro.

---

_Comment by @charliermarsh on 2024-06-05 23:38_

Ok, it's because `vtk` is being installed as a legacy `.egg-info` file, which we don't support. I think we can probably add support for discovering them, like we support `.egg-info` directories.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-05 23:38_

---

_Closed by @charliermarsh on 2024-06-06 01:00_

---

_Comment by @hoechenberger on 2024-06-06 05:00_

Wow, thanks for the super swift action, @charliermarsh! I'm looking forward to the next release!

---
