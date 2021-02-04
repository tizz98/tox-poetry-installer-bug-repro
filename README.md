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
    ModuleNotFoundError: No module named 'ipython_genutils'
    
    During handling of the above exception, another exception occurred:
    
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-install-60h8tk8n/ipypandex_68cec70a49ca4cd7b86c78b3788bf74a/setup.py", line 43, in <module>
        setup(
      File "/home/noteable/.pyenv/versions/3.8.6/lib/python3.8/distutils/core.py", line 148, in setup
        dist.run_commands()
      File "/home/noteable/.pyenv/versions/3.8.6/lib/python3.8/distutils/dist.py", line 966, in run_commands
        self.run_command(cmd)
      File "/home/noteable/.pyenv/versions/3.8.6/lib/python3.8/distutils/dist.py", line 985, in run_command
        cmd_obj.run()
      File "/tmp/pip-install-60h8tk8n/ipypandex_68cec70a49ca4cd7b86c78b3788bf74a/setup.py", line 40, in run
        raise RuntimeError("ipypandex requires an ipython installation to work")
    RuntimeError: ipypandex requires an ipython installation to work
```

### Explanation

The `ipython` package is a sub-dependency of `ipypandex` and when installing with `poetry` regularly it works fine because sub-dependencies are installed first. In this case though, the `tox-poetry-installer` plugin is just installing dependencies in alphabetical order which doesn't always work.

Even when specifying `ipython` explicitly as a dev dependency the same issue occurs.

### Proposed fix

`tox-poetry-installer` should use a similar approach as how the `poetry` cli installs packages, see https://github.com/python-poetry/poetry/blob/18a9e2dfad36332c9b6c53f7a1cf980b66325fe4/poetry/installation/installer.py#L223
