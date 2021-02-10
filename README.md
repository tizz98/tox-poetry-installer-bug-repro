# Reproduce tox-poetry-installer Bug

## How to reproduce

_Ensure you have `poetry==1.1.4` installed_

```bash
git clone git@github.com:tizz98/tox-poetry-installer-bug-repro.git
cd tox-poetry-installer-bug-repro

poetry --version  # 1.1.4
pyenv virtualenv 3.8.6 tpibr
pyenv local tpibr

poetry install
tox
```

### Expected result

Pytest should run the tests.

```
==================================== test session starts ====================================
platform linux -- Python 3.8.6, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/noteable/dev/tizz98/tox-poetry-installer-bug-repro
collected 1 item                                                                            

test_foo.py .                                                                         [100%]

===================================== 1 passed in 0.01s =====================================
```

### Error

```
py38 create: /home/noteable/dev/tizz98/tox-poetry-installer-bug-repro/.tox/py38
py38 tox-poetry-installer: Installing 96 dependencies from Poetry lock file
__________________________________________________________________________________________ summary __________________________________________________________________________________________
  py38: commands succeeded
  congratulations :)
Traceback (most recent call last):
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/utils/env.py", line 1070, in _run
    output = subprocess.check_output(
  File "/home/noteable/.pyenv/versions/3.8.6/lib/python3.8/subprocess.py", line 411, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/utils/_compat.py", line 217, in run
    raise CalledProcessError(
poetry.utils._compat.CalledProcessError: Command '['/home/noteable/dev/tizz98/tox-poetry-installer-bug-repro/.tox/py38/bin/pip', 'install', '--no-deps', '-r', '/tmp/pywin32-3002s811h8kreqs.txt']' returned non-zero exit status 1.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/noteable/.pyenv/versions/tpibr/bin/tox", line 8, in <module>
    sys.exit(cmdline())
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/session/__init__.py", line 44, in cmdline
    main(args)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/session/__init__.py", line 69, in main
    exit_code = session.runcommand()
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/session/__init__.py", line 197, in runcommand
    return self.subcommand_test()
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/session/__init__.py", line 225, in subcommand_test
    run_sequential(self.config, self.venv_dict)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/session/commands/run/sequential.py", line 9, in run_sequential
    if venv.setupenv():
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/venv.py", line 628, in setupenv
    status = self.update(action=action)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox/venv.py", line 275, in update
    self.hook.tox_testenv_install_deps(action=action, venv=self)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/hooks.py", line 286, in __call__
    return self._hookexec(self, self.get_hookimpls(), kwargs)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/manager.py", line 93, in _hookexec
    return self._inner_hookexec(hook, methods, kwargs)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/manager.py", line 84, in <lambda>
    self._inner_hookexec = lambda hook, methods, kwargs: hook.multicall(
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/callers.py", line 208, in _multicall
    return outcome.get_result()
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/callers.py", line 80, in get_result
    raise ex[1].with_traceback(ex[2])
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/pluggy/callers.py", line 187, in _multicall
    res = hook_impl.function(*args)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox_poetry_installer/hooks.py", line 148, in tox_testenv_install_deps
    installer.install(poetry, venv, dependencies)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/tox_poetry_installer/installer.py", line 48, in install
    pip.install(dependency)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/installation/pip_installer.py", line 86, in install
    self.run(*args)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/installation/pip_installer.py", line 129, in run
    return self._env.run_pip(*args, **kwargs)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/utils/env.py", line 1042, in run_pip
    return self._run(cmd, **kwargs)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/utils/env.py", line 1332, in _run
    return super(VirtualEnv, self)._run(cmd, **kwargs)
  File "/home/noteable/.pyenv/versions/3.8.6/envs/tpibr/lib/python3.8/site-packages/poetry/utils/env.py", line 1074, in _run
    raise EnvCommandError(e, input=input_)
poetry.utils.env.EnvCommandError: Command ['/home/noteable/dev/tizz98/tox-poetry-installer-bug-repro/.tox/py38/bin/pip', 'install', '--no-deps', '-r', '/tmp/pywin32-3002s811h8kreqs.txt'] errored with the following return code 1, and output: 
ERROR: Could not find a version that satisfies the requirement pywin32==300
ERROR: No matching distribution found for pywin32==300
```

### Explanation

The `pywin32` package is a subdependency of `jupyter-core` which is a subdependency of `nbformat`. When looking at the `poetry.lock` file, there is a line that says `pywin32 = {version = ">=1.0", markers = "sys_platform == \"win32\""}` which indicates this should only be installed on windows OSes.
