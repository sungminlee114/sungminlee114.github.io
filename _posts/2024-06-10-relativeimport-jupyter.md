---
layout: post
title: Relative-import error when using jupyter notebook
description: This article introduces a solution to the relative-import error when using the jupyter notebook.
date: 2024-06-10 15:57:00 +0900
categories: [Experiences, Debug, Test]
tags: python jupyter-notebook debug
---

## The annoying relative-import error
The annoying **relative-import error** frequently occurs when using python.
```python
ImportError: attempted relative import with no known parent package
```

When using jupyter notebook, solving it becomes harder because two commonly known solutions doesn't apply.


```python
sys.path.append(os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))
```
or

```python
python -m relative.modules
```

Specifically, this is because you can't access to `__file__` nor pass the `-m` argument when executing`.ipynb` files with jupyter notebook.

## Solution for the issue
After some workarounds, I found a new solution for the issue.
Instead of passing `-m`, **you can add the following lines at the top of each `.ipynb` file.**

```python
if __package__ is None:
    # Set the top level package
    # Analogus to python -m relative.modules
    __package__ = 'relative.modules'
```
This will enable you to use relative imports such as:
```python
from . import *
from .. import utils
```