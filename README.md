# namedtupled

[namedtuples](https://docs.python.org/3/library/collections.html) are immutable, performant and classy. **namedtupled** is a lightweight wrapper for recursively mapping namedtuples from nested dicts, lists, json and yaml. Inspired by [hangtwenty](https://gist.github.com/hangtwenty/5960435).


# Installation

```python
pip install namedtupled
```


# Getting started

```python
import namedtupled

data = {'binks': {'says': 'meow'}}
cat = namedtupled.map(data)

cat  # NT(binks=NT(says='meow'))

cat.binks.says  # 'meow'
```


## Usage

Create namedtuples with methods: [map](#map), [json](#json), [yaml](#yaml), [zip](#zip), [env](#env) and helper method [ignore](#ignore).

Optionally name your namedtuples by passing a `name` argument to any of the methods -- the default name is simply 'NT'.

```python
cat = namedtupled.map(data, name='Cat')

cat  # Cat(binks=NT(says='meow'))
```

### map()

Recursively convert mappings like nested dicts, or lists of dicts, into to namedtuples.

*args: mapping, name='NT'*

From dict:

```python
data = {'binks': {'says': 'meow'}}
cat = namedtupled.map(data)

cat.bink.says  # 'meow'
```

From list:

```python
data = [{'id': 'binks', 'says': 'meow'}, {'id': 'cedric', 'says': 'prrr'}]
cats = namedtupled.map(data)

cats[1].says  # 'prrr'
```

### json()

Map namedtuples from json data.

*args: data=None, path=None, name='NT'*

Inline:

```python
data = """{"binks": {"says": "meow"}}"""
cat = namedtupled.json(data)

cat.binks.says  # 'meow'
```

Or specify path for a json file:

```python
cat = namedtupled.json(path='cat.json')

cat.binks.says  # 'meow'
```

### yaml()

Map namedtuples from yaml data.

*args: data=None, path=None, name='NT'*

Inline:

```python
data = """
binks:
  says: meow
"""
cat = namedtupled.yaml(data)

cat.binks.says  # 'meow'
```

Or specify path for a yaml file:

```python
cat = namedtupled.yaml(path='cat.yaml')

cat.binks.says  # 'meow'
```

### zip()

Map namedtuples given a pair of key, value lists.

*args: keys=[], values=[], name='NT'*

Example:

```python
keys, values = ['id', 'says'], ['binks', 'prrr']
cat = namedtupled.zip(keys, values)

cat.says  # 'prrr'
```

### env()

Returns a namedtuple from a list of environment variables. If not found in shell, gets input with *input* or *getpass*.

*args: keys=[], name='NT', use_getpass=False*

In shell:

```bash
export USERNAME="binks"
export APIKEY="c4tnip!"
```

Then in python:

```python
variables = ['USERNAME', 'APIKEY']
env = namedtupled.env(variables)

env.username  # 'binks'
```

### ignore()

Use ignore to prevent a mapping from being converted to a namedtuple.

*args: mapping*

Example usage:

```python
data = {'binks': namedtupled.ignore({'says': 'meow'})}
cat = namedtupled.map(data)

cat.binks  # {'says': 'meow'}
```

# Development

PRs welcome, tests run with:

```python
pip install pytest pytest-cov pytest-datafiles
python -m pytest --cov=namedtupled/ tests
```
