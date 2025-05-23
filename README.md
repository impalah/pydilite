# pydilite
[![license]](/LICENSE)

---

pydilite is a lightweight dependency injection library for python supporting both sync and async functions.

It is strongly based on [![pythondi]](https://pypi.org/project/pythondi/)

## Installation

```python
pip install pydilite
```

```python
poetry add pydilite
```


## Usage

### Bind classes to the provider

```python
from pydilite import Provider

provider = Provider()
provider.bind(Repo, SQLRepo)
provider.bind(Usecase, CreateUsecase)
```

After binding, configure the provider to the container

```python
from pydilite import configure, configure_after_clear


# Inject with configure
configure(provider=provider)

# Or if you want to fresh inject, use `configure_after_clear`
configure_after_clear(provider=provider)
```

### Define injection

Define the kind of injection you want to use on your clases.


Import inject

```python
from pydilite import inject
```

Add type annotations that you want to inject dependencies

```python
class Usecase:
    def __init__(self, repo: Repo):
        self.repo = repo
```

Add decorator

```python
class Usecase:
    @inject()
    def __init__(self, repo: Repo):
        self.repo = repo
```

Initialize the destination class with no arguments as they are being injected automatically.

```python
usecase = Usecase()
```

Or, you can also inject manually through decorator arguments

```python
class Usecase:
    @inject(repo=SQLRepo)
    def __init__(self, repo):
        self.repo = repo
```

In this case, do not have to configure providers and type annotation.

### Lazy initializing

Using lazy initilization the injected classes will be built when used. It can be used to preinitialize a class
with parameters in the constructor.

```python
from pydilite import Provider

provider = Provider()
provider.bind(Repo, SQLRepo, lazy=True)
```

You can use lazy initializing through `lazy` option. (default `False`)

For singleton, use `lazy=False`.

```python
class Usecase:
    @inject(repo=SQLRepo)
    def __init__(self, repo):
        self.repo = repo
```

By default, manual injection is lazy. If you want a singleton, instantiate it like `repo=SQLRepo()`.


[license]: https://img.shields.io/badge/License-MIT%202.0-blue.svg
