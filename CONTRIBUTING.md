Contributing to the CommCare Data Export Tool
=============================================

Thank you for your interest in contributing! This document covers the
contribution process, coding standards, and release procedures.


Getting Started
---------------

1. Sign up for [GitHub](https://github.com) if you haven't already
2. Fork the repository at https://github.com/dimagi/commcare-export
3. Clone your fork, install into a virtualenv, and start a feature
   branch:

```shell
git clone git@github.com:your-username/commcare-export.git
cd commcare-export
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
uv pip install -e ".[test]"
git checkout -b my-feature-branch
```


Making Changes
--------------

1. Create a feature branch from `master`
2. Make your changes following the coding style below
3. Make sure the tests pass:
   ```shell
   pytest
   ```
4. Check type hints (if modifying typed modules):
   ```shell
   mypy --install-types commcare_export/ tests/ migrations/
   ```
5. Push and submit a pull request:
   ```shell
   git push -u origin my-feature-branch
   ```
6. Visit https://github.com/dimagi/commcare-export and submit a pull
   request.

For detailed testing instructions, including database setup and
troubleshooting, see the [Testing Guide](docs/testing.md).


Coding Style
------------

> Perfection is achieved, not when there is nothing more to add, but
> when there is nothing left to take away.
>
> -- Antoine de Saint-Exupéry

### Avoid using comments, docstrings, and type hints.

In Python, comments, docstrings, and type hints, are all forms of
source code documentation. We believe that documentation should explain
the code only when the code is not self-explanatory.

Don't use comments to indicate _what_ the code does; that should be
obvious from the code itself. Use comments to explain _why_ the code
does what it does, and only when it might not be clear.

Avoid docstrings on methods or functions where their purpose is clear
from the name. Use docstrings to give the purpose of a module or class,
if necessary.

Use reStructuredText format in docstrings.

Only use type hints when:

* it would be useful to know a parameter's class,
* or where a parameter's type is not obvious from its name,
* or a function's or method's return value is not obvious from the
  function's or method's name.

If you do use a type hint in a function or method definition, then
include type hints for all its parameters and its return value, for the
sake of readability. Use type aliases (e.g.
`type CredentialsType = tuple[UsernameType, PasswordType]`) where it
would clarify the type or purpose of a variable.

### Tests

The name of a test function/method should explain what it is testing.

Take advantage of pytest features where possible. e.g. Combine
repetitive tests using pytest parametrized tests.

Use [pytest-unmagic](https://github.com/dimagi/pytest-unmagic) to make
pytest fixtures explicit.

Use Pythonic assert statements.

Doctests can augment but should not replace unit tests. Use docstrings
with doctests for functions and methods where a doctest can demonstrate
usage or behavior in a simple way. For example,

```python
# some/module.py

def show_spaces(string):
    """
    Replaces spaces with a middle dot.

    >>> show_spaces('hello world ')
    'hello·world·'

    """
    return string.replace(' ', '\u00b7')
```

Run doctests from an appropriate test module. For example,

```python
# tests/some/module.py
import some.module as module

def test_doctests():
    results = doctest.testmod(module, optionflags=doctest.ELLIPSIS)
    assert results.failed == 0
```


Release Process
---------------

For maintainers only.

1. **Create a tag** for the release:
   ```shell
   git tag "X.Y.Z"
   ```

2. **Create the distribution**:
   ```shell
   uv build
   ```
   Ensure that version number of the archives in `dist/` matches the tag.

3. **Update `CHANGELOG.md`** and open a pull request.

4. When the PR is merged, **push the tag**:
   ```shell
   git push --tags
   ```

Pushing the tag will trigger the GitHub workflow to publish to TestPyPI,
then PyPI, and then create a new GitHub release using the content you
added in `CHANGELOG.md`.
