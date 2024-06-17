# PDM isolation issue

> This is a showcase project for https://github.com/pdm-project/pdm/issues/2944, not a project template.

This project shows an issue with installing packages in non-isolation mode with PDM.

This project will normally not install, since the build-system requires a non-existant dependency. This project does make clear the difference in how pip and pdm deal with isolation in the build environment.
 
First, let's create a venv with pip, since we need pip later:

```bash
pdm venv create --with-pip
```

The `--no-isolation` flag configured as default in `pyproject.toml`.

Output from `pdm install`:

```
STATUS: Resolving packages from lockfile...
All packages are synced to date, nothing to do.
Installing the project as an editable package...
  ✖ Update pdm-isolation-issue 0.0.0 -> 0.0.0 failed

See /Users/arjan/Library/Logs/pdm/pdm-install-1mphslqa.log for detailed debug log.
[CandidateNotFound]: Unable to find candidates for not-a-valid-dependency. There may exist some issues with the package name or network condition.
WARNING: Add '-v' to see the detailed traceback
```

This errors, since `not-a-valid-dependency` cannot be found.

If you try something similar with pip, it works fine:

```bash
pdm run pip install --no-build-isolation -e .
```

```
See how pip completely ignores the requires 
Obtaining file:///Users/arjan/Development/pdm-isolation-issue
  Checking if build backend supports build_editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: pdm-isolation-issue
  Building editable for pdm-isolation-issue (pyproject.toml) ... done
  Created wheel for pdm-isolation-issue: filename=pdm_isolation_issue-0.0.0-0.editable-py3-none-any.whl size=3174 sha256=22c4805dc722286ee127733e9e25371c09f2f74f6027f9237b69a616a51c7f7f
  Stored in directory: /private/var/folders/l8/5_rjsk6x11xcvhv7jj4k59mw0000gp/T/pip-ephem-wheel-cache-pbdbv6r3/wheels/6b/37/4f/9d916904a97d98f19fa901a471956ad96111cd8908787ee002
Successfully built pdm-isolation-issue
Installing collected packages: pdm-isolation-issue
Successfully installed pdm-isolation-issue-0.0.0
```

Pip completely ignores `build-system.requires` when installing in non-isolated mode.

